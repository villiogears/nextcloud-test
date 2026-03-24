# nextcloud-test

Nextcloud を nginx (Web サーバー) + MariaDB + Redis の構成で Docker Compose を使って構築するセットアップです。

## 構成

| サービス | イメージ | 説明 |
|---------|---------|------|
| `app`   | `nextcloud:fpm-alpine` | Nextcloud アプリケーション (PHP-FPM) |
| `web`   | `nginx:alpine` | Web サーバー (リバースプロキシ) |
| `db`    | `mariadb:11` | データベース |
| `redis` | `redis:alpine` | セッション / キャッシュ |

## 起動手順

### 1. 環境変数ファイルの作成

```bash
cp .env.example .env
```

`.env` を開き、パスワードなどを必要に応じて変更してください。  
`MYSQL_ROOT_HOST` はデフォルトで `172.%`（Docker の内部ネットワーク範囲を想定）に設定してあり、アプリケーションコンテナからの接続拒否 (`Host 'xxx' is not allowed to connect`) を防ぎつつ、不要に広い許可を避けています。環境に合わせて必要であれば調整してください。

### 2. コンテナの起動

```bash
docker compose up -d
```

### 3. ブラウザでアクセス

```
http://localhost
```

初回起動時は自動的にセットアップが完了します（`.env` で指定した管理者アカウントが作成されます）。

## 停止

```bash
docker compose down
```

データ (DB・Nextcloud ファイル) を含めて完全削除する場合:

```bash
docker compose down -v
```

## ポート変更

デフォルトは `80` ポートです。変更したい場合は `.env` の `HTTP_PORT` を編集してください。

```
HTTP_PORT=8080
```
