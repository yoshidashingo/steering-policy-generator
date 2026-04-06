# Scope Definition

## Target Agent Summary
- **Name**: sales-pipeline-agent
- **Type**: Hybrid Agent
- **Domain**: B2B SaaS営業 / パイプライン管理
- **Complexity**: Complex

## Scope Boundaries

### In Scope
| Area | Coverage | Notes |
|------|----------|-------|
| Workflow | 巡回→状況把握→対応判定→ディスパッチ→ループ→レポート | 日中巡回+夜間準備の2モード |
| Data Sources | Salesforce / Gmail / Google Calendar / Slack の4ソース統合巡回 | OAuth 4系統管理含む |
| Actions | 低リスク自動実行（SF更新、リマインダー）+ 高リスク承認制（メール、商談変更） | 2段階自律度 |
| Detection | 停滞商談 / 未返信 / 会議準備 / 金額変動 / 期限接近 / 未追跡リード / ステータス未反映 | 7種の検出ルール |
| Reporting | 日次サマリー（対応状況 + 翌日タスク） | 毎日配信 |
| Quality | 15品質次元、監査ログ、ロールバック機能 | |

### Out of Scope
- **週次/月次パイプラインレポート**: 日次サマリーのみ（将来拡張候補）
- **顧客への直接メール自動送信**: 承認制のため完全自律は対象外
- **Salesforceカスタムオブジェクトの動的対応**: 標準オブジェクト（Lead/Opportunity/Account/Contact/Activity）のみ
- **マルチチーム/マネージャー階層対応**: 単一チーム想定（将来拡張候補）
- **リアルタイム監視**: 1h間隔の定期巡回（リアルタイムWebhookは対象外）

## Estimated Structure

### Phase Structure
- **Phases**: 5（SURVEILLANCE, ASSESSMENT, DISPATCH, REPORTING, PACKAGING）
- **Operational Stages**: 16（ALWAYS: 12, CONDITIONAL: 4）
- **Build-Time Stages**: 2
- **Common Rule Files**: 12（standard 10 + domain-specific 2: action-authority.md, data-source-config.md）
- **Phase Rule Files**: 18（SURVEILLANCE 4 + ASSESSMENT 4 + DISPATCH 5 + REPORTING 3 + PACKAGING 2）
- **Total Files**: ~32（core-workflow 1 + common 12 + phase 18 + plugin ~1）
- **Estimated Total Lines**: ~5,000-7,000

### Phase 1: SURVEILLANCE（巡回）
- S1: Schedule Check（巡回タイミング判定）(ALWAYS)
- S2: Source Connection（4ソース接続・認証）(ALWAYS)
- S3: Data Collection（データ収集・統合ビュー構築）(ALWAYS)
- S4: Change Detection（変更・異常検出）(ALWAYS)

### Phase 2: ASSESSMENT（状況判定）
- A1: Deal Health Scoring（商談ヘルススコア算出）(ALWAYS)
- A2: Action Identification（対応アクション特定）(ALWAYS)
- A3: Priority Ranking（アクション優先度付け）(ALWAYS)
- A4: Authority Classification（自動実行/承認制の判定）(ALWAYS)

### Phase 3: DISPATCH（対応実行）
- D1: Auto-Execute（低リスクアクション自動実行）(CONDITIONAL — 自動実行対象がある場合)
- D2: Approval Request（高リスクアクション承認リクエスト）(CONDITIONAL — 承認制対象がある場合)
- D3: Approval Processing（承認結果の処理・実行）(CONDITIONAL — 承認済みアクションがある場合)
- D4: Loop Control（ループ判定：残件チェック、タイムアウト判定）(ALWAYS)
- D5: Rollback Check（実行結果の検証・ロールバック判定）(ALWAYS)

### Phase 4: REPORTING（レポート）
- R1: Daily Summary Generation（日次サマリー生成）(ALWAYS)
- R2: Next-Day Prep（翌日準備タスク生成・実行）(CONDITIONAL — 夜間モード時)
- R3: Audit Log Finalization（監査ログ確定・保存）(ALWAYS)

### Phase 5: PACKAGING（ビルドタイム — 運用フローとは独立）
- P1: Plugin Structure Generation (ALWAYS)
- P2: Automated Validation (ALWAYS)

## Quality Target
- **Level**: Premium
- **15 Dimensions Target**: 15/15 PASS
- **Key Quality Focus Areas**:
  - Dim 12: Domain Specificity Rate >= 40%（営業用語・パイプライン手順）
  - Dim 13: Example Coverage >= 2/file
  - Dim 15: Pitfall Reference Rate >= 50%（5 Pitfalls引用）

## Directory Structure

```text
.sales-pipeline-agent/
├── sales-pipeline-agent-rules/
│   └── core-workflow.md
└── sales-pipeline-agent-rule-details/
    ├── common/
    │   ├── welcome-message.md
    │   ├── process-overview.md
    │   ├── question-format-guide.md
    │   ├── content-validation.md
    │   ├── session-continuity.md
    │   ├── error-handling.md
    │   ├── overconfidence-prevention.md
    │   ├── terminology.md
    │   ├── quality-standards.md
    │   ├── output-structure-patterns.md
    │   ├── action-authority.md
    │   └── data-source-config.md
    ├── surveillance/
    │   ├── schedule-check.md
    │   ├── source-connection.md
    │   ├── data-collection.md
    │   └── change-detection.md
    ├── assessment/
    │   ├── deal-health-scoring.md
    │   ├── action-identification.md
    │   ├── priority-ranking.md
    │   └── authority-classification.md
    ├── dispatch/
    │   ├── auto-execute.md
    │   ├── approval-request.md
    │   ├── approval-processing.md
    │   ├── loop-control.md
    │   └── rollback-check.md
    ├── reporting/
    │   ├── daily-summary-generation.md
    │   ├── next-day-prep.md
    │   └── audit-log-finalization.md
    └── packaging/
        ├── plugin-structure-generation.md
        └── automated-validation.md
```
