# Endpoint Filters

## 目的

Minimal API の endpoint 前後に共通処理を差し込む endpoint filter を理解します。

## 前提

- [Minimal API](02_MinimalAPI.md) を読んでいる
- [Validation](06_Validation.md) を読んでいる

## 要点

- endpoint filter は Minimal API の個別 endpoint または group に適用できます。
- 入力検証、ログ、権限チェックの補助に使えます。
- request 全体の横断処理は middleware と役割を分けます。

## コード例

```csharp
// この例では「Endpoint Filters」の要点を最小のコードで確認します。
app.MapPost("/todos", (CreateTodoRequest request) => Results.Ok())
    .AddEndpointFilter(async (context, next) =>
    {
        // endpoint の第1引数を取り出します。引数順に依存する点に注意します。
        var request = context.GetArgument<CreateTodoRequest>(0);

        // endpoint 実行前に入力値を検証します。
        if (string.IsNullOrWhiteSpace(request.Title))
        {
            return Results.BadRequest("Title is required.");
        }

        // 検証に通った場合だけ endpoint 本体へ進みます。
        return await next(context);
    });
```

## コードの読み方

`AddEndpointFilter` は endpoint の直前に差し込む処理です。この例では `CreateTodoRequest` を取り出して `Title` を検証し、空なら endpoint 本体を実行せず `BadRequest` を返します。

filter は便利ですが、業務ロジックを入れすぎると endpoint の流れが見えにくくなります。共通 validation やログの補助にとどめるのが扱いやすいです。

## 実務での使い方

API group 単位で validation やログを足したいときに使います。複雑になったら専用 filter class へ分け、テストできる形にします。

## よくあるミス

- middleware と filter の責務を混ぜる。
- filter 内に業務ロジックを入れる。
- 引数順に強く依存し、変更に弱い filter にする。
- エラーレスポンス形式が endpoint ごとにばらつく。

## 関連リンク

- [Endpoint filters in Minimal API apps](https://learn.microsoft.com/aspnet/core/fundamentals/minimal-apis/min-api-filters)
