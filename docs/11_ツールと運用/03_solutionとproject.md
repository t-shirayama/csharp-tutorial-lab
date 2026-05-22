# solution と project

## 目的

.NET の solution と project の役割を理解します。

## 要点

- `.csproj` は1つのプロジェクト定義です。
- `.sln` は複数プロジェクトをまとめます。
- 実務ではアプリ、ライブラリ、テストを別 project に分けます。

## コマンド

```powershell
dotnet new sln -n Sample
dotnet new classlib -n Sample.Domain
dotnet new xunit -n Sample.Tests
dotnet sln add Sample.Domain Sample.Tests
```

## よくあるミス

- project 参照と NuGet 参照を混同する。
- 依存方向が循環する。
- solution に test project を追加し忘れる。

## 関連リンク

- [dotnet sln](https://learn.microsoft.com/dotnet/core/tools/dotnet-sln)
