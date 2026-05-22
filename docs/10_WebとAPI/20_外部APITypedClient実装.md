# 外部 API Typed Client 実装

## 目的

ASP.NET Core から外部 API を呼ぶときに、`HttpClientFactory`、options、DTO、timeout、error handling を組み合わせる基本を理解します。

## 前提

- [外部 API 連携](12_外部API連携.md) を読んでいる
- [HttpClientFactory](../04_標準ライブラリ/09_HttpClientFactory.md) を読んでいる
- [IOptions と Configuration](../07_設計と実務パターン/10_IOptionsとConfiguration.md) を読んでいる

## 要点

- 外部 API ごとに typed client を作ると、base address、timeout、header、response parsing、error handling を一箇所に集められます。
- URL、API key、timeout は code に直書きせず、configuration から渡します。secret は user secrets、環境変数、secret manager などで管理します。
- 外部 API の response DTO を domain model として使い回さず、自アプリ内の model へ mapping します。外部仕様変更の影響を閉じ込めやすくなります。
- `EnsureSuccessStatusCode` だけに頼ると、status code ごとの扱いを設計しにくい場合があります。`404`、`429`、`5xx`、timeout をどう扱うか決めます。
- timeout を設定しない外部 API 呼び出しは、障害時に thread や connection を長時間占有します。client、handler、operation の timeout を設計します。
- retry は万能ではありません。冪等な操作か、相手 API の rate limit、二重登録の可能性を確認します。

## options を定義する

```csharp
public sealed class WeatherApiOptions
{
    public string BaseUrl { get; init; } = string.Empty;

    public string ApiKey { get; init; } = string.Empty;

    public int TimeoutSeconds { get; init; } = 10;
}
```

## typed client を実装する

```csharp
public sealed class WeatherApiClient
{
    private readonly HttpClient httpClient;
    private readonly WeatherApiOptions options;
    private readonly ILogger<WeatherApiClient> logger;

    public WeatherApiClient(
        HttpClient httpClient,
        IOptions<WeatherApiOptions> options,
        ILogger<WeatherApiClient> logger)
    {
        this.httpClient = httpClient;
        this.options = options.Value;
        this.logger = logger;
    }

    public async Task<WeatherSummary?> GetCurrentAsync(string city, CancellationToken cancellationToken)
    {
        var path = $"/current?city={Uri.EscapeDataString(city)}";

        using var request = new HttpRequestMessage(HttpMethod.Get, path);
        request.Headers.Add("X-Api-Key", options.ApiKey);

        using var response = await httpClient.SendAsync(request, cancellationToken);

        if (response.StatusCode == HttpStatusCode.NotFound)
        {
            return null;
        }

        if (!response.IsSuccessStatusCode)
        {
            logger.LogWarning("Weather API failed with status {StatusCode}", response.StatusCode);
            throw new ExternalApiException("Weather API request failed.");
        }

        var dto = await response.Content.ReadFromJsonAsync<WeatherResponse>(cancellationToken);

        return dto is null ? null : new WeatherSummary(dto.City, dto.TemperatureCelsius);
    }
}
```

## DI に登録する

```csharp
builder.Services.Configure<WeatherApiOptions>(
    builder.Configuration.GetSection("WeatherApi"));

builder.Services.AddHttpClient<WeatherApiClient>((serviceProvider, client) =>
{
    var options = serviceProvider.GetRequiredService<IOptions<WeatherApiOptions>>().Value;

    client.BaseAddress = new Uri(options.BaseUrl);
    client.Timeout = TimeSpan.FromSeconds(options.TimeoutSeconds);
});
```

## appsettings の例

```json
{
  "WeatherApi": {
    "BaseUrl": "https://api.example.com",
    "TimeoutSeconds": 10
  }
}
```

API key は `appsettings.json` に直書きせず、user secrets や環境変数で渡します。

## コードの読み方

`WeatherApiClient` は外部 API 呼び出しの詳細を隠す class です。`HttpRequestMessage` に API key header を付け、`SendAsync` で呼び出します。`404` は「天気情報なし」として `null` を返し、それ以外の失敗 status はログを出して例外にしています。最後に外部 DTO の `WeatherResponse` から内部向けの `WeatherSummary` へ変換しています。

## 実務での使い方

外部 API は、自分たちで制御できない障害点です。timeout、retry、rate limit、監視、ログ、fallback、secret 管理を最初から設計します。API key や token をログに出さないようにし、request / response body に個人情報が含まれる場合は記録範囲を制限します。

## よくあるミス

- `new HttpClient()` を各所で直接作る。
- API key を source code や repository に commit する。
- timeout を設定せず、外部 API 障害時に待ち続ける。
- 外部 API の DTO を domain model として使い回す。
- non-success status をすべて同じ例外にして、retry 可否や利用者への返し方を判断できない。

## 練習問題

1. `WeatherApiOptions` を作成し、configuration から読み込む。
2. typed client を `AddHttpClient` で登録する。
3. `404` と `5xx` で扱いを分ける。
4. 外部 response DTO を内部 model に mapping する。

## 関連リンク

- [外部 API 連携](12_外部API連携.md)
- [HttpClientFactory](../04_標準ライブラリ/09_HttpClientFactory.md)
- [Make HTTP requests using IHttpClientFactory](https://learn.microsoft.com/aspnet/core/fundamentals/http-requests)
