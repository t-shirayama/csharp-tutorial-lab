# TextReader と Encoding

## 目的

テキストの読み書きに使う `TextReader` / `TextWriter` と、文字コードの扱いを理解します。

## 前提

- [File / Directory / Path](03_FileDirectoryPath.md) を読んでいる
- [Stream と FileStream](12_StreamとFileStream.md) を読んでいる

## 要点

- `TextReader` は文字列として読み取る抽象型です。
- `StreamReader` は stream からテキストを読みます。
- 文字化けを避けるため、encoding を明示します。

## コード例

```csharp
// この例では「TextReader と Encoding」の要点を最小のコードで確認します。
var path = Path.Combine(Environment.CurrentDirectory, "memo.txt");

await File.WriteAllTextAsync(path, "こんにちは", Encoding.UTF8);

using TextReader reader = new StreamReader(path, Encoding.UTF8);
var text = await reader.ReadToEndAsync();

Console.WriteLine(text);
```

## コードの読み方

`StreamReader` は file stream を文字として読み取る reader です。`Encoding.UTF8` を指定することで、環境の既定 encoding に依存しない読み書きになります。

## 実務での使い方

CSV、固定長ファイル、ログ、外部システム連携で使います。外部から来るファイルは UTF-8 とは限らないため、仕様書や実ファイルで encoding を確認します。

## よくあるミス

- encoding を指定せず、環境差で文字化けする。
- 大きなファイルを `ReadToEnd` で一度に読む。
- 改行コードの差を考慮しない。

## 練習問題

1. UTF-8 のテキストファイルを作る。
2. `StreamReader` で 1 行ずつ読む。
3. encoding を変えたときに文字化けする例を確認する。

## 関連リンク

- [TextReader Class](https://learn.microsoft.com/dotnet/api/system.io.textreader)
- [StreamReader Class](https://learn.microsoft.com/dotnet/api/system.io.streamreader)
- [Character encoding in .NET](https://learn.microsoft.com/dotnet/standard/base-types/character-encoding)
