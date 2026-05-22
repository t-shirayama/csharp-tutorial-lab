# 90_逆引き

エラー、やりたいこと、コマンドから記事を探すためのカテゴリです。

## 記事

- [01_dotnetが見つからない.md](01_dotnetが見つからない.md)
- [02_ビルドエラーの読み方.md](02_ビルドエラーの読み方.md)
- [03_null関連エラー.md](03_null関連エラー.md)
- [04_NuGet復元エラー.md](04_NuGet復元エラー.md)
- [05_よく使うdotnetコマンド.md](05_よく使うdotnetコマンド.md)
- [06_VSCodeで補完が効かない.md](06_VSCodeで補完が効かない.md)
- [07_DI登録漏れ.md](07_DI登録漏れ.md)
- [08_Options検証エラー.md](08_Options検証エラー.md)
- [09_EFCoreMigration失敗.md](09_EFCoreMigration失敗.md)
- [10_HttpClientタイムアウト.md](10_HttpClientタイムアウト.md)
- [11_CIでdotnet-test失敗.md](11_CIでdotnet-test失敗.md)
- [12_async-await誤用.md](12_async-await誤用.md)
- [13_非同期デッドロック.md](13_非同期デッドロック.md)
- [14_EFCoreN+1問題.md](14_EFCoreN+1問題.md)
- [15_CORSエラー.md](15_CORSエラー.md)
- [16_RateLimitと429.md](16_RateLimitと429.md)
- [17_SQLInjection対策.md](17_SQLInjection対策.md)
- [18_デプロイ失敗.md](18_デプロイ失敗.md)
- [19_Container起動失敗.md](19_Container起動失敗.md)
- [20_EFCore性能問題.md](20_EFCore性能問題.md)

## 症状から探す

- コマンドが動かない: [01_dotnetが見つからない.md](01_dotnetが見つからない.md), [04_NuGet復元エラー.md](04_NuGet復元エラー.md)
- build / test / deploy が失敗する: [02_ビルドエラーの読み方.md](02_ビルドエラーの読み方.md), [11_CIでdotnet-test失敗.md](11_CIでdotnet-test失敗.md), [18_デプロイ失敗.md](18_デプロイ失敗.md)
- null が原因で落ちる: [03_null関連エラー.md](03_null関連エラー.md)
- DI / Options で起動しない: [07_DI登録漏れ.md](07_DI登録漏れ.md), [08_Options検証エラー.md](08_Options検証エラー.md)
- EF Core が遅い、migration で詰まる: [09_EFCoreMigration失敗.md](09_EFCoreMigration失敗.md), [14_EFCoreN+1問題.md](14_EFCoreN+1問題.md), [20_EFCore性能問題.md](20_EFCore性能問題.md)
- HTTP / Web API が詰まる: [10_HttpClientタイムアウト.md](10_HttpClientタイムアウト.md), [15_CORSエラー.md](15_CORSエラー.md), [16_RateLimitと429.md](16_RateLimitと429.md)
- 非同期処理が止まる、追えない: [12_async-await誤用.md](12_async-await誤用.md), [13_非同期デッドロック.md](13_非同期デッドロック.md)
- security review で指摘された: [17_SQLInjection対策.md](17_SQLInjection対策.md)
- container が起動しない: [19_Container起動失敗.md](19_Container起動失敗.md)

## 書き方

- エラー文をそのまま見出しに含める。
- 原因、確認コマンド、解決手順を書く。
- 関連する基礎記事へリンクする。
