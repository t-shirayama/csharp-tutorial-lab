# 一覧 API のページング検索ソート

## 目的

一覧 API で必要になる paging、filtering、sorting の基本設計を理解します。

## 前提

- [Routing](04_Routing.md) を読んでいる
- [Request / Response](05_RequestResponse.md) を読んでいる
- [LINQ の基本](../03_コレクションとLINQ/04_LINQの基本.md) を読んでいる

## 要点

- 一覧 API は件数が増える前提で設計します。最初から page size の上限、sort の許可項目、filter 条件を決めます。
- `page` と `pageSize` は query parameter で受けることが多いです。`pageSize` に上限を設けないと、大量取得で DB や API が重くなります。
- sort は自由文字列ではなく、許可した項目だけを受け付けます。存在しない property 名をそのまま dynamic に評価すると危険です。
- 検索 keyword は `q` や `keyword` として受けます。部分一致、大文字小文字、文化圏、LIKE 検索の性能を考慮します。
- response には items だけでなく、`page`、`pageSize`、`totalCount` などの metadata を返すと client が UI を作りやすくなります。
- page 番号方式は扱いやすい一方、大量データやリアルタイム更新では cursor pagination も検討します。

## Request と Response の型

```csharp
public sealed record ProductListQuery(
    int Page = 1,
    int PageSize = 20,
    string? Q = null,
    string Sort = "name");

public sealed record PagedResponse<T>(
    IReadOnlyList<T> Items,
    int Page,
    int PageSize,
    int TotalCount);
```

## Minimal API の例

```csharp
app.MapGet("/products", async ([AsParameters] ProductListQuery query, IProductQueryService service) =>
{
    var page = Math.Max(query.Page, 1);
    var pageSize = Math.Clamp(query.PageSize, 1, 100);

    var sort = query.Sort.ToLowerInvariant();
    if (sort is not "name" and not "price" and not "createdat")
    {
        return Results.BadRequest("Unsupported sort field.");
    }

    var result = await service.SearchAsync(page, pageSize, query.Q, sort);

    var response = new PagedResponse<ProductResponse>(
        result.Items,
        page,
        pageSize,
        result.TotalCount);

    return Results.Ok(response);
});
```

## query service の境界

```csharp
public interface IProductQueryService
{
    Task<PagedResponse<ProductResponse>> SearchAsync(
        int page,
        int pageSize,
        string? keyword,
        string sort);
}
```

## コードの読み方

`[AsParameters]` は query string を `ProductListQuery` に bind します。`Math.Clamp` で `pageSize` を `1` から `100` に制限し、API 利用者が極端に大きい件数を要求できないようにしています。`sort` は許可した項目だけを受け付け、実際の検索処理は `IProductQueryService` に委ねています。

## 実務での使い方

一覧 API は画面表示、CSV 出力、外部連携で頻繁に使われます。仕様として、既定の sort、最大 page size、total count の有無、検索対象、権限による絞り込みを明確にします。EF Core を使う場合は、`Skip` / `Take` の前に `OrderBy` を指定し、必要な列だけを DTO に projection します。

## よくあるミス

- `pageSize` の上限を設けず、大量取得を許してしまう。
- sort field を自由文字列のまま扱う。
- `totalCount` が必要な画面なのに response に含めない。
- page 番号が 0 始まりか 1 始まりかを決めない。
- 認可条件を検索条件に含め忘れ、他ユーザーのデータが見える。

## 練習問題

1. `page` と `pageSize` を query parameter で受け取る。
2. `pageSize` の最大値を 100 に制限する。
3. `sort` の許可項目を `name` と `createdAt` に限定する。
4. `items` と `totalCount` を含む response を返す。

## 関連リンク

- [Request / Response](05_RequestResponse.md)
- [Routing](04_Routing.md)
- [ソートとページング](../03_コレクションとLINQ/14_ソートとページング.md)
