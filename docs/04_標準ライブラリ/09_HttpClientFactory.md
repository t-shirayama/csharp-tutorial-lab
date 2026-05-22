# HttpClientFactory

## 目的

`IHttpClientFactory` を使い、外部 API 呼び出しの `HttpClient` を安全に管理します。

## 前提

- [HttpClient](06_HttpClient.md) を読んでいる
- [DI コンテナの実装](../07_設計と実務パターン/09_DIコンテナの実装.md) を読んでいる

## 要点

- `new HttpClient()` を呼び出しごとに作るのは避けます。
- `IHttpClientFactory` は `HttpClient` の生成と handler lifetime を管理します。
- typed client にすると外部 API ごとの責務を分けやすくなります。

## typed client の例

```csharp
// WeatherApiClient 用の HttpClient を DI に登録します。
builder.Services.AddHttpClient<WeatherApiClient>(client =>
{
    // 外部 API の基準 URL と timeout を client ごとに設定します。
    client.BaseAddress = new Uri("https://api.example.com");
    client.Timeout = TimeSpan.FromSeconds(10);
});

public class WeatherApiClient
{
    // IHttpClientFactory が管理する HttpClient が注入されます。
    private readonly HttpClient httpClient;

    public WeatherApiClient(HttpClient httpClient)
    {
        this.httpClient = httpClient;
    }

    // cancellationToken を渡し、呼び出し元からキャンセルできるようにします。
    public async Task<string> GetAsync(CancellationToken cancellationToken)
        => await httpClient.GetStringAsync("/weather", cancellationToken);
}
```

## コードの読み方

`AddHttpClient<WeatherApiClient>` は typed client の登録です。`WeatherApiClient` が作られるとき、設定済みの `HttpClient` が constructor に渡されます。外部 API ごとの URL や timeout を client に閉じ込めることで、呼び出し側は HTTP の詳細を意識しなくて済みます。

## 実務での使い方

外部 API ごとに typed client を作り、URL、timeout、認証 header、ログ、リトライ方針をまとめます。秘密情報は設定や secret 管理から渡します。

## よくあるミス

- 呼び出しごとに `new HttpClient()` する。
- timeout を設定しない。
- API ごとの設定が `Program.cs` に散らばる。
- 例外や non-success status の扱いを決めていない。

## 関連リンク

- [IHttpClientFactory with .NET](https://learn.microsoft.com/dotnet/core/extensions/httpclient-factory)
