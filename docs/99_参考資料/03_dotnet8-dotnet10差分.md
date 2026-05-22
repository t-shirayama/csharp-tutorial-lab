# .NET 8 / .NET 10 差分

## 目的

実務で遭遇しやすい LTS バージョン差を確認する観点を整理します。

## 確認ポイント

- target framework: `net8.0`, `net10.0`
- SDK バージョン
- ASP.NET Core のテンプレート差分
- EF Core のバージョン
- NuGet パッケージの対応状況

## コマンド

```powershell
dotnet --list-sdks
dotnet --info
```

## 実務での注意

新規学習では現行 LTS を優先しつつ、現場では .NET 8 LTS の案件も残ります。移行時は breaking changes、パッケージ対応、CI/CD イメージを確認します。

## 参考リンク

- [.NET release cadence](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-core-roadmap)
- [Breaking changes in .NET](https://learn.microsoft.com/dotnet/core/compatibility/)
