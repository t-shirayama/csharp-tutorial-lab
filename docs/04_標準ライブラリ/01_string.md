# string

## 目的

文字列の比較、結合、分割、検索、空チェックを実務で安全に扱えるようにします。

## 要点

- `string` は不変です。変更しているように見える操作は新しい文字列を作ります。
- 空文字と null は意味が違います。
- 比較では大文字小文字やカルチャを意識します。

## コード例

```csharp
// この例では「string」の要点を最小のコードで確認します。
var name = "  Sato  ";
var normalized = name.Trim();

Console.WriteLine(string.IsNullOrWhiteSpace(normalized));
Console.WriteLine(normalized.Contains("sa", StringComparison.OrdinalIgnoreCase));
Console.WriteLine($"Hello, {normalized}");
```

## コードの読み方

このコード例は「string」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

入力値の前後空白除去、必須チェック、コード値の比較、ログメッセージ作成で頻出します。大量の連結には `StringBuilder` も検討します。

## よくあるミス

- null チェックなしで `Trim()` を呼ぶ。
- 文字列比較で大文字小文字やカルチャを考えない。
- ループ内で大量の文字列連結を行う。

## 練習問題

1. 空白だけの文字列を未入力として判定する。
2. 大文字小文字を無視してキーワード検索する。
3. CSV 風の文字列を `Split` で分割する。

## 関連リンク

- [String](https://learn.microsoft.com/dotnet/api/system.string)
