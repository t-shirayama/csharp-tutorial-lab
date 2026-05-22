# VS Code と C# Dev Kit

## 目的

VS Code で C# / .NET プロジェクトを読み書きし、補完、デバッグ、テスト実行を使える状態にします。

## 前提

- .NET SDK がインストール済み
- Visual Studio Code がインストール済み

## 要点

- VS Code では C# Dev Kit を入れると、プロジェクト認識、補完、デバッグ、テスト操作が使いやすくなります。
- `.csproj` や `.sln` を開くと、C# の解析が有効になります。
- CLI とエディタ機能を併用できる状態にしておくと、実務での調査が速くなります。

## 何をインストールするか

| 項目 | 選ぶもの |
| --- | --- |
| エディタ | Visual Studio Code User Installer x64 |
| C# 拡張 | C# Dev Kit |
| 依存拡張 | C#、.NET Install Tool は C# Dev Kit と一緒に自動インストールされます |
| 任意 | Git、GitHub Pull Requests、EditorConfig 関連拡張 |

C# Dev Kit の現在の要件は .NET 10 SDK です。先に [.NET SDK](01_dotnet-sdk.md) を入れてから拡張機能を入れると、初回起動時の切り分けが簡単です。

## 入手先

- [Visual Studio Code](https://code.visualstudio.com/)
- [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit)

VS Code は公式サイトから **Windows User Installer x64** を選びます。会社 PC で管理者権限がない場合も User Installer なら入れられることが多いです。

## インストールコマンド

PowerShell で `winget` が使える場合は、次のコマンドで VS Code をインストールできます。

```powershell
winget install --id Microsoft.VisualStudioCode --source winget
```

VS Code インストール後、C# Dev Kit を入れます。

```powershell
code --install-extension ms-dotnettools.csdevkit
```

`code` コマンドが見つからない場合は、VS Code を一度開き、Command Palette で `Shell Command: Install 'code' command in PATH` を実行するか、VS Code を再起動します。

## VS Code インストーラーで選ぶ設定

| 画面 | 推奨設定 |
| --- | --- |
| インストール先 | 既定のまま |
| PATH 追加 | 有効にする |
| `Open with Code` | 有効にすると右クリックから開けるため便利 |
| サポートされるファイル種類の editor 登録 | 任意。既定 editor を変えたくない場合は無効でもよい |

インストール後は VS Code を再起動します。

## 推奨拡張機能

- C# Dev Kit
- C#
- IntelliCode for C# Dev Kit

拡張機能画面で迷った場合は、まず **C# Dev Kit** だけを入れます。C# Dev Kit が必要な C# 関連拡張を案内してくれます。

## 動作確認

```powershell
dotnet new console -n VscodeCheck
code VscodeCheck
```

VS Code が開いたら、`Program.cs` を編集して `dotnet run` で実行します。

```powershell
Set-Location VscodeCheck
dotnet run
```

期待する状態:

- VS Code で `Program.cs` を開くと C# の色分けと補完が効く。
- `dotnet run` で `Hello, World!` が表示される。
- `F5` または Run and Debug からデバッグ実行できる。

## よくあるミス

- フォルダだけを開いていて `.csproj` がない場合、C# プロジェクトとして認識されません。
- 拡張機能を入れた直後は、VS Code の再読み込みが必要なことがあります。
- SDK の PATH が古い場合、VS Code の統合ターミナルだけ `dotnet` が見つからないことがあります。

## 実務での使い方

実務では、補完だけでなく「定義へ移動」「参照検索」「リネーム」「デバッグ」「テスト実行」を日常的に使います。コードを読む速度に直結するため、エディタ操作も学習対象に含めます。

## 練習問題

1. VS Code で `Program.cs` を開く。
2. `Console.WriteLine` にブレークポイントを置く。
3. デバッグ実行して処理が止まることを確認する。
