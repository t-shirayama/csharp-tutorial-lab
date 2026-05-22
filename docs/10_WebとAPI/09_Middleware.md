# Middleware

## 目的

ASP.NET Core の request pipeline に共通処理を差し込む middleware の役割を理解します。

## 前提

- [ASP.NET Core の構造](01_ASPNETCoreの構造.md) を読んでいる
- [グローバルエラーハンドリング](../07_設計と実務パターン/12_グローバルエラーハンドリング.md) を読んでいる

## 要点

- middleware は request と response の前後に処理を挟みます。
- 登録順が重要です。
- 認証、認可、例外処理、ログ、静的ファイルなどで使われます。

## コード例

```csharp
// この例では「Middleware」の要点を最小のコードで確認します。
app.Use(async (context, next) =>
{
    // endpoint が実行される前に request path を記録します。
    Console.WriteLine($"Request: {context.Request.Path}");

    // 次の middleware または endpoint に処理を渡します。
    await next(context);

    // endpoint 実行後に response status を記録します。
    Console.WriteLine($"Response: {context.Response.StatusCode}");
});
```

## コードの読み方

`app.Use` に渡した処理は request pipeline の一部になります。`await next(context)` より前が endpoint 実行前、後ろが endpoint 実行後の処理です。

`next` を呼ばない middleware はそこで処理を止めます。認証失敗やメンテナンス応答など意図がある場合を除き、呼び忘れに注意します。

## 実務での使い方

横断的な処理は endpoint ごとに書かず middleware に寄せます。ただし、特定 endpoint だけの入力検証は endpoint filter や validation の方が適しています。

## よくあるミス

- `await next(context)` を呼び忘れる。
- middleware の順序を理解せず認証認可が効かない。
- endpoint 固有の処理まで middleware に入れる。
- response 書き込み後に header を変更しようとする。

## 関連リンク

- [ASP.NET Core Middleware](https://learn.microsoft.com/aspnet/core/fundamentals/middleware/)
