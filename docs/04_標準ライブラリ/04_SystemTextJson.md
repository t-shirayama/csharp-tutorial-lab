# System.Text.Json

## 目的

.NET 標準の JSON 処理ライブラリ `System.Text.Json` の基本を理解します。

## 要点

- `JsonSerializer.Serialize` でオブジェクトを JSON にします。
- `JsonSerializer.Deserialize<T>` で JSON をオブジェクトに戻します。
- プロパティ名や null の扱いは option で制御します。

## コード例

```csharp
// この例では「System.Text.Json」の要点を最小のコードで確認します。
using System.Text.Json;

var product = new Product("Keyboard", 12000m);
var json = JsonSerializer.Serialize(product, new JsonSerializerOptions { WriteIndented = true });

Console.WriteLine(json);

var restored = JsonSerializer.Deserialize<Product>(json);
Console.WriteLine(restored?.Name);

public record Product(string Name, decimal Price);
```

## コードの読み方

このコード例は「System.Text.Json」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

API のリクエスト/レスポンス、設定ファイル、外部サービス連携で使います。公開 API では JSON のプロパティ名が契約になるため、変更に注意します。

## よくあるミス

- deserialize 結果が null になる可能性を無視する。
- C# のプロパティ名変更が JSON 契約を壊すことに気づかない。
- 日時や enum の表現を決めずに API を公開する。

## 練習問題

1. record を JSON に変換する。
2. JSON から record に戻す。
3. camelCase にする option を調べて試す。

## 関連リンク

- [System.Text.Json](https://learn.microsoft.com/dotnet/standard/serialization/system-text-json/overview)
