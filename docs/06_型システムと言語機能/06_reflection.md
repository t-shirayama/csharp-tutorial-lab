# reflection

## 目的

実行時に型情報を調べる reflection の入口を理解します。

## 要点

- reflection は型、プロパティ、メソッド、attribute を実行時に調べられます。
- フレームワークやシリアライザーの内部でよく使われます。
- 通常の業務ロジックでは使いすぎない方が読みやすく安全です。

## コード例

```csharp
// この例では「reflection」の要点を最小のコードで確認します。
var type = typeof(Product);

foreach (var property in type.GetProperties())
{
    Console.WriteLine(property.Name);
}

public class Product
{
    public string Name { get; set; } = "";
    public decimal Price { get; set; }
}
```

## コードの読み方

このコード例は「reflection」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

汎用マッピング、プラグイン、attribute 読み取り、テスト補助で使われます。性能、型安全性、リファクタリング耐性に注意します。

## よくあるミス

- 通常のメソッド呼び出しで済む場所に reflection を使う。
- プロパティ名を文字列で扱い、リネームに弱くなる。
- 例外やアクセス権限の扱いを考えない。

## 関連リンク

- [Reflection](https://learn.microsoft.com/dotnet/fundamentals/reflection/reflection)
