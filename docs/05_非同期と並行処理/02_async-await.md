# async / await

## 目的

`async` と `await` を使って、非同期処理を読みやすく書く方法を理解します。

## 要点

- `async` を付けたメソッド内で `await` を使えます。
- `await` は Task の完了を待ち、結果を取り出します。
- メソッド名には慣習として `Async` を付けます。

## コード例

```csharp
// この例では「async / await」の要点を最小のコードで確認します。
static async Task<int> GetLengthAsync(string url)
{
    using var httpClient = new HttpClient();
    var text = await httpClient.GetStringAsync(url);
    return text.Length;
}
```

## コードの読み方

このコード例は「async / await」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

ASP.NET Core、EF Core、HttpClient では async API が標準です。呼び出し元まで async を伝播させ、途中で同期ブロックしないようにします。

## よくあるミス

- `async void` を通常メソッドで使う。
- `await` し忘れて例外や実行順を見失う。
- async メソッド内で重い CPU 処理をそのまま実行する。

## 関連リンク

- [async and await](https://learn.microsoft.com/dotnet/csharp/asynchronous-programming/)
