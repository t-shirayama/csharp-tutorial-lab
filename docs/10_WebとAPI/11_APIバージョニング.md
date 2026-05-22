# API バージョニング

## 目的

公開 API を変更するときに、利用者との互換性を保つ設計を理解します。

## 前提

- [Request / Response](05_RequestResponse.md) を読んでいる
- [OpenAPI](08_OpenAPI.md) を読んでいる

## 要点

- API response の削除、名前変更、型変更は破壊的変更になりやすいです。
- version は URL、header、query parameter などで表現できます。
- 変更前に利用者、移行期間、廃止時期を決めます。

## 例

```text
GET /api/v1/todos
GET /api/v2/todos
```

## 実務での使い方

社内 API でも利用者が複数いるなら、変更履歴と移行手順を残します。OpenAPI の version と release note を合わせると追跡しやすくなります。

## よくあるミス

- response property を気軽に削除する。
- version を上げたのに OpenAPI が更新されない。
- v1 と v2 の違いをドキュメント化しない。
- 永遠に古い version を残し続ける。

## 関連リンク

- [ASP.NET API versioning](https://github.com/dotnet/aspnet-api-versioning)
