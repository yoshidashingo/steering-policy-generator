# 実装ノウハウ — ステアリングポリシー生成

## ディレクトリ構造の統一パターン

### 生成されるポリシーの標準構造

生成対象のエージェントには以下の構造を生成する:

```text
.<agent-name>/
├── aws-aidlc-rules/
│   └── core-workflow.md
└── aws-aidlc-rule-details/
    ├── common/
    ├── <phase-1>/
    ├── <phase-2>/
    └── <phase-N>/
```

### この構造を採用する理由

- AWS AI-DLC と同じパターンで統一し、ツール間の互換性を確保
- `aws-aidlc-rules/` にはワークフローの全体像（core-workflow.md）のみ配置
- `aws-aidlc-rule-details/` にはフェーズ別・共通の詳細ルールを配置
- エントリポイントが明確で、段階的なルール読み込みが可能

## ポリシー生成時の教訓

### Discovery フェーズが最重要

- Purpose Analysis で対象エージェントの本質を正確に捉えることが全体品質を決める
- ドメインリサーチは省略せず、最低でも Standard 深度で実施する
- 利用者の暗黙の期待を明示化する質問を必ず行う

### Design フェーズでの注意点

- ワークフローアーキテクチャは対象エージェントの型（Process/Task/Analytical/Hybrid）に合わせる
- 共通ルール（common/）は全フェーズで一貫して参照されるよう設計する
- フェーズ間の依存関係を明示的にマッピングする

### Generation フェーズでの注意点

- core-workflow.md 内のすべてのファイル参照が実ファイルに解決されることを検証する
- 共通ルールファイルは対象ドメインに適応させてから生成する（テンプレートのコピーではない）
- 2オプション完了メッセージのパターンを統一する

### Refinement フェーズでの注意点

- AI-DLC の 11 品質次元すべてを評価する
- 完全性レビューでは「すべてのワークフローパスに対応するルールファイルがあるか」を確認
- 一貫性レビューでは用語集（terminology.md）との整合性を確認

## バージョン管理との連携

- `.aidlc/` のルールは upstream (`awslabs/aidlc-workflows`) と同期する
- `.steering/` のルールはこのプロジェクト独自だが、AI-DLC の品質基準に準拠する
- Extensions 機構（v0.1.7）も `.steering/` 側で同等の仕組みを導入可能

## パス参照のルール

### core-workflow.md 内の参照

- 自ディレクトリの `aws-aidlc-rule-details/` を参照する
- 例: `.steering/aws-aidlc-rule-details/common/welcome-message.md`

### 過去の命名からの移行

以下の古いパスは使用しない:

- `.steering/steering-rule-details/` → `.steering/aws-aidlc-rule-details/` に統一済み

## ルーティングとCLAUDE.mdの連携

### 2つのコンテキストの分離

- リポジトリ改善 → `.aidlc/` ルールで駆動
- ポリシー生成 → `.steering/` ルールで駆動
- CLAUDE.md に判定テーブルを記載し、依頼内容に応じて自動選択

### 新しいルールセットの追加手順

1. `.<name>/aws-aidlc-rules/core-workflow.md` を作成
2. `.<name>/aws-aidlc-rule-details/common/` に共通ルールを配置
3. フェーズ別サブディレクトリを作成
4. CLAUDE.md のルーティングテーブルに追加
5. `.claude/rules/routing-<name>.md` にGlobルールを追加
