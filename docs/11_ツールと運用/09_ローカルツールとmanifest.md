# ローカルツールと manifest

## 目的

format、coverage、EF Core CLI などの .NET tool をリポジトリ単位で固定します。

## 前提

- [dotnet CLI](01_dotnetCLI.md) を読んでいる
- [NuGet](02_NuGet.md) を読んでいる

## 要点

- local tool は `.config/dotnet-tools.json` で管理します。
- tool version をチームで揃えられます。
- clone 後は `dotnet tool restore` で復元します。

## コマンド例

```powershell
dotnet new tool-manifest
dotnet tool install dotnet-ef
dotnet tool restore
dotnet tool run dotnet-ef -- --version
dotnet ef --version
```

`dotnet-ef` のように command が `dotnet-` で始まる local tool は、`dotnet tool run dotnet-ef -- --version` の長い形式でも、`dotnet ef --version` の短い形式でも呼び出せます。CI では先に `dotnet tool restore` を実行します。

## 実務での使い方

EF migration、coverage report、formatter など、プロジェクトに必要な tool は local tool にします。global install 前提にすると開発者や CI で version 差が出やすくなります。

## よくあるミス

- global tool に依存して CI で見つからない。
- tool manifest をコミットし忘れる。
- tool update 後に README や CI を更新しない。
- tool manifest の内容を確認せずに、信頼できない tool を復元する。

## 関連リンク

- [.NET tools](https://learn.microsoft.com/dotnet/core/tools/global-tools)
