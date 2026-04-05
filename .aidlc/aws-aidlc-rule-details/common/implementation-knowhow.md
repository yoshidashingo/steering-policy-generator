# 実装ノウハウ — リポジトリ改善

## ディレクトリ構造の統一パターン

### 標準構造

すべてのルールセットは以下の構造に従う:

```text
.<name>/
├── aws-aidlc-rules/
│   └── core-workflow.md
└── aws-aidlc-rule-details/
    ├── common/
    ├── <phase-1>/
    ├── <phase-2>/
    └── <phase-N>/
```

### 命名規則

- ルールセットのルートディレクトリ: ドット付きの小文字（例: `.aidlc/`, `.steering/`）
- ルールフォルダ: `aws-aidlc-rules/`（固定名）
- 詳細フォルダ: `aws-aidlc-rule-details/`（固定名）
- フェーズフォルダ: ハイフン区切り小文字（例: `inception/`, `construction/`）

## パス参照のルール

### core-workflow.md 内の参照

- 自ディレクトリの `aws-aidlc-rule-details/` を参照する
- 相対パスではなく、プロジェクトルートからの相対パスを使用
- 例: `.aidlc/aws-aidlc-rule-details/common/welcome-message.md`

### 他ツール由来の古いパスに注意

以下のパスは過去のツール固有のもので、使用しない:

- `.kiro/aws-aidlc-rule-details/` → `.aidlc/aws-aidlc-rule-details/` に統一済み
- `.amazonq/aws-aidlc-rule-details/` → `.aidlc/aws-aidlc-rule-details/` に統一済み
- `.steering/steering-rule-details/` → `.steering/aws-aidlc-rule-details/` に統一済み

## ルーティングルールの設計

### CLAUDE.md でのルーティング

- 「何をするか」でルールセットを切り替える
- リポジトリ改善 → `.aidlc/` ルール
- ポリシー生成 → `.steering/` ルール
- `.claude/rules/` に Glob パターン付きルールファイルを配置してコンテキスト別にガイド

### 新しいルールセットを追加する場合

1. `.<name>/aws-aidlc-rules/core-workflow.md` を作成
2. `.<name>/aws-aidlc-rule-details/common/` に共通ルールを配置
3. フェーズ別サブディレクトリを作成
4. CLAUDE.md のルーティングテーブルに追加
5. `.claude/rules/routing-<name>.md` にGlobルールを追加

## バージョン管理

- `.aidlc/VERSION` に現在のAI-DLCバージョンを記録する（例: `0.1.7`）
- upstream: `awslabs/aidlc-workflows` の `aidlc-rules/VERSION` と定期的に比較する
- 更新時は全ファイルをupstreamから取得し、パス参照のみローカルに適応させる

### upstreamからの更新手順

1. `gh api repos/awslabs/aidlc-workflows/contents/aidlc-rules/VERSION` でバージョン確認
2. 差分がある場合、各ファイルを `gh api` で取得して上書き
3. パス参照を `.aidlc/aws-aidlc-rule-details/` に統一（upstream はマルチパス解決）
4. `extensions/` ディレクトリの新規追加を確認
5. `.aidlc/VERSION` を更新

## Extensions 機構（v0.1.7 で追加）

- `aws-aidlc-rule-details/extensions/` 配下にオプショナルなルール拡張を配置
- 各拡張は `<name>.opt-in.md`（軽量opt-inプロンプト）と `<name>.md`（フルルール）のペア
- ワークフロー開始時は opt-in ファイルのみ読み込み、ユーザーが有効化したらフルルールを読み込む
- コンテキストウィンドウの節約に有効

### 現在の拡張

- `extensions/security/baseline/` — セキュリティベースライン
- `extensions/testing/property-based/` — プロパティベーステスト

## 品質に関する教訓

### Inception フェーズの品質を維持する

- AI-DLC の Inception フェーズ（目的分析、要件分析）は品質が高い傾向がある
- Construction 以降のフェーズではリサーチ能力を意識的に活用する
- 既存の高品質なスキルやプラグインの実装パターンを参考にする

### フェーズ間の品質差を防ぐ

- 各フェーズのルール詳細ファイルに同等の深さと具体性を持たせる
- 曖昧な指示を避け、具体的な手順とチェックリストを含める
- `common/quality-standards.md` で全フェーズに品質基準を適用する
