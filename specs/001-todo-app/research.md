# Research & Decisions: 個人用 ToDo アプリ

**Feature**: `001-todo-app`

仕様および要件に基づく技術調査・決定事項の記録。

## UIフレームワークとスタイリング

- **Decision**: Next.js App Router + Tailwind CSS + shadcn/ui
- **Rationale**: ユーザーからの必須要件。白基調・余白広め・角丸のモバイルファーストUIを素早く実現するため、コンポーネント指向であるshadcn/uiが最も適している。
- **Alternatives considered**: 個別のCSS-in-JS (Styled Components) や別のコンポーネントライブラリ (Material UI) は今回の指定から除外。

## ストレージ永続化機構

- **Decision**: HTML5 `localStorage` ＋ カスタムの安全なパース機構 (`try-catch` フォールバック対応)
- **Rationale**: 最大1000件程度のテキストデータであれば、パフォーマンス・容量ともに `localStorage` で十分まかなえ、実装工数も低い。データ破損時のクラッシュ回避要件は、パース失敗時に空のリストを返すラッパー関数を設けることで達成する。
- **Alternatives considered**: `IndexedDB` は非同期かつ大容量だが、個人用の1000件の簡易タスクリストにはオーバースペックであり実装が複雑になる。

## テスト手法 (ユニット／E2E)

- **Decision**: ユニットテストに `Vitest`（または `Jest`）、E2Eに `Playwright` を採用する。（※実装Phaseで確定する）
- **Rationale**: Next.js環境でのユニットテスト（フック、ロジック分離）には、軽量で高速なVitestが近年標準的。E2Eに関してはローカルでのブラウザ操作検証が容易なPlaywrightがshadcn/uiのコンポーネント挙動確認に最適。
- **Alternatives considered**: Cypress等も候補だが、App Routerとの相性およびCI実行速度の観点からPlaywrightを優先。

## 削除の誤操作軽減UI

- **Decision**: 削除実行直後にスナックバートーストを表示し、一定秒数内であれば「元に戻す（Undo）」ができる方式。
- **Rationale**: 「毎回確認ダイアログを出す」のは1〜2アクションを求めるUXにおいて操作のテンポを著しく阻害するため。shadcn/uiの `Toast` コンポーネントを活用してシームレスな体験を提供する。
- **Alternatives considered**: スワイプ削除（モバイル向け）はタッチイベントの実装コストが高くPCでの操作性との両立が難しいため、今回はトースト＋Undo方式とする。
