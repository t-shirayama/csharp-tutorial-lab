# release と versioning

## 目的

リリースとバージョン管理の基本を理解し、変更を安全に届けられるようにします。

## 要点

- バージョンは利用者との変更契約です。
- breaking change、feature、bug fix を区別します。
- リリースノートで変更内容と移行手順を残します。

## 実務での使い方

NuGet パッケージ、API、アプリケーションリリースで使います。API の breaking change はクライアント影響が大きいため、互換性維持や段階移行を考えます。

## よくあるミス

- breaking change を patch として出す。
- リリース内容が Git の差分を見ないと分からない。
- rollback 手順を用意しない。

## 練習問題

1. 変更を breaking / feature / fix に分類する。
2. リリースノートのテンプレートを作る。
3. API の v1 から v2 への移行方針を考える。

## 関連リンク

- [Semantic Versioning](https://semver.org/)
