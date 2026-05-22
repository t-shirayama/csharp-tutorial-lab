# NuGet 復元とパッケージソース

## 目的

.NET project が依存する package を復元し、`dotnet restore` で詰まったときに原因を切り分けられるようにします。

## 前提

- [.NET SDK のインストールと確認](01_dotnet-sdk.md) を完了している
- [Git とリポジトリ取得](07_Gitとリポジトリ取得.md) を読んでいる
- PowerShell で repository root に移動できる

## 要点

- NuGet は .NET の package 管理システムです。外部 library、test framework、tool などは NuGet package として参照されます。
- `dotnet restore` は project file に書かれた package を package source から取得し、build に必要な依存関係をそろえます。
- `dotnet build` は必要に応じて restore も行いますが、環境構築の切り分けでは `dotnet restore` を単独で実行すると原因を見つけやすくなります。
- package source には nuget.org、社内 feed、Azure Artifacts、GitHub Packages などがあります。実務では認証や proxy が必要なことがあります。
- restore 失敗時は、エラー全文、package 名、source URL、認証の有無、proxy、実行した directory を確認します。
- credential や token は secret です。設定ファイルや terminal 出力を共有するときは、URL や token が含まれていないか確認します。

## restore の基本

```powershell
Set-Location C:\Work\sample-app
dotnet restore
```

成功すると、必要な package が復元されます。失敗した場合は、最初の error と package source を確認します。

## package source を確認する

```powershell
dotnet nuget list source
```

出力例です。

```text
Registered Sources:
  1.  nuget.org [Enabled]
      https://api.nuget.org/v3/index.json
```

社内 feed を使う project では、nuget.org 以外の source が表示されることがあります。

## package cache を確認・削除する

package cache が壊れている可能性がある場合は、cache の場所を確認します。

```powershell
dotnet nuget locals all --list
```

cache を削除する場合は、影響を理解してから実行します。削除後は再度 package download が必要になります。

```powershell
dotnet nuget locals all --clear
dotnet restore
```

## package を追加する

```powershell
dotnet add package Microsoft.Extensions.Logging.Abstractions
```

追加後は `.csproj` に `PackageReference` が増えます。

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.Logging.Abstractions" Version="10.0.0" />
</ItemGroup>
```

## コードの読み方

`dotnet restore` は source code を実行するコマンドではなく、project が必要とする package を取得するコマンドです。`dotnet nuget list source` は、どこから package を取得しようとしているかを確認します。restore エラーでは、package が存在しないのか、source に到達できないのか、認証できていないのかを分けて読むことが重要です。

## 実務での使い方

社内 network、proxy、認証付き feed、CI 環境では、ローカルでは restore できるが CI では失敗する、またはその逆が起きることがあります。`NuGet.config`、CI の secret、`actions/setup-dotnet`、社内証明書などが関係するため、restore 失敗時は環境情報をセットで残します。

## よくあるミス

- `dotnet build` の失敗を compiler error だと思い込むが、実際は restore で失敗している。
- package source が disabled になっていることに気づかない。
- 社内 feed の credential が切れているのに、package が存在しないと判断する。
- cache 削除を何度も繰り返し、根本原因を確認しない。
- token を含む `NuGet.config` をそのまま共有する。

## 練習問題

1. `dotnet restore` を実行し、成功することを確認する。
2. `dotnet nuget list source` で package source を確認する。
3. `dotnet nuget locals all --list` で cache の場所を確認する。
4. package を1つ追加し、`.csproj` の差分を確認する。

## 関連リンク

- [NuGet documentation](https://learn.microsoft.com/nuget/)
- [dotnet restore](https://learn.microsoft.com/dotnet/core/tools/dotnet-restore)
- [dotnet nuget](https://learn.microsoft.com/dotnet/core/tools/dotnet-nuget)
