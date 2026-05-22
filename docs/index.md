# C# / .NET ナレッジベース

このワークスペースは、C# の環境構築から基礎文法、.NET 実務開発、Web API、テスト、運用までを段階的に学ぶためのメモ集です。

目的は、単に文法を覚えることではなく、実務でコードを読み、書き、直し、説明できる状態まで知識を積み上げることです。

## まず読むもの

1. [roadmap.md](roadmap.md) で学習順と到達目標を確認する。
2. [index_page.md](index_page.md) から現在の学習段階に合う記事を選ぶ。
3. [00_環境構築/01_dotnet-sdk.md](00_環境構築/01_dotnet-sdk.md) で .NET SDK を確認する。
4. [01_基礎文法/01_プログラム構造.md](01_基礎文法/01_プログラム構造.md) から C# の読み書きを始める。

## 学習方針

- C# の文法だけでなく、.NET CLI、NuGet、テスト、DI、ログ、データアクセス、ASP.NET Core まで扱う。
- コード例は Windows / PowerShell / VS Code で再現しやすい形にする。
- わからなかったこと、エラー、調査メモは [90_逆引き](90_逆引き/README.md) に残す。
- 新しい記事を追加したら、必ず [index_page.md](index_page.md) に追記する。

## 推奨環境

- Windows
- Visual Studio Code
- C# Dev Kit
- .NET 10 LTS を主軸にし、必要に応じて .NET 8 LTS との差分もメモする

## 記事の書き方

通常の記事は [_templates/article.md](_templates/article.md)、実習記事は [_templates/practice.md](_templates/practice.md) を元に作成します。
