# EF Core 性能問題

## 症状

API や batch が遅く、EF Core の query、DB、N+1、tracking、index のどれが原因か分からない。

## 主な原因

- 一覧 API で全件取得している。
- `Include` が多すぎて巨大な SQL になっている。
- N+1 が発生している。
- `AsNoTracking()` を使える読み取り処理で tracking している。
- 必要な列だけに projection していない。
- DB 側の index や実行計画を確認していない。

## 確認コード

```csharp
var query = dbContext.Products
    .Where(product => product.IsPublished)
    .OrderBy(product => product.Name)
    .Select(product => new ProductListItem(product.Id, product.Name, product.Price));

Console.WriteLine(query.ToQueryString());
```

## 解決手順

1. 遅い操作、入力条件、件数、実行時間を記録する。
2. `ToQueryString()` で SQL を確認する。
3. `Include`、`Select`、`Take`、`AsNoTracking()` を確認する。
4. N+1 があれば projection や適切な loading strategy に変える。
5. DB の実行計画と index を確認する。
6. 変更前後を同じ条件で測定する。

## よくあるミス

- LINQ の見た目だけで軽い query だと判断する。
- `ToListAsync()` の後に `Where` して DB ではなくメモリで絞り込む。
- cache を先に入れて、根本原因を残す。
- 本番と違う DB だけで性能判断する。

## 関連リンク

- [EF Core パフォーマンス診断](../09_データアクセス/15_EFCoreパフォーマンス診断.md)
- [N+1 問題](../09_データアクセス/07_N+1問題.md)
- [EF Core 診断](../09_データアクセス/13_EFCore診断.md)
