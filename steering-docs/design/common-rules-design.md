# Common Rules Design

## Design Context
- **Target Agent**: Steering Policy Maker (SPM) — Self-improvement
- **Workflow**: 5フェーズ×18ステージ（DISCOVERY→DESIGN→GENERATION→REFINEMENT→PACKAGING）
- **Quality Target**: Premium（15品質次元全PASS）
- **Domain**: AIエージェント向けステアリングポリシー作成（メタエージェント）
- **User**: Claude Codeユーザー（単一タイプ）。CLIベースの対話、マーケットプレイスからのプラグインインストール
- **Agent Type Coverage**: 4タイプ（Process/Task/Analytical/Hybrid）への生成対応。テスト・検証も4タイプ全てで実施

---

## Rules Inventory

| # | Rule File | 現行行数 | Adaptation Level | Key Changes | 目標行数 |
|---|-----------|---------|-----------------|-------------|---------|
| 1 | welcome-message.md | 133 | Heavy | 5フェーズ対応、PACKAGINGフェーズ追加、スキル化の案内 | ~160 |
| 2 | process-overview.md | 147 | Heavy | 18ステージ構成、修復判断ツリー概要、3層テスト概要 | ~200 |
| 3 | question-format-guide.md | 323 | Light | PACKAGING関連の質問例追加のみ | ~335 |
| 4 | content-validation.md | 236 | Medium | プラグイン構造バリデーション追加、SKILL.md行数制限ルール | ~280 |
| 5 | session-continuity.md | 159 | Medium | PACKAGINGフェーズ追加、Repair Loop History対応 | ~200 |
| 6 | error-handling.md | 346 | Heavy | PACKAGINGフェーズエラー、修復ループエラー、スモークテストエラー追加 | ~420 |
| 7 | overconfidence-prevention.md | 202 | Medium | GENERATION/REFINEMENT/PACKAGING向けガイダンス強化 | ~250 |
| 8 | terminology.md | 226 | Heavy | Common Misuse列・Related Terms列追加、PACKAGING用語追加 | ~320 |
| 9 | quality-standards.md | 311 | Heavy | 新規4品質次元(Dim12-15)定義、測定方法、承認基準改訂 | ~420 |
| 10 | output-structure-patterns.md | 438 | Medium | コンテンツ深度ガイダンス(Light/Medium/Heavy)追加 | ~500 |
| 11 | implementation-knowhow.md | 86 | Heavy | スキル化・プラグイン構造のノウハウ大幅追加 | ~180 |

**合計**: 現行2,607行 → 目標~3,265行（+~660行）

---

## Detailed Adaptation Specifications

### 1. welcome-message.md（Heavy Adaptation）

**現行**: 4フェーズ構成のASCII図、DISCOVERY開始の案内
**改善後**:

| 変更項目 | 内容 |
|---------|------|
| フェーズ図 | 5フェーズ（PACKAGING追加）のASCII図に更新 |
| Phase 5説明 | 「スキル化・プラグイン形式へのパッケージング」の簡潔な説明追加 |
| Key Principles | SPM 5つのコアルール（Domain Research由来）を表示 |
| Getting Started | 新規作成 vs ブラッシュアップの分岐を案内 |

**SPM 5つのコアルール（welcome-messageに掲載）**:
1. Discovery before Design — 目的分析とドメイン調査なしに設計を始めない
2. Approval gates at every stage — フェーズ遷移には必ずユーザー承認
3. Domain specificity > 40% — 生成コンテンツの40%以上がドメイン固有
4. 15-dimension quality target — 全ポリシーセットが15品質次元をPASS
5. Overconfidence prevention — ドメイン知識が不確実な場合は仮定せず質問する

---

### 2. process-overview.md（Heavy Adaptation）

**現行**: 4フェーズ15ステージの概要
**改善後**:

| 変更項目 | 内容 |
|---------|------|
| フェーズ/ステージ表 | 5フェーズ18ステージに更新（P1/P2/P3追加） |
| CONDITIONAL一覧 | D3 + P3 の2ステージ（CONDITIONAL判定基準付き） |
| 修復判断ツリー概要 | R1/R3からの分岐ロジックを簡潔に記載 |
| 3層テスト概要 | G4とP2で実施する構造/コンテンツ/スモークテストの概要 |
| ループ制限 | 最大3回、同一ターゲット2回でエスカレーション |

---

### 3. question-format-guide.md（Light Adaptation）

**現行**: 質問ファイル形式、多肢選択フォーマット、[Answer]:タグ
**改善後**:

| 変更項目 | 内容 |
|---------|------|
| PACKAGING質問例 | P1(プラグイン構造確認)、P3(マイグレーション確認)の質問テンプレート追加 |
| 修復ループ時の質問 | エスカレーション時の「続行/中断/スコープ変更」質問テンプレート |

---

### 4. content-validation.md（Medium Adaptation）

**現行**: Mermaid/ASCII/Markdown/相互参照バリデーション
**改善後**:

| 変更項目 | 内容 |
|---------|------|
| プラグイン構造バリデーション | plugin.json必須フィールド、SKILL.md行数制限(75-150行)、core-workflow行数(200-600行)、各ruleファイル行数(50-300行) |
| コンテンツ密度チェック | Domain Research Pattern 6（コンテンツ密度制限）に基づく行数ガード |
| Dim 12-15プレバリデーション | GENERATION時にドメイン特化率・具体例数の簡易チェックを実行するガイダンス |

---

### 5. session-continuity.md（Medium Adaptation）

**現行**: 4フェーズ対応、Welcome Backテンプレート、状態ファイル形式
**改善後**:

| 変更項目 | 内容 |
|---------|------|
| PACKAGINGフェーズ | Phase Progressセクション、Context Loading Priorityに追加 |
| Repair Loop History | 修復ループ履歴のstate file形式追加、ループ中の再開ロジック |
| Context Loading | PACKAGINGフェーズ: 全アーティファクト + プラグイン設計 を読み込み |

---

### 6. error-handling.md（Heavy Adaptation）

**現行**: DISCOVERY/DESIGN/GENERATION/REFINEMENTの各フェーズエラー
**改善後**:

| 変更項目 | 内容 |
|---------|------|
| PACKAGINGフェーズエラー | plugin.json構文エラー、SKILL.md行数超過、マイグレーション衝突、スモークテスト失敗 |
| 修復ループエラー | 最大回数到達、同一ターゲット再戻り、エスカレーション手順 |
| Integration Validation FAIL | 3層テスト各層のFAIL時リカバリー手順 |
| ドメインコンテンツ注入失敗 | Domain Research不足でドメイン特化率40%未達時の対処 |

**PACKAGINGフェーズ固有エラーシナリオ**:

| エラー | 重大度 | リカバリー |
|--------|--------|-----------|
| plugin.json必須フィールド欠落 | High | テンプレートから再生成 |
| SKILL.md行数超過(>150行) | Medium | コア機能に絞って要約、詳細はrule-detailsに委譲 |
| agents/定義とcore-workflow不整合 | High | core-workflow.mdのステージ構成を参照して再生成 |
| マイグレーション元(.steering/)変更検出 | Medium | diff確認後、ユーザーに手動マージ/上書きを確認 |
| スモークテストでフロー断絶 | Critical | Integration Validation結果と照合し、原因特定後G4へ戻る |

---

### 7. overconfidence-prevention.md（Medium Adaptation）

**現行**: 一般的な過信防止ガイダンス
**改善後**:

| 変更項目 | 内容 |
|---------|------|
| GENERATIONガイダンス | 「ドメイン知識が不十分なままテンプレートだけで生成しない」「ドメイン特化率40%未満は注入不足のサイン」 |
| REFINEMENTガイダンス | 「構造チェックだけでPASSとしない」「コンテンツ深度の主観評価を避け、Dim 12-15の定量指標を使用」 |
| PACKAGINGガイダンス | 「スモークテストなしでプラグイン完成としない」「4エージェントタイプ（Process/Task/Analytical/Hybrid）全てでの検証を省略しない」 |
| 修復ループガイダンス | 「修復2回目で改善が見られない場合は、問題分類自体を疑う」 |

---

### 8. terminology.md（Heavy Adaptation）

**現行**: 用語・定義の2列テーブル
**改善後**:

| 変更項目 | 内容 |
|---------|------|
| テーブル構造 | 4列: Term / Definition / Common Misuse / Related Terms |
| PACKAGING用語追加 | Plugin Structure, SKILL.md, marketplace.json, Migration, Smoke Test, Validation Agent |
| 品質用語追加 | Domain Specificity Rate, Example Coverage, Artifact Template, Pitfall Reference Rate |
| 修復ループ用語 | Repair Loop, Escalation, Max Retries, Same-Target Limit |

**テーブル構造例**:

| Term | Definition | Common Misuse | Related Terms |
|------|-----------|---------------|---------------|
| Domain Specificity Rate | Phase Ruleファイル内のドメイン固有行÷(総行数-空行-見出し) | 「ドメイン単語の出現率」と混同しがち。行レベルの判定であり、単語カウントではない | Dim 12, Content Quality, Domain Content Injection |
| Repair Loop | Quality Calibration FAILまたはCompleteness Review Gap検出時の修復サイクル | 「デバッグ」「リトライ」と同義ではない。品質次元マッピングに基づく構造的プロセス | Repair Judgment Tree, Escalation, Max Retries |

---

### 9. quality-standards.md（Heavy Adaptation）

**現行**: AI-DLC 11品質次元の定義と承認基準
**改善後**:

| 変更項目 | 内容 |
|---------|------|
| 品質次元 | 11→15次元（Dim 12-15追加） |
| 各次元にMeasurement Method追加 | 既存11次元にも測定方法の概要を追加 |
| 承認基準改訂 | APPROVED: 15全PASS / CONDITIONAL: 13+PASS(FAILがDim12-15のみ) / NEEDS REMEDIATION: 12以下 or Dim1-11にFAIL |
| 品質ダッシュボード形式 | スコアカードテンプレート（15次元×PASS/PARTIAL/FAIL + エビデンス列） |
| 閾値根拠セクション | Dim 12-15の各閾値がなぜその値なのかの根拠を記載 |

**品質ダッシュボードテンプレート（追加）**:

```markdown
## Quality Scorecard

| # | Dimension | Score | Evidence | Action |
|---|-----------|-------|----------|--------|
| 1 | Adaptive Workflow | PASS/FAIL | [ファイルパス:行番号] | [修復不要/要修復→戻り先] |
| ... | ... | ... | ... | ... |
| 15 | Pitfall Reference Rate | PASS/FAIL | [N/M = X%] | [修復不要/要修復→G3] |

**Summary**: [N]/15 PASS — [APPROVED/CONDITIONAL/NEEDS REMEDIATION]
```

---

### 10. output-structure-patterns.md（Medium Adaptation）

**現行**: ファイル構造パターン、セクション構成テンプレート
**改善後**:

| 変更項目 | 内容 |
|---------|------|
| コンテンツ深度ガイダンス | Light/Medium/Heavy別の推奨構成（セクション数、例の数、テンプレートの詳細度） |
| プラグインファイルパターン | SKILL.md, agents/*.md, commands/*.md の構造パターン追加 |
| 行数ガイドライン | ファイルタイプ別の推奨行数範囲（SKILL.md: 75-150行、Phase Rule: 100-300行等） |

**コンテンツ深度ガイダンス（追加）**:

| Depth | 対象 | セクション構成 | 例の数 | テンプレート |
|-------|------|-------------|--------|------------|
| Light | Simple Agent / Minimal Depth | Purpose, Steps(概要), Completion Message | 0-1 | 省略可 |
| Medium | Standard Agent / Standard Depth | Purpose, Prerequisites, Steps(詳細), Examples, Completion Message, Error Handling | 1-2 | 推奨 |
| Heavy | Complex Agent / Comprehensive Depth | Purpose, Prerequisites, Steps(詳細+サブステップ), Examples(GOOD/BAD), Completion Message, Error Handling, References | 2+ | 必須 |

---

### 11. implementation-knowhow.md（Heavy Adaptation）— ドメイン固有ルール

**現行**: 86行、基本的な実装ノウハウ
**改善後**:

| 変更項目 | 内容 |
|---------|------|
| スキル設計10パターン | Domain Researchで特定した10パターンの要約と参照 |
| プラグイン構造ガイド | .claude-plugin/構造、plugin.json必須フィールド、marketplace.json形式 |
| SKILL.md設計ガイド | アクティベーショントリガー、コアルール、Quality Gate、アンチパターンの記述方法 |
| エージェント設計ガイド | オーケストレーター/専門エージェントの分割パターン |
| コマンド設計ガイド | /spm:xxx形式のコマンド定義パターン |
| マイグレーションガイド | .steering/ → plugin構造への移行手順 |

---

## Cross-Rule Dependencies

| From | To | Dependency Type |
|------|-----|----------------|
| terminology.md | 全ファイル | 用語参照（全ファイルがterminology.mdの用語を使用） |
| quality-standards.md | quality-calibration (refinement) | 品質次元定義の参照元 |
| content-validation.md | 全generation/refinementファイル | バリデーションルール参照 |
| output-structure-patterns.md | 全generationファイル | ファイル構造パターン参照 |
| error-handling.md | 全phase ruleファイル | エラーシナリオの参照元 |
| process-overview.md | welcome-message.md | ワークフロー概要の整合性 |
| session-continuity.md | core-workflow.md | state file形式の整合性 |
| implementation-knowhow.md | packaging phase rules | プラグイン構造の参照元 |

---

## Design Principles

### 適用した Domain Research パターン

| Pattern # | Pattern Name | 適用先 |
|-----------|-------------|--------|
| 1 | 明示的アクティベーショントリガー | welcome-message.md（Getting Started分岐） |
| 2 | 4〜5個の記憶しやすいコアルール | welcome-message.md（5つのコアルール） |
| 3 | Quality Gate | quality-standards.md（15次元スコアカード） |
| 4 | 番号付きワークフロー | process-overview.md（18ステージの番号付きリスト、フェーズ内の順序を明示） |
| 5 | アンチパターンセクション | overconfidence-prevention.md（各フェーズのアンチパターン） |
| 6 | コンテンツ密度制限 | content-validation.md + output-structure-patterns.md（行数ガイド） |
| 7 | クロススキル参照 | implementation-knowhow.md（関連スキル参照） |
| 8 | ツール/MCP連携の明示 | implementation-knowhow.md（Domain Research時のContext7/Exa MCP連携手順、ツール不在時のフォールバック） |
| 9 | グレースフルデグラデーション | error-handling.md（MCPツール不在時のユーザー質問フォールバック、Domain Research不足時の類似ドメイン活用） |
| 10 | エビデンスファースト | quality-standards.md（スコアカードにEvidence列） |

---

## Estimated Effort Summary

| Adaptation Level | ファイル数 | ファイル一覧 | 合計追加行数 |
|-----------------|----------|------------|------------|
| Heavy (6) | 6 | welcome-message, process-overview, error-handling, terminology, quality-standards, implementation-knowhow | ~451行 |
| Medium (4) | 4 | content-validation, session-continuity, overconfidence-prevention, output-structure-patterns | ~195行 |
| Light (1) | 1 | question-format-guide | ~12行 |
| **合計** | **11ファイル** | | **~658行** |

> Scope Definitionの当初見積もり（+~400行）からの増加理由: DESIGN段階でPACKAGINGフェーズ対応、修復ループ制御、15品質次元測定方法の詳細化が必要と判明。+258行の増加は設計詳細化による自然な増加。
