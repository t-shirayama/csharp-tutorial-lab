# テストプロジェクト付き solution

## 目的

実務でよく使う、アプリ本体とテストを分けた solution 構成を CLI で作れるようにします。

## 前提

- [プロジェクト作成と実行](03_プロジェクト作成と実行.md) を読んでいる
- [NuGet 復元とパッケージソース](08_NuGet復元とパッケージソース.md) を読んでいる
- PowerShell で `dotnet` コマンドを実行できる

## 要点

- 実務では、アプリ本体 project と test project を分ける構成がよく使われます。
- solution file は複数 project をまとめて扱う入口です。repository root で `dotnet build` や `dotnet test` を実行しやすくなります。
- test project は本体 project を参照します。`dotnet add reference` で project 間の参照を明示します。
- xUnit、NUnit、MSTest などの test framework があります。このナレッジベースでは xUnit を基本例にします。
- `src/` と `tests/` に分けると、本体 code と test code の場所が分かりやすくなります。
- clone 後に `dotnet restore`、`dotnet build`、`dotnet test` が通ることは、開発環境が整っているかの重要な確認です。

## solution を作る

```powershell
dotnet new sln -n SampleApp
New-Item -ItemType Directory -Path src, tests
```

## アプリ本体 project を作る

```powershell
dotnet new classlib -n SampleApp.Core -o src/SampleApp.Core
dotnet sln add src/SampleApp.Core/SampleApp.Core.csproj
```

## test project を作る

```powershell
dotnet new xunit -n SampleApp.Core.Tests -o tests/SampleApp.Core.Tests
dotnet sln add tests/SampleApp.Core.Tests/SampleApp.Core.Tests.csproj
dotnet add tests/SampleApp.Core.Tests/SampleApp.Core.Tests.csproj reference src/SampleApp.Core/SampleApp.Core.csproj
```

## build と test を実行する

```powershell
dotnet restore
dotnet build
dotnet test
```

期待する状態:

- `dotnet build` が成功する。
- `dotnet test` が成功し、test 件数が表示される。
- solution root でコマンドが実行できる。

## 最小のテスト例

本体 project に次の class を追加します。

```csharp
namespace SampleApp.Core;

public static class PriceCalculator
{
    public static decimal CalculateSubtotal(decimal unitPrice, int quantity)
    {
        if (unitPrice < 0)
        {
            throw new ArgumentOutOfRangeException(nameof(unitPrice));
        }

        if (quantity <= 0)
        {
            throw new ArgumentOutOfRangeException(nameof(quantity));
        }

        return unitPrice * quantity;
    }
}
```

test project に次の test を追加します。

```csharp
using SampleApp.Core;

namespace SampleApp.Core.Tests;

public class PriceCalculatorTests
{
    [Fact]
    public void CalculateSubtotal_ReturnsUnitPriceTimesQuantity()
    {
        var subtotal = PriceCalculator.CalculateSubtotal(1200m, 3);

        Assert.Equal(3600m, subtotal);
    }
}
```

## コードの読み方

`dotnet new sln` は solution file を作ります。`dotnet sln add` は solution に project を登録します。`dotnet add reference` は test project から本体 project を参照する設定です。テスト例では、入力 `1200m` と `3` に対して、期待値 `3600m` になることを `Assert.Equal` で確認しています。

## 実務での使い方

新規 project を作るときは、最初から test project を置くと、後で品質確認を追加しやすくなります。CI では repository root で `dotnet test` を実行する構成にしておくと、ローカルと CI の確認手順を揃えられます。

## よくあるミス

- test project を solution に追加し忘れ、`dotnet test` の対象にならない。
- test project から本体 project への reference を追加し忘れる。
- repository root ではなく project directory だけで確認してしまう。
- test framework の package restore 失敗を build error と混同する。

## 練習問題

1. `SampleApp` solution を作成する。
2. `src/` に class library、`tests/` に xUnit project を作る。
3. test project から本体 project を参照する。
4. 最小の計算 method と test を追加し、`dotnet test` を成功させる。

## 関連リンク

- [dotnet sln](https://learn.microsoft.com/dotnet/core/tools/dotnet-sln)
- [dotnet test](https://learn.microsoft.com/dotnet/core/tools/dotnet-test)
- [Unit testing C# with xUnit](https://learn.microsoft.com/dotnet/core/testing/unit-testing-csharp-with-xunit)
