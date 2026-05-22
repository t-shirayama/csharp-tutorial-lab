# Migration

## 目的

EF Core Migration で DB スキーマ変更を管理する基本を理解します。

## 要点

- Migration はモデル変更を DB スキーマ変更として記録します。
- チーム開発では migration ファイルもレビュー対象です。
- 本番適用手順を事前に決めます。

## コマンド

```powershell
dotnet ef migrations add AddProducts
dotnet ef database update
dotnet ef migrations script
dotnet ef migrations script --idempotent
dotnet ef migrations bundle
```

## 実務での使い方

開発環境では `database update`、本番では SQL script 化して承認後に適用することがあります。複数環境の適用状態が揃っていない場合は `--idempotent` で冪等 script を作る選択肢があります。デプロイ工程で単一実行ファイルとして扱いたい場合は migration bundle も候補です。データ移行を伴う変更は特に注意します。

## よくあるミス

- migration を確認せずにコミットする。
- 本番データがある前提を忘れて NOT NULL カラムを追加する。
- 複数人の migration が衝突する。
- production 起動時に無条件で `MigrateAsync()` を実行し、適用内容をレビューできない。

## 関連リンク

- [Migrations](https://learn.microsoft.com/ef/core/managing-schemas/migrations/)
- [Applying migrations](https://learn.microsoft.com/ef/core/managing-schemas/migrations/applying)
