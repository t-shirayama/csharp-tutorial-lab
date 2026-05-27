# IEnumerable と IEnumerator

## 目的

LINQ や foreach の土台である `IEnumerable<T>` の考え方を理解します。

## 要点

- `IEnumerable<T>` は順番に列挙できることを表す interface です。
- `foreach` は `IEnumerable<T>` を列挙します。
- 実務では、読み取り用途の結果を `IEnumerable<T>` で返すことがあります。

## コード例

```csharp
// この例では「IEnumerable と IEnumerator」の要点を最小のコードで確認します。
IEnumerable<int> numbers = new List<int> { 1, 2, 3 };

foreach (var number in numbers)
{
    Console.WriteLine(number);
}
```

## コードの読み方

このコード例は「IEnumerable と IEnumerator」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

メソッドの戻り値を `IEnumerable<T>` にすると、呼び出し側は「列挙できる」ことだけに依存します。ただし、複数回列挙する場合や件数が必要な場合は `List<T>` に materialize する判断も必要です。

## よくあるミス

- `IEnumerable<T>` を何度も列挙して、同じ処理や DB クエリを繰り返す。
- 件数が必要なのに毎回 `Count()` を呼ぶ。
- 遅延評価を理解せず、実行タイミングを誤解する。

## 関連リンク

- [IEnumerable&lt;T&gt;](https://learn.microsoft.com/dotnet/api/system.collections.generic.ienumerable-1)
