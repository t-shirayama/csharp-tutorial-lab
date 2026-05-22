# HttpClient

## 目的

外部 API へ HTTP リクエストを送る `HttpClient` の基本を理解します。

## 要点

- `HttpClient` は HTTP 通信を行うためのクラスです。GET や POST を送るだけでなく、header、content、status code、timeout、cancellation まで含めて扱います。
- 実務では使い捨てではなく、DI や `IHttpClientFactory` と組み合わせます。request ごとに `new HttpClient()` すると socket 枯渇を招くことがあり、逆に長寿命 client では DNS 更新への追従も考える必要があります。
- タイムアウト、ステータスコード、キャンセルを考慮します。外部 API は必ず成功するとは限らないため、失敗時の retry、fallback、利用者へのエラー応答まで設計します。
- `EnsureSuccessStatusCode()` は 2xx 以外を例外にします。便利ですが、404 や 409 のように業務上意味のある status を個別に扱いたい場合は、自分で status code を判定します。
- request body や response body には個人情報や secret が含まれることがあります。ログへ丸ごと出すのではなく、必要な情報だけを masking して記録します。
- 外部 API 呼び出しは境界です。呼び出し元の業務 service に HTTP の詳細を散らさず、typed client や専用 service に閉じ込めるとテストと変更がしやすくなります。

## コード例

次は学習用の最小例です。実務コードでは、要求ごとに `new HttpClient()` するのではなく、`IHttpClientFactory` または長寿命の `HttpClient` と `SocketsHttpHandler.PooledConnectionLifetime` を使います。

```csharp
// この例では「HttpClient」の要点を最小のコードで確認します。
using var httpClient = new HttpClient();

var response = await httpClient.GetAsync("https://example.com");
response.EnsureSuccessStatusCode();

var body = await response.Content.ReadAsStringAsync();
Console.WriteLine(body.Length);
```

## コードの読み方

このコード例は「HttpClient」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

外部 API 連携、社内サービス呼び出し、Webhook、認証サーバー連携で使います。ASP.NET Core では `IHttpClientFactory` を使って named client や typed client を定義することが多いです。

## よくあるミス

- リクエストごとに `new HttpClient()` してソケット枯渇を招く。
- 1つの長寿命 `HttpClient` を使う場合に DNS 変更へ追従する設定を考えない。
- ステータスコードを見ずに本文だけ読む。
- タイムアウトやキャンセルを設定しない。

## 練習問題

1. GET リクエストを送り、ステータスコードを表示する。
2. 失敗時に `EnsureSuccessStatusCode` の挙動を確認する。
3. `CancellationToken` を渡す形に書き換える。

## 関連リンク

- [HttpClient](https://learn.microsoft.com/dotnet/fundamentals/networking/http/httpclient)
- [Guidelines for using HttpClient](https://learn.microsoft.com/dotnet/fundamentals/networking/http/httpclient-guidelines)
