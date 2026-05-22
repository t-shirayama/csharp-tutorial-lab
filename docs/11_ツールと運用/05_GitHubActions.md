# GitHub Actions

## 目的

GitHub Actions で .NET のビルドとテストを自動実行する入口を理解します。

## コード例

```yaml
name: ci

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - uses: actions/setup-dotnet@v5
        with:
          dotnet-version: '10.0.x'
      - run: dotnet restore
      - run: dotnet build --no-restore
      - run: dotnet test --no-build
```

## 実務での使い方

PR ごとに build、test、format、脆弱性チェックを走らせます。失敗ログを読めることも実務スキルです。

`global.json` で SDK を固定している場合は、`actions/setup-dotnet` の `global-json-file` 入力を使うか、workflow の `dotnet-version` と `global.json` の方針を揃えます。古い self-hosted runner では最新 major の Action が動かない場合があるため、runner version も確認します。

## よくあるミス

- ローカルと CI の SDK バージョンが違う。
- restore / build / test の依存関係を理解しない。
- secret をログに出す。
- `global.json` と workflow の SDK 指定が別々に更新される。

## 関連リンク

- [Building and testing .NET](https://docs.github.com/actions/automating-builds-and-tests/building-and-testing-net)
- [actions/setup-dotnet](https://github.com/actions/setup-dotnet)
