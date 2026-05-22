# null と空コレクション

## 目的

コレクションで null と空をどう扱うかを理解し、呼び出し側が扱いやすい API を書けるようにします。

## 要点

- null は「値がない」、空コレクションは「0件」を表します。
- 一覧を返すメソッドでは、基本的に null ではなく空コレクションを返します。
- 入力として null を受け取る可能性がある場合は、早めに検証します。

## コード例

```csharp
// この例では「null と空コレクション」の要点を最小のコードで確認します。
static IReadOnlyList<string> FindNames(string keyword)
{
    if (string.IsNullOrWhiteSpace(keyword))
    {
        return Array.Empty<string>();
    }

    return new[] { "Sato", "Suzuki" }
        .Where(name => name.Contains(keyword, StringComparison.OrdinalIgnoreCase))
        .ToList();
}
```

## コードの読み方

このコード例は「null と空コレクション」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

呼び出し側に毎回 null チェックを強制しない API は扱いやすくなります。検索結果が0件であることは異常ではないため、空コレクションとして表すのが自然です。

## よくあるミス

- 0件を null で返す。
- null と空の意味を決めずに実装する。
- `items.Count == 0` を呼ぶ前に items が null かどうかを考えていない。

## 練習問題

1. 条件に合う商品がない場合に空リストを返すメソッドを書く。
2. null を受け取った場合に例外にするか空扱いにするかを決める。
3. 呼び出し側の null チェックが減る API に書き換える。

## 関連リンク

- [Nullable reference types](https://learn.microsoft.com/dotnet/csharp/nullable-references)
