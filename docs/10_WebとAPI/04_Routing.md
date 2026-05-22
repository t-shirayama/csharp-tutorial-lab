# Routing

## 目的

URL と処理を対応付ける routing の基本を理解します。

## 要点

- route template でパスを定義します。
- `{id:int}` のように制約を付けられます。
- REST ではリソース名と HTTP method を組み合わせます。

## コード例

```csharp
// この例では「Routing」の要点を最小のコードで確認します。
app.MapGet("/products/{id:int}", (int id) => Results.Ok(id));
app.MapPost("/products", (CreateProductRequest request) => Results.Created($"/products/1", request));
```

## コードの読み方

このコード例は「Routing」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

URL は API 契約です。命名、階層、複数形、バージョニング方針をチームで決めます。

## よくあるミス

- 動詞を URL に入れすぎる。
- route が衝突する。
- int 制約などを付けず、想定外の入力が action まで届く。

## 関連リンク

- [Routing in ASP.NET Core](https://learn.microsoft.com/aspnet/core/fundamentals/routing)
