# TPL Dataflow の入口

## 目的

複数段の非同期 pipeline を作る TPL Dataflow の入口を理解します。

## 前提

- [Task の基本](01_Taskの基本.md) を読んでいる
- [Channel の入口](06_Channelの入口.md) を読んでいる

## 要点

- TPL Dataflow は block をつなげて pipeline を作る library です。
- `TransformBlock` は入力を変換します。
- `ActionBlock` は入力を処理します。
- backpressure や並列度を設定できます。

## パッケージ

```powershell
dotnet add package System.Threading.Tasks.Dataflow
```

## コード例

```csharp
// この例では「TPL Dataflow の入口」の要点を最小のコードで確認します。
using System.Threading.Tasks.Dataflow;

var transform = new TransformBlock<string, string>(text => text.ToUpperInvariant());
var output = new ActionBlock<string>(text => Console.WriteLine(text));

transform.LinkTo(output, new DataflowLinkOptions { PropagateCompletion = true });

await transform.SendAsync("hello");
transform.Complete();

await output.Completion;
```

## コードの読み方

`TransformBlock` が文字列を大文字に変換し、`ActionBlock` が出力します。`PropagateCompletion` により、前段の完了が後段へ伝わります。

## 実務での使い方

画像処理、ファイル変換、外部 API 連携、ETL のように、複数段の処理を安全に流したい場合に候補になります。単純な producer / consumer なら `Channel<T>` の方が軽いこともあります。

## よくあるミス

- `Complete` を呼ばず、後段が終了しない。
- `Completion` を待たず、処理途中でアプリが終了する。
- 並列度や bounded capacity を設定せず、外部サービスへ過剰に流す。

## 練習問題

1. `TransformBlock<int, int>` で 2 倍にする pipeline を作る。
2. `ActionBlock<int>` で出力する。
3. bounded capacity を設定して backpressure を確認する。

## 関連リンク

- [Dataflow](https://learn.microsoft.com/dotnet/standard/parallel-programming/dataflow-task-parallel-library)
- [System.Threading.Tasks.Dataflow](https://www.nuget.org/packages/System.Threading.Tasks.Dataflow)
