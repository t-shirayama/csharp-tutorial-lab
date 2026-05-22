# 外部 API 連携

## 目的

ASP.NET Core アプリから外部 API を呼ぶときの責務分割、設定、エラー処理を理解します。

## 前提

- [HttpClientFactory](../04_標準ライブラリ/09_HttpClientFactory.md) を読んでいる
- [IOptions と Configuration](../07_設計と実務パターン/10_IOptionsとConfiguration.md) を読んでいる

## 要点

- 外部 API ごとに typed client を作ります。
- BaseUrl、timeout、API key は設定から渡します。
- HTTP status、timeout、retry、ログ、監視を設計します。
- 外部 API の response をそのまま内部 model にしない方が安全です。

## 構成例

```text
WeatherApiClient
WeatherApiOptions
WeatherResponse
WeatherService
```

## 実務での使い方

外部 API 障害は自アプリの障害として見えます。失敗時の fallback、利用者に返す status、監視アラート、retry 可能性を事前に決めます。

typed client、options、timeout、non-success handling の実装例は [外部 API Typed Client 実装](20_外部APITypedClient実装.md) で扱います。

## よくあるミス

- API key をコードに直書きする。
- timeout なしで待ち続ける。
- 外部 API の DTO を domain model として使い回す。
- non-success status を成功扱いする。

## 関連リンク

- [Make HTTP requests using IHttpClientFactory](https://learn.microsoft.com/aspnet/core/fundamentals/http-requests)
