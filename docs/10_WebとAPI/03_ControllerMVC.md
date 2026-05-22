# Controller / MVC

## 目的

Controller ベースの Web API と MVC の基本構造を理解します。

## 要点

- Controller は関連する endpoint をクラスにまとめます。
- attribute routing で URL と action を対応付けます。
- 大きめの API では構造化しやすいです。

## コード例

```csharp
// この例では「Controller / MVC」の要点を最小のコードで確認します。
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet("{id:int}")]
    public IActionResult Get(int id) => Ok(new { Id = id });
}
```

## コードの読み方

`[ApiController]` は Web API 向けの動作を有効にし、`[Route("api/[controller]")]` は controller 名を使って URL を作ります。`[HttpGet("{id:int}")]` は `GET /api/products/1` のような request を `Get` action に対応付けます。`Ok(...)` は `200 OK` の response を返します。

## 実務での使い方

複数 endpoint、filter、model binding、validation、認証属性を整理したい場合に向いています。Controller に業務ロジックを詰め込まないよう service に分けます。

Minimal API との選択基準は [Minimal API と Controller の選び方](16_MinimalAPIとControllerの選び方.md)、HTTP response の方針は [Status Code と ProblemDetails](17_StatusCodeとProblemDetails.md) で扱います。

## よくあるミス

- Controller が肥大化する。
- HTTP と業務ロジックの境界が混ざる。
- status code の方針がばらつく。

## 関連リンク

- [Create web APIs with ASP.NET Core](https://learn.microsoft.com/aspnet/core/web-api/)
