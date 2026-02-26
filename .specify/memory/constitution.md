<!--
Sync Impact Report:
- Version change: 1.0.0 → 2.0.0
- Modified principles: None
- Added sections: None
- Removed sections: V. No Source Comments (ソースコードコメント禁止)
- Templates requiring updates: None
- Follow-up TODOs: None
-->
# Speckit Project Constitution

## Core Principles (基本原則)

### I. Clean Code (クリーンコード)
保守性の高いコード記述を心がけること。関数は単一責任原則に従い、変数名は明確で意図が伝わるものにする。技術的負債を最小限に抑えるよう努める。

### II. Simple UX (シンプルなUX)
ユーザー体験を第一に考え、直感的で使いやすいインターフェースを設計すること。不要な複雑さを排除し、ユーザーが迷わずに操作できるようにする。

### III. Responsive Design (レスポンシブデザイン)
モバイルファーストのアプローチを採用し、全てのデバイス（スマートフォン、タブレット、PC）で快適に動作するように設計・実装すること。

### IV. No Testing (テストコード不要)
単体テスト（Unit）、結合テスト（Integration）、E2Eテストの実装は**行わない**こと。テストコードの記述に時間を割くのではなく、機能実装とUX改善に注力する。

## Technology Stack (技術スタック)

- **Framework**: Next.js
- **UI Library**: React / shadcn/ui
- **Styling**: Tailwind CSS

## Conventions (規約)

### File Naming (ファイル命名規則)
全てのファイル名は `kebab-case`（例：`my-component.tsx`）とする。

## Governance (ガバナンス)

**Version**: 2.0.0 | **Ratified**: 2026-02-20 | **Last Amended**: 2026-02-24
