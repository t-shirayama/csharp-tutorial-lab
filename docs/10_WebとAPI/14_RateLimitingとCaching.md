# Rate Limiting と Caching

## 目的

API を守る rate limiting と、response を効率化する HTTP caching の入口を理解します。

## 前提

- [Request / Response](05_RequestResponse.md) を読んでいる
- [Middleware](09_Middleware.md) を読んでいる

## 要点

- rate limiting は短時間に多すぎる request を制限します。
- HTTP caching は `Cache-Control`、`ETag`、`Last-Modified` などで再取得を減らします。
- 認証済み user ごとの response と共有 cache は慎重に分けます。
- cache は速くしますが、古いデータを返す可能性も作ります。

## Rate limiting の例

```csharp
// この例では固定 window の rate limiter を登録します。
builder.Services.AddRateLimiter(options =>
{
    options.AddFixedWindowLimiter("api", limiterOptions =>
    {
        limiterOptions.PermitLimit = 100;
        limiterOptions.Window = TimeSpan.FromMinutes(1);
    });
});

var app = builder.Build();

app.UseRateLimiter();

app.MapGet("/products", () => Results.Ok())
    .RequireRateLimiting("api");
```

## Cache-Control の例

```csharp
// この例では短時間だけ public cache を許可します。
app.MapGet("/catalog", (HttpResponse response) =>
{
    response.Headers.CacheControl = "public,max-age=60";
    return Results.Ok(new[] { "Keyboard", "Mouse" });
});
```

## コードの読み方

rate limiting は API への流量を制御し、cache は同じ response の再取得を減らします。どちらも middleware や header として働くため、endpoint の業務処理とは分けて考えます。

## 実務での使い方

login、search、外部 API proxy、重い集計 endpoint では rate limiting を検討します。商品 catalog のように短時間なら古くてもよいデータは cache しやすい一方、個人情報や権限で内容が変わる response は cache 設計に注意します。

## よくあるミス

- rate limiting を入れず、外部からの burst で DB を巻き込む。
- user ごとの response に public cache を付ける。
- cache invalidation の方針を決めずに cache する。
- 429 response の意味を client と共有しない。

## 関連リンク

- [Rate limiting middleware](https://learn.microsoft.com/aspnet/core/performance/rate-limit)
- [Response caching](https://learn.microsoft.com/aspnet/core/performance/caching/response)
