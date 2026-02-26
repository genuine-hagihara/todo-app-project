# Quickstart: 個人用 ToDo アプリ

**Feature**: `001-todo-app`

このToDoアプリの開発・実行手順です。

## 必須環境 (Prerequisites)

- **Node.js**: v18 以降推奨 (App Router対応のため)
- **パッケージマネージャー**: `pnpm` (コマンド例は全てpnpmで記載)

## セットアップ手順

1. **パッケージのインストール**
   ```bash
   pnpm install
   ```

2. **開発サーバーの起動**
   ```bash
   pnpm dev
   ```

   起動後、ブラウザで `http://localhost:3000` にアクセスしてください。

## テストの実行 (要件に従い追加)

- ユニットテスト (`use-todos`, `storage` 等のロジックテスト):
  ```bash
  pnpm test:unit
  ```

- E2Eテスト (ブラウザ上での操作検証):
  ```bash
  pnpm test:e2e
  ```

## 運用時の注意事項

- このアプリはサーバーサイドのデータベースを持ちません。全てのデータはブラウザ（クライアント）の `localStorage` に保存されます。
- そのため、シークレットエスケープやユーザー認証用の `.env.local` 等の環境変数は必要ありません。
