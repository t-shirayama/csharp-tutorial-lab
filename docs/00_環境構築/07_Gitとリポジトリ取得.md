# Git とリポジトリ取得

## 目的

実務で既存の C# / .NET プロジェクトを取得し、作業開始できる状態にするために、Git の最小操作を理解します。

## 前提

- [インストール手順チェックリスト](06_インストール手順チェックリスト.md) を完了している
- PowerShell を使える
- GitHub、Azure DevOps、社内 Git などの repository URL が分かっている

## 要点

- 実務では、空の project を作るより既存 repository を clone して作業を始めることが多いです。
- Git for Windows を入れると、PowerShell から `git` コマンドを使えます。VS Code の Source Control view も内部では Git を使います。
- clone 後は、まず `git status`、`dotnet --info`、`dotnet restore`、`dotnet build` を確認します。いきなり修正せず、現在の環境で再現できる状態を確認します。
- 改行コードや文字コードの設定は、チームの `.editorconfig` や repository の方針に合わせます。個人判断で大量の改行差分を出さないようにします。
- 認証が必要な repository では、ブラウザー認証、Personal Access Token、会社の SSO などが使われます。token や password はチャットや issue に貼り付けません。
- clone した後に `bin`、`obj`、`site` のような生成物が差分に出る場合は、`.gitignore` や作業ディレクトリを確認します。

## Git のインストール

PowerShell で `winget` が使える場合は、次のコマンドで Git for Windows をインストールできます。

```powershell
winget install --id Git.Git --source winget
```

インストール後は PowerShell と VS Code を開き直し、次を確認します。

```powershell
git --version
Get-Command git
```

## repository を clone する

```powershell
Set-Location C:\Work
git clone https://github.com/example/sample-app.git
Set-Location sample-app
git status
```

`git status` で `working tree clean` と表示されれば、clone 直後の差分はありません。

## clone 後の確認

```powershell
dotnet --info
dotnet restore
dotnet build
```

`global.json` がある repository では、その SDK version がローカルに入っているかを確認します。

```powershell
dotnet --list-sdks
Get-Content -Encoding UTF8 global.json
```

## 作業開始前の確認

```powershell
git branch
git pull
git status
```

作業 branch を作る場合は、チームの命名規則に合わせます。

```powershell
git switch -c feature/add-order-validation
```

## コードの読み方

`git clone` は repository をローカルに複製します。`Set-Location sample-app` で repository root に移動し、`git status` で現在の差分状態を確認します。`dotnet restore` は package を復元し、`dotnet build` は現在の SDK と source code で build できるかを確認します。

## 実務での使い方

新しい案件に入ったら、まず README、`global.json`、solution file、CI 設定を確認します。自分の環境だけで起きる問題なのか、repository の現在状態なのかを分けるため、clone 直後に build 結果を記録しておくと調査しやすくなります。

## よくあるミス

- repository root ではなく subfolder で `dotnet build` し、solution が見つからない。
- clone 直後の build 結果を確認せず、修正後の問題か環境問題か分からなくなる。
- 改行コードだけの大量差分を作る。
- token や password を command history、issue、チャットに貼り付ける。
- `bin` や `obj` の生成物を commit しようとする。

## 練習問題

1. 任意の練習 repository を clone する。
2. `git status` で差分がないことを確認する。
3. `dotnet restore` と `dotnet build` を実行する。
4. 作業 branch を作成して、再度 `git status` を確認する。

## 関連リンク

- [Git for Windows](https://gitforwindows.org/)
- [GitHub Docs: Cloning a repository](https://docs.github.com/repositories/creating-and-managing-repositories/cloning-a-repository)
- [Git documentation](https://git-scm.com/doc)
