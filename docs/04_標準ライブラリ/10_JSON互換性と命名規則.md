# JSON 互換性と命名規則

## 目的

`System.Text.Json` で API 契約を壊さないための命名規則と互換性の考え方を理解します。

## 前提

- [System.Text.Json](04_SystemTextJson.md) を読んでいる
- [Request / Response](../10_WebとAPI/05_RequestResponse.md) を読んでいる

## 要点

- C# は PascalCase、JSON は camelCase がよく使われます。
- API の property 名を変更すると client 互換性に影響します。
- 追加は比較的安全ですが、削除や型変更は破壊的変更になりやすいです。

## コード例

```csharp
// この例では「JSON 互換性と命名規則」の要点を最小のコードで確認します。
var options = new JsonSerializerOptions
{
    // C# の PascalCase property を JSON では camelCase として出力します。
    PropertyNamingPolicy = JsonNamingPolicy.CamelCase,

    // 学習やログ確認では読みやすくなります。本番 API では必要性を判断します。
    WriteIndented = true
};
```

## コードの読み方

`JsonSerializerOptions` は JSON の出力形式を決める設定です。`PropertyNamingPolicy` を camelCase にすると、`DisplayName` は JSON では `displayName` になります。API 利用者から見ると property 名は契約なので、変更時は互換性を確認します。

## 属性で名前を固定する例

```csharp
// この例では「JSON 互換性と命名規則」の要点を最小のコードで確認します。
public class TodoResponse
{
    // C# 側の Id という名前とは別に、JSON 上の名前を固定します。
    [JsonPropertyName("todo_id")]
    public int Id { get; set; }
}
```

## 実務での使い方

外部公開 API では JSON 形式が契約になります。名前、null の扱い、日付形式、enum の表現をチームで揃えます。

## よくあるミス

- C# の property 名変更で JSON 名も意図せず変える。
- null と未指定の違いを考えない。
- 日付をローカル時刻の文字列で返す。
- enum を数値で返して意味が伝わらない。

## 関連リンク

- [Customize property names and values with System.Text.Json](https://learn.microsoft.com/dotnet/standard/serialization/system-text-json/customize-properties)
