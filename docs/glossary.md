# GLOSSARY

C# / .NET の用語を短く整理します。詳しい説明は各カテゴリの記事へリンクします。

## C# 言語

Microsoft が開発している、.NET 上でよく使われるプログラミング言語です。業務アプリ、Web API、デスクトップアプリ、ゲーム開発などで使われます。

## .NET

C# などの言語でアプリケーションを作るための実行環境、標準ライブラリ、CLI、SDK を含むプラットフォームです。

## SDK

Software Development Kit の略です。アプリを作るためのコンパイラ、CLI、テンプレート、ライブラリを含みます。

## CLR

Common Language Runtime の略です。.NET アプリを実行する基盤です。

## assembly

.NET アプリやライブラリの配置単位です。型情報、メタデータ、IL、リソースなどを含みます。

## metadata

assembly に含まれる型、メンバー、属性などの情報です。reflection や serializer、DI container などが参照します。

## AssemblyLoadContext

.NET runtime が assembly を読み込む context です。plugin や依存関係の分離で使います。

## plugin contract

host application と plugin の間で共有する interface や DTO です。plugin の具象型へ直接依存しないための境界になります。

## NuGet

.NET のパッケージ管理システムです。外部ライブラリの追加や更新に使います。

## repository

source code、設定、履歴を Git などで管理する単位です。実務では repository を clone して restore、build、test を確認してから作業を始めます。

## package source

NuGet package を取得する配布元です。nuget.org、社内 feed、Azure Artifacts、GitHub Packages などがあります。

## LINQ

Language Integrated Query の略です。コレクションやデータに対して、問い合わせのような書き方で処理できます。

## delegate

メソッドを値として扱うための型です。コールバック、イベント、LINQ、非同期処理の土台として使われます。

## event

ある出来事を購読者へ通知するための仕組みです。内部的には delegate と関連しますが、外部からの呼び出しを制限できます。

## GC

Garbage Collection の略です。不要になった managed object のメモリを .NET runtime が回収する仕組みです。

## finalizer

GC が object を回収する前に呼び出す可能性がある後始末です。呼び出しタイミングは予測できず、通常は `IDisposable` を優先します。

## boxing

値型を `object` や interface として扱うために heap 上へ包む処理です。頻繁に起きると allocation が増えます。

## IDisposable

ファイル、DB 接続、HTTP response など、明示的に解放したい resource を扱うための interface です。`using` と組み合わせて使います。

## スコープ

変数や名前が有効な範囲です。ブロック、メソッド、クラスなどの単位で変数を参照できる場所が決まります。

## 値オブジェクト

ID ではなく値そのものの意味で等価性を判断する小さな型です。金額、メールアドレス、期間などの表現に使います。

## materialize

遅延評価の LINQ クエリを `ToList()` などで実際のコレクションに確定させることです。

## TimeProvider

.NET の時刻取得を抽象化する型です。現在時刻やタイマーをテストで差し替える用途に使います。

## TimeSpan

期間や経過時間を表す .NET の型です。timeout、retry interval、cache lifetime などで使います。

## StringBuilder

大量の文字列を効率よく組み立てるための型です。loop での文字列連結や report 生成で使います。

## Parse

文字列を数値や日時など別の型へ変換する処理です。ユーザー入力のように失敗が想定される場合は、例外ではなく `TryParse` で分岐として扱うことが多いです。

## DateTimeOffset

日時と UTC からの offset を一緒に表す .NET の型です。外部 API、ログ、保存データなどで時差の誤解を減らしたい場合に使われます。

## using

`IDisposable` な object を使い終わったときに確実に解放するための C# 構文です。ファイル、stream、DB 接続などの resource 管理で使います。

## IHttpClientFactory

`HttpClient` の生成と管理を DI 経由で行う仕組みです。接続管理、名前付き client、typed client に使います。

## バックプレッシャー

producer が consumer より速くデータを投入するときに、待機や破棄などで流量を抑える考え方です。`Channel<T>` の bounded channel で扱います。

## IAsyncEnumerable

非同期に順次値を返す stream を表す interface です。大量データや逐次取得を `await foreach` で扱えます。

## required

object 初期化時に必須 property の設定をコンパイラに促す C# の修飾子です。

## init

object 初期化時だけ値を設定できる property setter です。作成後に変更しにくい型を作るときに使います。

## async / await

非同期処理を書くための C# の構文です。I/O 待ちを含む処理でよく使います。

## ValueTask

同期的に完了することが多い高頻度 API で allocation を抑えるための非同期戻り値です。通常の業務コードでは `Task` を優先します。

## IProgress&lt;T&gt;

長い処理の進捗を呼び出し側へ通知するための interface です。CLI、desktop app、batch などで使います。

## Nullable&lt;T&gt;

値型に null を許すための struct です。`int?` は `Nullable<int>` の短縮形です。

## record struct

値型として振る舞う record です。小さな値オブジェクトで、値の等価性と copy しやすさを両立したい場合に候補になります。

## Result&lt;T&gt;

処理の成功と失敗を戻り値として表す generic 型の設計例です。validation error や domain error のような想定できる失敗を通常の分岐として扱うときに使われます。

## attribute

型、property、method などへ metadata を付ける仕組みです。framework や reflection で読み取られて動作に影響することがあります。

## Span&lt;T&gt;

配列や文字列などの連続したメモリ領域をコピーせずに扱うための stack-only 型です。性能が必要な parsing や buffer 処理で使います。

## Memory&lt;T&gt;

連続したメモリ領域を表す型です。`Span<T>` と違い、field に保持したり async method をまたいで扱いやすい特徴があります。

## ReadOnlySequence&lt;T&gt;

複数 segment に分かれた連続データを表す型です。高性能 I/O や parser で使われます。

## System.IO.Pipelines

`PipeReader` と `PipeWriter` によって producer / consumer 形式の buffer 処理を支援する API です。

## SynchronizationContext

非同期処理の継続をどの context で実行するかに関係する仕組みです。UI アプリや古い ASP.NET の deadlock 理解で重要になります。

## Rx

Reactive Extensions の略です。`IObservable<T>` と LINQ 風の演算子で、時間を伴う event stream を扱うライブラリです。

## TPL Dataflow

block をつなげて非同期 pipeline を構築するライブラリです。複数段の処理、backpressure、並列度制御に使います。

## covariance / contravariance

generic interface や delegate の型変換規則です。C# では `out` が共変性、`in` が反変性を表します。

## generic constraints

generic type parameter に `where T : ...` で条件を付ける仕組みです。使える型や member の前提を明確にします。

## DI

Dependency Injection の略です。依存するオブジェクトを外から渡し、テストしやすく変更に強い設計にする考え方です。

## DI lifetime

DI コンテナーがサービスのインスタンスをどの期間使い回すかを表す考え方です。ASP.NET Core では主に Singleton、Scoped、Transient を使います。

## Application Service

Use case の流れを管理する層です。Controller と Domain の間に置き、入力検証、Domain の呼び出し、永続化、transaction 境界などを調整します。

## DTO

Data Transfer Object の略です。API request / response や外部サービス連携など、境界を越えてデータを運ぶための型です。

## Domain Model

業務ルールや状態変更の条件を表す model です。単なるデータ入れ物ではなく、業務上不正な状態を防ぐ責務を持たせます。

## Options パターン

設定値を `IOptions<T>` などの型付きオブジェクトとして扱う .NET の設定管理パターンです。設定の bind、validation、DI との連携に使います。

## 構造化ログ

ログメッセージに `OrderId` などの名前付き値を含め、検索や集計をしやすくするログの書き方です。

## ProblemDetails

HTTP API のエラー詳細を標準的な JSON 形式で表すための仕様です。ASP.NET Core ではエラーレスポンスの統一に使います。

## status code

HTTP response の結果を表す3桁の番号です。API では `200`、`201`、`400`、`401`、`403`、`404`、`409`、`500` などを使い分けます。

## pagination

一覧 API でデータを page 単位に分けて返す設計です。`page`、`pageSize`、`totalCount` などの query parameter と metadata を扱います。

## WebApplicationFactory

ASP.NET Core の integration test で、API を test server として起動し `HttpClient` から検証するための型です。

## 境界値テスト

上限、下限、0、1、空文字、null など、不具合が出やすい境界の入力を確認するテストです。

## flaky test

同じ code でも環境やタイミングによって成功したり失敗したりする不安定なテストです。時刻、乱数、外部 API、共有状態、sleep などが原因になります。

## 品質ゲート

変更を main branch に入れる前に通す確認です。`dotnet test`、format、静的解析、coverage、review checklist などを組み合わせます。

## Hosted Service

ASP.NET Core や .NET Worker Service で、アプリ起動中にバックグラウンド処理を実行する仕組みです。`IHostedService` や `BackgroundService` を使います。

## xUnit

.NET でよく使われるテストフレームワークです。`[Fact]` や `[Theory]` を使ってテストを記述し、`dotnet test` で検証します。

## モック

テストで外部依存の呼び出しや結果を置き換える代用品です。呼び出し回数や引数の検証に使うことがあります。

## スタブ

テストで決まった値を返す単純な代用品です。呼び出し検証より、テスト対象へ入力条件を与える目的で使います。

## Testcontainers

テスト実行時に Docker コンテナで DB や外部サービスを起動し、実物に近い統合テストを行うためのライブラリです。

## カバレッジ

テストがコードのどの範囲を実行したかを示す指標です。数字だけでなく、重要な仕様や分岐が検証されているかを確認します。

## ASP.NET Core

.NET で Web アプリや Web API を作るためのフレームワークです。

## EF Core

Entity Framework Core の略です。.NET からデータベースを扱うための ORM です。

## Change Tracker

EF Core が Entity の変更状態を追跡する仕組みです。更新処理では便利ですが、読み取り専用 query では `AsNoTracking()` を検討します。

## Dapper

SQL を明示的に書き、結果を object に軽く mapping する .NET の micro ORM です。複雑 SQL や read model で候補になります。

## Raw SQL

LINQ ではなく SQL 文字列を明示して DB に送る方法です。複雑 query や性能調整で使いますが、parameter 化と保守性に注意します。

## SQL injection

user input を SQL 文字列へ直接埋め込むことで、意図しない SQL が実行される脆弱性です。parameter 化や allow list で防ぎます。

## read / write 分離

更新用 model と読み取り用 model、または context を分ける設計です。一覧や集計を軽くしやすい一方で、整合性と同期遅延の扱いが必要です。

## 実行計画

DB が SQL をどのように実行するかを示す計画です。index、join、sort、scan の負荷を確認し、性能問題の原因を調べます。

## 楽観的同時実行制御

更新時に他の処理が同じデータを変更していないかを検出する方式です。EF Core では concurrency token などで扱います。

## Middleware

ASP.NET Core の request pipeline に差し込む処理です。認証、ログ、例外処理など横断的な処理に使います。

## Endpoint Filter

Minimal API の endpoint 前後に処理を差し込む仕組みです。入力検証や共通処理に使います。

## OpenAPI

HTTP API の endpoint、request、response、認証方式などを機械可読に記述する仕様です。クライアント生成や API 契約の確認に使います。

## API バージョニング

公開 API の互換性を保ちながら変更するため、URL、header、query などで API version を扱う設計です。

## CORS

Cross-Origin Resource Sharing の略です。browser が異なる origin の API を呼ぶときに、server 側が許可範囲を示す仕組みです。

## Rate limiting

短時間に多すぎる request を制限し、API や backend resource を守る仕組みです。

## HTTP caching

`Cache-Control`、`ETag`、`Last-Modified` などで response の再利用可否を伝える仕組みです。

## .NET local tool

リポジトリ単位で version を固定して使う .NET CLI tool です。tool manifest で管理します。

## migration bundle

EF Core migration を適用するための単一実行ファイルです。SQL script と並ぶ production 適用手段の候補です。
