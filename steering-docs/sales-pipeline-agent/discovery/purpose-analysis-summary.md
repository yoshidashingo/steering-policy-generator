# Purpose Analysis Summary

## Target Agent
- **Name**: sales-pipeline-agent
- **Description**: 営業パイプライン管理エージェント。Salesforce・Gmail・Google Calendar・Slackの4ソースを定期巡回し、対応が必要な案件を検出。低リスクアクションは自動実行、高リスクは提案・承認制で、対応件数が0件になるまでループする自律型Hybridエージェント。

## Agent Classification
- **Type**: Hybrid Agent
- **Confidence**: High
- **Reasoning**: 巡回（Surveillance）→ 状況把握（Assessment）→ 並行ディスパッチ（Dispatch）→ ループ（Loop until done）のパターン。時間帯に応じたモード切り替え（日中巡回 / 夜間準備）、アクション種別に応じた自律度切り替え（自動実行 / 承認制）の2軸Hybrid。

### 4-Type Score
| Type | Score | Reasoning |
|------|-------|-----------|
| Process | 2 (Partial) | 巡回→対応のサイクルはプロセス的だが、固定フェーズではなくループ型 |
| Task | 1 (Weak) | 個別アクションはタスク的だが、全体は巡回・判断を含む |
| Analytical | 2 (Partial) | データ統合・停滞検出・予測は分析的だが主機能ではない |
| Hybrid | 3 (Strong) | 巡回/対応/レポートのモード切り替え、自律度の動的変更が明確にHybrid |

## Core Capabilities

### Primary
- 4ソース定期巡回（Salesforce / Gmail / Google Calendar / Slack）と統合ビュー構築
- 対応要否の判定（停滞商談 / 未返信 / ミーティング準備 / 金額変動 / クロージング期限接近）
- 未追跡リードの検出・追跡提案
- ステータス未反映の検出・更新提案

### Secondary
- 低リスクアクションの自動実行（Salesforceステータス更新、リマインダー送信）
- 日次サマリー生成（当日対応状況 + 翌日優先タスクリスト）
- 夜間の翌日準備タスク自動実行

### Quality
- 承認制による高リスクアクションの安全弁（メール送信、商談変更）
- 巡回結果の監査ログ（何を検出し、何を実行/提案したか）

### Interaction
- 対応提案の通知（Slack or メール）
- 承認リクエストの送信・承認結果の受信
- 日次サマリーの配信

### Error
- データソース接続失敗時のフォールバック（部分巡回続行）
- Salesforce API制限到達時の待機・リトライ
- 誤ったステータス更新の検出・ロールバック

## Complexity Assessment
- **Overall**: Complex
- **Estimated Phases**: 4-5
- **Estimated Stages**: 14-18
- **Key Complexity Drivers**:
  - 4データソースの統合巡回（OAuth 4系統の認証管理、トークンリフレッシュ）
  - 時間帯別モード切り替え（日中1h巡回 / 夜間準備）の状態遷移
  - 自律度の動的変更（自動実行 / 承認制）— 2軸の組み合わせ管理
  - 並行ディスパッチ + ループ制御（同一商談への同時更新の排他制御）
  - Salesforce APIレート制限への対応
  - 4ソース間のデータ整合性保証（矛盾検出ロジック）
- **Complexity Upgrade Reasoning**: 4データソース統合・OAuth管理・並行排他制御・ループ終了条件設計が相互に絡み、Standard では過小評価

### ループ制御設計（Red Team M2 fix）
- **最大ループ回数**: 1巡回サイクルあたり最大5ループ
- **タイムアウト**: 1サイクルの最大処理時間 45分（次の巡回開始まで15分のバッファ）
- **巡回中の新規検出**: 現サイクルの対応リストには追加せず、次サイクルへ繰り延べ
- **承認待ちタイムアウト**: 承認リクエスト送信後30分で自動リマインダー、2時間で次サイクルへ繰り延べ（ブロックしない）

## Domain Profile
- **Domain**: B2B SaaS営業 / パイプライン管理
- **Familiarity**: Well-known
- **Risk Level**: Medium-High（商談データの誤更新、顧客メール誤送信、APIレート制限による巡回中断）
