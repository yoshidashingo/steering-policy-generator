# Common Rules Design

## Common Rules 評価マトリクス

| # | ファイル | 採用 | Adaptation Level | Key Adaptation Points |
|---|---------|------|-----------------|----------------------|
| 1 | `welcome-message.md` | YES | Heavy | パイプライン管理エージェント紹介、4ソース統合巡回の説明、日中/夜間モードの説明、対応検出7種の一覧 |
| 2 | `process-overview.md` | YES | Heavy | 5フェーズ（SURVEILLANCE→ASSESSMENT→DISPATCH→REPORTING + PACKAGING）のフロー図、ループ制御説明 |
| 3 | `question-format-guide.md` | YES | Light | 形式はSPM準拠。承認リクエストの質問テンプレート（商談名・推奨アクション・理由・承認/却下の選択肢）を追加 |
| 4 | `content-validation.md` | YES | Medium | Salesforceレコード更新バリデーション、承認フローの入力チェック、APIレスポンス検証、矛盾検出フラグの形式 |
| 5 | `session-continuity.md` | YES | Heavy | 巡回サイクル単位の状態管理（ループカウント、タイムアウト、承認待ちキュー、API使用量を追跡） |
| 6 | `error-handling.md` | YES | Heavy | 5 Pitfalls対応: CRMデータ鮮度劣化/過度な自動化/通知疲れ/API制限/データソース間不整合 + ロールバックエラー |
| 7 | `overconfidence-prevention.md` | YES | Medium | 停滞判定の過信防止（多層判定なしでの判定禁止）、自動実行/承認制境界の保守的判定、商談金額・ステージ変更への慎重な姿勢 |
| 8 | `terminology.md` | YES | Heavy | Domain Research 12用語（パイプライン/商談/リード/ステージ/クロージング日/アクティビティ/加重パイプライン/BANT/フォーキャスト/SFA/ガバナーリミット/パイプラインカバレッジ）+ エージェント固有用語 |
| 9 | `quality-standards.md` | YES | Medium | 15品質次元をパイプライン管理ドメインに適応 |
| 10 | `output-structure-patterns.md` | YES | Medium | 日次サマリー/承認リクエスト/自動実行通知/ロールバック通知/監査ログのテンプレート |
| 11 | `action-authority.md` | YES | N/A（新規） | 自動実行/承認制の境界定義（低リスク/高リスクの分類基準、例外ケース、承認フロータイムアウトルール） |
| 12 | `data-source-config.md` | YES | N/A（新規） | 4ソースの接続設定・OAuth管理・APIレート制限・フォールバック設定 |

**Total: Standard 10ファイル + Domain-specific 2ファイル = 12ファイル**

## Domain-Specific Common Rules

### `action-authority.md`（新規 — Pitfall 2対策の中核）

**Purpose**: 自動実行/承認制の境界を明確に定義し、エージェントが誤って高リスクアクションを自動実行しないよう制約する

**Adaptation Level**: N/A（パイプライン管理ドメイン固有のためゼロから作成）

**Content Outline**:

#### 低リスクアクション（自動実行可）
以下のアクションは承認なしで自動実行する:

| アクション | 対象 | 条件 |
|-----------|------|------|
| Salesforce Activityログ記録 | Gmail送受信、カレンダーミーティング実施、Slack言及 | 記録漏れを検出した場合 |
| 内部リマインダー送信 | 担当営業担当者（Slack DM） | 承認リクエスト30分タイムアウト時、会議24時間前 |
| ステージの前進更新 | Salesforce商談 | Prospecting→Qualification等、後退でない場合のみ |
| 会議準備サマリー生成 | Salesforceレコード + Gmail + 過去議事録 | ミーティング24時間前 |
| パイプラインヘルスレポート生成 | 読み取り専用 | 日次サマリー・週次分析 |
| 重複チケット/リードの検出通知 | 読み取り専用 | 重複候補の提示のみ（マージは承認制） |

#### 高リスクアクション（承認制）
以下のアクションは必ず承認を得てから実行する:

| アクション | 対象 | リスク根拠 |
|-----------|------|-----------|
| 顧客へのメール送信 | Gmail経由の外部送信 | 顧客との関係に直接影響。誤送信は信頼毀損（Pitfall 2） |
| 商談金額の変更 | Salesforce Opportunityの金額フィールド | フォーキャスト・売上報告への影響。誤変更はビジネス判断を誤らせる |
| ステージの後退 | Salesforce Opportunityのステージフィールド | 受注確率・優先度・担当者モチベーションへの影響 |
| 商談クローズ（Won/Lost） | Salesforce Opportunity | 売上計上・KPI・コミッションへの影響 |
| 新規リードの商談化 | SalesforceリードからOpportunityへの変換 | 誤変換は営業プロセスの歪み |
| 既存レコードの削除 | SalesforceオブジェクトのDELETE操作 | 復元不可のデータ損失リスク |
| 重複レコードのマージ | Salesforceリード/商談のマージ | データ損失・履歴喪失のリスク |
| Salesforceカスタムフィールドの変更 | 標準外フィールド | 業務プロセスへの予期しない影響 |

#### 例外ケース
- 夜間モード中の顧客向けアクション: 承認済みであっても翌営業日8:00まで実行を保留する
- 承認者が不在（2時間タイムアウト）の場合: 承認済みアクションも翌サイクルに繰り延べる
- ロールバック判定: 誤実行が検出された自動実行アクションはロールバックを優先し、事後に担当者へ通知する

#### 承認フローのタイムアウトルール
```
承認リクエスト送信
  → 30分後: 自動リマインダー送信（Slack DM）
  → 2時間後: 未承認の場合、そのアクションを次サイクルへ繰り延べ
  → 繰り延べ後: 次サイクルのASSESSMENTフェーズで再評価（アクションの必要性が変化している可能性）
```

#### GOOD/BAD例
- **GOOD**: ミーティング後にSalesforceへActivityログを自動記録した。顧客へのフォローアップメールは承認リクエストとして担当者に送付した
- **BAD**: 承認2時間タイムアウト後も待機し続けてエージェントが停止した。次サイクルへの繰り延べ処理を行わなかった

---

### `data-source-config.md`（新規 — Pitfall 4・Pitfall 5対策の中核）

**Purpose**: 4データソースの接続設定、OAuth管理、APIレート制限対応、フォールバック設定を定義する

**Adaptation Level**: N/A（パイプライン管理ドメイン固有のためゼロから作成）

**Content Outline**:

#### Salesforce接続設定
```
認証方式: OAuth 2.0 Connected App
スコープ: api, refresh_token, offline_access
APIバージョン: v59.0以上（Bulk API 2.0対応）

ガバナーリミット管理（Pitfall 4対策）:
  - 日次API呼び出し上限: 組織別（Enterprise: 150,000/日）
  - 同時接続数上限: 10
  - クエリ行数上限: SOQL 1クエリあたり50,000行
  - DML操作上限: 1トランザクションあたり150件

使用量モニタリング:
  - 使用率確認: 毎サイクル開始時（S1: Schedule Check）
  - 使用率 >= 80%: セーフモード（巡回頻度を2時間に延長）
  - 使用率 >= 95%: 読み取り専用モード（DML操作停止）
  - アラート: 使用率80%超で管理者にSlack通知

バルクAPI活用:
  - 大量レコード（50件以上）の一括取得はBulk API 2.0を使用
  - SOQLクエリは必要フィールドのみSELECT（SELECT *は禁止）
  - リトライ間隔: 指数バックオフ（初回1秒、最大30秒、最大5回）
```

#### Gmail接続設定
```
認証方式: Google OAuth 2.0
スコープ: gmail.readonly（読み取り専用 — 送信は別スコープで明示制御）
送信用スコープ: gmail.send（承認済みアクション実行時のみ使用）

レート制限:
  - 1ユーザーあたり: 250 quota units/秒
  - 1日あたり: 1,000,000 quota units
  - 対応: バッチ処理でリクエストを集約

データ取得範囲:
  - 差分取得: 前回巡回タイムスタンプ以降のメッセージのみ
  - 取得対象: 送受信メール（Subject, From, To, Date, Body snippet）
  - 除外: 社内メールの除外フィルター（設定可能）
```

#### Google Calendar接続設定
```
認証方式: Google OAuth 2.0
スコープ: calendar.readonly
レート制限: 1,000,000 queries/day（ユーザーあたり）

データ取得範囲:
  - 取得期間: 過去7日〜今後14日
  - 取得対象: 外部参加者を含むミーティング（顧客ミーティング）
  - 紐づけルール: 参加者メールアドレスとSalesforceリード/コンタクトの一致判定
```

#### Slack接続設定
```
認証方式: Slack OAuth 2.0（Bot Token）
スコープ: channels:read, channels:history, im:write（DM送信）

データ取得範囲:
  - 監視対象チャンネル: 設定ファイルで指定（例: #sales-general, #deal-updates）
  - 取得期間: 前回巡回タイムスタンプ以降のメッセージ
  - キーワードフィルター: 商談名・顧客名・担当者名での絞り込み

通知送信先:
  - 担当者個人DM: 承認リクエスト・リマインダー・緊急アラート
  - チームチャンネル: 日次サマリー
```

#### フォールバック設定（Pitfall 4・Pitfall 5対策）
```
部分障害時の動作:
  - Salesforce障害: 巡回中断。Gmail/Calendar/Slackのみで停滞外の検出は続行。障害をレポートに明記
  - Gmail障害: 未返信検出をスキップ。他3ソースで巡回続行
  - Calendar障害: 会議準備検出をスキップ。他3ソースで巡回続行
  - Slack障害: Slack由来のアクティビティ検出をスキップ。多層判定でSlackスコアを0として扱う

フォールバック優先度:
  1. Salesforce: 商談データのマスターソース。障害時は承認制アクションを停止し読み取り専用で続行
  2. Gmail: 未返信検出に必須。障害時は該当検出カテゴリをスキップ
  3. Google Calendar: 会議準備に必須。障害時は該当検出カテゴリをスキップ
  4. Slack: 多層判定の補助ソース。障害時はスコア0で代替

全ソース障害: 巡回をスキップし、障害通知を送信（最終通知チャンネルに別途設定）
```

#### トークンリフレッシュ管理
```
- 各ソースのアクセストークン有効期限を事前にチェック（S2: Source Connection）
- 有効期限30分以内: リフレッシュトークンでアクセストークンを更新
- リフレッシュトークン失効: 障害扱いとしてフォールバック処理へ移行し、管理者にアラート
- トークン情報は暗号化して保管（監査ログへのトークン記録は禁止）
```

#### GOOD/BAD例
- **GOOD**: Salesforce API使用率が82%に達したためセーフモードに切り替え、巡回間隔を2時間に延長してガバナーリミット超過を回避した
- **BAD**: Calendar APIが障害中にもかかわらず会議準備サマリーの生成を試み続け、タイムアウトで巡回全体が停止した

---

## Standard Common Rules の Adaptation 詳細

### `welcome-message.md`（Heavy Adaptation）

**Key Adaptation Points**:
- エージェント名・役割: 「営業パイプライン管理エージェント。Salesforce/Gmail/Google Calendar/Slackを定期巡回し、対応が必要な商談を検出・対応する」
- 2モードの説明: 日中巡回モード（1時間サイクル）と夜間準備モードの違い
- 2段階自律度の説明: 低リスク自動実行と高リスク承認制の区別
- 対応検出7種の一覧と、各検出に対するエージェントの行動概要

### `process-overview.md`（Heavy Adaptation）

**Key Adaptation Points**:
- 5フェーズ構成のフロー説明（SURVEILLANCE→ASSESSMENT→DISPATCH→REPORTING + PACKAGING）
- ループ制御の説明（最大5ループ、45分タイムアウト）
- 承認フロータイムアウト（30分リマインダー、2時間繰り延べ）のユーザー向け説明
- Pitfall 2対策として自動実行と承認制の違いを明示

### `session-continuity.md`（Heavy Adaptation）

**Key Adaptation Points**:
- セッション単位はチケット単位ではなく「巡回サイクル単位」
- 状態追跡項目: ループカウント、タイムアウト経過時間、承認待ちキュー、API使用量、ソース接続状態
- 繰り延べリスト管理: 現サイクルで対応できなかったアクションを次サイクルに引き継ぐ仕組み
- ロールバック状態管理: 自動実行前の事前状態を保持する

### `error-handling.md`（Heavy Adaptation）

**Key Adaptation Points — 5 Pitfalls対応**:

| エラーシナリオ | Pitfall対応 | 対処法 |
|-------------|-----------|--------|
| CRMデータ鮮度劣化検出（最終更新が30日超） | Pitfall 1 | 担当者に更新リマインダー送信、停滞判定時は多層判定（Gmail/Slack）を必須化 |
| 自動実行後に予期しない変更を検出 | Pitfall 2 | ロールバックAPIを即時実行し担当者通知、自動実行ロジックの見直しフラグ |
| 承認リクエストの応答率が低下（Pitfall 3）| Pitfall 3 | 通知集約率を上げ、重複通知を抑制、日次ダイジェスト移行オプションを提案 |
| Salesforce APIガバナーリミット接近 | Pitfall 4 | セーフモード移行、バルクAPI切り替え、管理者アラート、巡回頻度低下 |
| ソース間データ矛盾検出 | Pitfall 5 | 矛盾フラグを付与しAssessment時に人間判断を促す、自動判定を保留 |
| Salesforce接続失敗（OAuth失効） | — | トークンリフレッシュ試行→失敗時は管理者アラート、次サイクルまで当該ソースをスキップ |
| ロールバック失敗 | — | 管理者緊急通知、手動修正ガイドを提示、該当アクションの自動実行を一時停止 |

### `terminology.md`（Heavy Adaptation）

**追加エージェント固有用語**:
| 用語 | 定義 | 備考 |
|------|------|------|
| 巡回サイクル | エージェントが4ソースを一巡する処理単位。日中は1時間間隔 | ループと混同注意 — ループは1サイクル内の反復 |
| ヘルススコア | 商談の健全性を0-100で数値化したスコア。停滞度・エンゲージメント・期限リスクを複合評価 | 単一指標（例: ステージ滞留日数）のみでの判定は禁止 |
| 多層判定 | Salesforceの状態に加え、Gmail/Calendar/Slackのアクティビティを加味した停滞判定 | Pitfall 5対策の核心 |
| 繰り延べ | 現サイクルで対応できなかったアクションを次サイクルに先送りすること | タイムアウト、ループ上限到達、承認タイムアウト時に発生 |
| セーフモード | Salesforce API使用率80%超時に切り替わる保守的巡回モード。巡回頻度を2時間に低下 | 読み取り専用モード（95%超）と区別すること |

## Common Rules Summary

| ファイル | 推定行数 | Adaptation Level |
|---------|---------|-----------------|
| welcome-message.md | 80-120 | Heavy |
| process-overview.md | 100-150 | Heavy |
| question-format-guide.md | 60-100 | Light |
| content-validation.md | 100-150 | Medium |
| session-continuity.md | 100-150 | Heavy |
| error-handling.md | 180-230 | Heavy |
| overconfidence-prevention.md | 80-120 | Medium |
| terminology.md | 120-180 | Heavy |
| quality-standards.md | 120-180 | Medium |
| output-structure-patterns.md | 100-150 | Medium |
| action-authority.md（新規） | 150-200 | N/A |
| data-source-config.md（新規） | 180-230 | N/A |
| **合計** | **~1,370-1,760** | |
