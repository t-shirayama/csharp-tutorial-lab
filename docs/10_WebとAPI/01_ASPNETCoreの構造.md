# ASP.NET Core の構造

## 目的

ASP.NET Core アプリの入口、DI、ミドルウェア、ルーティングの全体像を理解します。

## 要点

- `Program.cs` がアプリの入口です。アプリ起動時に設定を読み込み、DI 登録を行い、HTTP request を処理する pipeline を組み立てます。
- `builder.Services` で DI 登録を行います。Controller、Minimal API、middleware、Hosted Service などから使う service はここで登録し、実装の生成を framework に任せます。
- `app.Use...` や `app.Map...` で HTTP pipeline を構成します。`UseAuthentication`、`UseAuthorization`、`UseExceptionHandler` などは順序が意味を持つため、ただ並べるのではなく request の流れとして読みます。
- `app.Map...` は URL と処理を結び付けます。Minimal API なら `MapGet` や `MapPost`、Controller なら属性 route や conventional route によって endpoint が作られます。
- ASP.NET Core の調査では、まず `Program.cs` を読みます。設定、DI、middleware、routing のどこで振る舞いが決まっているかを確認すると、実行時エラーや認証漏れの原因を追いやすくなります。
- `Program.cs` に業務処理を詰め込みすぎると見通しが悪くなります。起動構成は `Program.cs`、業務ルールは service、入力検証は validator や endpoint filter など、責務を分けます。

## コード例

```csharp
// この例では「ASP.NET Core の構造」の要点を最小のコードで確認します。
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddEndpointsApiExplorer();

var app = builder.Build();

app.MapGet("/health", () => Results.Ok("OK"));

app.Run();
```

## コードの読み方

`WebApplication.CreateBuilder(args)` は設定、DI、logging などの起動準備を作ります。`builder.Services.AddEndpointsApiExplorer()` は endpoint 情報を収集できるようにする登録です。`builder.Build()` の後は DI 登録ではなく request pipeline の構成に移り、`app.MapGet("/health", ...)` で `/health` への GET request と処理を結び付けています。

## 実務での使い方

調査ではまず `Program.cs` を読み、サービス登録、ミドルウェア、ルーティング、設定読み込みを確認します。

実務の API では、ここに認証認可、OpenAPI、CORS、rate limiting、error handling、endpoint group などが追加されます。CRUD の全体像は [CRUD API の実装パターン](15_CRUD APIの実装パターン.md)、エラー形式は [Status Code と ProblemDetails](17_StatusCodeとProblemDetails.md) で扱います。

## よくあるミス

- DI 登録漏れで実行時エラーになる。
- ミドルウェア順序を誤る。
- `Program.cs` に処理を詰め込みすぎる。

## 関連リンク

- [ASP.NET Core fundamentals](https://learn.microsoft.com/aspnet/core/fundamentals/)
