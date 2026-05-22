# SQL Injection 対策

## 症状

SQL に user input を文字列連結している、または review で SQL injection の危険を指摘された。

## 主な原因

- user input を SQL 文字列に直接連結している。
- Raw SQL を使うときに parameter 化していない。
- 検索条件や sort 条件をそのまま SQL に埋め込んでいる。
- ORM を使っているから常に安全だと思い込んでいる。

## 悪い例

```csharp
var sql = "SELECT * FROM Products WHERE Name = '" + name + "'";
```

## 対処例

```csharp
var products = await dbContext.Products
    .FromSql($"SELECT * FROM Products WHERE Name = {name}")
    .AsNoTracking()
    .ToListAsync(cancellationToken);
```

Dapper の場合も parameter を使います。

```csharp
var products = await connection.QueryAsync<ProductListItem>(
    "SELECT Id, Name, Price FROM Products WHERE Name = @Name",
    new { Name = name });
```

## 確認手順

1. SQL 文字列に `+` や interpolation で user input を入れていないか探す。
2. Raw SQL、Dapper、ADO.NET の parameter 指定を確認する。
3. sort column など parameter 化できない箇所は allow list で選択する。
4. review で SQL と入力値の境界を説明できるようにする。

## よくあるミス

- single quote を escape すれば十分だと思う。
- column name や sort direction を user input のまま SQL に入れる。
- `FromSqlRaw` と `FromSql` の違いを確認しない。
- log に secret や個人情報を含む SQL parameter を出す。

## 関連リンク

- [Raw SQL と Dapper 併用](../09_データアクセス/16_RawSQLとDapper併用.md)
- [ADO.NET の入口](../09_データアクセス/02_ADONETの入口.md)
- [SQL Queries](https://learn.microsoft.com/ef/core/querying/sql-queries)
