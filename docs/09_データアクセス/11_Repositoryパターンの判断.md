# Repository パターンの判断

## 目的

EF Core を使うときに Repository を追加すべきか、DbContext を直接使うべきかを判断します。

## 前提

- [EF Core の基本](03_EFCoreの基本.md) を読んでいる
- [レイヤードアーキテクチャ](../07_設計と実務パターン/07_レイヤードアーキテクチャ.md) を読んでいる

## 要点

- EF Core の `DbContext` 自体が Unit of Work と Repository に近い役割を持ちます。
- 単純な CRUD だけなら独自 Repository は過剰なことがあります。
- 複雑な query、永続化技術の隠蔽、テスト境界が必要なら検討します。

## 判断表

| 状況 | 判断 |
| --- | --- |
| 小規模 CRUD | DbContext 直接でもよい |
| 複雑な query を再利用する | query service や repository を検討 |
| 永続化技術を隠したい | interface を検討 |
| EF の機能を多用する | 抽象化で隠しすぎない |

## 実務での使い方

Repository を作る場合も、`GetAll()` のような汎用メソッドだらけにせず、業務上意味のある操作にします。読み取りは query service、更新は command service に分ける方法もあります。

## よくあるミス

- すべての entity に機械的に repository を作る。
- `IQueryable<T>` を外へ漏らして抽象化が崩れる。
- EF Core の強みを隠しすぎて逆に複雑にする。
- テストしやすさだけを理由に不自然な repository を作る。

## 関連リンク

- [DbContext lifetime, configuration, and initialization](https://learn.microsoft.com/ef/core/dbcontext-configuration/)
