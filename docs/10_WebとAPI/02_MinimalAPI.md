# Minimal API

## 目的

少ないコードで Web API を作る Minimal API の基本を理解します。

## 要点

- `MapGet`, `MapPost`, `MapPut`, `MapDelete` でエンドポイントを定義します。
- 小さな API やマイクロサービスで簡潔に書けます。
- 複雑になったら endpoint group や Controller を検討します。

## コード例

```csharp
// この例では「Minimal API」の要点を最小のコードで確認します。
app.MapGet("/todos/{id:int}", (int id) => Results.Ok(new { Id = id, Title = "Learn C#" }));
```

## コードの読み方

`MapGet("/todos/{id:int}", ...)` は `/todos/1` のような GET request を受け取る endpoint です。`{id:int}` は route parameter を `int` に制限し、handler の `(int id)` に渡します。`Results.Ok(...)` は HTTP `200 OK` と JSON response を返します。

## 実務での使い方

ヘルスチェック、内部 API、小さな CRUD で使いやすいです。validation、認証、レスポンス型の整理を怠るとすぐ読みにくくなります。

endpoint が増える場合は `MapGroup` や extension method で分割します。Controller との選び方は [Minimal API と Controller の選び方](16_MinimalAPIとControllerの選び方.md)、CRUD の組み立ては [CRUD API の実装パターン](15_CRUD APIの実装パターン.md) で扱います。

## よくあるミス

- 1つの `Program.cs` に全 endpoint を詰め込む。
- 入力 validation を省略する。
- レスポンス形式が endpoint ごとにばらつく。

## 関連リンク

- [Minimal APIs](https://learn.microsoft.com/aspnet/core/fundamentals/minimal-apis)
