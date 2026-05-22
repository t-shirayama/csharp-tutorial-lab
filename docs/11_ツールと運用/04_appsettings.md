# appsettings

## 目的

ASP.NET Core などで使う `appsettings.json` と環境別設定を理解します。

## 要点

- `appsettings.json` は基本設定です。
- `appsettings.Development.json` などで環境別に上書きできます。
- 秘密情報は appsettings に直接置かず、User Secrets や環境変数を使います。

## コード例

```json
{
  "ExternalApi": {
    "BaseUrl": "https://example.com"
  }
}
```

## 実務での使い方

接続文字列、外部 API URL、ログレベル、機能フラグを管理します。必須設定は起動時に validation します。

## よくあるミス

- 本番秘密情報をコミットする。
- 環境ごとの上書き順を理解しない。
- 設定名を文字列で散らばらせる。

## 関連リンク

- [Configuration in ASP.NET Core](https://learn.microsoft.com/aspnet/core/fundamentals/configuration/)
