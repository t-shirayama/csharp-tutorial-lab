# using と namespace

## 目的

名前空間と `using` の役割を理解し、クラスを整理して読みやすく保ちます。

## 要点

- namespace は型名の衝突を避け、コードを分類します。
- `using` は名前空間を短く参照するために使います。
- file-scoped namespace を使うとインデントを浅くできます。

## コード例

```csharp
// この例では「using と namespace」の要点を最小のコードで確認します。
namespace MyApp.Products;

public class Product
{
    public string Name { get; set; } = "";
}
```

## コードの読み方

このコード例は「using と namespace」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

フォルダ構成と namespace をある程度揃えると、探しやすくなります。global using は便利ですが、何が暗黙で使えるか分かりにくくなる場合があります。

## よくあるミス

- namespace とフォルダ構成が大きくずれる。
- 同じ名前の型を複数作って衝突する。
- 不要な using が大量に残る。

## 関連リンク

- [Namespaces](https://learn.microsoft.com/dotnet/csharp/fundamentals/types/namespaces)
