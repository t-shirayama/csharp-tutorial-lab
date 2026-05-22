# dotnet format

## 目的

`dotnet format` でコードスタイルを整え、機械的な差分を減らします。

## 要点

- format はコードの見た目や一部の analyzer 指摘を整えます。
- CI でチェックすると、レビューでスタイル指摘が減ります。
- `.editorconfig` と組み合わせます。

## コマンド

```powershell
dotnet format
dotnet format --verify-no-changes
```

## 実務での使い方

プルリクエスト前、CI、保存時フォーマットで使います。プロジェクト全体の方針は `.editorconfig` に置きます。

## よくあるミス

- 大量のフォーマット差分を機能変更と混ぜる。
- チームでルールを決めずに個人設定だけで整形する。
- CI とローカルで SDK バージョンが違う。

## 関連リンク

- [dotnet format](https://learn.microsoft.com/dotnet/core/tools/dotnet-format)
