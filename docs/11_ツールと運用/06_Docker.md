# Docker

## 目的

.NET アプリをコンテナで実行する基本を理解します。

## 要点

- Dockerfile でビルド手順と実行環境を定義します。
- multi-stage build でイメージを小さくします。
- 設定値や秘密情報はイメージに焼き込まないようにします。

## Dockerfile 例

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime
WORKDIR /app
COPY ./publish .
ENTRYPOINT ["dotnet", "App.dll"]
```

## 実務での使い方

ローカル環境統一、CI/CD、本番デプロイで使います。DB や外部サービスは docker compose で合わせて起動することもあります。

## よくあるミス

- 開発用秘密情報をイメージに含める。
- ポート、環境変数、ボリュームの設定を忘れる。
- コンテナ内のタイムゾーンやファイルパス差を考えない。

## 関連リンク

- [.NET Docker images](https://learn.microsoft.com/dotnet/core/docker/introduction)
