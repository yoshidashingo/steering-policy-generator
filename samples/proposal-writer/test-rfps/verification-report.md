# ワークフロー検証レポート

**検証日**: 2026-02-10
**検証対象**: proposal-writer ステアリングポリシー core-workflow.md + 23 rule detail files
**検証者**: proposal-writer 検証エージェント

---

## 1. 簡易RFP（社内タスク管理Webアプリケーション開発）

### RFP特性サマリー
- **発注者**: 株式会社テスト商事 情報システム部
- **想定予算**: 500万〜800万円
- **契約期間**: 6ヶ月
- **RFPページ数**: 約2ページ（< 10ページ）
- **要件数**: 必須6件、任意3件、非機能4件 = 計13件
- **評価方式**: 総合評価（技術50点 + 実績・体制20点 + 価格30点）
- **競争形態**: 総合評価 → 複数社競争入札が想定される
- **提出形式**: PDF、20ページ以内、構成自由

### フェーズ実行順序: PASS

4フェーズ（ANALYSIS → PLANNING → WRITING → REVIEW）は core-workflow.md の定義通り順序実行される。各フェーズ内のステージも以下の順序で正しく実行される：

1. ANALYSIS: RFP Intake → Requirements Extraction → Compliance Matrix
2. PLANNING: Proposal Structure Design → Win Strategy（CONDITIONAL判定） → Content Planning
3. WRITING: Executive Summary → Technical Response → Management Response（CONDITIONAL判定） → Pricing Framework（CONDITIONAL判定）
4. REVIEW: Compliance Check → Quality Review → Final Assembly

### CONDITIONALステージ判定

| ステージ | 判定 | 理由 |
|---------|------|------|
| Win Strategy | **Execute** | 総合評価方式であり複数社競争が想定される。評価基準に定性的評価（技術提案50点、実績・体制20点）が含まれる。純粋な価格評価ではなく、差別化戦略が有効。 |
| Management Response | **Execute** | 評価基準に「実績・体制」（20点）が明示されている。体制に関する情報提供が求められており、management/staffing scoringに該当する。 |
| Pricing Framework | **Execute** | 評価基準に「価格」（30点）が明示されている。想定予算（500万〜800万円）が提示されており、価格提案が必要。 |

### Adaptive Depth: **Minimal**

**判定根拠**:
- RFPは約2ページの短く明確な構成（< 10ページ）
- 要件は明確に番号付け（REQ-001〜REQ-013）
- 評価基準は3項目でシンプル
- 提出構成は自由形式
- rfp-intake.md の定義: 「Minimal: Short, well-structured RFP (< 10 pages) -- Quick summary and key dates」に合致

### 出力アーティファクトリスト

| ファイルパス | ステージ | 生成有無 |
|------------|---------|---------|
| `proposal-docs/analysis/rfp-summary.md` | RFP Intake | 生成 |
| `proposal-docs/analysis/requirements.md` | Requirements Extraction | 生成 |
| `proposal-docs/analysis/compliance-matrix.md` | Compliance Matrix | 生成 |
| `proposal-docs/planning/proposal-structure.md` | Proposal Structure Design | 生成 |
| `proposal-docs/planning/win-strategy.md` | Win Strategy | 生成 |
| `proposal-docs/planning/content-plan.md` | Content Planning | 生成 |
| `proposal-docs/proposal/executive-summary.md` | Executive Summary | 生成 |
| `proposal-docs/proposal/technical-response.md` | Technical Response | 生成 |
| `proposal-docs/proposal/management-response.md` | Management Response | 生成 |
| `proposal-docs/proposal/pricing-framework.md` | Pricing Framework | 生成 |
| `proposal-docs/review/compliance-check-report.md` | Compliance Check | 生成 |
| `proposal-docs/review/quality-review-report.md` | Quality Review | 生成 |
| `proposal-docs/review/submission-checklist.md` | Final Assembly | 生成 |
| `proposal-docs/proposal-state.md` | 全フェーズ | 生成 |
| `proposal-docs/audit.md` | 全フェーズ | 生成 |

**合計: 15ファイル**（CONDITIONALステージすべて実行のため、全ファイルが生成される）

---

## 2. 中規模RFP（住民サービスポータルシステム構築）

### RFP特性サマリー
- **発注者**: さくら市 デジタル推進課
- **想定予算**: 8,000万円（税込）
- **契約期間**: 約22ヶ月（開発12ヶ月 + 運用保守10ヶ月）
- **RFPページ数**: 約5ページ（ただし内容密度が高く要件数が多い → Standard相当）
- **要件数**: 住民向け7件、職員向け4件、基盤7件、任意3件、参加資格3件 = 計24件+
- **評価方式**: 総合評価落札方式（技術80点 + 価格20点）
- **競争形態**: 競争入札（プレゼンテーション審査あり）
- **提出形式**: A4版PDF、本文31ページ以内、構成指定あり、CD-R 2部 + 印刷5部

### フェーズ実行順序: PASS

4フェーズは正しく順序実行される。core-workflow.md の定義通り。

### CONDITIONALステージ判定

| ステージ | 判定 | 理由 |
|---------|------|------|
| Win Strategy | **Execute** | 競争入札形式。評価基準に技術提案（40点）、プロジェクト管理（15点）、実績・信頼性（15点）、運用保守計画（10点）と定性評価が80点中60点を占める。さらにプレゼンテーション審査が含まれる（core-workflow.md: 「RFP includes oral presentation or demonstration phase」に該当）。 |
| Management Response | **Execute** | 評価基準に「プロジェクト管理」（15点）が明示。提案書構成に「5. プロジェクト管理計画」が必須セクションとして指定（推進体制、開発スケジュール、リスク管理計画）。「実績一覧」セクションも必須。management-response.md の Execute IF条件「RFP requests management or organizational approach」「Evaluation criteria include management/staffing scoring」「Team structure or key personnel section required」すべてに該当。 |
| Pricing Framework | **Execute** | 評価基準に「価格」（20点）が明示。提案書構成に「8. 見積書（別紙）」が必須。pricing-framework.md の Execute IF条件「Evaluation criteria include price scoring」「Budget or cost structure section required」に該当。 |

### Adaptive Depth: **Standard**

**判定根拠**:
- RFPは内容密度が高いが50ページ未満
- 要件は構造化されカテゴリ別に整理（A/B/C/D）
- 評価基準は5項目で中程度の複雑さ
- 提出構成が指定済み（8セクション構成）
- ページ制限あり（31ページ以内）
- 複数巻構成ではない（単巻）
- rfp-intake.md の定義: 「Standard: Medium RFP (10-50 pages) -- Full section-by-section analysis」に合致

### 出力アーティファクトリスト

| ファイルパス | ステージ | 生成有無 |
|------------|---------|---------|
| `proposal-docs/analysis/rfp-summary.md` | RFP Intake | 生成 |
| `proposal-docs/analysis/requirements.md` | Requirements Extraction | 生成 |
| `proposal-docs/analysis/compliance-matrix.md` | Compliance Matrix | 生成 |
| `proposal-docs/planning/proposal-structure.md` | Proposal Structure Design | 生成 |
| `proposal-docs/planning/win-strategy.md` | Win Strategy | 生成 |
| `proposal-docs/planning/content-plan.md` | Content Planning | 生成 |
| `proposal-docs/proposal/executive-summary.md` | Executive Summary | 生成 |
| `proposal-docs/proposal/technical-response.md` | Technical Response | 生成 |
| `proposal-docs/proposal/management-response.md` | Management Response | 生成 |
| `proposal-docs/proposal/pricing-framework.md` | Pricing Framework | 生成 |
| `proposal-docs/review/compliance-check-report.md` | Compliance Check | 生成 |
| `proposal-docs/review/quality-review-report.md` | Quality Review | 生成 |
| `proposal-docs/review/submission-checklist.md` | Final Assembly | 生成 |
| `proposal-docs/proposal-state.md` | 全フェーズ | 生成 |
| `proposal-docs/audit.md` | 全フェーズ | 生成 |

**合計: 15ファイル**

---

## 3. 複雑RFP（全社デジタルトランスフォーメーション推進支援）

### RFP特性サマリー
- **発注者**: グローバルマニュファクチャリング株式会社（東証プライム上場）
- **想定年間予算**: 3億〜5億円（3年間）
- **契約期間**: 36ヶ月（3年間、年次更新）
- **RFPページ数**: 約7ページ（ただし要件・構成の複雑さが極めて高い → Comprehensive相当）
- **要件数**: コンサルティング5件、テクノロジー6件、人材育成4件、マネジメント5件、任意5件、参加資格5件 = 計30件+
- **評価方式**: 技術80点 + 価格20点 + プレゼンテーション評価（技術評価の一部）
- **競争形態**: コンペティション形式（5〜8社想定）、2段階審査（1次 + 役員プレゼン）
- **提出形式**: 3巻構成（技術58ページ、マネジメント30ページ、価格制限なし）、PDF各巻個別、電子+印刷10部

### フェーズ実行順序: PASS

4フェーズは正しく順序実行される。core-workflow.md の定義通り。

### CONDITIONALステージ判定

| ステージ | 判定 | 理由 |
|---------|------|------|
| Win Strategy | **Execute** | コンペティション形式で5〜8社が参加予定。定性評価が80点/100点。プレゼンテーション審査あり（60分＋30分質疑）。役員プレゼンも実施。現行ベンダー（日立、アクセンチュア）が存在し、強力な競争環境。win-strategy.md の Execute IF条件すべてに該当。 |
| Management Response | **Execute** | 第2巻「マネジメント提案書」が独立巻として必須（30ページ以内）。評価基準に「プロジェクト管理・体制」（15点）が明示。REQ-D01〜D05にマネジメント要件が5件。主要メンバー経歴書、類似プロジェクト実績も必須。management-response.md の Execute IF条件すべてに該当。 |
| Pricing Framework | **Execute** | 第3巻「価格提案書」が独立巻として必須。評価基準に「価格評価」（20点）が明示。見積総括表、年度別・フェーズ別費用内訳、人月単価表が必須。pricing-framework.md の Execute IF条件すべてに該当。 |

### Adaptive Depth: **Comprehensive**

**判定根拠**:
- 3巻構成の複雑なRFP（第1巻58ページ、第2巻30ページ、第3巻制限なし）
- 要件が多数かつ4カテゴリ（コンサルティング、テクノロジー、人材育成、マネジメント）に跨る
- 3フェーズ構成（戦略策定6ヶ月、基盤構築12ヶ月、全社展開18ヶ月）の複雑なプロジェクト
- 評価基準が細分化（6項目＋加点最大25点）
- 2段階審査プロセス（1次 → 役員プレゼン）
- 規制要件（ITAR/EAR、ISO 27001/SOC 2/ISMAP）
- 既存システム連携（SAP S/4HANA、Salesforce）
- rfp-intake.md の定義: 「Comprehensive: Large RFP (50+ pages, multiple volumes/attachments) -- Detailed analysis with attachment inventory」 — RFPドキュメント自体は短いが、提案書要求が3巻・88ページ超と実質的に50+ページ相当の複雑さ

### 出力アーティファクトリスト

| ファイルパス | ステージ | 生成有無 |
|------------|---------|---------|
| `proposal-docs/analysis/rfp-summary.md` | RFP Intake | 生成 |
| `proposal-docs/analysis/requirements.md` | Requirements Extraction | 生成 |
| `proposal-docs/analysis/compliance-matrix.md` | Compliance Matrix | 生成 |
| `proposal-docs/planning/proposal-structure.md` | Proposal Structure Design | 生成 |
| `proposal-docs/planning/win-strategy.md` | Win Strategy | 生成 |
| `proposal-docs/planning/content-plan.md` | Content Planning | 生成 |
| `proposal-docs/proposal/executive-summary.md` | Executive Summary | 生成 |
| `proposal-docs/proposal/technical-response.md` | Technical Response | 生成 |
| `proposal-docs/proposal/management-response.md` | Management Response | 生成 |
| `proposal-docs/proposal/pricing-framework.md` | Pricing Framework | 生成 |
| `proposal-docs/review/compliance-check-report.md` | Compliance Check | 生成 |
| `proposal-docs/review/quality-review-report.md` | Quality Review | 生成 |
| `proposal-docs/review/submission-checklist.md` | Final Assembly | 生成 |
| `proposal-docs/proposal-state.md` | 全フェーズ | 生成 |
| `proposal-docs/audit.md` | 全フェーズ | 生成 |

**合計: 15ファイル**

---

## 4. 参照パス検証

### core-workflow.md からの参照パス検証

core-workflow.md では全てのパスが相対パス（ディレクトリ名のみ）で記述されている。以下の参照について実際のファイル存在を確認した。

| 参照元（core-workflow.md内） | 参照パス | 実ファイル | 結果 |
|---------------------------|---------|-----------|------|
| Common Rules | `common/process-overview.md` | `proposal-writer-rule-details/common/process-overview.md` | **PASS** |
| Common Rules | `common/session-continuity.md` | `proposal-writer-rule-details/common/session-continuity.md` | **PASS** |
| Common Rules | `common/content-validation.md` | `proposal-writer-rule-details/common/content-validation.md` | **PASS** |
| Common Rules | `common/question-format-guide.md` | `proposal-writer-rule-details/common/question-format-guide.md` | **PASS** |
| Welcome Message | `common/welcome-message.md` (注: パスは `.proposal-writer/proposal-writer-rule-details/common/welcome-message.md`) | `proposal-writer-rule-details/common/welcome-message.md` | **PASS** |
| RFP Intake | `analysis/rfp-intake.md` | `proposal-writer-rule-details/analysis/rfp-intake.md` | **PASS** |
| Requirements Extraction | `analysis/requirements-extraction.md` | `proposal-writer-rule-details/analysis/requirements-extraction.md` | **PASS** |
| Compliance Matrix | `analysis/compliance-matrix.md` | `proposal-writer-rule-details/analysis/compliance-matrix.md` | **PASS** |
| Proposal Structure Design | `planning/proposal-structure-design.md` | `proposal-writer-rule-details/planning/proposal-structure-design.md` | **PASS** |
| Win Strategy | `planning/win-strategy.md` | `proposal-writer-rule-details/planning/win-strategy.md` | **PASS** |
| Content Planning | `planning/content-planning.md` | `proposal-writer-rule-details/planning/content-planning.md` | **PASS** |
| Executive Summary | `writing/executive-summary.md` | `proposal-writer-rule-details/writing/executive-summary.md` | **PASS** |
| Technical Response | `writing/technical-response.md` | `proposal-writer-rule-details/writing/technical-response.md` | **PASS** |
| Management Response | `writing/management-response.md` | `proposal-writer-rule-details/writing/management-response.md` | **PASS** |
| Pricing Framework | `writing/pricing-framework.md` | `proposal-writer-rule-details/writing/pricing-framework.md` | **PASS** |
| Compliance Check | `review/compliance-check.md` | `proposal-writer-rule-details/review/compliance-check.md` | **PASS** |
| Quality Review | `review/quality-review.md` | `proposal-writer-rule-details/review/quality-review.md` | **PASS** |
| Final Assembly | `review/final-assembly.md` | `proposal-writer-rule-details/review/final-assembly.md` | **PASS** |

### rule detail ファイル間の相互参照検証

| 参照元ファイル | 参照先パス | 存在 | 結果 |
|-------------|-----------|------|------|
| rfp-intake.md | `common/output-structure-patterns.md` | あり | **PASS** |
| rfp-intake.md | `common/error-handling.md` | あり | **PASS** |
| requirements-extraction.md | `common/output-structure-patterns.md` | あり | **PASS** |
| win-strategy.md | `common/overconfidence-prevention.md` | あり | **PASS** |
| content-planning.md | `common/overconfidence-prevention.md` | あり | **PASS** |
| quality-review.md | `common/quality-standards.md` | あり | **PASS** |
| session-continuity.md | `common/quality-standards.md` | あり | **PASS** |
| content-validation.md | `common/quality-standards.md` | あり | **PASS** |
| content-validation.md | `common/terminology.md` | あり | **PASS** |
| overconfidence-prevention.md | `common/question-format-guide.md` | あり | **PASS** |
| overconfidence-prevention.md | `common/quality-standards.md` | あり | **PASS** |

### 参照パス検証結果: **PASS**

全18件のcore-workflow.md → rule detail参照、および11件のrule detail間相互参照がすべて正しく解決される。ファイルの欠落や不整合は検出されなかった。

---

## 5. 11品質次元カバレッジ

### ファイル一覧（全24ファイル）

| # | ファイル | カテゴリ |
|---|---------|---------|
| 1 | `core-workflow.md` | ワークフロー定義 |
| 2 | `common/process-overview.md` | 共通ルール |
| 3 | `common/session-continuity.md` | 共通ルール |
| 4 | `common/content-validation.md` | 共通ルール |
| 5 | `common/question-format-guide.md` | 共通ルール |
| 6 | `common/error-handling.md` | 共通ルール |
| 7 | `common/overconfidence-prevention.md` | 共通ルール |
| 8 | `common/terminology.md` | 共通ルール |
| 9 | `common/quality-standards.md` | 共通ルール |
| 10 | `common/output-structure-patterns.md` | 共通ルール |
| 11 | `common/welcome-message.md` | 共通ルール |
| 12 | `analysis/rfp-intake.md` | Analysisフェーズ |
| 13 | `analysis/requirements-extraction.md` | Analysisフェーズ |
| 14 | `analysis/compliance-matrix.md` | Analysisフェーズ |
| 15 | `planning/proposal-structure-design.md` | Planningフェーズ |
| 16 | `planning/win-strategy.md` | Planningフェーズ |
| 17 | `planning/content-planning.md` | Planningフェーズ |
| 18 | `writing/executive-summary.md` | Writingフェーズ |
| 19 | `writing/technical-response.md` | Writingフェーズ |
| 20 | `writing/management-response.md` | Writingフェーズ |
| 21 | `writing/pricing-framework.md` | Writingフェーズ |
| 22 | `review/compliance-check.md` | Reviewフェーズ |
| 23 | `review/quality-review.md` | Reviewフェーズ |
| 24 | `review/final-assembly.md` | Reviewフェーズ |

### 品質次元カバレッジマトリクス

#### Dimension 1: Adaptive Workflow
**定義**: 全ステージがALWAYS/CONDITIONALに分類され、明確な基準がある
**判定**: **PASS**

| 対応ファイル | カバレッジ内容 |
|------------|-------------|
| `core-workflow.md` | 全13ステージの ALWAYS/CONDITIONAL 分類を定義。CONDITIONAL ステージ（Win Strategy, Management Response, Pricing Framework）の Execute IF / Skip IF 条件を明記。 |
| `common/process-overview.md` | Adaptive Execution Model セクションでステージ分類の全体像を説明。Depth Adaptation の3段階定義。 |
| `common/terminology.md` | ALWAYS/CONDITIONAL の用語定義。4フェーズ各ステージの分類を明記。 |
| 各phase rule file (12ファイル) | 各ステージに Execution Classification セクションがあり、Type: ALWAYS または CONDITIONAL と Execute IF / Skip IF を個別に定義。 |

#### Dimension 2: Mandatory Checkpoints
**定義**: 重要決定に標準化された2オプションメッセージでユーザー承認を求める
**判定**: **PASS**

| 対応ファイル | カバレッジ内容 |
|------------|-------------|
| `core-workflow.md` | 全ステージに「Wait for Explicit Approval」と「DO NOT PROCEED until user confirms」を明記。フェーズ完了時のA/B選択パターンを定義。「NO EMERGENT BEHAVIOR」ルールで2オプション形式を強制。 |
| `common/output-structure-patterns.md` | 「Stage Completion (Standard 2-Option)」パターンを定義: REVIEW REQUIRED + WHAT'S NEXT（A: Request Changes / B: Continue）。Phase Completion、Workflow Completion パターンも定義。 |
| 各phase rule file (12ファイル) | 各ファイルに Completion Message セクションがあり、全て2オプション形式（A: Request Changes / B: Continue to [Next Stage]）で統一。 |

#### Dimension 3: Question File Format
**定義**: 全質問が専用ファイル＋多肢選択形式＋「Other」オプション＋[Answer]:タグを使用
**判定**: **PASS**

| 対応ファイル | カバレッジ内容 |
|------------|-------------|
| `common/question-format-guide.md` | 質問ファイルの完全な仕様を定義: Rule 1（専用ファイル）、Rule 2（多肢選択A-D+Other）、Rule 3（[Answer]:タグ）、Rule 4（回答検証・矛盾検出）。Clarification Question File Templateも定義。 |
| `core-workflow.md` | 「MANDATORY: Question File Format」セクションで question-format-guide.md への参照を義務化。 |
| 各phase rule file (12ファイル) | 全ファイルに Question Generation セクションと Question File Template があり、A/B/C/D+(Other) + [Answer]: 形式で統一。 |

#### Dimension 4: Content Validation
**定義**: ファイル作成前の検証ルール（構造、相互参照、書式）
**判定**: **PASS**

| 対応ファイル | カバレッジ内容 |
|------------|-------------|
| `common/content-validation.md` | Pre-Creation Validation Checklist（5カテゴリ: Document Structure, Cross-Reference, Formatting, Content Integrity, Special Character）。Proposal-Specific Validation Rules（Compliance Matrix, Requirements Traceability, Section Length, Consistency）。Severity Levels（BLOCKING/WARNING/INFO）。 |
| `core-workflow.md` | 「MANDATORY: Content Validation」セクションで content-validation.md の参照を義務化。Compliance Matrix ステージで content-validation.md のロードを明示的に要求。 |
| `common/output-structure-patterns.md` | 全出力パターン（RFP Summary, Requirements, Compliance Matrix, Proposal Section, Submission Checklist）を定義し、検証のベースラインを提供。 |
| 各writing/review phase file | Validation ステップで content-validation.md への参照を含む。 |

#### Dimension 5: Audit Trail
**定義**: ISO 8601タイムスタンプ、完全な生入力ログ、追記専用
**判定**: **PASS**

| 対応ファイル | カバレッジ内容 |
|------------|-------------|
| `core-workflow.md` | 「Prompts Logging Requirements」セクションで6つのMANDATORYルールを定義。ISO 8601形式。「ALWAYS append...NEVER overwrite」ルール。Audit Log Format テンプレート。Correct Tool Usage for audit.md ガイダンス。 |
| `common/process-overview.md` | State Management セクションで audit.md の役割を定義: 「User inputs (complete, raw, never summarized)」。 |
| `common/output-structure-patterns.md` | Audit File Pattern テンプレートを定義。 |
| `common/session-continuity.md` | audit.md の最新エントリをセッション再開時にロードする手順を定義。 |
| 各phase rule file (12ファイル) | 全ステージの冒頭に「MANDATORY: Log any user input during this stage in audit.md」を明記。承認前後のログ取得を義務化。 |

#### Dimension 6: Error Handling
**定義**: フェーズ別リカバリ手順が全重要度レベルで定義
**判定**: **PASS**

| 対応ファイル | カバレッジ内容 |
|------------|-------------|
| `common/error-handling.md` | 全フェーズ（Analysis, Planning, Writing, Review）のエラーパターンと復旧手順を網羅的に定義。Error Severity Levels（Critical/High/Medium/Low）。Recovery Procedures（Partial Stage Completion, Missing Artifacts, User Restart, User Skip）。Escalation Guidelines。Error/Recovery Logging Format。 |
| `common/process-overview.md` | Error Handling セクションで基本原則を定義。 |
| 各phase rule file (12ファイル) | 全ファイルに Error Handling セクションがあり、ステージ固有のエラーパターン（Error → Solution → Workaround/Do Not Proceed）を定義。全て `common/error-handling.md` を参照。 |

#### Dimension 7: Overconfidence Prevention
**定義**: 「疑わしきは質問」方針のステージ別ガイダンス
**判定**: **PASS**

| 対応ファイル | カバレッジ内容 |
|------------|-------------|
| `common/overconfidence-prevention.md` | Core Principle（「Never assume when you can verify」）。ALWAYS Ask vs. Can Decide の分類（RFP Interpretation, Company Capabilities, Strategic Decisions, Compliance Decisions）。フェーズ別 Red Flags（Analysis, Planning, Writing, Review）。Success Indicators / FAILING indicators。 |
| `core-workflow.md` | Key Principles で「Evidence-Based」を定義。 |
| win-strategy.md | Validation ステップで `common/overconfidence-prevention.md` を参照。Discriminator が検証不可の場合の対応を定義。 |
| executive-summary.md, technical-response.md, management-response.md, pricing-framework.md | Prerequisites で `common/overconfidence-prevention.md` のロードを要求。Validation ステップで未検証の主張を禁止。 |
| content-planning.md | Prerequisites で `common/overconfidence-prevention.md` のロードを要求。 |

#### Dimension 8: Depth Levels
**定義**: RFP複雑さに基づく適応的深度（Minimal/Standard/Comprehensive）
**判定**: **PASS**

| 対応ファイル | カバレッジ内容 |
|------------|-------------|
| `core-workflow.md` | 「Adaptive Workflow Principle」セクションで5つの評価軸を定義。Requirements Extraction の Depth 判定基準を明記。 |
| `common/process-overview.md` | 「Depth Adaptation」セクションで3段階の定義（Minimal, Standard, Comprehensive）と適用条件を説明。 |
| `common/terminology.md` | 「Depth Levels」セクションで3段階の正式定義を提供。 |
| 各phase rule file (12ファイル) | Adaptive Depth セクションを持つファイル: rfp-intake.md, requirements-extraction.md, compliance-matrix.md, proposal-structure-design.md, content-planning.md, executive-summary.md, technical-response.md。各ファイルでステージ固有の Minimal/Standard/Comprehensive の動作を定義。 |

**補足**: win-strategy.md, management-response.md, pricing-framework.md はCONDITIONALステージであるため、Adaptive Depth セクションがない（Execute/Skip の2値判定が優先される）。review 系3ファイルも Adaptive Depth の明示的定義がないが、compliance-check.md, quality-review.md, final-assembly.md は入力内容に応じて自然に深度が調整される設計。

#### Dimension 9: Session Continuity
**定義**: ステートファイル追跡、コンテキストロード、アーティファクト検証付きセッション再開
**判定**: **PASS**

| 対応ファイル | カバレッジ内容 |
|------------|-------------|
| `common/session-continuity.md` | 完全な State File Format テンプレート（Phase Progress, Current Position, Key Decisions, Conditional Stage Decisions）。Session Resumption Procedure（5ステップ: Detect → Load → Validate → Load Context → Present Summary）。State Update Rules（When/How）。Error Recovery During Resumption（State File Missing, Corrupted, Artifacts Missing）。 |
| `core-workflow.md` | 「Progress Tracking: Update proposal-state.md」を Key Principles で義務化。「Plan-Level Checkbox Enforcement」で即時更新ルールを定義。 |
| `common/process-overview.md` | State Management セクションで proposal-state.md と audit.md の役割を定義。 |

#### Dimension 10: Terminology Standardization
**定義**: 全ファイルに渡る包括的用語集の適用
**判定**: **PASS**

| 対応ファイル | カバレッジ内容 |
|------------|-------------|
| `common/terminology.md` | 包括的用語集: Phase vs Stage の使い分け、4フェーズの正式名称と各ステージ名、RFP/Procurement用語（RFP, Evaluation Criteria, Compliance Matrix, Win Theme, Ghost Strategy, Discriminator, Proof Point）、Proposal Content用語、Compliance用語（Mandatory/Desirable/Informational/Compliant/Partially Compliant/Non-Compliant/Exception）、Depth Levels、Artifact Types、Common Abbreviations。Consistency Rules（4ルール）。 |
| `common/content-validation.md` | 「Content Integrity Validation」で技術用語の一貫性チェック、頭字語定義を要求。「Consistency Validation Across Sections」で用語統一を検証。 |
| `core-workflow.md` | 全体で統一された用語（Phase, Stage, ALWAYS, CONDITIONAL）を使用。 |

#### Dimension 11: Standardized Completion Messages
**定義**: 全ステージが REVIEW REQUIRED + WHAT'S NEXT（2オプション）で終了
**判定**: **PASS**

| 対応ファイル | カバレッジ内容 |
|------------|-------------|
| `core-workflow.md` | 「NO EMERGENT BEHAVIOR」ルールで標準化された2オプション完了メッセージを強制。「All stages MUST use standardized 2-option completion messages as defined in their respective rule files. DO NOT create 3-option menus or other emergent navigation patterns.」 |
| `common/output-structure-patterns.md` | 「Stage Completion (Standard 2-Option)」パターンを定義。Phase Completion パターン（A: Proceed / B: Review）。Workflow Completion パターン（PROPOSAL COMPLETE + NEXT STEPS）。 |
| `common/quality-standards.md` | Dimension 11 の PASS/PARTIAL/FAIL 基準を定義: 「PASS: Every stage uses the exact 2-option format. No 3-option menus. No emergent navigation patterns.」 |
| 各phase rule file (12ファイル) | 全ファイルに Completion Message セクションがあり、統一された形式を採用: REVIEW REQUIRED → 成果物サマリー → WHAT'S NEXT → A) Request Changes / B) Continue to [Next Stage]。 |

### 品質次元カバレッジ総合結果

| # | 品質次元 | 判定 | 主要カバレッジファイル数 |
|---|---------|------|---------------------|
| 1 | Adaptive Workflow | **PASS** | 15+ |
| 2 | Mandatory Checkpoints | **PASS** | 14+ |
| 3 | Question File Format | **PASS** | 14+ |
| 4 | Content Validation | **PASS** | 16+ |
| 5 | Audit Trail | **PASS** | 16+ |
| 6 | Error Handling | **PASS** | 14+ |
| 7 | Overconfidence Prevention | **PASS** | 8+ |
| 8 | Depth Levels | **PASS** | 10+ |
| 9 | Session Continuity | **PASS** | 3 |
| 10 | Terminology Standardization | **PASS** | 3+ |
| 11 | Standardized Completion Messages | **PASS** | 15+ |

**全11次元: PASS**

---

## 6. 総合判定

### 判定: **PASS**

### 所見

#### A. フェーズ実行順序
3件のサンプルRFPすべてにおいて、4フェーズ（ANALYSIS → PLANNING → WRITING → REVIEW）の順序実行が正しく動作する。core-workflow.md のフェーズ間遷移（Phase Complete メッセージ + A/B選択）が明確に定義されており、フェーズの飛ばしや逆行を防止する構造になっている。

#### B. CONDITIONALステージ判定
3件のサンプルRFPすべてにおいて、3つのCONDITIONALステージ（Win Strategy, Management Response, Pricing Framework）がいずれも **Execute** と判定された。これは3件すべてが競争入札形式であり、管理体制と価格の両方が評価対象に含まれるためである。

**注**: テストカバレッジの観点から、以下のシナリオのサンプルRFPが存在しないことを指摘する：
- **随意契約（Sole Source）**: Win Strategy が Skip となるケース
- **技術・価格のみ評価**: Management Response が Skip となるケース
- **価格別途提示**: Pricing Framework が Skip となるケース

これらのSkipパターンの検証には追加のサンプルRFPが必要である。ただし、条件定義自体は core-workflow.md と各 rule detail ファイルに正しく記載されており、論理的には正しく動作すると判断する。

#### C. Adaptive Depth
3件のRFPに対して適切なDepth Levelが判定された：
- 簡易RFP → **Minimal**: 短く構造化された要件に合致
- 中規模RFP → **Standard**: 中程度の複雑さと構造化された評価基準に合致
- 複雑RFP → **Comprehensive**: 多巻構成と高度な要件に合致

**注**: Depth Level の判定基準はページ数ベース（rfp-intake.md: < 10ページ / 10-50ページ / 50+ページ）が主要指標だが、複雑RFPのように RFP ドキュメント自体は短くても提案書要求が極めて複雑なケースでは、ページ数だけでは判定が難しい場合がある。ただし core-workflow.md の Adaptive Workflow Principle が5つの評価軸（ドキュメントサイズ・構造・明確さ、応答コンポーネント、評価基準の複雑さ、競争コンテキスト、規制要件）を定義しているため、総合的な判断が可能である。

#### D. 参照の正当性
core-workflow.md から 18件の rule detail ファイルへの参照、および rule detail ファイル間の11件の相互参照が全て正しく解決される。ファイルの欠落、パスの誤り、循環参照は検出されなかった。

#### E. 11品質次元カバレッジ
全11品質次元が適切なファイルでカバーされており、PASS判定である。特筆すべき設計上の強みは以下の通り：

1. **多層的カバレッジ**: 各品質次元が core-workflow.md（トップレベル定義）、common/ ファイル（詳細ルール）、各phase rule file（ステージ固有実装）の3層で担保されている
2. **quality-standards.md による自己検証**: 11品質次元自体がファイルとして定義され、PASS/PARTIAL/FAILの基準が明確に記載されている
3. **一貫した構造**: 全12の phase rule file が統一されたセクション構造（Purpose, Prerequisites, Execution Classification, Adaptive Depth, Execution Steps, Question Generation, Output Artifacts, Completion Message, Error Handling）を持つ

#### F. 改善提案（任意）

1. **Skipパターンのテスト**: CONDITIONALステージのSkip判定を検証するための追加サンプルRFP（随意契約、価格別途提示等）の作成を推奨
2. **Depth Level判定の明確化**: RFPドキュメントのページ数と提案書要求の複雑さが乖離するケースでの判定ガイダンスの追加を検討（現在はcore-workflow.mdの5軸で対応可能だが、より明示的なガイドがあると望ましい）
3. **Review系ステージのAdaptive Depth**: compliance-check.md, quality-review.md, final-assembly.md にも Adaptive Depth セクションを追加すると、3層の一貫性がさらに向上する

---

**検証完了**: 2026-02-10
