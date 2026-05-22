# Parallel 処理

## 目的

CPU 処理を並列実行する `Parallel` と、非同期 I/O との違いを理解します。

## 要点

- `Parallel` は CPU バウンドな処理の並列化に使います。
- I/O 待ちには async / await を優先します。
- 共有状態を変更すると競合に注意が必要です。

## コード例

```csharp
// この例では「Parallel 処理」の要点を最小のコードで確認します。
var numbers = Enumerable.Range(1, 10);

Parallel.ForEach(numbers, number =>
{
    Console.WriteLine($"{number}: {number * number}");
});
```

## コードの読み方

このコード例は「Parallel 処理」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

画像処理、重い計算、変換処理など CPU を使う作業で検討します。Web API のリクエスト処理内で安易に使うと、サーバー全体のスレッドを圧迫することがあります。

## よくあるミス

- I/O 処理に `Parallel.ForEach` を使う。
- 複数スレッドから同じ List に Add する。
- 並列化すれば必ず速くなると思い込む。

## 関連リンク

- [Data parallelism](https://learn.microsoft.com/dotnet/standard/parallel-programming/data-parallelism-task-parallel-library)
