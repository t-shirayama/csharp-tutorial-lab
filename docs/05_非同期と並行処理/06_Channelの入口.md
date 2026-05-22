# Channel の入口

## 目的

生産者と消費者を分けて非同期にデータを受け渡す `Channel<T>` の入口を理解します。

## 要点

- Channel は非同期キューのように使えます。
- producer が書き込み、consumer が読み取ります。
- 大量に投入される可能性がある場合は、`CreateBounded` で容量を決めてバックプレッシャーをかけます。
- バックグラウンド処理やパイプライン処理で役立ちます。

## コード例

```csharp
// この例では「Channel の入口」の要点を最小のコードで確認します。
var channel = Channel.CreateUnbounded<string>();

await channel.Writer.WriteAsync("job-1");
channel.Writer.Complete();

await foreach (var item in channel.Reader.ReadAllAsync())
{
    Console.WriteLine(item);
}
```

投入量を制限したい場合は、bounded channel を使います。

```csharp
// この例では「Channel の入口」の要点を最小のコードで確認します。
var channel = Channel.CreateBounded<string>(capacity: 100);
```

既定では容量が空くまで `WriteAsync` が待機します。ログや telemetry のように古いデータを落としてよい処理では、`BoundedChannelFullMode.DropOldest` などの方針を明示します。

## コードの読み方

このコード例は「Channel の入口」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

ログの非同期処理、ジョブキュー、入力を順に処理するパイプラインなどで使えます。完了通知、キャンセル、例外処理の設計が重要です。

## よくあるミス

- Writer を Complete せず consumer が待ち続ける。
- 無制限 Channel に大量投入してメモリを圧迫する。
- 満杯時に待つのか、捨てるのか、失敗させるのかを決めない。
- 失敗時の再試行や破棄方針を決めていない。

## 関連リンク

- [Channels](https://learn.microsoft.com/dotnet/core/extensions/channels)
