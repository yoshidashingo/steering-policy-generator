# Scope Definition

## Target Agent Summary
- **Name**: Steering Policy Maker (SPM)
- **Type**: Process Agent
- **Domain**: AIエージェント向けステアリングポリシー作成（メタエージェント）
- **Complexity**: Complex
- **Task**: 既存ポリシーのブラッシュアップ + スキル化 + マーケットプレイス対応

---

## Scope Boundaries

### In Scope

| Area | Coverage | Notes |
|------|----------|-------|
| **ワークフロー** | 既存4フェーズ15ステージの全面品質向上 | フェーズ構造自体は維持。各ステージのコンテンツ深度を向上 |
| **品質** | AI-DLC 11次元 + 新規4次元（計15次元） | 新規: ドメイン特化率、具体例カバレッジ、成果物テンプレート完備、ピットフォール参照率 |
| **Construction改善** | GENERATION/REFINEMENTフェーズの品質底上げ | 弱点検出ループ、成果物テンプレート追加、適応的深度追加、修復判断ツリー |
| **スキル化** | プラグイン形式への変換 | plugin.json, marketplace.json, SKILL.md, agents/, commands/ |
| **マーケットプレイス** | GitHub経由の配布・自動アップデート | autoUpdate設定、セマンティックバージョニング |
| **自動テスト** | 構造テスト + コンテンツテスト + 動作テスト | コマンドまたはエージェントとして実装 |
| **ユーザー** | Claude Codeユーザー（単一タイプ） | プラットフォーム: Claude Code のみ |
| **エージェントタイプ対応** | 4タイプ（Process/Task/Analytical/Hybrid）への生成対応拡充 | 各タイプ向けの具体例・テンプレート充実 |
| **ドメイン** | AIエージェントのワークフロー設計・品質管理 | ステアリングポリシー固有のドメイン知識 |
| **エラー** | 全フェーズのドメイン固有エラーシナリオ | GENERATION/REFINEMENTのエラーハンドリング強化が最優先 |

### Out of Scope

- マルチLLMプラットフォーム対応: ユーザー回答でClaude Codeのみと確認（v2.0で検討）
- エージェント間オーケストレーション: 現フェーズでは不要（将来検討）
- GUI/Webインターフェース: CLI（Claude Code）が前提
- 既存ポリシーのマイグレーションツール: 手動移行で対応可能
- 課金・ライセンス管理: OSSとして配布（MIT License）

---

## Estimated Structure（コンパクトサマリー）

> **Note**: DESIGN Phase (Workflow Architecture) にてPhase 5: PACKAGINGを追加決定。4→5フェーズ、15→18ステージに変更。

- **Phases**: 5（DISCOVERY, DESIGN, GENERATION, REFINEMENT, PACKAGING）
- **Total Stages**: 18（ALWAYS: 16, CONDITIONAL: 2）
- **Common Rule Files**: 11（標準: 10, ドメイン固有: 1 — implementation-knowhow.md）
- **Phase Rule Files**: 18（discovery: 4, design: 4, generation: 4, refinement: 3, packaging: 3）
- **Total Policy Files**: 30（core-workflow 1 + common 11 + phase rules 18）
- **Total New Plugin Files**: 15（agents 4 + skills 4 + commands 3 + plugin config 2 + rules 1 + validation-agent 1）
- **Grand Total Files**: 45
- **Estimated Current Lines**: ~7,500
- **Estimated Target Lines**: ~11,200（ポリシー ~9,800 + プラグイン ~1,400）

## Preliminary Phase Structure

### DISCOVERY Phase
- Purpose Analysis (ALWAYS)
- Domain Research (ALWAYS — Adaptive depth)
- Stakeholder Identification (CONDITIONAL — Execute IF: 複数ユーザータイプ)
- Scope Definition (ALWAYS)

### DESIGN Phase
- Workflow Architecture (ALWAYS)
- Common Rules Design (ALWAYS)
- Phase Rules Design (ALWAYS)
- Quality Mechanisms Design (ALWAYS)

### GENERATION Phase
- Core Workflow Generation (ALWAYS)
- Common Rules Generation (ALWAYS)
- Phase Rules Generation (ALWAYS)
- Integration Validation (ALWAYS)

### REFINEMENT Phase
- Completeness Review (ALWAYS)
- Consistency Review (ALWAYS)
- Quality Calibration (ALWAYS)

### PACKAGING Phase
- Plugin Structure Generation (ALWAYS)
- Automated Validation (ALWAYS)
- Migration Execution (CONDITIONAL — Execute IF: 既存ポリシーのブラッシュアップ)

---

## 既存構造（実測値ベース）

```
.steering/
├── aws-aidlc-rules/
│   └── core-workflow.md                    # 505行 → 品質向上
└── aws-aidlc-rule-details/
    ├── common/          (11ファイル)       # 2,607行 → 内容充実化
    ├── discovery/       (4ファイル)        # 1,081行 → 微調整
    ├── design/          (4ファイル)        # 1,161行 → 微調整
    ├── generation/      (4ファイル)        # 1,201行 → 大幅改善（最優先）
    └── refinement/      (3ファイル)        # 945行 → 大幅改善
```

## 新規プラグイン構造

```
steering-policy-maker/                      # 新規プラグインディレクトリ
├── .claude-plugin/
│   ├── plugin.json                         # プラグインメタデータ
│   └── marketplace.json                    # マーケットプレイス公開メタデータ
├── agents/
│   ├── spm-orchestrator.md                 # メインオーケストレーター
│   ├── domain-researcher.md                # ドメイン調査専門
│   ├── policy-generator.md                 # ポリシー生成専門
│   └── quality-reviewer.md                 # 品質検証専門
├── skills/
│   ├── steering-policy-creation/
│   │   └── SKILL.md                        # メインスキル（~120行）
│   ├── domain-research/
│   │   └── SKILL.md                        # ドメイン調査スキル（~90行）
│   ├── quality-calibration/
│   │   └── SKILL.md                        # 品質評価スキル（~100行）
│   └── policy-templates/
│       └── SKILL.md                        # テンプレートスキル（~80行）
├── commands/
│   ├── new-policy.md                       # /spm:new-policy
│   ├── resume-policy.md                    # /spm:resume-policy
│   └── validate-policy.md                  # /spm:validate-policy
├── rules/
│   └── spm-standards.md                    # 品質基準ルール（ファイルツリーに配置、plugin.jsonには非参照）
└── steering-rule-details/                  # 既存ファイルを改善して配置
    ├── common/          (11ファイル)
    ├── discovery/       (4ファイル)
    ├── design/          (4ファイル)
    ├── generation/      (4ファイル)
    └── refinement/      (3ファ���ル)
```

**マイグレーション方針**: Phase 1-2で`.steering/`内のファイルを改善 → Phase 3で改善済みファイルを`steering-policy-maker/steering-rule-details/`にコピーしてプラグイン構造を構築 → 完了後`.steering/`はレガシーとして非推奨化（削除はしない）

---

## ファイル数サマリー

| カテゴリ | 既存 | 改善 | 新規 | 合計 |
|---------|------|------|------|------|
| core-workflow.md | 1 | 1 | 0 | 1 |
| common rules | 11 | 11 | 0 | 11 |
| discovery rules | 4 | 4 | 0 | 4 |
| design rules | 4 | 4 | 0 | 4 |
| generation rules | 4 | 4 | 0 | 4 |
| refinement rules | 3 | 3 | 0 | 3 |
| **小計: ポリシーフ��イル** | **27** | **27** | **0** | **27** |
| plugin config | 0 | 0 | 2 | 2 |
| agents | 0 | 0 | 4 | 4 |
| skills (SKILL.md) | 0 | 0 | 4 | 4 |
| commands | 0 | 0 | 3 | 3 |
| rules (ファイルツリーのみ) | 0 | 0 | 1 | 1 |
| validation agent | 0 | 0 | 1 | 1 |
| **小計: プラグインファイル** | **0** | **0** | **15** | **15** |
| **総合計** | **27** | **27** | **15** | **42** |

---

## 推定行数（実測値ベース）

| カテゴリ | 現行行数（実測） | 目標行数 | 変化 |
|---------|---------------|---------|------|
| core-workflow.md | 505 | ~600 | +~100行（品質次元追加、弱点検出ループ） |
| common rules (11) | 2,607 | ~3,000 | +~400行（terminology: Common Misuse追加、quality-standards: 4次元追加、output-structure-patterns: 深度ガイダンス追加等） |
| discovery rules (4) | 1,081 | ~1,200 | +~120行（微調整） |
| design rules (4) | 1,161 | ~1,350 | +~190行（微調整） |
| generation rules (4) | 1,201 | ~1,850 | +~650行（成果物テンプレート、ヒューリスティクス、適応的深度） |
| refinement rules (3) | 945 | ~1,400 | +~450行（コンテンツ品質検証、修復判断ツリー） |
| **ポリシー小計** | **7,500** | **~9,400** | **+~1,910行** |
| プラグインファイル (15) | 0 | ~1,400 | 新規 |
| **総合計** | **7,500** | **~10,800** | **+~3,310行** |

---

## 改善優先度マトリクス

### Phase 1: Construction品質改善（最優先）

**実行順序**: 品質フレームワーク定義 → GENERATIONコンテンツ改善 → REFINEMENT検証改善

| 順序 | 対象ファイル | 改善内容 | 推定追加行数 |
|------|------------|---------|------------|
| 1-1 | core-workflow.md | 弱点検出ループ、新品質次元の参照追加 | +100行 |
| 1-2 | refinement/quality-calibration.md | 新規4品質次元定義、修復判断ツリー追加 | +200行 |
| 1-3 | generation/common-rules-generation.md | 主要5ファイルの成果物テンプレート（Light/Medium/Heavy例）追加 | +200行 |
| 1-4 | generation/phase-rules-generation.md | ドメイン固有コンテンツ生成ヒューリスティクス（Step 3c改善）追加 | +100行 |
| 1-5 | generation/core-workflow-generation.md | 適応的深度（Minimal/Standard/Comprehensive）追加 | +80行 |
| 1-6 | generation/integration-validation.md | コンテンツ品質バリデーション追加 | +70行 |
| 1-7 | refinement/completeness-review.md | コンテンツ深度検証手順追加 | +100行 |
| 1-8 | refinement/consistency-review.md | ドメイン特化率測定手順追加 | +80行 |

**Phase 1 合計**: +930行（8ファイル）
**実行順序の根拠**: 品質フレームワーク（core-workflow + quality-calibration）を先に定義し、GENERATIONファイルがそのフレームワークを参照できるようにする。トップダウンアプローチ。

### Phase 2: 共通ルール・Inception微調整

| # | 対象ファイル | 改善内容 | 推定追加行数 |
|---|------------|---------|------------|
| 2-1 | common/terminology.md | Common Misuse列、Related Terms列の追加 | +80行 |
| 2-2 | common/quality-standards.md | 新規4品質次元の定義追加 | +100行 |
| 2-3 | common/output-structure-patterns.md | コンテンツ深度ガイダンス追加 | +60行 |
| 2-4 | common/overconfidence-prevention.md | GENERATION/REFINEMENT向けガイダンス強化 | +40行 |
| 2-5 | common/implementation-knowhow.md | スキル化・プラグイン構造のノウハウ追加 | +50行 |
| 2-6 | discovery/* + design/* | 微調整（スキル化参照の追加等） | +120行 |

**Phase 2 合計**: +450行

### Phase 3: スキル化・プラグイン化

| # | 対象 | 内容 | 推定行数 |
|---|------|------|---------|
| 3-1 | .claude-plugin/ | plugin.json + marketplace.json | ~60行 |
| 3-2 | agents/ (4ファイル) | 専門エージェント定義（各エージェントを明示パスで列挙） | ~400行 |
| 3-3 | skills/ (4 SKILL.md) | スキルエントリポイント | ~390行 |
| 3-4 | commands/ (3ファイル) | コマンド定義 | ~200行 |
| 3-5 | rules/spm-standards.md | 品質基準ルール | ~80行 |
| 3-6 | validation agent | 構造テスト+コンテンツテスト（コマンドとして実装） | ~270行 |
| 3-7 | マイグレーション | .steering/ → steering-policy-maker/steering-rule-details/ へコピー | 行数変化なし |

**Phase 3 合計**: ~1,400行（新規）+ マイグレーション

---

## Quality Target

- **Level**: Premium
- **11 Dimensions Target**: All 11 PASS（現行AI-DLC基準）
- **Additional 4 Dimensions**: All 4 PASS（新規基準）
  - **Dim 12: ドメイン特化率**: 各Phase Ruleファイルのドメイン固有行÷総行 ≥ 40%
  - **Dim 13: 具体例カバレッジ**: 各Phase Ruleファイルにドメイン固有例 ≥ 2個
  - **Dim 14: 成果物テンプレート完備**: 期待出力テンプレートを持つファイルの割合 = 100%
  - **Dim 15: ピットフォール参照率**: エラーハンドリングがdomain-research由来のピットフォールを参照する割合 ≥ 50%
- **Key Quality Focus Areas**:
  1. GENERATIONファイルの成果物テンプレート完備率: 25% → 100%
  2. ドメイン特化率: 全Phase Ruleファイルで >40%
  3. 具体例カバレッジ: 全Phase Ruleファイルで ≥2例
  4. スキル品質: ECCスキル分析で特定した10設計パターン準拠

---

## Effort Estimate

| フェーズ | 作業内容 | 規模感 |
|---------|---------|-------|
| **DESIGN** | ワークフロー改善設計、品質次元設計、スキル構造設計 | 中（既存設計の拡張） |
| **GENERATION** | 既存27ファイル改善 + 新規15ファイル生成 | 大（最大の作業量） |
| **REFINEMENT** | 15品質次元でのキャリブレーション | 中（新基準での再検証） |
