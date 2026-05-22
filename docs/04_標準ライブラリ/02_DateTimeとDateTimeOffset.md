# DateTime と DateTimeOffset

## 目的

日時を扱う基本と、`DateTime` と `DateTimeOffset` の使い分けを理解します。

## 要点

- `DateTime.Now` はローカル時刻、`DateTime.UtcNow` は UTC です。
- `DateTimeOffset` は時刻と UTC からのオフセットを持ちます。
- 実務では保存や API 連携でタイムゾーンを明確にします。

## コード例

```csharp
// この例では「DateTime と DateTimeOffset」の要点を最小のコードで確認します。
var now = DateTimeOffset.Now;
var utcNow = DateTimeOffset.UtcNow;

Console.WriteLine(now);
Console.WriteLine(utcNow);
Console.WriteLine(now.ToString("yyyy-MM-dd HH:mm:ss"));
```

## コードの読み方

このコード例は「DateTime と DateTimeOffset」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

DB 保存や API の日時は UTC または `DateTimeOffset` で扱う方針を決めます。画面表示ではユーザーのタイムゾーンに変換します。

## よくあるミス

- ローカル時刻と UTC を混ぜる。
- 日付だけの値に時刻やタイムゾーンの意味を持たせる。
- 文字列化した日時を再パースして誤差やタイムゾーンずれを起こす。

## 練習問題

1. 現在時刻を UTC とローカルで表示する。
2. 日付を `yyyy-MM-dd` 形式で表示する。
3. 期限日が現在より過去かどうか判定する。

## 関連リンク

- [DateTimeOffset](https://learn.microsoft.com/dotnet/api/system.datetimeoffset)
