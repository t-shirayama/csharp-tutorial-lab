# Regex

## 目的

正規表現で文字列の形式チェックや抽出を行う基本を理解します。

## 要点

- `Regex.IsMatch` は形式チェックに使います。
- 複雑な正規表現は読みにくくなりやすいため、名前やコメントで補います。
- 入力検証では正規表現だけに頼りすぎない判断も必要です。

## コード例

```csharp
// この例では「Regex」の要点を最小のコードで確認します。
using System.Text.RegularExpressions;

var postalCode = "123-4567";
var isValid = Regex.IsMatch(postalCode, @"^\d{3}-\d{4}$");

Console.WriteLine(isValid);
```

## コードの読み方

このコード例は「Regex」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

郵便番号、簡単なコード値、ログ行の抽出などに使います。メールアドレスや URL の完全な検証は難しいため、専用 API や仕様確認を優先します。

## よくあるミス

- 正規表現が複雑すぎて誰も保守できない。
- `^` と `$` を忘れて部分一致になっている。
- ユーザー入力の正規表現で性能問題を起こす。

## 関連リンク

- [Regular expressions in .NET](https://learn.microsoft.com/dotnet/standard/base-types/regular-expressions)
