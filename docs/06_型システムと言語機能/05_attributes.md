# attributes

## 目的

型やメソッドにメタデータを付ける attributes の基本を理解します。

## 要点

- attribute は角括弧で付けます。
- フレームワークやライブラリが attribute を読んで動作を変えることがあります。
- ASP.NET Core、テスト、JSON、DI 周辺でよく見ます。

## コード例

```csharp
// この例では「attributes」の要点を最小のコードで確認します。
[Obsolete("Use NewMethod instead.")]
static void OldMethod()
{
}
```

## コードの読み方

このコード例は「attributes」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

xUnit の `[Fact]`、ASP.NET Core の `[HttpGet]`、JSON の `[JsonPropertyName]` などで使います。attribute は宣言的で便利ですが、処理の流れが見えにくくなる面もあります。

## よくあるミス

- attribute が何に読まれているか理解しない。
- 設定を attribute に散らばらせすぎる。
- 自作 attribute を作っただけで、読む処理を実装していない。

## 関連リンク

- [Attributes](https://learn.microsoft.com/dotnet/csharp/advanced-topics/reflection-and-attributes/)
