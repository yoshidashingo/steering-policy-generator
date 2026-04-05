# Domain Research Summary

## Domain Overview
- **Domain**: AIエージェント向けステアリングポリシー作成（メタエージェント）
- **Research Depth**: Comprehensive
- **Reasoning**: メタエージェント（自身がポリシーを生成）、高リスク伝播、スキル化という新要件、既存品質ギャップの解消が必要
- **Research Sources**: 既存SPMポリシー26ファイル分析、ECC 10+スキル分析、OMC構造分析、マーケットプレイスアーキテクチャ調査

---

## 1. Best Practices

### 1.1 スキル設計パターン（既存高品質スキル実分析）

10スキルの実ファイルを分析して抽出した設計パターン。各パターンに具体例・反例・SPMへの適用方針を付記する。

#### Pattern 1: 明示的アクティベーショントリガー

具体的なユースケースで「いつこのスキルを使うか」を記述する。抽象的な説明は不可。

**GOOD** (`market-research` SKILL.md lines 11-17):
```
## When to Activate
- Researching a market, category, company, investor, or technology trend
- Competitive analysis or landscape mapping
- Market sizing or TAM estimation
- Technology trend evaluation
```

**BAD** (仮想的な悪い例):
```
## When to Activate
- When you need market information
- When doing research
```

**SPMへの適用**: SPMのSKILL.mdに以下のトリガーを定義:
- 新しいAIエージェント用のワークフロールールを構築する時
- アドホックなエージェント動作を構造化されたポリシーに変換する時
- 既存のエージェントポリシーをAI-DLC品質基準にアップグレードする時

#### Pattern 2: 4〜5個の記憶しやすいコアルール

記憶しやすく、検証可能で、ドメインに特化した少数のルール。

**GOOD** (`content-engine` SKILL.md lines 20-29):
```
## Core Rules
1. Adapt for the platform. Do not cross-post the same thing everywhere.
2. Hooks matter more than summaries.
3. One clear idea per post.
4. Use specifics over slogans.
5. Keep the ask small and clear.
```

**BAD** (仮想的な悪い例):
```
## Core Rules
1. Be good at content creation.
2. Follow best practices.
3. Write quality content.
```

**SPMへの適用**: SPMの5つのコアルール:
1. Discovery before Design — 目的分析とドメイン調査なしに設計を始めない
2. Approval gates at every stage — フェーズ遷移には必ずユーザー承認
3. Domain specificity > 40% — 生成コンテンツの40%以上がドメイン固有
4. 11-dimension quality target — 全ポリシーセットがAI-DLC 11品質次元をPASS
5. Overconfidence prevention — ドメイン知識が不確実な場合は仮定せず質問する

#### Pattern 3: Quality Gate（納品前チェックリスト）

スキル出力の品質を保証する4〜6項目の最終チェック。

**GOOD** (`market-research` SKILL.md lines 62-68):
```
## Quality Gate
- All numbers are sourced or labeled as estimates
- Old data (>18 months) is flagged
- Risks and counterarguments are included
- The recommendation follows from the evidence
- Sources are listed with dates
```

**BAD**: Quality Gateセクションがないスキル（検証なしで出力される）

**SPMへの適用**: ポリシーセット納品前に:
- [ ] 全11品質次元がPASS
- [ ] 各ファイルのドメイン特化率 > 40%
- [ ] 全相互参照が解決済み
- [ ] 全ワークフローパスが有効なエンドポイントに到達
- [ ] エラーハンドリングがドメイン固有のピットフォールをカバー

#### Pattern 4: 番号付きワークフロー

複雑なマルチステップ処理は明示的に番号付けし、順序を明確にする。

**GOOD** (`frontend-slides` SKILL.md: 7ステップワークフロー):
```
1. Detect Mode (new / convert / enhance)
2. Discover Content (source material)
3. Discover Style (brand, colors, typography)
4. Build (HTML generation)
5. Enforce Viewport Fit (responsive validation)
6. Validate (deliverable checklist)
7. Deliver (open in browser)
```

**SPMへの適用**: 既存の15ステージを4フェーズに番号付けして明示（現在の構造を維持）

#### Pattern 5: アンチパターンセクション

「やってはいけないこと」を具体的に列挙。抽象的な禁止ではなく、視覚的に認識できる具体例。

**GOOD** (`frontend-slides` SKILL.md lines 163-169):
```
## Anti-Patterns
- generic startup gradients with no visual identity
- long bullet walls (>6 items per slide)
- fixed-height content boxes that clip on resize
- code blocks that need scrolling
```

**SPMへの適用**: SPMのアンチパターン:
- ドメイン調査なしでのポリシー生成（汎用的な出力を生む）
- マルチユーザーエージェントでの利害関係者分析スキップ
- テンプレートのみの生成（ドメインコンテンツ注入なし）
- 構造チェックのみのバリデーション（コンテンツ深度チェックなし）
- FAIL品質次元の修復なしでの続行

#### Pattern 6: コンテンツ密度制限

出力の膨張を防ぐハード制約を設定する。

**GOOD** (`frontend-slides`): スライド最大6箇条、コードブロック最大10行
**GOOD** (`investor-outreach`): 転送用ブリーフ100語以内

**SPMへの適用**:
- SKILL.md: 75〜150行
- core-workflow.md: 200〜500行
- 各Common Ruleファイル: 50〜200行
- 各Phase Ruleファイル: 100〜300行

#### Pattern 7: クロススキル参照

関連スキルへのリンクで、エコシステム内の位置づけを明示。

**GOOD** (`frontend-slides` lines 175-178):
```
## Related ECC Skills
- `frontend-patterns` for component and interaction patterns
- `liquid-glass-design` when borrowing Apple glass aesthetics
- `e2e-testing` if you need automated browser verification
```

**SPMへの適用**: `domain-research`, `quality-calibration`, `policy-templates` の3スキルを相互参照

#### Pattern 8: ツール/MCP連携の明示

外部ツール呼び出しの手順と代替手段を明記。

**GOOD** (`documentation-lookup` SKILL.md lines 30-55):
```
## Workflow
1. Call resolve-library-id with the library name
2. Select best match from results
3. Call query-docs with the library ID and question
4. Limit: Do not call query-docs more than 3 times per question
```

**SPMへの適用**: ドメイン調査時のContext7/Exa MCP連携手順を明記。ツール不在時はユーザーへの質問にフォールバック。

#### Pattern 9: グレースフルデグラデーション

ツールやリソースが利用不可な場合の代替動作を定義。

**GOOD** (`documentation-lookup`): "If unclear after 3 calls, use best information rather than guessing"
**GOOD** (`frontend-slides`): "If browser automation is available, use it to verify..."

**SPMへの適用**: MCPツール不在時はユーザーに直接質問。ドメインリサーチ不十分な場合は最も近い類似ドメインのプラクティスを出発点とする。

#### Pattern 10: エビデンスファースト

形容詞ではなく証拠・数値・具体例で主張を裏付ける。

**GOOD** (`market-research` lines 23-27):
```
## Research Standards
- Every claim needs a source — or mark it "estimate" / "inference"
- Prefer data from the last 24 months
- Include at least one contrarian or downside view
- Separate fact from inference from recommendation
```

**SPMへの適用**: 品質キャリブレーション時に各次元のスコアに具体的なエビデンス（ファイルパス、行番号、カウント）を要求。

### 1.2 マーケットプレイス配布パターン（実構造分析）

ECC v1.9.0のプラグイン構造を実際に分析した結果:

#### plugin.json構造
```json
{
  "name": "steering-policy-maker",
  "version": "1.0.0",
  "description": "Meta-agent that generates AI-DLC quality steering policies for any AI agent.",
  "author": { "name": "Shingo YOSHIDA" },
  "repository": "https://github.com/yoshidashingo/steering-policy-generator",
  "license": "MIT",
  "keywords": ["steering-policy", "meta-agent", "ai-dlc", "quality-calibration"],
  "agents": [
    "./agents/spm-orchestrator.md",
    "./agents/domain-researcher.md",
    "./agents/policy-generator.md",
    "./agents/quality-reviewer.md"
  ],
  "skills": ["./skills/"],
  "commands": ["./commands/"],
  "rules": ["./rules/"]
}
```

#### marketplace.json構造
```json
{
  "$schema": "https://anthropic.com/claude-code/marketplace.schema.json",
  "name": "steering-policy-maker",
  "description": "Generate AI-DLC quality steering policies for any AI agent",
  "owner": { "name": "Shingo YOSHIDA" },
  "plugins": [{
    "name": "steering-policy-maker",
    "source": "./",
    "version": "1.0.0",
    "category": "workflow",
    "tags": ["agents", "skills", "policy-generation", "quality-assurance"],
    "strict": false
  }]
}
```

#### 自動アップデート設定（ユーザー側 settings.json）
```json
{
  "extraKnownMarketplaces": {
    "steering-policy-maker": {
      "source": {
        "source": "github",
        "repo": "yoshidashingo/steering-policy-generator"
      },
      "autoUpdate": true
    }
  }
}
```

### 1.3 ポリシー生成の品質パターン（根本原因分析付き）

| パターン | DISCOVERY（高品質） | GENERATION（低品質） | 根本原因 |
|---------|-------------------|---------------------|---------|
| **判断ガイダンス** | 判断ツリー型: スコアリング → 分類 → Hybrid判定 | タスク実行型: 読む → テンプレート適用 → 書く | DISCOVERYは意思決定を支援、GENERATIONはタスク実行を指示 |
| **不確実性の扱い** | 「迷ったら聞け」哲学。質問生成セクション完備 | 「仕様は十分」と仮定。質問セクションなし | GENERATIONに弱点検出・質問メカニズムがない |
| **成果物テンプレート** | 全4ファイルに期待出力の完全テンプレートあり | 4ファイル中1ファイルのみ（integration-validation.md） | 生成者が「何を書くべきか」の具体像を持てない |
| **ドメイン特化** | 70%がドメイン固有コンテンツ | 40%がドメイン固有（残りは汎用手順） | ドメインリサーチ結果の参照が不十分 |

---

## 2. Quality Standards

### 2.1 AI-DLC 11品質次元（測定方法付き）

| # | 次元 | 測定方法 | PASS基準 | FAIL基準 |
|---|------|---------|---------|---------|
| 1 | Adaptive Workflow | ステージ分類カウント | 全ステージにALWAYS/CONDITIONAL明記 | 未分類ステージあり |
| 2 | Mandatory Checkpoints | 承認ゲートカウント | 全フェーズ遷移+設計決定に承認ゲート | 承認なしの遷移あり |
| 3 | Question File Format | 質問ファイル検査 | 全質問が選択式+Other+[Answer]:タグ | チャット内質問あり |
| 4 | Content Validation | バリデーションルール有無 | Mermaid/ASCII/Markdown/相互参照の検証ルール定義済み | 検証ルールなし |
| 5 | Audit Trail | 監査ログ検査 | ISO 8601タイムスタンプ、原文保持、追記型 | 要約または上書き |
| 6 | Error Handling | エラーシナリオ網羅率 | 全フェーズにシナリオ別回復手順 | 汎用エスカレーションのみ |
| 7 | Overconfidence Prevention | 質問生成ガイダンス有無 | ステージ別質問カテゴリ、レッドフラグ定義 | 質問ガイダンスなし |
| 8 | Depth Levels | 適応的深度セクション有無 | Minimal/Standard/Comprehensive定義+選択基準 | 単一深度のみ |
| 9 | Session Continuity | 状態ファイル+再開テンプレート有無 | 状態ファイル形式+コンテキスト読込優先度 | 状態管理なし |
| 10 | Terminology | 用語集検査 | ドメイン用語+正誤例+関連用語+誤用パターン | 用語集なし |
| 11 | Completion Messages | 完了メッセージ形式検査 | 全ステージがREVIEW REQUIRED+2択WHAT'S NEXT | 非標準形式あり |

### 2.2 追加品質基準（本リサーチで特定）

| # | 基準 | 測定方法 | PASS基準 |
|---|------|---------|---------|
| 12 | ドメイン特化率 | 各Phase Ruleファイルのドメイン固有行÷総行 | >40% |
| 13 | 具体例カバレッジ | 各Phase Ruleファイルのドメイン固有例の数 | ≥2例/ファイル |
| 14 | 成果物テンプレート完備 | 期待出力テンプレートを持つファイルの割合 | 100% |
| 15 | ピットフォール参照 | エラーハンドリングがdomain-research由来のピットフォールを参照する割合 | >50% |

**ドメイン特化率の測定手順**:
1. 対象ファイルの全行を数える（空行・ヘッダー除く）
2. ドメイン固有行を数える: ドメイン用語を含む行、ドメイン固有の例、ドメイン固有のエラーシナリオ
3. ドメイン特化率 = ドメイン固有行 ÷ 全行 × 100%
4. 基準: 40%以上でPASS、20-39%でPARTIAL、20%未満でFAIL

---

## 3. Construction品質ギャップ分析（定量データ）

### 3.1 ファイル別定量比較

| ファイル | フェーズ | 行数 | ステップ数 | コードブロック | テーブル行 | チェック項目 | 品質スコア |
|---------|--------|------|----------|-------------|----------|------------|----------|
| purpose-analysis.md | DISCOVERY | 269 | 7 | 4 | 18 | 9 | **8/10** |
| domain-research.md | DISCOVERY | 255 | 8 | 2 | 16 | 10 | **8/10** |
| scope-definition.md | DISCOVERY | 333 | 9 | 4 | 15 | 6 | **9/10** |
| stakeholder-identification.md | DISCOVERY | 224 | 8 | 2 | 12 | 2 | **7/10** |
| **DISCOVERY合計** | | **1,081** | **32** | **12** | **61** | **27** | **平均 8.0** |
| core-workflow-generation.md | GENERATION | 254 | 10 | 2 | 0 | 45 | **7/10** |
| common-rules-generation.md | GENERATION | 321 | 6 | 0 | 0 | 7 | **6/10** |
| phase-rules-generation.md | GENERATION | 277 | 9 | 2 | 0 | 13 | **6/10** |
| integration-validation.md | GENERATION | 349 | 9 | 14 | 36 | 30 | **8/10** |
| **GENERATION合計** | | **1,201** | **34** | **18** | **36** | **95** | **平均 6.75** |

**重要な発見**: GENERATIONはDISCOVERYより総行数が多い（1,201 vs 1,081）にもかかわらず品質スコアが低い（6.75 vs 8.0）。行数は多いが、手続き的な記述が中心でアクション密度が低い。

### 3.2 品質ギャップの根本原因

#### 原因1: 成果物テンプレートの欠如

| フェーズ | 期待出力テンプレート完備率 |
|---------|----------------------|
| DISCOVERY | **4/4ファイル** (100%) |
| GENERATION | **1/4ファイル** (25%) — integration-validation.mdのみ |

`common-rules-generation.md`は10+ルールファイルを生成する責任を持つが、各ファイルの期待出力例がゼロ。

**悪い例（現状の common-rules-generation.md Step 4）**:
```markdown
#### File 1: `terminology.md`
**Adaptation Level**: Heavy
**Key Content**:
- Phase/Stage hierarchy specific to target agent
- All domain-specific terms from domain research
**Must Include**:
- Correct/incorrect usage examples
```

**良い例（改善案）**: 各適応レベルで10-15行の具体的な出力例を付記:
```markdown
#### File 1: `terminology.md`
**Adaptation Level**: Heavy

**Light適応の例**（コードレビューエージェント — 既知ドメイン）:
| Term | Definition | Usage |
|------|-----------|-------|
| Review | コード変更の品質検査 | 「Reviewを開始する」 |
| Approval | レビュー完了の承認 | 「Approvalを得る」 |

**Heavy適応の例**（医療コンプライアンスエージェント — 専門ドメイン）:
| Term | Definition | Context | Common Misuse |
|------|-----------|---------|---------------|
| GxP | Good Practice規制群の総称 | 医薬品・医療機器の品質管理 | GMP（製造のみ）と混同 |
| CAPA | Corrective And Preventive Action | 逸脱発生時の是正・予防措置 | 是正のみで予防を省略 |
```

#### 原因2: ドメイン固有コンテンツ生成ガイダンスの欠如

`phase-rules-generation.md` Step 3c は6行の汎用指示:
```markdown
##### 3c. Add Domain-Specific Content
For each step in the file:
- Add domain-specific instructions and considerations
- Include domain terminology (verified against terminology.md)
- Add domain-specific examples where helpful
```

**改善案**: 構造化されたヒューリスティクスに置き換え:
1. 各実行ステップについて、domain-research-summary.mdの「Best Practices」を確認。マッピング可能なプラクティスがあればサブステップとして組み込む
2. 各エラーハンドリングについて、domain-research-summary.mdの「Common Pitfalls」を確認。該当ステージで発生しうるピットフォールを名前付きエラーシナリオとして追加
3. ドメイン固有コンテンツの定量チェック: 各ファイルの実行ステップの40%以上がドメイン固有の用語・例・参照を含むこと
4. 3c完了後に「ドメインコンテンツ検証」サブステップを追加。閾値未満のファイルをフラグ

#### 原因3: GENERATIONに適応的深度がない

DISCOVERYの`domain-research.md`にはMinimal/Standard/Comprehensiveの優れた深度分類があるが、GENERATIONには一切ない。Simple Task AgentもComplex Hybrid Agentも同じ生成プロセスを経る。

**改善案**: core-workflow-generation.mdに適応的深度を追加:
- **Minimal**（Simple, 2-3フェーズ）: 150-250行。Planテンプレート省略、直接生成
- **Standard**（Standard, 3-4フェーズ）: 250-400行。現行の2パート実行
- **Comprehensive**（Complex/Hybrid, 5+フェーズ）: 400-500行。モード検出セクション追加、条件分岐フロー文書化

### 3.3 TOP 3 改善優先度

| 優先度 | 改善内容 | 対象ファイル | 期待効果 |
|--------|---------|------------|---------|
| 1 | 全GENERATIONファイルに成果物テンプレート追加 | common-rules-generation.md, phase-rules-generation.md, core-workflow-generation.md | テンプレート完備率 25%→100% |
| 2 | ドメイン固有コンテンツ生成ヒューリスティクス追加 | phase-rules-generation.md Step 3c | ドメイン特化率の測定・保証が可能に |
| 3 | GENERATIONに適応的深度追加 | core-workflow-generation.md | エージェント複雑度に応じた生成品質の最適化 |

---

## 4. Common Pitfalls（具体例付き）

### Pitfall 1: Construction品質ギャップ

- **Description**: DISCOVERY/DESIGNフェーズに比べGENERATION/REFINEMENTの出力品質が低い
- **Cause**: GENERATIONが「仕様は十分」と仮定し、テンプレート適用に終始する
- **Impact**: 生成されたポリシーが構造的に正しいが内容が浅い・汎用的

**BAD（現状のGENERATION出力の例）**:
```markdown
### Step 1: Analyze Input
**Action**: Analyze the input data
**Input**: User-provided data
**Output**: Analysis results
**Validation**: Verify analysis is complete
```

**GOOD（ドメイン特化されたGENERATION出力の例）**:
```markdown
### Step 1: Analyze Input Requirements Document
**Action**: Parse the PRD for functional/non-functional requirements classification
**Input**: User-provided PRD (plain text or structured markdown)
**Output**: Requirements matrix with priority (P0-P3) and type (functional/NFR/constraint)
**Validation**: All requirements have priority assignment; ambiguous items flagged for clarification
**Domain Note**: PRDs often contain implicit NFRs in "assumptions" sections — extract these explicitly
```

- **Prevention**: GENERATIONに「弱点検出ループ」を追加:
  - 生成 → コンテンツ品質バリデーション → 弱パターン検出 → ドメインガイダンスで補強 → 再バリデーション
- **Detection**: ドメイン特化率 <40% のファイルを自動フラグ。成果物テンプレートとの差分チェック。

### Pitfall 2: テンプレート依存

- **Description**: Phase Rulesが構造テンプレートに依存し、ドメイン固有コンテンツが不足
- **Cause**: output-structure-patterns.mdがフォーマット指示のみで内容ガイダンスがない

**BAD（テンプレートそのまま）**:
```markdown
## Error Handling
### [Error 1]
- **Issue**: [What went wrong]
- **Solution**: [How to fix]
```

**GOOD（ドメイン固有エラーハンドリング）**:
```markdown
## Error Handling
### Ambiguous Regulatory Scope
- **Issue**: User's compliance domain spans multiple regulations (HIPAA + SOX)
- **Cause**: Cross-functional agent serving both healthcare and finance teams
- **Solution**: Create regulation-specific policy branches; ask user to prioritize primary regulation
- **Recovery**: Split scope definition into per-regulation tracks; merge at Quality Mechanisms Design
```

- **Prevention**: 各ステップの「ドメイン固有コンテンツ要件」を明記。output-structure-patterns.mdにコンテンツ深度ガイダンスを追加
- **Detection**: 各ファイルのエラーハンドリングセクションがdomain-research由来のピットフォールを参照しているか検査（目標: >50%参照率）

### Pitfall 3: バリデーション = 存在チェック止まり

- **Description**: REFINEMENTが「ファイルが存在するか」「セクションがあるか」の確認のみ

**BAD（現状のquality-calibration.mdのバリデーション）**:
```markdown
1. Count total stages in generated core-workflow.md
2. Count stages with explicit ALWAYS classification
3. Count stages with explicit CONDITIONAL classification
```

**GOOD（コンテンツ深度を含むバリデーション）**:
```markdown
1. Count total stages — verify all classified ALWAYS/CONDITIONAL
2. For CONDITIONAL stages: verify criteria reference domain-specific conditions from domain-research.md
3. Sample 3 phase rule files: verify >40% domain-specific content
4. Verify at least 2 domain-specific examples per phase rule file
5. Verify error handling references domain pitfalls (not generic "return to prior phase")
```

- **Prevention**: Quality Calibrationに「コンテンツ深度」次元（Dimension 12-15）を追加
- **Detection**: 品質キャリブレーションスコアカードにコンテンツ品質列を追加

### Pitfall 4: REFINEMENT修復ガイダンス欠如

- **Description**: FAILした次元に対する修復手順が「該当フェーズに戻る」のみ

**修復判断ツリー（改善案）**:
```
FAILした次元を分析:
├── 共通ルール系（Terminology, Question Format, Error Handling）が1-2次元FAIL
│   → common-rules-generation.md再実行、ドメイン固有コンテンツ追加
│   → 再実行後、consistency-reviewから再検証
│
├── Phase Ruleファイルが3+件で薄い/汎用的（<100行 or ドメイン特化率<20%）
│   → phase-rules-generation.md Step 3c再実行
│   → 各ファイルにドメイン例追加、用語反映、ピットフォールカバレッジ追加
│   → 再実行後、completeness-reviewから再検証
│
├── core-workflowが不整合/不完全
��   → core-workflow-generation.md Step 6再実行
│   → ステージ記述にドメイン言語を反映
│   → 再実行後、integration-validationから再検証
│
└── 3+次元が同時FAIL（系統的品質問題）
    → DESIGN phaseに戻り、影響のある設計決定を修正
    → 修正後、GENERATION phase全体を再実行
```

### Pitfall 5: スキル化時のコンテンツ圧縮

- **Description**: 26ファイル・数千行のポリシーセットをスキル形式に変換する際の情報損失
- **Prevention**: 二層アーキテクチャ（次セクション「5. スキル化アーキテクチャ」参照）
- **Detection**: スキル化前後の11品質次元スコア比較

---

## 5. スキル化アーキテクチャ（ターゲット構造）

### 5.1 プラグインファイルツリー

```
steering-policy-maker/
├── .claude-plugin/
│   ├── plugin.json                    # プラグインメタデータ
│   └── marketplace.json               # マーケットプレイス公開メタデータ
├── agents/
│   ├── spm-orchestrator.md            # メインオーケストレーター
│   ���── domain-researcher.md           # DISCOVERY: ドメイン調査
���   ├── policy-generator.md            # GENERATION: ポリシー生成
│   └── quality-reviewer.md            # REFINEMENT: 品質検証
��── skills/
│   ├── steering-policy-creation/
│   │   └── SKILL.md                   # エントリポイント（~120行）
│   ├── domain-research/
���   │   └── SKILL.md                   # ドメイン調査パターン（~90行）
│   ├── quality-calibration/
│   │   └��─ SKILL.md                   # 11次元品質評価（~100行）
│   └── policy-templates/
│       └── SKILL.md                   # 出力構造パターン（~80行）
├── commands/
│   ├── new-policy.md                  # /spm:new-policy <agent-description>
│   ├── resume-policy.md               # /spm:resume-policy
│   └── validate-policy.md             # /spm:validate-policy <path>
├── rules/
│   └── spm-standards.md               # 品質基準（全エージェント共通）
└── steering-rule-details/             # 詳細ルール（SKILL.mdから参照）
    ├── common/                        # 10ファイル（既存構造維持）
    ├── discovery/                     # 4ファイル
    ├── design/                        # 4ファイル
    ├── generation/                    # 4ファイル
    └── refinement/                    # 3ファイル
```

### 5.2 設計判断の根拠

| 決定 | 根拠 |
|------|------|
| 4専門エージェント | ECCパターン準拠。各エージェントを独立呼び出し可能。コンテキストウィンドウ圧力を軽減 |
| SKILL.md + rule-details/ 二層構造 | Pitfall 5（圧縮時の情報損失）を回避。SKILL.mdはコンパクトなエントリポイント、rule-details/が全深度を保持 |
| 3コマンド | new-policy（主ワークフロー）、resume-policy（セッション継続）、validate-policy（スタンドアロン検証） |
| `strict: false` | ユーザーが選択的にインストール可能（例: quality-calibrationスキルのみ） |
| rules/ディレクトリ | 全SPMエージェント作業に品質基準を強制。ECCのcoding-standardsと同パターン |

### 5.3 コンテキストウィンドウ予算概算

| コンポーネント | 推定行数 | 推定トークン |
|-------------|---------|------------|
| SKILL.md（エントリポイント） | ~120行 | ~2,000 |
| 該当フェーズのrule-details（4ファイル） | ~1,000行 | ~15,000 |
| common rule-details（必要分） | ~500行 | ~7,500 |
| 状態ファイル + 監査ログ | ~200行 | ~3,000 |
| **合計（1フェーズ実行時）** | **~1,820行** | **~27,500** |

1Mコンテキストウィンドウの~2.7%。十分な余裕あり。全ファイル一括読み込み（~4,500行、~67,500トークン）でも~6.7%。

---

## 6. 自動テスト・検証フレームワーク設計

### 6.1 テストレベル

| レベル | テスト対象 | 実装方式 |
|--------|---------|---------|
| **構造テスト** | 参照整合性、フロー完全性、ファイル存在 | Markdownパーサー + 相互参照グラフ検証 |
| **コンテンツテスト** | ドメイン特化率、具体例カバレッジ、品質次元 | テキスト分析 + チェックリスト照合 |
| **動作テスト** | 生成ポリシーでサンプルエージェントを駆動 | Claude Code + テストシナリオ実行 |

### 6.2 テストケース例

**構造テスト**:
- [ ] core-workflow.mdの全ファイル参照が実在するファイルに解決する
- [ ] 全ワークフローパスが有効なエンドポイントに到達する（デッドエンドなし）
- [ ] CONDITIONAL ステージに Execute IF / Skip IF が明記されている
- [ ] 全Phase Ruleファイルが Completion Message セクションを持つ

**コンテンツテスト**:
- [ ] 各Phase Ruleファイルのドメイン特化率 ≥ 40%
- [ ] 各Phase Ruleファイルにドメイン固有の例 ≥ 2個
- [ ] エラーハンドリングがdomain-research由来のピットフォールを ≥ 50%参照
- [ ] 用語集にドメイン固有用語が ≥ 10語（Common Misuse列含む）

**動作テスト**:
- [ ] サンプル入力「コードレビューエージェント」でDISCOVERY完走
- [ ] サンプル入力「医療コンプライアンスエージェント」でComprehensive深度が選択される
- [ ] セッション中断→再開で状態ファイルから正しく復帰

---

## 7. Domain Terminology

| Term | Definition | Context | Related Terms | Common Misuse |
|------|-----------|---------|---------------|---------------|
| **SPM** | Steering Policy Maker。他のAIエージェント用ポリシーを生成するメタエージェント | システム全体の呼称 | ステアリングポリシー、メタエージェント | 「SPM」を生成されたポリシー自体の意味で使う |
| **AI-DLC** | AI-Driven Development Life Cycle。品質ベンチマークのリファレンス実装 | 品質基準の参照先 | 11品質次元、SDLC | SDLCと混同（AI-DLCはAI固有） |
| **Inception** | DISCOVERY + DESIGN フェーズの総称（RUP由来） | フェーズグループの呼称 | DISCOVERY, DESIGN | 「DISCOVERY」のみを指すと誤解 |
| **Construction** | GENERATION + REFINEMENT フェーズの総称（RUP由来） | フェーズグループの呼称 | GENERATION, REFINEMENT | 「GENERATION」のみを指すと誤解 |
| **Quality Gate** | スキル・ポリシーの納品前品質チェックリスト | 各ステージ完了時のチェック | チェックポイント、承認ゲート | 承認ゲート（ユーザー承認）と混同 |
| **ドメイン特化率** | 生成コンテンツ中のドメイン固有行÷総行（目標>40%） | 品質測定指標 | コンテンツ深度 | 行数比ではなくファイル数比と誤解 |
| **弱点検出ループ** | GENERATION時のコンテンツ品質自動検証→補強サイクル | GENERATION改善パターン | フィードバックループ | 単なるバリデーションと混同 |
| **プラグイン** | エージェント・スキル・フック・コマンドをまとめた配布単位 | マーケットプレイス配布 | スキル、パッケージ | スキル（単一知識モジュール）と混同 |
| **SKILL.md** | スキルの必須エントリポイントファイル。フロントマター+コンテンツ | スキル形式の中核 | plugin.json | core-workflow.mdと混同 |
| **marketplace.json** | マーケットプレイス配布用メタデータ | 配布・発見に使用 | plugin.json | plugin.json（内部構造）と混同 |

---

## 8. Reference Implementations（分析付き）

### 8.1 Everything Claude Code (ECC) — 構造分析

- **規模**: 150+スキル、38エージェント、68コマンド
- **パス**: `~/.claude/plugins/cache/everything-claude-code/everything-claude-code/1.9.0/`

**SPMが学ぶべき設計決定**:
1. **スキルの自律性**: 各SKILL.mdが単独で機能する。前提知識を要求しない。SPMのrule-detailsファイルは相互依存が強いため、SKILL.mdレイヤーで自律性を確保する必要がある
2. **agents配列の明示**: plugin.jsonでエージェントをフルパスで列挙。スキルはディレクトリ参照。SPMも同パターンを採用
3. **選択的インストール**: `strict: false` + manifest-driven pipeline。SPMのquality-calibrationスキルだけを単独利用可能にする

### 8.2 Oh-My-Claude Code (OMC) — オーケストレーション分析

- **規模**: 32スキル、28エージェント
- **パス**: `~/.claude/plugins/cache/omc/oh-my-claudecode/4.10.2/`

**SPMが学ぶべき設計決定**:
1. **ブリッジベースMCPサーバー**: 外部ツール連携をMCPで抽象化。SPMのドメインリサーチ時のツール連携に応用可能
2. **インテリジェントモデルルーティング**: タスク複雑度に応じてモデルを切り替え。SPMのDISCOVERY（Opus推奨）とGENERATION（Sonnet可）の使い分けに参考

### 8.3 高品質スキルベンチマーク Top 5

| スキル | 行数 | なぜ高品質か | SPMへの示唆 |
|--------|------|------------|------------|
| frontend-slides | 184+330 | 7ステップワークフロー、コンテンツ密度制限テーブル、アンチパターン6例、外部プリセットファイル分離 | マルチファイル構造の参考。rule-details/分離の根拠 |
| documentation-lookup | 90 | MCP呼び出しの4ステップ明示、呼出回数制限（3回まで）、フォールバック定義 | ツール連携パターンの参考。ドメインリサーチ手順に適用 |
| market-research | 75 | 5つのリサーチ基準、エビデンス重視、反論含む要件 | 品質キャリブレーションのエビデンス要件に適用 |
| article-writing | 85 | 声紋キャプチャワークフロー、禁止フレーズリスト、6次元スタイル分析 | ドメイン固有パターン抽出手法の参考 |
| content-engine | 88 | プラットフォーム別適応、5コアルール、リパーパスフロー | エージェントタイプ別適応パターンの参考 |

---

## 9. Knowledge Gaps

| # | Gap | 重要度 | なぜ重要か | 暫定対応 |
|---|-----|--------|---------|---------|
| 1 | マーケットプレイス公開審査プロセスの詳細 | Medium | 公開前に満たすべき品質基準が不明 | ECC/OMCのREADME/CONTRIBUTINGを参考に自主基準を設定 |
| 2 | 自動アップデート時の破壊的変更の取り扱い | High | ユーザーの既存ポリシーとの互換性維持 | セマンティックバージョニング採用、CHANGELOG.md必須化 |
| 3 | コンテキストウィンドウ実測値 | Low | 概算済み（~27,500トークン/フェーズ）、実測で確認が望ましい | 概算値で十分。実装後に実測して調整 |
| 4 | 生成ポリシーの動作テスト方法論 | High | サンプルエージェント実行の具体的手順が未確立 | DESIGN phaseで詳細設計。テストシナリオ3パターンを定義済み |
| 5 | 競合・代替アプローチの調査 | Medium | 他のメタエージェント/ポリシー生成システムとの比較が未実施 | SPM自体がリファレンス実装の立場。差別化よりも品質に注力 |
