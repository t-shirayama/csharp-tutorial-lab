# EF Core クエリ設計

## 目的

EF Core の LINQ が SQL に変換されることを意識し、性能と保守性のあるクエリを書けるようにします。

## 前提

- [EF Core の基本](03_EFCoreの基本.md) を読んでいる
- [N+1 問題](07_N+1問題.md) を読んでいる

## 要点

- `IQueryable<T>` は DB に問い合わせる式です。
- `ToListAsync()` などで SQL が実行されます。
- 必要な列だけ `Select` すると転送量を減らせます。
- `AsNoTracking()` は読み取り専用 query で有効です。

## コード例

```csharp
// この例では「EF Core クエリ設計」の要点を最小のコードで確認します。
var todos = await db.Todos
    // 更新しない一覧取得なので、変更追跡を無効にします。
    .AsNoTracking()
    // 完了していない TODO だけを DB 側で絞り込みます。
    .Where(todo => !todo.Done)
    // DB 側で期限日順に並び替えます。
    .OrderBy(todo => todo.DueDate)
    // API 一覧に必要な列だけを DTO に projection します。
    .Select(todo => new TodoListItem(todo.Id, todo.Title, todo.DueDate))
    // ここで SQL が実行され、結果が List として確定します。
    .ToListAsync(cancellationToken);
```

## コードの読み方

この query は `ToListAsync` まで DB に送られません。`Where`、`OrderBy`、`Select` は SQL に変換される式として積み上がります。

ポイントは、必要な条件と列を DB 側に伝えてから結果を取得していることです。先に `ToListAsync()` してから絞り込むと、不要なデータを大量にメモリへ持ち込む可能性があります。

## 実務での使い方

一覧 API では絞り込み、並び替え、ページング、projection をセットで考えます。詳細 API と一覧 API で必要な列は違うため、同じ entity をそのまま返さないようにします。

## よくあるミス

- 先に `ToListAsync()` してからメモリ上で絞り込む。
- entity を API response として直接返す。
- index を考えずに検索条件を増やす。
- SQL ログを見ずに性能を推測する。

## 関連リンク

- [Efficient querying](https://learn.microsoft.com/ef/core/performance/efficient-querying)
