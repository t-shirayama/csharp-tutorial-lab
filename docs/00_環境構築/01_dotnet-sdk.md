# .NET SDK のインストールと確認

## 目的

C# の開発に必要な .NET SDK を確認し、CLI でプロジェクトを作成できる状態にします。

## 前提

- Windows
- PowerShell
- Visual Studio Code

## 要点

- C# を実務で使う場合、言語だけでなく .NET SDK と CLI の理解が必要です。SDK には compiler、runtime、template、build tool が含まれ、開発 PC でコードを書いて実行するための土台になります。
- Runtime だけを入れても C# のビルドやプロジェクト作成はできません。学習や開発をする PC では **SDK**、アプリを動かすだけのサーバーでは **Runtime**、という違いを押さえます。
- `dotnet` コマンドでプロジェクト作成、ビルド、実行、テストを行います。VS Code や CI/CD も裏側では CLI を使うため、`dotnet build`、`dotnet run`、`dotnet test` の結果を読めることが重要です。
- 2026年時点では .NET 10 LTS を主軸にし、現場によって .NET 8 LTS も使われます。既存案件では target framework、`global.json`、CI の SDK version を確認してからインストールする version を決めます。
- インストール後に `dotnet` が見つからない場合は、PATH 反映前の terminal を使っていることがあります。PowerShell と VS Code を開き直し、`dotnet --info` で認識されている SDK と環境を確認します。
- 複数 SDK が入っていても通常は共存できます。どの version が使われるかは、インストール済み SDK、カレントディレクトリの `global.json`、project の target framework によって決まります。

## 何をインストールするか

Windows では **.NET SDK x64** をインストールします。Runtime だけでは C# のビルドやプロジェクト作成ができないため、必ず SDK を選びます。SDK には開発に必要な runtime も含まれるため、学習・開発目的では runtime を別途入れる必要はありません。

| 項目 | 選ぶもの |
| --- | --- |
| 種類 | SDK |
| バージョン | .NET 10 LTS。現場指定がある場合は .NET 8 LTS も可 |
| OS | Windows |
| アーキテクチャ | x64 |
| インストール対象 | 現在の PC 全体。通常は既定設定でよい |

システム全体にインストールする場合は、UAC や管理者権限の確認が出ることがあります。会社 PC では、権限がない場合に情報システム部門へ依頼します。

## 入手先

公式サイトから入れる場合は、次のページで **.NET SDK** を選びます。

- [.NET 10.0 downloads](https://dotnet.microsoft.com/download/dotnet/10.0)
- [.NET を Windows にインストールする](https://learn.microsoft.com/dotnet/core/install/windows)

ダウンロードページでは、Runtime ではなく **SDK** の Windows x64 installer を選びます。

## インストールコマンド

PowerShell で `winget` が使える場合は、次のコマンドでインストールできます。

```powershell
winget install --id Microsoft.DotNet.SDK.10 --source winget
```

.NET 8 LTS が必要な現場では次を使います。

```powershell
winget install --id Microsoft.DotNet.SDK.8 --source winget
```

## インストーラーで選ぶ設定

通常は既定設定のままで問題ありません。

| 画面 | 設定 |
| --- | --- |
| インストール先 | 既定のまま |
| PATH | SDK installer が自動設定するため、手動変更しない |
| 追加 workload | 最初は不要。必要になったら後で追加 |
| 完了後 | PowerShell と VS Code を開き直す |

インストール後に古い terminal を使い続けると、`dotnet` が見つからないことがあります。PowerShell と VS Code はいったん閉じて開き直します。

## 確認コマンド

```powershell
dotnet --version
dotnet --list-sdks
dotnet --info
```

期待する状態:

- `dotnet --version` で `10.` から始まる SDK version が表示される。
- `dotnet --list-sdks` に `10.0.xxx` が表示される。
- `dotnet --info` の `Base Path` が SDK の場所を指している。

## 最小プロジェクトの作成

```powershell
dotnet new console -n HelloCSharp
Set-Location HelloCSharp
dotnet run
```

期待される出力例:

```text
Hello, World!
```

## よくあるミス

- `dotnet` が見つからない場合は、SDK が未インストール、または PATH が反映されていない可能性があります。
- Windows では SDK インストール後に VS Code を再起動しないと、統合ターミナルに PATH が反映されないことがあります。
- Runtime だけでは開発できません。SDK が必要です。
- x86 installer を選ぶ必要は通常ありません。Windows x64 を選びます。

## 実務での使い方

現場ではソリューション、複数プロジェクト、テストプロジェクトを `dotnet` CLI で操作する場面があります。Visual Studio を使う場合でも、CLI の基本を知っていると CI/CD やトラブル調査で役立ちます。

## 関連リンク

- [.NET を Windows にインストールする](https://learn.microsoft.com/dotnet/core/install/windows)
- [.NET CLI 概要](https://learn.microsoft.com/dotnet/core/tools/)
