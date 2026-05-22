# Environment と設定値

## 目的

環境変数や実行環境の情報を取得する基本を理解します。

## 要点

- `Environment.GetEnvironmentVariable` で環境変数を取得できます。
- 実務では秘密情報や環境差分をコードに直書きしません。
- ASP.NET Core では `IConfiguration` と `appsettings` を使うことが多いです。

## コード例

```csharp
// この例では「Environment と設定値」の要点を最小のコードで確認します。
var environmentName = Environment.GetEnvironmentVariable("DOTNET_ENVIRONMENT") ?? "Production";

Console.WriteLine(environmentName);
Console.WriteLine(Environment.CurrentDirectory);
Console.WriteLine(Environment.MachineName);
```

## コードの読み方

このコード例は「Environment と設定値」の基本形を確認するためのものです。上から順に、値や object を用意し、C# の構文や .NET API を使い、最後に結果を確認します。まず入力、処理、出力の 3 つに分けて読むと、初学者でも流れを追いやすくなります。

## 実務での使い方

DB 接続文字列、API キー、環境名、外部サービス URL などは環境ごとに変わります。コードに埋め込まず、設定として注入できる形にします。

## よくあるミス

- 秘密情報をソースコードや Git に入れる。
- 開発環境のパスを前提にして本番で壊れる。
- 設定値がない場合の扱いを決めていない。

## 練習問題

1. 任意の環境変数を PowerShell で設定し、C# から読む。
2. 値がない場合のデフォルト値を決める。
3. 設定値の必須チェックをメソッドに分ける。

## 関連リンク

- [Environment](https://learn.microsoft.com/dotnet/api/system.environment)
