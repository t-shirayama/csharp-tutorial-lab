# 13_ランタイムと高度なCSharp

C# と .NET runtime の深い仕組み、性能、メモリ、assembly、plugin model を扱うカテゴリです。

## 記事

- [01_CLRとマネージコード.md](01_CLRとマネージコード.md)
- [02_GCとオブジェクト寿命.md](02_GCとオブジェクト寿命.md)
- [03_ファイナライゼーション.md](03_ファイナライゼーション.md)
- [04_ボックス化と値型.md](04_ボックス化と値型.md)
- [05_アセンブリとメタデータ.md](05_アセンブリとメタデータ.md)
- [06_AssemblyLoadContextとプラグイン.md](06_AssemblyLoadContextとプラグイン.md)
- [07_SpanとMemory.md](07_SpanとMemory.md)
- [08_ReadOnlySequenceとPipelines.md](08_ReadOnlySequenceとPipelines.md)

## 到達目標

- CLR、managed code、GC の役割を説明できる。
- object lifetime、finalization、IDisposable の違いを説明できる。
- boxing、Span、Memory、Pipelines の性能上の意味を説明できる。
- assembly、metadata、AssemblyLoadContext の入口を理解し、plugin や load 問題を調査できる。
- plugin contract、discovery、unload、依存 assembly 解決の注意点を説明できる。

## 次に進むカテゴリ

- [99_参考資料](../99_参考資料/README.md)
- [90_逆引き](../90_逆引き/README.md)
