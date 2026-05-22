# Request / Response

## 目的

HTTP request と response の基本を理解し、API の入出力を設計できるようにします。

## 要点

- request は method、path、query、header、body を持ちます。
- response は status code、header、body を持ちます。
- DTO を使って外部契約と内部モデルを分けます。

## コード例

```csharp
// この例では「Request / Response」の要点を最小のコードで確認します。
public record CreateProductRequest(string Name, decimal Price);
public record ProductResponse(int Id, string Name, decimal Price);
```

## コードの読み方

`CreateProductRequest` は client から受け取る入力 DTO、`ProductResponse` は client へ返す出力 DTO です。request と response を分けることで、作成時には不要な `Id` を request に含めず、response には作成後の `Id` を含められます。record を使うと、API 契約の shape を短く表現できます。

## 実務での使い方

API の response は利用者との契約です。Entity をそのまま返さず、必要な項目だけを DTO として返すと変更に強くなります。

一覧 API の query と metadata は [一覧 API のページング検索ソート](18_一覧APIのページング検索ソート.md)、status code と error body は [Status Code と ProblemDetails](17_StatusCodeとProblemDetails.md) で扱います。

## よくあるミス

- DB Entity をそのまま公開する。
- エラー時の response 形式がばらつく。
- status code と body の意味が合っていない。

## 関連リンク

- [HTTP overview](https://developer.mozilla.org/docs/Web/HTTP/Overview)
