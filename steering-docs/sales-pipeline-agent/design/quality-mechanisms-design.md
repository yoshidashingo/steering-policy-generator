# Quality Mechanisms Design

## 15品質次元 — 営業パイプライン管理エージェントへの適応

### Standard Dimensions（Dim 1-11）

| # | 次元 | 営業パイプラインへの適応 | 測定方法 |
|---|------|----------------------|---------|
| 1 | Adaptive Workflow | 16運用ステージ: 12 ALWAYS + 4 CONDITIONAL（D1, D2, D3, R2）。2軸Hybrid（時間帯モード + 自動実行/承認制） | 全ステージにALWAYS/CONDITIONAL分類あり |
| 2 | Mandatory Checkpoints | 8-10 CP: CP-1〜CP-8（以降はオプション追加可）。全CPで2-option completion message | 全CPでREVIEW REQUIRED + WHAT'S NEXT |
| 3 | Question File Format | 承認リクエストのフォーマット（商談名・推奨アクション・理由・[承認する]/[却下する]の選択肢）。SPM準拠 | 全承認リクエストがフォーマット準拠 |
| 4 | Content Validation | Salesforceレコード更新バリデーション、APIレスポンス検証、矛盾検出フラグ形式、承認フロー入力チェック | チェックリスト完備 |
| 5 | Audit Trail | サイクル単位の監査ログ（アクションID/対象レコードID/実行者/承認者/結果/理由）。ISO 8601、append-only | ログ完全性チェック（記録数と実行数の一致） |
| 6 | Error Handling | 7シナリオ: CRMデータ鮮度劣化 / 自動実行後の予期しない変更 / 通知応答率低下 / APIガバナーリミット接近 / ソース間矛盾 / OAuth失効 / ロールバック失敗（5 Pitfalls対応） | 全フェーズ・全severity対応 |
| 7 | Overconfidence Prevention | 停滞判定の過信防止（多層判定なし禁止）、自動実行/承認制境界の保守的判定（境界曖昧→承認制）、商談金額・ステージ変更への慎重な姿勢 | Red flags定義済み |
| 8 | Depth Levels | Minimal（低リスクActivity記録、P4案件）/ Standard（停滞検出・未返信、P3）/ Comprehensive（大型商談・期限接近・金額変動・P1-P2）— A3の優先度で自動選択 | 3レベル+選択基準+ステージ適用ルール |
| 9 | Session Continuity | 巡回サイクル単位の状態ファイル（ループカウント/タイムアウト/承認待ちキュー/API使用量/ソース接続状態/繰り延べリスト） | state file format定義 |
| 10 | Terminology Standardization | 12用語のglossary（パイプライン/商談/リード/ステージ/クロージング日/アクティビティ/加重パイプライン/BANT/フォーキャスト/SFA/ガバナーリミット/パイプラインカバレッジ）+ エージェント固有5用語 | Common Misuse列付き |
| 11 | Standardized Completion Messages | 全ステージにREVIEW REQUIRED + WHAT'S NEXT（2 options: 承認して次へ / 修正してやり直し） | No emergent patterns |

### Domain-Specific Dimensions（Dim 12-15）

| # | 次元 | 測定方法 | 閾値 | 主要ソース |
|---|------|---------|------|---------|
| 12 | Domain Specificity Rate | 営業用語・パイプライン手順・Salesforce操作等のドメイン固有行 ÷ （総行数 - 空行 - 見出し行） | >= 40% | terminology.md, domain-research-summary |
| 13 | Example Coverage | 各Phase Ruleファイル内のGOOD/BADパターン数 | >= 2/file | 全18 Phase Ruleファイルに2例設計 |
| 14 | Artifact Template Completeness | テンプレートを含むべきファイル ÷ 実際にテンプレートがあるファイル | = 100% | D2（承認リクエスト）/ D3（承認結果処理）/ R1（日次サマリー）/ R2（会議準備サマリー）/ R3（監査ログエントリ）/ A1（ヘルススコアレポート）= 6ファイル |
| 15 | Pitfall Reference Rate | error-handling内でDomain Research 5 Pitfallsを引用するアイテム ÷ 全エラーアイテム | >= 50% | Pitfall 1-5: CRMデータ鮮度劣化/過度な自動化/通知疲れ/API制限/データソース間不整合 |

## Checkpoint System

| CP | 後続ステージ | ゲート内容 | 判定基準 |
|----|-----------|----------|---------|
| CP-1 | S1: Schedule Check | スケジュール・時間帯モード・API状態確認 | 時間帯モード確定、API使用率正常（<80%）またはセーフモード移行完了 |
| CP-2 | S4: Change Detection | 変更検出リスト確認 | 7種の検出ルールが正しく適用、多層判定が実施、矛盾フラグが適切に付与されている |
| CP-3 | A4: Authority Classification | アクション分類確認 | 全アクションが自動実行/承認制/保留に分類、境界曖昧ケースが承認制に分類 |
| CP-4 | D3: Approval Processing | 承認済みアクション実行前確認 | 承認内容と実行予定の一致、夜間制約の確認 |
| CP-5 | D5: Rollback Check | ロールバックチェック確認 | 全実行アクションの検証完了、異常なし or ロールバック完了 |
| CP-6 | R1: Daily Summary Generation | 日次サマリー確認 | 通知階層化・重複抑制が適用、CRMデータ鮮度警告が必要な場合は含まれている |
| CP-7 | R2: Next-Day Prep | 翌日準備内容確認（夜間のみ） | 翌日ミーティングサマリー・期限接近アラートの準備完了 |
| CP-8 | P1: Plugin Structure | プラグイン構造確認 | 構造・参照整合性、全32ファイルの存在確認 |

## 3層自動テスト

### Layer 1: Structure Test（構造テスト）

| チェック | 対象 | PASS基準 |
|---------|------|---------|
| ファイル存在確認 | 全32ファイル（core 1 + common 12 + phase 18 + plugin ~1） | 全ファイル存在 |
| 相互参照解決 | core-workflow.md内の全参照 | 100%解決（リンク切れなし） |
| Markdown妥当性 | 全.mdファイル | 見出し階層の飛びなし・リスト・コードブロック正常 |
| フローパス完全性 | 全フェーズの全パス | デッドエンドなし（全パスが終端に到達） |
| ファイル数一貫性 | 全設計ドキュメント間のファイル数 | 全ドキュメントで同一値（18 Phase Rules, 12 Common Rules） |
| action-authority.md参照 | D1, D2, D3, A4 | action-authority.mdへの正しい参照あり |
| data-source-config.md参照 | S2, S3 | data-source-config.mdへの正しい参照あり |

### Layer 2: Content Test（コンテンツテスト）

| チェック | 対象 | PASS基準 |
|---------|------|---------|
| Dim 12: Domain Specificity | 全18 Phase Ruleファイル | >= 40% per file |
| Dim 13: Example Coverage | 全18 Phase Ruleファイル | >= 2 GOOD/BAD per file |
| Dim 14: Artifact Templates | D2, D3, R1, R2, R3, A1 | 100%（6/6ファイルにテンプレートあり） |
| Dim 15: Pitfall Reference | error-handling.mdのセクション | >= 50%（5 Pitfallsのうち少なくとも3つ引用） |
| ループ制御設定値 | D4, S1 | 最大5ループ・45分タイムアウト・30分リマインダー・2時間繰り延べが明記 |
| 承認境界一貫性 | action-authority.md, A4, D1, D2 | 低リスク/高リスクの分類が全ファイルで矛盾なく一致 |
| 多層判定必須化 | S4, A1 | Salesforce単独判定での停滞検出が禁止されていることが明記 |
| 夜間モード制約 | S1, D2, D3, R2 | 夜間に顧客向けアクションを実行しない制約が明記 |
| API使用量対応 | S1, D1 | セーフモード・読み取り専用モードの条件が明記 |
| Completion message format | 全ステージ | 2-option（Request Changes / Continue）形式 |

### Layer 3: Smoke Test（スモークテスト）

| チェック | フロー | PASS基準 |
|---------|-------|---------|
| 日中巡回フロー（正常） | S1→S2→S3→S4→A1→A2→A3→A4→D1→D4→D5→R1→R3 | 全ステージ到達可能、ループ1回で残件0 |
| 夜間準備フロー | S1（夜間）→R2→R1→R3 | SURVEILLANCEをスキップしてR2に直行 |
| 承認フロー（承認） | ...A4→D2→D3（承認）→D4→D5→R1→R3 | 承認後に実行完了、監査ログに承認者記録 |
| 承認フロー（却下） | ...A4→D2→D3（却下）→D4（残件0）→D5→R1→R3 | 却下理由が監査ログに記録、同アクションを再提案しない |
| 承認タイムアウトフロー | ...D2→D4（30分リマインダー）→D4（2時間繰り延べ）→D5→R1→R3 | リマインダー送信後、2時間で繰り延べ処理が完了 |
| ループ上限フロー | ...D4（ループ5回）→D5（残件繰り延べ）→R1→R3 | 5ループ到達で終了、残件が次サイクルに繰り延べ |
| 45分タイムアウトフロー | ...D4（45分経過）→D5（残件繰り延べ）→R1→R3 | タイムアウトフラグ付きで次サイクルに繰り延べ |
| ロールバックフロー | ...D5（異常検出）→ロールバック実行→担当者通知→R1→R3 | ロールバック完了記録が監査ログに残る |
| データソース障害フロー | S2（Gmail障害）→S3（3ソースで続行）→S4（未返信検出スキップ）→... | フォールバック状態がレポートに明記 |
| Salesforce APIセーフモードフロー | S1（使用率82%）→セーフモード移行→S2→S3→S4→...→D1（Bulk API）→D4→D5→R1 | セーフモード移行後、Bulk APIで実行。使用率正常化まで2時間間隔 |
| 検出なしフロー | S4（0件検出）→R1（サマリー生成）→R3 | ASSESSMENTをスキップしてREPORTINGへ直行 |
| 5 Pitfalls検証フロー | 各Pitfall対応ロジックが正しく動作するか | Pitfall 1-5が全てカバーされている |

## Repair Judgment Tree — 全次元マッピング

| 次元 | FAIL分類 | 戻り先 |
|------|---------|--------|
| Dim 1: Adaptive Workflow | Design | Workflow Architecture 再設計 |
| Dim 2: Mandatory Checkpoints | Structural | Core Workflow Gen |
| Dim 3: Question File Format | Content | Common Rules Gen（question-format-guide.md） |
| Dim 4: Content Validation | Content | Common Rules Gen（content-validation.md） |
| Dim 5: Audit Trail | Content | Phase Rules Gen（audit-log-finalization.md） |
| Dim 6: Error Handling | Content | Common Rules Gen（error-handling.md）/ Phase Rules Gen |
| Dim 7: Overconfidence Prevention | Content | Common Rules Gen（overconfidence-prevention.md） |
| Dim 8: Depth Levels | Design | Workflow Architecture 再設計 |
| Dim 9: Session Continuity | Content | Common Rules Gen（session-continuity.md） |
| Dim 10: Terminology | Content | Common Rules Gen（terminology.md） |
| Dim 11: Completion Messages | Structural | Core Workflow Gen |
| Dim 12: Domain Specificity | Content | Phase Rules Gen（該当フェーズのファイル） |
| Dim 13: Example Coverage | Content | Phase Rules Gen（例が不足しているファイル） |
| Dim 14: Artifact Templates | Content | Phase Rules Gen（D2, D3, R1, R2, R3, A1） |
| Dim 15: Pitfall Reference | Content | Common Rules Gen（error-handling.md） |

### Domain固有 Repair シナリオ

| 症状 | Pitfall起因 | 分類 | 戻り先 |
|------|-----------|------|--------|
| 停滞商談の誤検出多発 | Pitfall 1 / Pitfall 5 | Content | change-detection.md（多層判定ロジック強化） |
| 高リスクアクションが自動実行されている | Pitfall 2 | Content | action-authority.md + auto-execute.md 修正 |
| 担当者が通知を無視・対応率低下 | Pitfall 3 | Content | daily-summary-generation.md + approval-request.md（通知集約強化） |
| Salesforce API制限でサイクル中断 | Pitfall 4 | Design | schedule-check.md + loop-control.md（セーフモード閾値見直し） |
| ソース間矛盾による誤判定多発 | Pitfall 5 | Content | data-collection.md（矛盾検出パターン拡充） |
| ロールバック失敗によるデータ不整合 | Pitfall 2 | Content | rollback-check.md（ロールバックAPI呼び出し修正） |

## Loop Control Rules

| ルール | 値 | 根拠 |
|--------|---|------|
| Max loops / cycle | 5 | 1サイクル45分内で対応しきれない件数は次サイクルへ。無限ループ防止 |
| Cycle timeout | 45分 | 1時間サイクルに対して15分バッファを確保。次の巡回開始に干渉しない |
| Approval timeout（リマインダー） | 30分 | 担当者の対応漏れ防止。通知疲れを考慮した適切な間隔（Pitfall 3対策） |
| Approval timeout（繰り延べ） | 2時間 | 承認待ちでエージェントをブロックしない。業務時間内での対応を想定 |
| 新規検出の扱い | 次サイクル繰り延べ | 現サイクル中の対応件数爆発・重複通知を防止（Pitfall 3対策） |
| Max retries（修復ループ） | 3 | コンテキスト爆発防止 |
| Same-target repair limit | 2 | 同一対象への2回連続失敗は問題分類誤りを示唆 |
| P2→P1 loop | 最大2回（別カウンター） | プラグイン修正は限定的 |

## 品質モニタリング指標（運用フェーズ向け）

以下の指標を週次でレビューすることで、エージェントの品質低下を早期検出する:

| 指標 | 測定方法 | 警告閾値 | Pitfall対応 |
|------|---------|---------|-----------|
| 承認却下率 | 却下数 ÷ 承認リクエスト総数 | > 30% | Pitfall 2: 自動判定ロジックの境界設計見直し |
| 通知応答率 | 承認対応数 ÷ 承認リクエスト送信数 | < 60% | Pitfall 3: 通知集約・バッチサイズの調整 |
| 停滞誤検出率 | 担当者による「誤検出」フィードバック ÷ 停滞検出総数 | > 20% | Pitfall 1 / Pitfall 5: 多層判定ロジックの閾値調整 |
| API使用率ピーク | 1日の最大Salesforce API使用率 | > 80% | Pitfall 4: バルクAPI活用・巡回頻度の見直し |
| ソース間矛盾検出率 | 矛盾フラグ付き案件 ÷ 全検出件数 | > 15% | Pitfall 5: 矛盾検出パターンの精度向上 |
| ロールバック発生率 | ロールバック実行数 ÷ 自動実行総数 | > 5% | Pitfall 2: 実行前検証ロジックの強化 |
| サイクルタイムアウト率 | タイムアウト発生サイクル ÷ 総サイクル数 | > 10% | 設計: ループ上限・タイムアウト設定の見直し |
