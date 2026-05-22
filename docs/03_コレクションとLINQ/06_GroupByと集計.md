# GroupBy と集計

## 目的

一覧データをグループ化し、件数や合計を計算する基本を理解します。

## 要点

- `GroupBy` はキーごとに要素をまとめます。
- `Count`, `Sum`, `Average`, `Max`, `Min` で集計できます。
- 集計結果は DTO や record に変換すると扱いやすくなります。

## コード例

```csharp
// この例では「GroupBy と集計」の要点を最小のコードで確認します。
var sales = new[]
{
    new Sale("Tokyo", 1000m),
    new Sale("Tokyo", 2000m),
    new Sale("Osaka", 1500m)
};

var summaries = sales
    .GroupBy(sale => sale.Area)
    .Select(group => new SaleSummary(group.Key, group.Count(), group.Sum(sale => sale.Amount)))
    .ToList();

foreach (var summary in summaries)
{
    Console.WriteLine($"{summary.Area}: {summary.Count}件 / {summary.TotalAmount}円");
}

public record Sale(string Area, decimal Amount);
public record SaleSummary(string Area, int Count, decimal TotalAmount);
```

## コードの読み方

このコード例は「GroupBy と集計」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

売上集計、ステータス別件数、カテゴリ別合計などで使います。DB 側で集計するか、メモリ上で集計するかはデータ量と性能を見て判断します。

## よくあるミス

- 大量データを全件メモリに読み込んでから集計する。
- `GroupBy` 後の型が分からず、処理を複雑にする。
- null や空文字のキーをどう扱うか決めていない。

## 練習問題

1. 点数一覧を科目別に合計する。
2. 商品一覧をカテゴリ別に件数集計する。
3. 集計結果を record に変換する。

## 関連リンク

- [Group query results](https://learn.microsoft.com/dotnet/csharp/linq/group-query-results)
