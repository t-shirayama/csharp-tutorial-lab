# SQL の基礎

## 目的

C# から DB を扱う前提として、基本的な SQL を理解します。

## 要点

- `SELECT` で取得、`INSERT` で追加、`UPDATE` で更新、`DELETE` で削除します。
- `WHERE` で対象を絞ります。
- 実務ではインデックス、トランザクション、実行計画も関係します。

## コード例

```sql
SELECT Id, Name, Price
FROM Products
WHERE Price >= 1000;
```

## 実務での使い方

ORM を使っていても、生成される SQL を読む力は必要です。性能問題や障害調査では SQL と DB の知識が効きます。

## よくあるミス

- WHERE なしで UPDATE / DELETE する。
- 必要以上の列を取得する。
- 件数が多いテーブルでインデックスを考えない。

## 関連リンク

- [SQL Server documentation](https://learn.microsoft.com/sql/sql-server/)
