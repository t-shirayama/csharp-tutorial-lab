# EF Core パフォーマンス診断

## 目的

EF Core の処理が遅いときに、SQL、件数、index、tracking、projection を順に確認し、勘ではなく根拠を持って改善できるようにします。

## 前提

- [EF Core の基本](03_EFCoreの基本.md) を読んでいる
- [EF Core クエリ設計](08_EFCoreクエリ設計.md) を読んでいる
- [EF Core 診断](13_EFCore診断.md) を読んでいる

## 要点

- まず「どの endpoint / command が遅いか」を決め、再現条件と入力件数を記録します。
- EF Core の LINQ は SQL に変換されます。`ToQueryString()` で実際の SQL を確認します。
- 一覧取得では entity 全体ではなく、必要な列だけを DTO に projection します。
- 更新しない検索では `AsNoTracking()` を使い、Change Tracker の負荷を下げます。
- N+1、過剰な `Include`、client evaluation、index 不足、不要な sort がよくある原因です。
- 最後は DB 側の実行計画と index を確認します。C# 側だけで判断しません。

## 調査の順番

1. 遅い操作、入力条件、件数、実行時間を記録する。
2. `ToQueryString()` または logging で SQL を確認する。
3. `Include`、`Select`、`Where`、`OrderBy`、`Skip` / `Take` の位置を見る。
4. `AsNoTracking()` を使える読み取り処理か確認する。
5. DB の実行計画で table scan、index scan、sort、join の負荷を見る。
6. 改善後に同じ条件で再測定する。

## SQL を確認する

```csharp
var query = dbContext.Products
    .Where(product => product.IsPublished)
    .OrderBy(product => product.Name)
    .Select(product => new ProductListItem(
        product.Id,
        product.Name,
        product.Price));

// 開発時の調査では、LINQ から生成される SQL を先に確認します。
Console.WriteLine(query.ToQueryString());

var products = await query.ToListAsync(cancellationToken);
```

## 読み取り専用 query を軽くする

```csharp
var products = await dbContext.Products
    .AsNoTracking()
    .Where(product => product.IsPublished)
    .OrderBy(product => product.Name)
    .Select(product => new ProductListItem(
        product.Id,
        product.Name,
        product.Price))
    .Take(50)
    .ToListAsync(cancellationToken);
```

## 悪い例: entity を丸ごと取得する

```csharp
var products = await dbContext.Products
    .Include(product => product.Reviews)
    .Where(product => product.IsPublished)
    .ToListAsync(cancellationToken);
```

一覧画面で `Reviews` の本文や全件が不要なら、`Include` は過剰です。必要な件数、列、関連だけに絞ります。

## コードの読み方

`ToQueryString()` は DB に送る SQL を確認するための調査用 API です。`AsNoTracking()` は entity の変更追跡をしないため、読み取り専用の一覧や詳細表示で効果があります。`Select` で `ProductListItem` に変換しているのは、画面や API response に必要な列だけを取得するためです。`Take(50)` は無制限取得を避けるための上限です。

## 実務での使い方

PR で query を変更したら、生成 SQL、想定件数、index の有無を確認します。遅い query を見つけたら、いきなり cache を入れるのではなく、まず SQL と実行計画を見ます。`AsNoTracking()`、projection、pagination、index、N+1 解消の順に確認すると、原因を切り分けやすいです。

## よくあるミス

- `Include` を増やせば安全だと思い、巨大な SQL にする。
- API の一覧で全件取得してから C# 側で page 化する。
- `AsNoTracking()` を更新処理にも付けて、変更が保存されない原因にする。
- `ToListAsync()` の前に `Where` や `Take` を書かず、メモリ上で絞り込む。
- SQL を見ずに LINQ の見た目だけで性能を判断する。

## 練習問題

1. `ToQueryString()` で生成 SQL を表示する。
2. `AsNoTracking()` あり、なしの違いを説明する。
3. 一覧 API の query を response DTO への projection に変える。
4. `Include` が必要な query と不要な query を分類する。

## 関連リンク

- [EF Core クエリ設計](08_EFCoreクエリ設計.md)
- [Change Tracker と Loading Strategy](12_ChangeTrackerとLoadingStrategy.md)
- [EF Core 診断](13_EFCore診断.md)
- [Efficient Querying](https://learn.microsoft.com/ef/core/performance/efficient-querying)
