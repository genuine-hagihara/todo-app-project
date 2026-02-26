# Data Model: 個人用 ToDo アプリ

**Feature**: `001-todo-app`

## 概要

このアプリはクライアントサイド（ブラウザの localStorage）でのみデータを管理する。
サーバーサイドによるRDBMSなどのデータ永続化は持たず、状態（State）と localStorage を常に同期させることで要件を満たす。

## 型定義 (TypeScript)

### Task (エンティティ)

```typescript
export interface Task {
  /** タスクの一意な識別子 (UUID v4 等を利用) */
  id: string;
  /** タスクの内容 (空白のみを含まない文字列) */
  text: string;
  /** タスクの完了状態 (true: 完了, false: 未完了) */
  completed: boolean;
  /** タスクの作成日時 (ISO 8601 文字列形式を推奨) */
  createdAt: string;
}
```

### FilterState (列挙・ユニオン型)

```typescript
export type FilterState = 'all' | 'active' | 'completed';
```

---

## 永続化設計 (LocalStorage)

### ストレージキー構造

- `TODO_APP_TASKS`: (型: `Task[]` のJSON文字列) 全てのタスクリストを保持
- `TODO_APP_FILTER`: (型: `FilterState` の文字列) 最後にユーザーが選択していたフィルタ状態（リロード時の復元用）

### 安全な復元と破損回避プロセス (`lib/storage.ts`)

localStorageからデータをロードする際は、以下のチェックをパスして復元する。

1. **JSON パースの安全性**: `try..catch` ブロックで囲む。
   - 例: `"{"` (破損データ) が入っている場合は捕捉し、エラーログを出力しつつ `[]` (空配列) を返す。
2. **型バリデーション (Zod等の利用、または手動検証)**:
   - 配列であるか？
   - 各要素が `id`, `text`, `completed` を適切な型で持っているか？
   - 万が一旧形式（形式不一致）があれば、パースを破棄するか部分的にマイグレーションしてフォールバックする。

### 容量上限 (Edge Case) ハンドリング

- `localStorage` の一般的な上限は約 5MB。
- **データ更新フロー**: 追加や編集時に `localStorage.setItem()` を実行するが、ここで `QuotaExceededError` が発生した場合は例外を捕捉し、「トースト（エラーメッセージ）」などでユーザーへ容量不足を通知する。アプリ自体の描画は維持する。
