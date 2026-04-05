# Quality Mechanisms Design

## Summary
- **Checkpoint Levels**: 2（Plan-level + Stage-level）
- **Audit Trail**: ISO 8601タイムスタンプ、完全ロギング、Repair Loop History対応
- **Content Validation**: 6コンテンツタイプ（Markdown, Mermaid, ASCII, JSON, YAML, Plugin Structure）
- **Error Categories**: 5フェーズ×4重大度（Critical/High/Medium/Low）
- **Depth Levels**: 3（Minimal/Standard/Comprehensive）
- **Quality Dimensions Covered**: 15/15（既存11 + 新規4）
- **Repair Mechanism**: 修復判断ツリー + ループ制御（最大3回 + 同一ターゲット2回でエスカレーション）
- **Automated Testing**: 3層テスト（構造 + コンテンツ + スモーク）× 2実行ポイント（G4, P2）

---

## 1. Checkpoint System Design

### Plan-Level Checkpoints（ステージ内）

各ステージの実行計画にチェックボックスを埋め込み、進捗を追跡する。

| ルール | 内容 |
|--------|------|
| 即時更新 | ステップ完了と同じインタラクション内でチェックボックスを更新 |
| 粒度 | 1ステップ = 1チェックボックス。サブステップが3つ以上ある場合はサブステップ単位 |
| Two-Partステージ | Part 1(Planning)のチェックボックス完了→承認→Part 2(Generation)のチェックボックス開始 |

### Stage-Level Checkpoints（ワークフロー全体）

steering-state.mdで全18ステージの進捗を追跡する。

| 状態遷移 | 条件 |
|---------|------|
| PENDING → IN PROGRESS | ステージの実行を開始した時 |
| IN PROGRESS → COMPLETED | ユーザー承認を得た時（タイムスタンプ記録） |
| PENDING → SKIPPED | CONDITIONALステージのSkip IF条件を満たした時（理由記録） |
| COMPLETED → IN PROGRESS | Repair Loopで戻された時（Repair Loop History記録） |

### 18 Checkpoint定義

CP-01〜CP-18はWorkflow Architectureの Checkpoint Map に準拠。各CPの合格基準はWorkflow Architecture文書を参照。

---

## 2. Audit Trail Design

### ログ形式

```markdown
## [Stage Name]
**Timestamp**: YYYY-MM-DDTHH:MM:SSZ
**User Input**: "[Complete raw input — never summarized]"
**AI Response**: "[Action taken or decision made]"
**Context**: [Phase / Stage / Step]

---
```

### 追加ログタイプ

| ログタイプ | 発生条件 | 追加フィールド |
|-----------|---------|--------------|
| Approval Log | 各CP通過時 | `**Checkpoint**: CP-[N]` `**Result**: APPROVED/SKIPPED` |
| Repair Loop Log | 修復ループ発生時 | `**Loop #**: [N]` `**Source**: [Stage]` `**Target**: [Stage]` `**Dimension**: Dim [N]` `**Reason**: [分類]` |
| Validation Log | G4/P2実行時 | `**Test Layer**: [構造/コンテンツ/スモーク]` `**Result**: PASS/FAIL` `**Details**: [詳細]` |
| Escalation Log | エスカレーション発生時 | `**Loop Count**: [N]` `**User Choice**: [続行/中断/スコープ変更]` |

### 運用ルール

| ルール | 内容 |
|--------|------|
| Append-Only | audit.mdは追記のみ。既存エントリを上書き・削除しない |
| 完全記録 | ユーザー入力は要約せず完全な原文を記録 |
| タイムスタンプ | 全エントリにISO 8601形式 |
| ツール使用 | Read→Editで追記。Writeによる全上書きは禁止 |

---

## 3. Content Validation Design

### コンテンツタイプ別バリデーション

| タイプ | バリデーション項目 | フォールバック |
|--------|-----------------|-------------|
| Markdown | 見出し階層、リンク整合性、リスト形式統一、コードブロック開閉 | 構文エラー箇所をコメントで注記 |
| Mermaid | ノードID英数字、特殊文字エスケープ、フローチャート構文 | テキスト代替を常に併記 |
| ASCII Diagram | 文字種制限(+-\|^v<>スペース)、行幅統一、角の整列 | テキスト代替を常に併記 |
| JSON (plugin.json等) | 構文チェック、必須フィールド存在確認 | テンプレートから再生成 |
| YAML | インデント整合性、構文チェック | JSONに変換して検証 |
| Plugin Structure | ファイル存在、参照整合性、行数ガイド適合 | 不適合ファイルリストを生成しP1で修正 |

### ドメイン固有バリデーション（SPM特有）

| チェック項目 | 対象フェーズ | 基準 |
|------------|-----------|------|
| ドメイン特化率プレチェック | GENERATION (G3) | terminology.md用語出現行数÷総行数 >= 40% |
| 具体例数チェック | GENERATION (G3) | GOOD/BAD例コードブロック >= 2個/ファイル |
| テンプレート完備チェック | GENERATION (G2, G3) | 出力セクションを持つべきファイルの100%にテンプレートあり |
| 行数ガイド適合 | GENERATION (G2), PACKAGING (P2) | SKILL.md: 75-150行、core-workflow: 200-600行、Phase Rule: 100-300行、Common Rule: 50-200行 |

---

## 4. Overconfidence Prevention Design

### フェーズ別リスク領域とトリガー

#### DISCOVERY Phase
| リスク | トリガー | 対処 |
|--------|---------|------|
| ドメイン知識の過信 | 類似ドメインの経験がある場合に固有の違いを無視 | Domain Researchで最低項目数の調査を強制 |
| 複雑度の過小評価 | 「Simple」分類を安易に選択 | 判定指標（フェーズ数、ステージ数目安）で客観化 |

#### DESIGN Phase
| リスク | トリガー | 対処 |
|--------|---------|------|
| 既存パターンへの過度な依拠 | 全エージェントを同一パターンで設計 | 4タイプ別テンプレートで強制分岐 |
| 品質次元の形式的カバー | 「全次元カバー済み」と安易に判定 | Dim 12-15の測定方法が未定義ならPENDING |

#### GENERATION Phase（最重要）
| リスク | トリガー | 対処 |
|--------|---------|------|
| テンプレートのみ生成 | ドメインコンテンツ注入なしで「完了」 | ドメイン特化率40%プレチェックを必須化 |
| 具体例の省略 | 「例は省略」「詳細は後で」 | 各Phase Ruleに最低2例を必須化 |
| 構造チェックのみで品質PASS | 構造テストPASSでコンテンツテストをスキップ | 3層テスト全層を必須化 |

#### REFINEMENT Phase
| リスク | トリガー | 対処 |
|--------|---------|------|
| 主観的品質判定 | 「十分なレベルです」と定性評価 | Dim 12-15の定量指標を使用。エビデンス列に具体値を記載 |
| 修復ループの回避 | FAILを見て見ぬふり | 修復判断ツリーに強制的にルーティング |

#### PACKAGING Phase
| リスク | トリガー | 対処 |
|--------|---------|------|
| スモークテスト省略 | 「構造テストPASSなので大丈夫」 | P2を必須ステージ（ALWAYS）として定義 |
| 一部タイプのスキップ | 「Hybridは複雑なので後で」 | 4タイプ全てのスモークテストを必須化 |

---

## 5. Error Recovery Design

### フェーズ別エラーカテゴリ

#### DISCOVERY Phase

| エラー | 重大度 | 検出 | リカバリー |
|--------|--------|------|-----------|
| エージェントタイプ分類の誤り | High | DESIGN Phase開始後に判明 | D1に戻り再分類。DESIGNの影響範囲を評価 |
| Domain Research不十分 | Medium | GENERATION時にドメイン特化率40%未達 | D2に戻るか、G3で追加調査 |
| Scope過大/過小 | Medium | GENERATION/REFINEMENT時に判明 | スコープ修正をユーザーに提案 |

#### DESIGN Phase

| エラー | 重大度 | 検出 | リカバリー |
|--------|--------|------|-----------|
| フェーズ構造不適切 | Critical | REFINEMENT R3で判明 | E1に戻り再設計（修復判断ツリー: 設計問題→E1） |
| 品質次元定義不備 | High | R3で測定不能 | E4に戻り定義修正（修復判断ツリー: 品質基準問題→E4） |
| Common Ruleの設計漏れ | Medium | G2生成時に判明 | E2を参照して補完 |

#### GENERATION Phase

| エラー | 重大度 | 検出 | リカバリー |
|--------|--------|------|-----------|
| core-workflowのフロー断絶 | Critical | G4構造テスト | G1に戻り再生成 |
| ドメイン特化率40%未達 | High | G4コンテンツテスト | G3に戻りドメインコンテンツ追加注入 |
| テンプレート欠如 | Medium | G4コンテンツテスト | G2/G3に戻りテンプレート追加 |
| 相互参照切れ | High | G4構造テスト | 参照元/先を特定し修正 |

#### REFINEMENT Phase

| エラー | 重大度 | 検出 | リカバリー |
|--------|--------|------|-----------|
| 修復ループ3回到達 | Critical | R3ループカウント | ユーザーエスカレーション（続行/中断/スコープ変更） |
| 同一ターゲット2回到達 | High | Repair Loop History | ユーザーエスカレーション（問題分類の再評価を提案） |
| Completeness Gap検出 | High | R1チェックリスト | 修復判断ツリーで分岐（構造→G1、コンテンツ→G3） |

#### PACKAGING Phase

| エラー | 重大度 | 検出 | リカバリー |
|--------|--------|------|-----------|
| plugin.json構文エラー | High | P2構造テスト | テンプレートから再生成 |
| SKILL.md行数超過 | Medium | P2コンテンツテスト | コア機能に絞り要約。詳細はrule-detailsに委譲 |
| スモークテストフロー断絶 | Critical | P2スモークテスト | G4結果と照合。ポリシー側なら修復ループ、プラグイン構造側ならP1再生成 |
| マイグレーション衝突 | Medium | P3実行時 | ユーザーに手動マージ/上書きを確認 |

---

## 6. Depth Adaptation Design

### 深度レベル定義

| Level | 対象シナリオ | 特性 |
|-------|-----------|------|
| **Minimal** | Simple Agent、Task Agent、低リスクドメイン | 2-3フェーズ、基本ステージのみ、Quality Standard level |
| **Standard** | Standard Agent、中程度のドメイン | 3-4フェーズ、標準的なステージ数、一部CONDITIONAL |
| **Comprehensive** | Complex Agent、Process Agent、高リスクドメイン | 4-5フェーズ、多数のステージ、Premium level、全品質次元詳細測定 |

### 深度選択基準

| 判定要素 | Minimal | Standard | Comprehensive |
|---------|---------|----------|---------------|
| Agent Type | Task | Analytical / Hybrid | Process |
| Complexity | Simple | Standard | Complex |
| Domain Risk | Low | Medium | High |
| Stakeholders | 1 | 2-3 | 4+ |
| Phase Count | 2-3 | 3-4 | 4-5 |

### 深度がステージに与える影響

| ステージ | Minimal | Standard | Comprehensive |
|---------|---------|----------|---------------|
| D2: Domain Research | 3項目調査 | 7項目調査 | 12項目調査 |
| G1: Core Workflow Gen | 200-300行 | 300-450行 | 450-600行 |
| G2: Common Rules Gen | Light template | Medium template | Heavy template |
| G3: Phase Rules Gen | 例なし〜1例 | 1-2例 | 2+例、GOOD/BAD |
| R3: Quality Calibration | 11次元 | 15次元(簡易) | 15次元(詳細+エビデンス) |

---

## 7. Completion Message Design

### Stage完了メッセージ（標準2オプション）

```markdown
### REVIEW REQUIRED
**[Stage Name] is complete.** [1-3行の成果サマリー]

### WHAT'S NEXT
**A) Request Changes** — フィードバックに基づいて修正します
**B) Continue to [Next Stage]** — 次のステージに進みます
```

### Phase完了メッセージ

```markdown
### PHASE COMPLETE: [Phase Name]
**[Phase Name]の全ステージが完了しました。**

**成果物:**
- [Artifact 1]: [説明]
- [Artifact 2]: [説明]

**A) Proceed to [Next Phase]** — 次のフェーズに進みます
**B) Review [Current Phase] outputs** — 成果物を確認します
```

### Workflow完了メッセージ

```markdown
### WORKFLOW COMPLETE
**ステアリングポリシーの生成が完了しました。**

**最終成果物:**
- ポリシーファイル: [N]ファイル ([N]行)
- プラグイン構造: [N]ファイル
- 品質スコア: [N]/15 PASS

**次のステップ:**
1. 生成されたポリシーセットを確認してください
2. プラグインをインストールしてテストしてください
3. フィードバックがあれば改善リクエストしてください
```

### エスカレーションメッセージ（修復ループ）

```markdown
### ESCALATION REQUIRED
**修復ループが[N]回目に達しました。**

現在のFAIL次元: [Dim N: 名前] (Score: [値])
これまでの修復履歴:
- Loop #1: [Source] → [Target]: [Reason]
- Loop #2: [Source] → [Target]: [Reason]

**選択してください:**
**A) 続行** — 同じアプローチで再度修復を試みます
**B) 中断** — 現状のまま納品します（品質注記付き）
**C) スコープ変更** — 品質目標またはスコープを調整します
```

---

## 8. 15品質次元 Coverage Map

| # | Dimension | Covered By | Design Location | 測定タイミング |
|---|-----------|------------|-----------------|-------------|
| 1 | Adaptive Workflow | Workflow Architecture | workflow-architecture.md (Stage表) | R3 |
| 2 | Mandatory Checkpoints | Checkpoint System | 本文書 Section 1 + workflow-architecture.md (CP Map) | R3 |
| 3 | Question File Format | Question Format Guide | common-rules-design.md (Section 3) | R2 |
| 4 | Content Validation | Content Validation Rules | 本文書 Section 3 + common-rules-design.md (Section 4) | G4, R2 |
| 5 | Audit Trail | Audit Trail System | 本文書 Section 2 | R2 |
| 6 | Error Handling | Error Recovery Framework | 本文書 Section 5 + common-rules-design.md (Section 6) | R1 |
| 7 | Overconfidence Prevention | Prevention Measures | 本文書 Section 4 + common-rules-design.md (Section 7) | R1 |
| 8 | Depth Levels | Depth Adaptation | 本文書 Section 6 | R3 |
| 9 | Session Continuity | State Tracking + Session Design | workflow-architecture.md (State Tracking) + common-rules-design.md (Section 5) | R2 |
| 10 | Terminology | Terminology Glossary | common-rules-design.md (Section 8) | R2 |
| 11 | Completion Messages | Message System | 本文書 Section 7 | R2 |
| 12 | Domain Specificity Rate | Dim 12 Definition + G3 Injection | workflow-architecture.md (15次元表) + phase-rules-design.md (G3) | G4, R2, R3 |
| 13 | Example Coverage | Dim 13 Definition + G3 Injection | workflow-architecture.md (15次元表) + phase-rules-design.md (G3) | G4, R1, R3 |
| 14 | Artifact Template Completeness | Dim 14 Definition + G2/G3 Templates | workflow-architecture.md (15次元表) + phase-rules-design.md (G2, G3) | G4, R1, R3 |
| 15 | Pitfall Reference Rate | Dim 15 Definition + G3 Injection | workflow-architecture.md (15次元表) + phase-rules-design.md (G3) | G4, R3 |

**VERIFY**: 15/15 dimensions have explicit design coverage.

---

## 9. Repair Judgment Tree Design

### 品質次元→問題分類→戻り先マッピング

（Workflow Architecture文書の「品質次元→問題分類マッピング」と完全整合）

| FAIL Dimension | 問題分類 | 戻り先 | 判定根拠 |
|---------------|---------|-------|---------|
| Dim 1 (Adaptive Workflow) | 設計問題 | E1 | ステージ分類の誤り |
| Dim 2 (Mandatory Checkpoints) | 構造的問題 | G1 | core-workflowのCP定義漏れ |
| Dim 3 (Question File Format) | コンテンツ問題 | G3 | Phase Ruleの質問形式不備 |
| Dim 4 (Content Validation) | コンテンツ問題 | G3 | バリデーションルール不足 |
| Dim 5 (Audit Trail) | 構造的問題 | G1 | state tracking設計の不備 |
| Dim 6 (Error Handling) | コンテンツ問題 | G3 | エラーシナリオ不足 |
| Dim 7 (Overconfidence Prevention) | コンテンツ問題 | G3 | ガイダンス不足 |
| Dim 8 (Depth Levels) | コンテンツ問題 | G3 | 深度定義不足 |
| Dim 9 (Session Continuity) | 構造的問題 | G1 | state file設計の不備 |
| Dim 10 (Terminology) | コンテンツ問題 | G2 | 用語集の不備 |
| Dim 11 (Completion Messages) | コンテンツ問題 | G3 | メッセージ形式不備 |
| Dim 12 (Domain Specificity) | コンテンツ問題 | G3 | ドメイン固有コンテンツ不足 |
| Dim 13 (Example Coverage) | コンテンツ問題 | G3 | 具体例不足 |
| Dim 14 (Artifact Template) | コンテンツ問題 | G2/G3 | テンプレート欠如 |
| Dim 15 (Pitfall Reference) | コンテンツ問題 | G3 | ピットフォール参照不足 |

### ループ制御ルール

| ルール | 内容 | 実装 |
|--------|------|------|
| 最大修復回数 | 全体で3回 | steering-state.md Repair Loop Historyのエントリ数をカウント |
| 同一ターゲット制限 | 同じステージへの2回目の戻りで警告 | Repair Loop Historyの Target 列を検索 |
| エスカレーション | 制限到達時にユーザーに判断を委ねる | エスカレーションメッセージ（Section 7参照）を表示 |
| P2ループ | P2→P1は最大2回（全体3回とは別カウント） | P2専用カウンター |
| 履歴記録 | 全ループをsteering-state.mdに記録 | `[timestamp] Loop #[N]: [Source] → [Target]: [Reason] (dimension: Dim [N])` |

### R1/R3の修復判断フロー

```
R1 (Completeness Review) でGap検出:
  ├── ファイル欠落・フロー断絶 → [REPAIR: structure] → G1
  └── コンテンツ不足（テンプレートなし、例なし） → [REPAIR: content] → G3

R3 (Quality Calibration) でFAIL検出:
  ├── Dim 1 → [REPAIR: design] → E1
  ├── Dim 2, 5, 9 → [REPAIR: structure] → G1
  ├── Dim 10 → [REPAIR: content] → G2
  ├── Dim 3, 4, 6, 7, 8, 11, 12, 13, 15 → [REPAIR: content] → G3
  ├── Dim 14 → [REPAIR: content] → G2 or G3（テンプレート種別に応じて判断）
  └── 品質次元定義自体に問題 → [REPAIR: criteria] → E4
```

### R2 (Consistency Review) の位置づけ

R2は情報収集・測定フェーズであり、修復判断は行わない。R2で検出した不一致（用語、構造パターン、ドメイン特化率）はR3のQuality Calibrationに引き継がれ、R3の修復判断ツリーで処理される。R2自体にREPAIRパスがないのは意図的な設計。

---

## 10. Automated Testing Design

### テスト実行ポイント

| 実行ポイント | テスト対象 | テスト層 |
|------------|----------|---------|
| G4: Integration Validation | 生成されたポリシーファイル群 | 構造テスト + コンテンツテスト + スモークテスト |
| P2: Automated Validation | プラグイン構造全体 | 構造テスト + コンテンツテスト + スモークテスト |

### G4テスト仕様

| テスト層 | チェック項目 | 合否基準 |
|---------|-----------|---------|
| 構造テスト | ファイル存在確認、core-workflow全参照解決、Markdown構文、フローパス完走 | エラー0件 |
| コンテンツテスト | Dim 12(>=40%), Dim 13(>=2/file), Dim 14(=100%), Dim 15(>=50%) | 全項目閾値以上 |
| スモークテスト | core-workflowの全ステージを仮想的にたどり、step→output→next stageの連鎖が成立 | ブロッキング不整合0件 |

### P2テスト仕様

| テスト層 | チェック項目 | 合否基準 |
|---------|-----------|---------|
| 構造テスト | plugin.json必須フィールド、全参照ファイル存在、ディレクトリ構造 | エラー0件 |
| コンテンツテスト | SKILL.md(75-150行)、core-workflow(200-600行)、Phase Rule(100-300行)、Common Rule(50-200行) | 全ファイル範囲内 |
| スモークテスト | 4タイプ(Process/Task/Analytical/Hybrid)で/spm:new-policyの簡易フロー実行 | ブロッキングエラー0件 |

### テスト結果レポート形式

```markdown
## Validation Report

### Structure Tests
- Total checks: [N]
- PASS: [N]
- FAIL: [N]
  - [FAIL項目1]: [詳細]

### Content Tests
- Dim 12 (Domain Specificity): [N]% (threshold: 40%) — [PASS/FAIL]
- Dim 13 (Example Coverage): [N] examples/file (threshold: 2) — [PASS/FAIL]
- Dim 14 (Artifact Templates): [N]% (threshold: 100%) — [PASS/FAIL]
- Dim 15 (Pitfall Reference): [N]% (threshold: 50%) — [PASS/FAIL]

### Smoke Tests
- Agent Types tested: [N]/4
  - Process: [PASS/FAIL]
  - Task: [PASS/FAIL]
  - Analytical: [PASS/FAIL]
  - Hybrid: [PASS/FAIL]

### Overall: [PASS/FAIL]
```

---

## 15次元承認基準

| 判定 | 条件 | アクション |
|------|------|----------|
| **APPROVED** | 15次元全PASS | PACKAGINGフェーズ（or Delivery）に進行 |
| **CONDITIONAL APPROVAL** | 13次元以上PASS、FAILがDim 12-15のみ | ユーザー判断。進行可だがコンテンツ品質に注記 |
| **NEEDS REMEDIATION** | 12次元以下PASS、またはDim 1-11にFAILあり | 修復判断ツリーで該当ステージに戻る |
