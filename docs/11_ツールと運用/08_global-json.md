# global.json

## 目的

リポジトリで使う .NET SDK バージョンを `global.json` で揃えます。

## 前提

- [SDK バージョン管理](../00_環境構築/04_SDKバージョン管理.md) を読んでいる
- [dotnet CLI](01_dotnetCLI.md) を読んでいる

## 要点

- `global.json` は SDK 選択に影響します。
- チーム開発では SDK の揺れを減らします。
- CI の SDK 指定とも合わせます。

## 作成例

```powershell
dotnet new globaljson --sdk-version 10.0.100
```

## 実務での使い方

SDK 更新時は `global.json`、CI、README、開発者環境の更新をまとめて扱います。更新後は build、test、format を確認します。

## よくあるミス

- `global.json` が古く、CI だけ失敗する。
- インストールされていない SDK version を指定する。
- runtime version と SDK version を混同する。

## 関連リンク

- [global.json overview](https://learn.microsoft.com/dotnet/core/tools/global-json)
