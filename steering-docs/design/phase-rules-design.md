# Phase Rules Design

## File Inventory

| Phase | Stage | File Name | Classification | 現行行数 | 目標行数 | 改善重点 |
|-------|-------|-----------|---------------|---------|---------|---------|
| DISCOVERY | D1: Purpose Analysis | purpose-analysis.md | ALWAYS | 269 | ~290 | 4タイプ別分析テンプレート追加 |
| DISCOVERY | D2: Domain Research | domain-research.md | ALWAYS | 255 | ~290 | MCP連携手順明示、Adaptive Depth最低項目数定義 |
| DISCOVERY | D3: Stakeholder ID | stakeholder-identification.md | CONDITIONAL | 224 | ~230 | 微調整のみ |
| DISCOVERY | D4: Scope Definition | scope-definition.md | ALWAYS | 333 | ~360 | 5フェーズ対応、マイグレーション計画テンプレート |
| DESIGN | E1: Workflow Architecture | workflow-architecture.md | ALWAYS | 317 | ~340 | 修復判断ツリー設計ガイド追加 |
| DESIGN | E2: Common Rules Design | common-rules-design.md | ALWAYS | 241 | ~260 | Plugin対応ルール設計ガイド追加 |
| DESIGN | E3: Phase Rules Design | phase-rules-design.md | ALWAYS | 265 | ~290 | PACKAGINGフェーズルール設計ガイド追加 |
| DESIGN | E4: Quality Mechanisms | quality-mechanisms-design.md | ALWAYS | 338 | ~380 | 15品質次元設計、修復判断ツリーテンプレート |
| GENERATION | G1: Core Workflow Gen | core-workflow-generation.md | ALWAYS | 254 | ~350 | 適応的深度(Minimal/Standard/Comprehensive)、PACKAGINGフェーズ生成 |
| GENERATION | G2: Common Rules Gen | common-rules-generation.md | ALWAYS | 321 | ~450 | 主要5ファイルの成果物テンプレート(Light/Medium/Heavy) |
| GENERATION | G3: Phase Rules Gen | phase-rules-generation.md | ALWAYS | 277 | ~420 | ドメインコンテンツ注入ヒューリスティクス、具体例注入プロセス |
| GENERATION | G4: Integration Validation | integration-validation.md | ALWAYS | 349 | ~450 | 3層テスト(構造+コンテンツ+スモーク)、コンテンツ品質メトリクス |
| REFINEMENT | R1: Completeness Review | completeness-review.md | ALWAYS | 267 | ~380 | コンテンツ深度検証、テンプレート完備率チェック |
| REFINEMENT | R2: Consistency Review | consistency-review.md | ALWAYS | 289 | ~380 | ドメイン特化率測定手順、4タイプ別一貫性チェック |
| REFINEMENT | R3: Quality Calibration | quality-calibration.md | ALWAYS | 389 | ~520 | 15品質次元スコアカード、修復判断ツリー実装、ループ制御 |
| PACKAGING | P1: Plugin Structure Gen | plugin-structure-generation.md | ALWAYS | 0 (新規) | ~250 | plugin.json/agents/skills/commands生成手順 |
| PACKAGING | P2: Automated Validation | automated-validation.md | ALWAYS | 0 (新規) | ~200 | 3層テスト(構造+コンテンツ+スモーク)プラグイン版 |
| PACKAGING | P3: Migration Execution | migration-execution.md | CONDITIONAL | 0 (新規) | ~150 | .steering/→steering-policy-maker/steering-rule-details/マイグレーション手順 |

**Total Phase Rule Files**: 18
**Current Total Lines**: 4,388（既存15ファイル）
**Target Total Lines**: ~6,040（+~1,652行。うち新規3ファイル~600行、既存15ファイル改善+~1,052行）

---

## Phase 1: DISCOVERY

### D1: Purpose Analysis
**File**: `discovery/purpose-analysis.md`
**Classification**: ALWAYS
**Purpose**: ユーザーの目的からターゲットエージェントの種類・能力・複雑度を分析する

#### 改善内容（+~21行）

| 項目 | 現行 | 改善後 |
|------|------|--------|
| Step 2: エージェント分類 | 汎用的な分類基準 | 4タイプ別分析テンプレート（各タイプの典型的な特徴チェックリスト追加） |
| Step 5: 複雑度評価 | Simple/Standard/Complex基準 | 各レベルの具体的な判定指標追加（フェーズ数、ステージ数、品質次元数の目安） |

#### Execution Steps Outline
1. Capture Initial Purpose Statement（現行維持）
2. Classify Target Agent Type — **改善: 4タイプ別チェックリスト**
3. Identify Core Capabilities（現行維持）
4. Assess Domain Profile（現行維持）
5. Determine Complexity Level — **改善: 定量的判定指標**
6. Analyze Existing Policies (if brush-up)（現行維持）
7-9. Question Generation / Presentation / Approval（現行維持）

#### Error Scenarios
- 現行のエラーシナリオ維持
- 追加なし（DISCOVERYは安定）

---

### D2: Domain Research
**File**: `discovery/domain-research.md`
**Classification**: ALWAYS（Adaptive Depth: Minimal/Standard/Comprehensive）
**Purpose**: ドメイン固有のベストプラクティス・ピットフォール・標準を調査する

#### 改善内容（+~35行）

| 項目 | 現行 | 改善後 |
|------|------|--------|
| Adaptive Depth定義 | 3段階の説明のみ | 各レベルの最低調査項目数: Minimal(3), Standard(7), Comprehensive(12) |
| MCP連携 | 暗黙的 | Step 2-3にMCPツール連携手順を明示（Context7→公式ドキュメント、Exa→Web調査、gh search→実装例） |
| ツール不在時 | 記載なし | Pattern 9適用: MCPツール不在時はユーザーへの直接質問にフォールバック |

#### Execution Steps Outline
1. Load Purpose Analysis Context（現行維持）
2. Determine Research Depth — **改善: 最低項目数の定義**
3. Execute Domain Research — **改善: MCPツール連携手順追加**
4-5. Identify Best Practices / Pitfalls（現行維持）
6. Document Domain Standards（現行維持）
7-9. Question Generation / Summary / Approval（現行維持）

---

### D3: Stakeholder Identification
**File**: `discovery/stakeholder-identification.md`
**Classification**: CONDITIONAL
**Purpose**: 複数ユーザータイプの特定と要件マッピング

#### 改善内容（+~6行）
- CONDITIONAL判定基準をWorkflow Architectureと整合（現行とほぼ同一、微調整のみ）

---

### D4: Scope Definition
**File**: `discovery/scope-definition.md`
**Classification**: ALWAYS
**Purpose**: ポリシーセットの境界・見積もり・品質目標を定義する

#### 改善内容（+~27行）

| 項目 | 現行 | 改善後 |
|------|------|--------|
| フェーズ構造 | 4フェーズ前提 | 5フェーズ（PACKAGING追加）対応。Scope DefinitionテンプレートにPACKAGINGセクション |
| 見積もり精度 | 推定ベース | `wc -l`実測値取得の手順を明示。改善後の行数差分算出テンプレート |
| マイグレーション | 記載なし | ブラッシュアップ時のマイグレーション計画テンプレート追加 |

---

## Phase 2: DESIGN

### E1: Workflow Architecture
**File**: `design/workflow-architecture.md`
**Classification**: ALWAYS
**Purpose**: フェーズ/ステージ構造・依存関係・チェックポイント設計

#### 改善内容（+~23行）

| 項目 | 現行 | 改善後 |
|------|------|--------|
| 修復判断ツリー | 記載なし | 修復判断ツリー設計ガイド: 品質次元→問題分類→戻り先マッピングの設計手順追加 |
| PACKAGINGパターン | 記載なし | Architecture Patterns by Agent Typeに「PACKAGING拡張」パターン追加 |
| ループ制限設計 | 記載なし | 修復ループの最大回数・エスカレーション条件の設計ガイダンス |

---

### E2: Common Rules Design
**File**: `design/common-rules-design.md`
**Classification**: ALWAYS
**Purpose**: 共通ルールの選定・ドメイン適応設計

#### 改善内容（+~19行）
- Step 9（Domain-Specific Rules）にプラグイン関連ルール設計ガイダンス追加
- implementation-knowhow.mdの設計指針（スキル10パターン反映方法）追加

---

### E3: Phase Rules Design
**File**: `design/phase-rules-design.md`
**Classification**: ALWAYS
**Purpose**: 各フェーズのルールファイル構造・内容アウトライン設計

#### 改善内容（+~25行）
- PACKAGINGフェーズのルール設計ガイダンス追加（P1/P2/P3の設計パターン）
- Step 3: Design Individual Stage Rule Files に「Domain Content Injection Point」設計項目追加

---

### E4: Quality Mechanisms Design
**File**: `design/quality-mechanisms-design.md`
**Classification**: ALWAYS
**Purpose**: チェックポイント・監査・バリデーション・品質次元設計

#### 改善内容（+~42行）

| 項目 | 現行 | 改善後 |
|------|------|--------|
| 品質次元 | 11次元テンプレート | 15次元テンプレート（Dim 12-15の設計ガイド追加） |
| 修復判断ツリー | 記載なし | 品質次元→問題分類マッピングテンプレート、ループ制御設計テンプレート |
| 承認基準 | 11次元ベース | 15次元対応の段階的承認基準テンプレート |
| 自動テスト設計 | 記載なし | 3層テスト（構造/コンテンツ/スモーク）の設計ガイダンス |

---

## Phase 3: GENERATION — 最重要改善領域

### G1: Core Workflow Generation
**File**: `generation/core-workflow-generation.md`
**Classification**: ALWAYS（Two-Part: Planning + Generation）
**Purpose**: ターゲットエージェントのcore-workflow.md生成

#### 改善内容（+~96行）

| 項目 | 現行 | 改善後 |
|------|------|--------|
| 適応的深度 | なし | Minimal/Standard/Comprehensiveの3段階。各段階でcore-workflowの詳細度を調整 |
| PACKAGING生成 | なし | Phase 5: PACKAGINGセクション生成ガイド追加 |
| 成果物テンプレート | なし | core-workflow.mdのLight/Medium/Heavy出力テンプレート追加 |

**成果物テンプレート設計（新規）**:

| Depth | core-workflow構成 | 推定行数 |
|-------|-----------------|---------|
| Light (Simple/Task Agent) | 2-3フェーズ、各フェーズ概要+ステージリスト | 200-300行 |
| Medium (Standard Agent) | 3-4フェーズ、各ステージに実行概要+承認ゲート | 300-450行 |
| Heavy (Complex/Process Agent) | 4-5フェーズ、各ステージに詳細手順+適応的深度+エラーハンドリング | 450-600行 |

---

### G2: Common Rules Generation
**File**: `generation/common-rules-generation.md`
**Classification**: ALWAYS
**Purpose**: 共通ルールファイル群の生成

#### 改善内容（+~129行）— 成果物テンプレート大幅追加

| 項目 | 現行 | 改善後 |
|------|------|--------|
| 生成手順 | ファイル単位の生成ステップ | 生成順序の依存関係を明示（terminology→quality-standards→他） |
| 成果物テンプレート | なし（現行カバー率25%） | 主要5ファイルのLight/Medium/Heavyテンプレート追加 |
| 品質チェック | 相互参照確認のみ | 行数ガイドライン適合チェック追加 |

**主要5ファイル テンプレート設計（新規）**:

| File | Light | Medium | Heavy |
|------|-------|--------|-------|
| terminology.md | 用語10-20語、2列テーブル | 用語20-40語、4列テーブル（Definition/Misuse/Related） | 用語40+語、4列+使用例セクション |
| quality-standards.md | 11次元リスト+承認基準 | 15次元+測定概要+承認基準+スコアカードテンプレート | 15次元+詳細測定方法+閾値根拠+スコアカード+改善ガイド |
| error-handling.md | フェーズ別エラー各2-3シナリオ | フェーズ別エラー各4-5シナリオ+重大度分類 | フェーズ別エラー各5+シナリオ+重大度+リカバリー手順+ピットフォール参照 |
| welcome-message.md | エージェント名+概要+開始手順 | +ワークフロー図+フェーズ説明 | +コアルール+Getting Started分岐+プラットフォーム説明 |
| process-overview.md | フェーズ/ステージ一覧 | +CONDITIONAL判定基準+チェックポイント一覧 | +修復ループ概要+3層テスト概要+ループ制限 |

---

### G3: Phase Rules Generation
**File**: `generation/phase-rules-generation.md`
**Classification**: ALWAYS（Adaptive Depth対応）
**Purpose**: フェーズ別ルールファイル群の生成

#### 改善内容（+~143行）— ドメインコンテンツ注入が最重要

| 項目 | 現行 | 改善後 |
|------|------|--------|
| 生成手順 Step 3 | 汎用的な「Generate Each Phase Rule File」 | ドメインコンテンツ注入ヒューリスティクス（5ステップ）を追加 |
| 具体例注入 | 記載なし | 各ファイルにGOOD/BAD例を最低2つ挿入するプロセス定義 |
| 品質チェック | 基本的な確認のみ | ドメイン特化率40%プレチェック追加 |
| PACKAGINGルール生成 | 記載なし | P1/P2/P3ルールファイルの生成ガイダンス追加 |

**ドメインコンテンツ注入ヒューリスティクス（新規、G3の中核改善）**:

```
Step 3c: Domain Content Injection
  3c-1. Domain Research Summaryを読み込み
  3c-2. 各Phase Ruleファイルの以下セクションにドメイン固有コンテンツをマッピング:
        - Execution Steps → ドメイン固有のアクション・チェック
        - Examples → GOOD/BAD例（Domain Researchのベストプラクティス/ピットフォールから導出）
        - Error Handling → Domain Research由来のピットフォールをエラーシナリオとして追加
  3c-3. 各ファイルのドメイン特化率を簡易計測（terminology.md用語の出現行数 ÷ 総行数）
  3c-4. 40%未満のファイルを特定し、追加注入対象としてマーク
  3c-5. マークされたファイルに対し、Domain Researchから追加の具体例・手順・チェック項目を注入
```

---

### G4: Integration Validation
**File**: `generation/integration-validation.md`
**Classification**: ALWAYS
**Purpose**: 生成されたポリシーセットの構造・コンテンツ・動作バリデーション

#### 改善内容（+~101行）— 3層テスト追加

| 項目 | 現行 | 改善後 |
|------|------|--------|
| バリデーション範囲 | 構造チェック（ファイル存在、参照整合性） | 3層テスト: 構造テスト + コンテンツテスト + スモークテスト |
| コンテンツ品質 | なし | Dim 12-15のプレバリデーション（ドメイン特化率、具体例数、テンプレート完備率、ピットフォール参照率） |
| レポート形式 | 基本的なPass/Fail | 3層テスト別の詳細レポート + 全体PASS/FAIL判定 |
| FAIL時のアクション | 「GENERATIONへ戻る」のみ | 修復判断ツリーの簡易版を適用（構造→G1、コンテンツ→G3） |

**3層テスト詳細設計（新規）**:

| テスト層 | チェック項目 | 合否基準 |
|---------|-----------|---------|
| **構造テスト** | ファイル存在確認、core-workflow.mdの全参照解決、Markdown構文、フローパス完走（全ステージ到達可能） | エラー0件 |
| **コンテンツテスト** | Dim 12: ドメイン特化率>=40% (全Phase Rule)、Dim 13: 具体例>=2個/file、Dim 14: テンプレート完備=100%、Dim 15: ピットフォール参照>=50% | 全項目閾値以上 |
| **スモークテスト** | core-workflow.mdの全ステージを仮想的にたどり、各ステージのstep→output→next stageの連鎖が論理的に成立することを確認 | ブロッキング不整合0件 |

---

## Phase 4: REFINEMENT — 重要改善領域

### R1: Completeness Review
**File**: `refinement/completeness-review.md`
**Classification**: ALWAYS
**Purpose**: ワークフローパス網羅性 + コンテンツ深度の検証

#### 改善内容（+~113行）

| 項目 | 現行 | 改善後 |
|------|------|--------|
| 検証範囲 | 構造完全性（ファイル存在、パス網羅） | + コンテンツ深度検証（テンプレート有無、具体例数、セクション充足度） |
| チェックリスト | 構造チェックリスト | + コンテンツ深度チェックリスト追加 |
| Gap分類 | なし（一律GENERATION戻り） | 修復判断ツリー適用（構造Gap→G1、コンテンツGap→G3） |
| レポート形式 | 基本的なPass/Fail | 構造完全性スコア + コンテンツ深度スコアの2軸レポート |

**コンテンツ深度チェックリスト（新規）**:
- [ ] 全Phase Ruleファイルに「Purpose」「Prerequisites」「Steps」「Error Handling」「Completion Message」セクションがある
- [ ] 全Phase Ruleファイルに具体例（GOOD/BAD）が2つ以上ある
- [ ] 全GENERATIONファイルに成果物テンプレートがある
- [ ] 全Error Handlingセクションにドメイン固有シナリオが含まれている
- [ ] 全Completion Messageが「REVIEW REQUIRED」+「WHAT'S NEXT」形式

---

### R2: Consistency Review
**File**: `refinement/consistency-review.md`
**Classification**: ALWAYS
**Purpose**: 用語統一 + 構造パターン一貫性 + ドメイン特化率測定

#### 改善内容（+~91行）

| 項目 | 現行 | 改善後 |
|------|------|--------|
| 用語チェック | terminology.md照合 | + Common Misuse列の活用（誤用パターンの検出） |
| ドメイン特化率 | なし | Dim 12: 各Phase Ruleファイルのドメイン特化率測定手順 |
| 4タイプ一貫性 | なし | 4エージェントタイプ（Process/Task/Analytical/Hybrid）への言及が一貫しているかチェック |
| レポート形式 | 不一致リスト | + ドメイン特化率ヒートマップ（ファイル別の特化率一覧） |

---

### R3: Quality Calibration
**File**: `refinement/quality-calibration.md`
**Classification**: ALWAYS
**Purpose**: 15品質次元スコアリング + 修復判断ツリー + ループ制御

#### 改善内容（+~131行）— 最大の改善ファイル

| 項目 | 現行 | 改善後 |
|------|------|--------|
| 品質次元 | 11次元のスコアリング | 15次元（Dim 12-15追加: 測定方法・閾値・閾値根拠を含む） |
| スコアカード | 基本的なテーブル | Evidence列追加（ファイルパス:行番号、カウント値等の具体的エビデンス） |
| 修復判断ツリー | なし | 品質次元→問題分類→戻り先マッピング（15次元全てのマッピング表） |
| ループ制御 | なし | 最大3回制限、同一ターゲット2回でエスカレーション、Repair Loop History記録 |
| 承認基準 | 11次元ベース | 15次元対応: APPROVED(15全PASS)/CONDITIONAL(13+,FAILがDim12-15のみ)/NEEDS REMEDIATION(12以下 or Dim1-11にFAIL) |
| エスカレーション | なし | 「修復ループが{N}回目です。続行/中断/スコープ変更」の質問テンプレート |

---

## Phase 5: PACKAGING — 全て新規

### P1: Plugin Structure Generation
**File**: `packaging/plugin-structure-generation.md`
**Classification**: ALWAYS
**Purpose**: GENERATION/REFINEMENTで品質検証済みのポリシーをClaude Codeプラグイン形式にパッケージング

#### Execution Steps Outline
1. Load REFINEMENT Results — 15次元全PASSを確認
2. Load Scope Definition — プラグイン構造設計を参照
3. Generate Plugin Configuration — plugin.json + marketplace.json
4. Generate Agent Definitions — spm-orchestrator, domain-researcher, policy-generator, quality-reviewer
5. Generate Skill Entry Points — 4 SKILL.md（steering-policy-creation, domain-research, quality-calibration, policy-templates）
6. Generate Commands — new-policy, resume-policy, validate-policy
7. Generate Rules — spm-standards.md
8. Copy Rule Details — GENERATION/REFINEMENTで改善済みファイルをsteering-rule-details/に配置
9. Present Plugin Structure for Approval

#### Output Artifacts
- `steering-policy-maker/` ディレクトリ全体（Scope Definitionの構造に準拠）

#### Error Scenarios
| エラー | 重大度 | リカバリー |
|--------|--------|-----------|
| SKILL.md行数超過(>150行) | Medium | コア機能に絞り、詳細はrule-detailsに委譲 |
| plugin.json構文エラー | High | テンプレートから再生成 |
| agents/定義とcore-workflow不整合 | High | core-workflow.mdのステージ構成を参照して再生成 |
| スキル10パターン未適用 | Medium | implementation-knowhow.mdのチェックリストで照合 |

#### Completion Message
- Summary: 生成ファイル数、ディレクトリ構造、SKILL.md行数
- Next: P2 (Automated Validation)

---

### P2: Automated Validation
**File**: `packaging/automated-validation.md`
**Classification**: ALWAYS
**Purpose**: プラグイン全体の構造・コンテンツ・動作テスト

#### Execution Steps Outline
1. Load Plugin Structure — P1で生成したファイル群を読み込み
2. Execute Structure Tests — plugin.json必須フィールド、全参照ファイル存在、ディレクトリ構造
3. Execute Content Tests — SKILL.md行数(75-150行)、core-workflow行数(200-600行)、ruleファイル行数(50-300行)
4. Execute Smoke Tests — 4エージェントタイプ（Process/Task/Analytical/Hybrid）で/spm:new-policyの簡易フロー実行
5. Generate Validation Report
6. Present Results — PASS: P3へ / FAIL: P1へ戻る（最大2回）

#### Error Scenarios
| エラー | 重大度 | リカバリー |
|--------|--------|-----------|
| 構造テストFAIL | High | P1で該当ファイルを再生成 |
| コンテンツテストFAIL（行数超過） | Medium | 該当ファイルを要約・分割 |
| スモークテストFAIL（フロー断絶） | Critical | Integration Validation(G4)結果と照合し、原因がポリシーかプラグイン構造か判定 |
| スモークテストFAIL（特定タイプのみ） | High | 該当タイプのテンプレート/分岐を修正 |

#### Completion Message
- Summary: 3層テスト結果（構造/コンテンツ/スモーク各PASS/FAIL）、4タイプ別スモーク結果
- Next: P3 (Migration Execution) or Delivery

---

### P3: Migration Execution
**File**: `packaging/migration-execution.md`
**Classification**: CONDITIONAL
**Purpose**: 既存ポリシー構造から新プラグイン構造へのマイグレーション

**Execute IF**: 既存ポリシーのブラッシュアップ（既存のレガシー構造が存在）
**Skip IF**: 新規ポリシー生成（マイグレーション元がない）

#### Execution Steps Outline
1. Identify Migration Source — 既存ディレクトリ構造を確認
2. Create File Path Mapping — 旧パス→新パスの対応表生成
3. Execute Copy — 改善済みファイルをsteering-rule-details/にコピー
4. Verify Integrity — コピー後のファイル整合性確認
5. Update References — CLAUDE.md、routing rules等の参照パスを更新
6. Mark Legacy as Deprecated — 旧ディレクトリにDEPRECATED注記追加（削除はしない）
7. Present Migration Report for Approval

#### Error Scenarios
| エラー | 重大度 | リカバリー |
|--------|--------|-----------|
| マイグレーション元の変更検出 | Medium | diff確認後、ユーザーに手動マージ/上書きを確認 |
| 参照パス更新漏れ | High | 全ファイルのgrep検索で旧パス参照を検出・修正 |
| レガシー構造の削除要求 | Low | 「非推奨化のみ、削除はユーザー判断」を案内 |

#### Completion Message
- Summary: マイグレーション済みファイル数、更新された参照数、レガシー構造の状態
- Next: Delivery

---

## Cross-Reference Matrix

| Source File | Common Rules参照 | 前ステージ出力参照 | 後ステージへの出力 |
|-------------|----------------|----------------|----------------|
| D1: purpose-analysis | question-format-guide, terminology | (なし — 最初のステージ) | D2, D4, E1 |
| D2: domain-research | question-format-guide, terminology, content-validation | D1出力 | D4, G3, R3 |
| D3: stakeholder-id | question-format-guide | D1, D2出力 | D4 |
| D4: scope-definition | question-format-guide, terminology | D1, D2, (D3)出力 | E1, P1 |
| E1: workflow-architecture | content-validation | D全出力 | E2, E3, E4, G1 |
| E2: common-rules-design | output-structure-patterns | E1出力 | G2 |
| E3: phase-rules-design | output-structure-patterns | E1, E2出力 | G3 |
| E4: quality-mechanisms | quality-standards | E1, E2, E3出力 | G1, R3 |
| G1: core-workflow-gen | content-validation, process-overview | E全出力 | G2, G3, G4 |
| G2: common-rules-gen | output-structure-patterns, terminology | E2出力, G1出力 | G3, G4 |
| G3: phase-rules-gen | terminology, error-handling | E3出力, D2出力, G1, G2出力 | G4 |
| G4: integration-validation | content-validation, quality-standards | G1, G2, G3出力 | R1 |
| R1: completeness-review | quality-standards | G全出力 | R2 |
| R2: consistency-review | terminology, quality-standards | G全出力, R1出力 | R3 |
| R3: quality-calibration | quality-standards | G全出力, R1, R2出力 | P1 (or Repair Loop) |
| P1: plugin-structure-gen | implementation-knowhow | R3出力(全PASS), D4出力 | P2 |
| P2: automated-validation | content-validation | P1出力 | P3 |
| P3: migration-execution | (なし) | P1, P2出力 | Delivery |

---

## Coverage Verification

- [x] 全18ステージに対応するルールファイル設計がある
- [x] CONDITIONAL 2ステージ（D3, P3）にExecute IF/Skip IF基準がある
- [x] 全Phase遷移が文書化されている（D→E→G→R→P）
- [x] 全承認ゲートにCompletion Message設計がある
- [x] 孤立ステージなし（全ステージが前後のステージと接続）
- [x] デッドエンドステージなし（全ステージにNext Step定義あり）
- [x] 修復ループの戻り先が全て有効なステージを指している
- [x] Domain Research 10パターンの適用先が全て明確
