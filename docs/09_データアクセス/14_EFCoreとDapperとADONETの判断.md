# EF Core と Dapper と ADO.NET の判断

## 目的

DB アクセス方式の選択肢を知り、要件に合うものを選べるようにします。

## 前提

- [ADO.NET の入口](02_ADONETの入口.md) を読んでいる
- [EF Core の基本](03_EFCoreの基本.md) を読んでいる

## 要点

- EF Core は Entity と LINQ を中心に開発できます。
- Dapper は SQL を自分で書き、object mapping を軽く支援します。
- ADO.NET は低レベル API で、制御は強いですが記述量が増えます。
- どれか 1 つに統一するより、境界と責務を明確にして選びます。

## コード例

```csharp
// この例では Dapper で SQL と parameter を明示します。
var products = await connection.QueryAsync<Product>(
    "select Id, Name, Price from Products where Price >= @Price",
    new { Price = 1000m });
```

## コードの読み方

Dapper は SQL をそのまま書き、結果を `Product` に mapping します。SQL の形を完全に制御したい場合に便利ですが、migration や tracking は EF Core のようには提供されません。

## 判断表

| 方式 | 向いている場面 | 注意点 |
| --- | --- | --- |
| EF Core | CRUD、業務アプリ、migration 管理 | 生成 SQL と tracking を理解する。 |
| Dapper | 複雑 SQL、性能重視の read model | SQL 管理と mapping の責務が増える。 |
| ADO.NET | 低レベル制御、特殊 provider、学習目的 | 記述量が多く、resource 管理が必要。 |

## 実務での使い方

基本 CRUD は EF Core、重い集計や read model は Dapper、特殊な接続制御は ADO.NET という分担もあります。混在させる場合は transaction、connection、責務境界を明確にします。

## よくあるミス

- EF Core を使っているのに生成 SQL を見ない。
- Dapper にしただけで自動的に速くなると思う。
- SQL 文字列を連結して injection risk を作る。
- 複数方式を無秩序に混ぜる。

## 練習問題

1. EF Core の query と同じ条件を Dapper SQL で書く。
2. どちらが読みやすいか、変更に強いかを比較する。
3. 自分のプロジェクトならどこに SQL を置くか決める。

## 関連リンク

- [EF Core](https://learn.microsoft.com/ef/core/)
- [Dapper](https://github.com/DapperLib/Dapper)
- [ADO.NET](https://learn.microsoft.com/dotnet/framework/data/adonet/)
