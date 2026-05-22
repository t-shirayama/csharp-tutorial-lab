# interface

## 目的

`interface` を使って、実装ではなく「できること」に依存する考え方を理解します。

## 要点

- interface は、クラスが提供する機能の契約を表します。
- 呼び出し側は具体クラスではなく interface に依存できます。
- テスト、差し替え、DI と相性がよいです。

## コード例

```csharp
// この例では「interface」の要点を最小のコードで確認します。
IMessageSender sender = new ConsoleMessageSender();
sender.Send("Hello");

public interface IMessageSender
{
    void Send(string message);
}

public class ConsoleMessageSender : IMessageSender
{
    public void Send(string message)
    {
        Console.WriteLine(message);
    }
}
```

## コードの読み方

このコード例は「interface」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

メール送信、時刻取得、外部 API 呼び出し、ファイル操作など、テスト時に差し替えたい処理でよく使います。interface は「何でも抽象化する道具」ではなく、具体実装を差し替える理由があるときに効果が出ます。

## よくあるミス

- 1つの実装しかなく差し替え予定もないのに、機械的に interface を作る。
- interface が大きすぎて、実装クラスに不要なメソッドを強制する。
- 名前が `IManager` や `IService` だけで、何をする契約か分からない。

## 練習問題

1. `IClock` interface を作り、現在時刻を返すメソッドを定義する。
2. `SystemClock` で実装する。
3. テスト用に固定時刻を返す `FixedClock` を作る。

## 関連リンク

- [Interfaces](https://learn.microsoft.com/dotnet/csharp/fundamentals/types/interfaces)
