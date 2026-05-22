# NuGet

## 目的

.NET のパッケージ管理である NuGet の基本を理解します。

## コマンド

```powershell
dotnet add package Microsoft.Extensions.Logging
dotnet list package
dotnet restore
```

## 実務での使い方

外部ライブラリ追加、脆弱性対応、バージョン更新で使います。パッケージ追加時はライセンス、メンテ状況、依存関係を確認します。

## よくあるミス

- 不要なパッケージを増やす。
- major version 更新の破壊的変更を確認しない。
- restore エラーの原因をログで追わない。

## 関連リンク

- [NuGet documentation](https://learn.microsoft.com/nuget/)
