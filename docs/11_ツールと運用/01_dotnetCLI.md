# dotnet CLI

## 目的

.NET 開発で日常的に使う `dotnet` コマンドを整理します。

## よく使うコマンド

```powershell
dotnet --info
dotnet new console -n Sample
dotnet build
dotnet run
dotnet test
dotnet format
```

## 実務での使い方

CI/CD、障害調査、ビルド再現、テスト実行で必須です。IDE に頼らず CLI で再現できることが重要です。

## よくあるミス

- 実行ディレクトリを間違える。
- solution と project のどちらを指定しているか意識しない。
- SDK バージョン差を確認しない。

## 関連リンク

- [.NET CLI](https://learn.microsoft.com/dotnet/core/tools/)
