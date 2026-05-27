# 認証付きメモ API

## 目的

認証・認可を含む実務寄りの API を作り、ユーザーごとのデータ保護を理解します。

## 作るもの

ログイン済みユーザーだけが自分のメモを作成、取得、更新、削除できる API です。

## 学習ポイント

- Authentication / Authorization
- ユーザー ID と所有者チェック
- JWT または Cookie 認証
- 認可失敗時のレスポンス

## 認証方式の選び方

| 方式 | 向いている場面 | 注意点 |
| --- | --- | --- |
| Cookie 認証 | browser で同一 site の画面と API を扱う | CSRF 対策が必要 |
| JWT Bearer | SPA、mobile app、外部 client から API を呼ぶ | token 失効と漏えい時対応を設計する |
| ASP.NET Core Identity | user 登録、password reset、role などをまとめて扱う | 仕組みが大きいため要件に合うか確認する |
| 外部 IdP | Microsoft Entra ID など組織認証を使う | tenant、audience、claim mapping を確認する |

最初の学習では JWT Bearer か Cookie のどちらかに絞ります。実務では client の種類、token の保管場所、logout、refresh、監査ログ、rate limit を合わせて設計します。

## 実装ステップ

1. 認証方式を1つ選び、選んだ理由を README に残す。
2. `UserId` を claim から取得する helper を用意する。
3. `Memo` entity に `OwnerUserId` を持たせる。
4. query では必ず `OwnerUserId == currentUserId` を条件に入れる。
5. 未認証は `401 Unauthorized`、他人の resource は `404 Not Found` または `403 Forbidden` の方針を決める。
6. 認可失敗、成功操作、重要な変更を構造化ログに出す。
7. 自分のメモだけ取得できる integration test を追加する。

## 完了条件

- 未認証ユーザーはメモ API を使えない。
- ユーザーは自分のメモだけ操作できる。
- 認可失敗が適切な HTTP ステータスで返る。
- 重要な操作がログに残る。
- JWT / Cookie の選択理由と token 失効方針を説明できる。
