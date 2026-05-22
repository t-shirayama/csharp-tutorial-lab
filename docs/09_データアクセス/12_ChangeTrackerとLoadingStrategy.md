# Change Tracker と Loading Strategy

## 目的

EF Core の Change Tracker と、関連データの読み込み方法を理解します。

## 前提

- [EF Core の基本](03_EFCoreの基本.md) を読んでいる
- [N+1 問題](07_N+1問題.md) を読んでいる

## 要点

- Change Tracker は Entity の変更状態を追跡します。
- 更新する Entity は tracking が必要です。
- 読み取り専用 query は `AsNoTracking()` を検討します。
- 関連データは eager loading、explicit loading、lazy loading の違いを理解して選びます。

## コード例

```csharp
// この例では読み取り専用 query で tracking を外します。
var products = await dbContext.Products
    .AsNoTracking()
    .Where(product => product.Price >= 1000m)
    .ToListAsync();
```

## コードの読み方

`AsNoTracking()` は取得した Entity を Change Tracker に載せません。画面表示や API response のための読み取りでは、追跡を外すことで memory と処理を抑えられることがあります。

## 読み込み方法の比較

| 方法 | 例 | 向いている場面 | 注意点 |
| --- | --- | --- | --- |
| Eager loading | `Include` | 必要な関連を最初から読む | `Include` を増やしすぎると巨大 query になる。 |
| Explicit loading | `Entry(...).Collection(...).LoadAsync()` | 条件に応じて後から読む | 呼び出し箇所が増えると N+1 に近づく。 |
| Lazy loading | navigation property access | 小さな prototype | query 発行が見えにくく、実務では慎重に使う。 |

## 実務での使い方

一覧 API は projection を優先し、必要な列だけ `Select` で取得します。更新処理では tracking Entity を使うか、必要な property だけ attach / update するかを明確にします。

## よくあるミス

- すべての query を tracking のままにする。
- `Include` を増やして性能問題を別の形にする。
- lazy loading により、ループ内で大量 query を発行する。
- `DbContext` を長く保持しすぎ、Change Tracker が肥大化する。

## 練習問題

1. 一覧取得 query に `AsNoTracking()` を付ける。
2. `Include` と projection の SQL の違いを確認する。
3. 更新処理で tracking が必要な理由を説明する。

## 関連リンク

- [Change tracking in EF Core](https://learn.microsoft.com/ef/core/change-tracking/)
- [Related data](https://learn.microsoft.com/ef/core/querying/related-data/)
