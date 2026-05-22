# C# バージョン差分

## 目的

C# のバージョンによって使える構文が異なることを意識します。

## 見るポイント

- プロジェクトの `LangVersion`
- target framework
- チームのコーディング方針
- CI の SDK バージョン

## 代表的な確認先

- [What's new in C#](https://learn.microsoft.com/dotnet/csharp/whats-new/)
- [C# language versioning](https://learn.microsoft.com/dotnet/csharp/language-reference/configure-language-version)

## 実務での注意

新しい構文は便利ですが、チーム全員の環境、CI、本番ビルドで使えることを確認してから採用します。既存コードでは古い書き方も読める必要があります。
