# ILogger と構造化ログ

## 目的

`ILogger<T>` を DI で受け取り、検索しやすく安全な構造化ログを書けるようにします。

## 前提

- [ログ設計](04_ログ設計.md) を読んでいる
- [DI コンテナの実装](09_DIコンテナの実装.md) を読んでいる

## 要点

- ログは文字列ではなく、名前付きプロパティを持つイベントとして出します。
- 例外ログでは例外オブジェクトを `LogError` に渡します。
- CorrelationId や業務 ID を入れると、障害調査で追跡しやすくなります。
- パスワード、トークン、個人情報はログに出しません。

## コード例

```csharp
// この例では「ILogger と構造化ログ」の要点を最小のコードで確認します。
public class OrderService
{
    private readonly ILogger<OrderService> logger;

    public OrderService(ILogger<OrderService> logger)
    {
        this.logger = logger;
    }

    public void Complete(int orderId)
    {
        logger.LogInformation("Completing order. OrderId={OrderId}", orderId);

        try
        {
            // 業務処理
            logger.LogInformation("Order completed. OrderId={OrderId}", orderId);
        }
        catch (Exception exception)
        {
            logger.LogError(exception, "Failed to complete order. OrderId={OrderId}", orderId);
            throw;
        }
    }
}
```

## 悪い例

```csharp
// この例では「ILogger と構造化ログ」の要点を最小のコードで確認します。
logger.LogInformation($"Order completed. OrderId={orderId}");
```

文字列補間では、ログ基盤が `OrderId` を構造化された値として扱えません。

## CorrelationId の入口

```csharp
// この例では「ILogger と構造化ログ」の要点を最小のコードで確認します。
using (logger.BeginScope(new Dictionary<string, object>
{
    ["CorrelationId"] = correlationId
}))
{
    logger.LogInformation("Processing request.");
}
```

`BeginScope` を使うと、同じ処理範囲のログに共通の値を付けられます。

## コードの読み方

このコード例は「ILogger と構造化ログ」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

API リクエスト、外部サービス呼び出し、バッチ処理、重要な業務イベントで使います。ログは「何が起きたか」「どの対象か」「なぜ失敗したか」を後から追える粒度にします。

## よくあるミス

- 文字列補間で構造化ログを壊す。
- Error ログに例外オブジェクトを渡さない。
- 成功ログを出しすぎ、必要なログが埋もれる。
- PII、アクセストークン、接続文字列を出力する。

## レビュー観点

- ログレベルが適切か。
- 調査に必要な ID が入っているか。
- 例外ログが1か所に集約されているか。
- ログだけで業務上の状態遷移を追えるか。

## 関連リンク

- [Logging in .NET](https://learn.microsoft.com/dotnet/core/extensions/logging)
- [Logging in ASP.NET Core](https://learn.microsoft.com/aspnet/core/fundamentals/logging/)
