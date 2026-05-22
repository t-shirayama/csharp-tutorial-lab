# DbContext と Entity

## 目的

EF Core の中心である `DbContext` と Entity の役割を理解します。

## 要点

- Entity は DB の行に対応するオブジェクトです。
- DbContext は Entity の取得、変更追跡、保存を担当します。
- `SaveChangesAsync` で変更を DB に反映します。

## コード例

```csharp
// この例では「DbContext と Entity」の要点を最小のコードで確認します。
public class AppDbContext : DbContext
{
    public DbSet<Product> Products => Set<Product>();
}

public class Product
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public decimal Price { get; set; }
}
```

## コードの読み方

このコード例は「DbContext と Entity」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

DbContext は短い単位で使い、リクエストごとの Scoped が基本です。Entity にどこまで業務ロジックを持たせるかはアーキテクチャ方針によります。

## よくあるミス

- DbContext を長期間保持する。
- Entity を API レスポンスとしてそのまま公開する。
- 変更追跡を理解せず、意図しない UPDATE を起こす。

## 関連リンク

- [DbContext](https://learn.microsoft.com/ef/core/dbcontext-configuration/)
