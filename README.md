# C# / .NET ナレッジベース

このリポジトリは MkDocs / Material for MkDocs で静的サイト化する C# / .NET 学習ナレッジベースです。

公開用の記事は [docs/index.md](docs/index.md) から始まります。レビュー、ファクトチェック、アーカイブなどの管理記録は `作業/` に残し、サイトのナビゲーションには含めません。

## ローカル表示

ローカル環境を汚さないため、Python や MkDocs は Docker container 内で実行します。
事前に Docker Desktop を起動しておきます。

```powershell
docker compose up --build
```

ブラウザで `http://127.0.0.1:8000/` を開きます。

## ビルド

```powershell
docker compose run --rm docs mkdocs build --clean
```

生成物は `site/` に出力されます。

コンテナを停止する場合は次を実行します。

```powershell
docker compose down
```

## GitHub Pages

GitHub Actions では [.github/workflows/deploy-pages.yml](.github/workflows/deploy-pages.yml) が Docker image を build し、`mkdocs build` の生成物を GitHub Pages へアップロードします。

GitHub 側では、リポジトリの `Settings` → `Pages` → `Build and deployment` を `GitHub Actions` に設定します。
