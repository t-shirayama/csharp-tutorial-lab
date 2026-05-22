# SDK バージョン管理

## 目的

ローカル、CI、チーム内で使う .NET SDK バージョンを揃える方法を理解します。

## 前提

- [.NET SDK](01_dotnet-sdk.md) がインストール済み
- `dotnet --list-sdks` を実行できる

## 要点

- `dotnet --version` は現在選ばれている SDK を表示します。
- `dotnet --list-sdks` はインストール済み SDK を表示します。
- `global.json` でリポジトリが使う SDK バージョンを固定できます。
- 実務ではローカルと CI の SDK バージョン差を減らします。

## SDK を追加インストールする場合

すでに .NET 10 SDK が入っていても、現場が .NET 8 LTS を使っている場合は .NET 8 SDK も追加で入れます。複数 SDK は共存できます。

```powershell
winget install --id Microsoft.DotNet.SDK.8 --source winget
winget install --id Microsoft.DotNet.SDK.10 --source winget
```

公式 installer を使う場合は、必要なバージョンの **SDK x64** をそれぞれインストールします。

- [.NET 10.0 downloads](https://dotnet.microsoft.com/download/dotnet/10.0)
- [.NET 8.0 downloads](https://dotnet.microsoft.com/download/dotnet/8.0)

インストーラー設定は通常すべて既定のままで問題ありません。追加 workload は後から入れられるため、最初は選ばなくて構いません。

## 確認コマンド

```powershell
dotnet --version
dotnet --list-sdks
dotnet --info
```

## global.json の例

プロジェクト root で次のコマンドを実行すると `global.json` を作れます。

```powershell
dotnet new globaljson --sdk-version 10.0.100 --roll-forward latestFeature
```

生成される内容の例です。

```json
{
  "sdk": {
    "version": "10.0.100",
    "rollForward": "latestFeature"
  }
}
```

`version` には、チームで使う SDK の version を書きます。`10`、`10.0`、`10.0.x` のような省略形や wildcard は使えないため、`10.0.100` のような完全な SDK version を指定します。実際に入っている SDK version は `dotnet --list-sdks` で確認します。

## 設定の考え方

| 設定 | 推奨 |
| --- | --- |
| `version` | チームで決めた SDK version。例: `10.0.100` |
| `rollForward` | `latestFeature` から始めると patch / feature band 更新に追従しやすい |
| 配置場所 | solution root または repository root |

厳密に同じ SDK だけを許可したい現場では `rollForward` を使わない、または方針を別途決めます。学習や小規模開発では `latestFeature` が扱いやすいです。

## 実務での使い方

プロジェクト root に `global.json` を置くと、想定外の SDK で build される事故を減らせます。CI では `actions/setup-dotnet` などの設定も同じ方針に揃えます。

## よくあるミス

- PC に新しい SDK を入れただけでチーム全員の環境が揃ったと思う。
- `global.json` のバージョンがインストールされていない。
- .NET runtime と SDK を混同する。
- CI の SDK バージョンだけ古い。
- `global.json` の `version` に `10.0.x` のような無効な表記を書く。
- `global.json` を project folder ではなく別の階層に置き、意図した場所で効いていない。

## 練習問題

1. `dotnet --list-sdks` でインストール済み SDK を確認する。
2. `global.json` を作成する。
3. `dotnet --version` の表示が変わるか確認する。
4. CI で使う SDK バージョンも同じにする方針を書く。

## 関連リンク

- [global.json overview](https://learn.microsoft.com/dotnet/core/tools/global-json)
- [.NET SDK overview](https://learn.microsoft.com/dotnet/core/sdk)
