# OpenAPI を契約として整える

## 目的

OpenAPI を単なる自動生成結果ではなく、API 利用者との契約として整備する観点を理解します。

## 前提

- [OpenAPI](08_OpenAPI.md) を読んでいる
- [Status Code と ProblemDetails](17_StatusCodeとProblemDetails.md) を読んでいる
- [API バージョニング](11_APIバージョニング.md) を読んでいる

## 要点

- OpenAPI は endpoint 一覧ではなく、request、response、status code、認証方式、利用例を共有する契約です。
- 成功 response だけでなく、`400`、`401`、`403`、`404`、`409`、`500` などの error response も仕様に含めます。
- API 利用者が request body や response body を理解できるよう、DTO の property 名、必須項目、型、nullable、例を揃えます。
- 認証が必要な endpoint は security scheme と requirement を明示します。認証が必要かどうかが仕様から分からない API は利用しにくくなります。
- versioning する場合は、version ごとの document や path を整理します。v1 と v2 の差分は changelog や migration guide と合わせます。
- Development でだけ Swagger UI を公開するなど、公開範囲を制御します。内部 endpoint や secret 情報を production に出さないようにします。

## endpoint metadata を付ける例

```csharp
app.MapGet("/products/{id:int}", async (int id, IProductService service) =>
    {
        var product = await service.FindAsync(id);
        return product is null ? Results.NotFound() : Results.Ok(product);
    })
    .WithName("GetProductById")
    .WithTags("Products")
    .Produces<ProductResponse>(StatusCodes.Status200OK)
    .ProducesProblem(StatusCodes.Status404NotFound)
    .ProducesProblem(StatusCodes.Status500InternalServerError);
```

## request と response を明示する例

```csharp
app.MapPost("/products", async (CreateProductRequest request, IProductService service) =>
    {
        var product = await service.CreateAsync(request.Name, request.Price);
        var response = new ProductResponse(product.Id, product.Name, product.Price);

        return Results.Created($"/products/{product.Id}", response);
    })
    .WithName("CreateProduct")
    .WithTags("Products")
    .Accepts<CreateProductRequest>("application/json")
    .Produces<ProductResponse>(StatusCodes.Status201Created)
    .ProducesProblem(StatusCodes.Status400BadRequest)
    .ProducesProblem(StatusCodes.Status409Conflict);
```

## 公開範囲を制御する

```csharp
builder.Services.AddOpenApi();

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
}
```

## コードの読み方

`.Produces<ProductResponse>(StatusCodes.Status200OK)` は、成功時の response body と status code を OpenAPI に伝えます。`.ProducesProblem` は error response が `ProblemDetails` 形式であることを示します。`WithName` は endpoint 名、`WithTags` は OpenAPI 上の分類に使われます。

## 実務での使い方

フロントエンド、mobile app、外部 partner、test automation は OpenAPI を見て実装することがあります。仕様が実装とずれると障害につながるため、PR では endpoint の実装だけでなく OpenAPI の見え方も確認します。公開 API では、breaking change の有無も OpenAPI diff で確認します。

## よくあるミス

- 成功 response だけを定義し、error response を書かない。
- nullable や required の設定が実装とずれる。
- 認証が必要な endpoint が仕様上は匿名に見える。
- Swagger UI を production へ無条件公開する。
- version を上げても OpenAPI document や client 生成を更新しない。

## 練習問題

1. `GET /products/{id}` に `200`、`404`、`500` の response metadata を付ける。
2. `POST /products` に request body と `201` response を明示する。
3. 認証が必要な endpoint を OpenAPI 上で分かるようにする。
4. Development 以外では OpenAPI endpoint を公開しない設定にする。

## 関連リンク

- [OpenAPI](08_OpenAPI.md)
- [Status Code と ProblemDetails](17_StatusCodeとProblemDetails.md)
- [OpenAPI support in ASP.NET Core](https://learn.microsoft.com/aspnet/core/fundamentals/openapi/overview)
