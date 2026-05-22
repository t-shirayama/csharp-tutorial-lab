# OpenAPI

## 目的

API 仕様を機械可読に表現する OpenAPI の基本を理解します。

## 要点

- OpenAPI は endpoint、request、response、認証方式を記述します。
- .NET 10 では ASP.NET Core の組み込み OpenAPI 機能として `AddOpenApi` と `MapOpenApi` を使えます。
- Swagger UI で対話的に確認したい場合は、Swashbuckle などの追加パッケージを使います。
- 仕様は利用者との契約です。

## .NET 10 の組み込み OpenAPI 例

```csharp
// この例では「OpenAPI」の要点を最小のコードで確認します。
builder.Services.AddOpenApi();

if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
}
```

`MapOpenApi` は OpenAPI document を公開する endpoint を追加します。公開範囲を誤ると内部仕様を外部へ見せてしまうため、通常は Development などに限定します。

## Swashbuckle を使う場合の例

```csharp
// この例では「OpenAPI」の要点を最小のコードで確認します。
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}
```

.NET 9 以降では Swashbuckle は既定では含まれません。Swagger UI が必要な場合や既存プロジェクトが Swashbuckle 前提の場合に、追加パッケージとして採用します。

## コードの読み方

`AddOpenApi` は OpenAPI document を生成するための service を登録します。`MapOpenApi` は document を公開する endpoint を追加します。`if (app.Environment.IsDevelopment())` で囲むことで、開発環境だけで確認できるようにしています。Swashbuckle の例では、document 生成に加えて Swagger UI を使えるようにしています。

## 実務での使い方

フロントエンド連携、外部公開 API、テスト、クライアントコード生成で使います。公開前にレスポンス例やエラー形式も確認します。

response type、status code、認証方式まで契約として整える方法は [OpenAPI を契約として整える](19_OpenAPIを契約として整える.md) で扱います。

## よくあるミス

- 実装と仕様がずれる。
- エラーレスポンスを仕様に書かない。
- 認証が必要な endpoint の説明が不足する。
- OpenAPI document や Swagger UI を production に無条件公開する。

## 関連リンク

- [OpenAPI support in ASP.NET Core](https://learn.microsoft.com/aspnet/core/fundamentals/openapi/overview)
- [Get started with Swashbuckle and ASP.NET Core](https://learn.microsoft.com/aspnet/core/tutorials/getting-started-with-swashbuckle)
