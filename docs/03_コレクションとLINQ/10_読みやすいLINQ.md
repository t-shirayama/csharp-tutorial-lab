# 読みやすい LINQ

## 目的

LINQ を短く書くだけでなく、仕様が読み取れる形に整える判断を身につけます。

## 前提

- [LINQ の基本](04_LINQの基本.md) を読んでいる
- [LINQ の性能と ToList](09_LINQの性能とToList.md) を読んでいる

## 要点

- LINQ は読みやすさのために使います。
- 長い chain は途中変数や private method に分けます。
- 複雑な条件は名前を付けるとレビューしやすくなります。

## 改善例

```csharp
// この例では「読みやすい LINQ」の要点を最小のコードで確認します。
var activeCustomers = customers
    .Where(customer => customer.IsActive)
    .Where(customer => customer.LastOrderDate >= borderDate)
    .OrderBy(customer => customer.Name)
    .ToList();
```

条件が複雑な場合は method に分けます。

```csharp
// この例では「読みやすい LINQ」の要点を最小のコードで確認します。
var activeCustomers = customers
    .Where(IsRecentlyActive)
    .OrderBy(customer => customer.Name)
    .ToList();
```

## コードの読み方

このコード例は「読みやすい LINQ」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

集計、絞り込み、変換の意図を chain の順序で表現します。1行で書けることより、仕様変更時に安全に直せることを優先します。

## よくあるミス

- 1行に詰め込みすぎる。
- 副作用のある処理を LINQ に混ぜる。
- `Where` 条件が長すぎて仕様が読めない。
- method syntax と query syntax を無秩序に混ぜる。

## レビュー観点

- chain の各段階に意図があるか。
- 条件式に名前を付けた方が読みやすくならないか。
- 副作用が混ざっていないか。
- `ToList()` の位置が適切か。

## 練習問題

1. 長い LINQ chain を途中変数で分ける。
2. 複雑な predicate を method に切り出す。
3. 副作用のある LINQ を `foreach` に書き換える。

## 関連リンク

- [LINQ in C#](https://learn.microsoft.com/dotnet/csharp/linq/)
