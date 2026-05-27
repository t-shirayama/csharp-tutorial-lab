# CLR とマネージコード

## 目的

.NET アプリを実行する CLR と、managed code の基本を理解します。

## 前提

- [.NET SDK のインストールと確認](../00_環境構築/01_dotnet-sdk.md) を読んでいる
- [C# のプログラム構造](../01_基礎文法/01_プログラム構造.md) を読んでいる

## 要点

- CLR は Common Language Runtime の略です。
- C# は IL に compile され、runtime によって実行されます。
- managed code は、GC、型安全性、例外処理など runtime の管理下で動きます。

## コード例

```csharp
// この例では「CLR とマネージコード」の要点を最小のコードで確認します。
Console.WriteLine(Environment.Version);
Console.WriteLine(System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription);
```

## コードの読み方

`Environment.Version` と `RuntimeInformation.FrameworkDescription` で、実行中の .NET runtime 情報を確認できます。SDK の version と runtime の version は別物です。

## 実務での使い方

runtime mismatch、container image、production 障害調査、AOT / trimming、ライブラリ互換性の理解に関係します。普段は意識しなくても動きますが、問題が起きたときに runtime と SDK を分けて考える必要があります。

## よくあるミス

- SDK version と runtime version を混同する。
- C# の文法機能と .NET runtime の機能を混同する。
- managed code なら resource 解放を一切気にしなくてよいと思う。

## 関連リンク

- [.NET fundamentals](https://learn.microsoft.com/dotnet/fundamentals/)
- [.NET runtime overview](https://learn.microsoft.com/dotnet/core/introduction)
