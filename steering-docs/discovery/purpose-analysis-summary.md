# Purpose Analysis Summary

## Target Agent
- **Name**: Steering Policy Maker (SPM)
- **Description**: 目的別AIエージェント用のステアリングポリシーを自動生成するメタエージェント。4フェーズのワークフロー（DISCOVERY → DESIGN → GENERATION → REFINEMENT）を通じて、AI-DLC品質基準に基づくポリシーセットを作成する。
- **Task Type**: ブラッシュアップ（既存ポリシーの改善）

## Agent Classification
- **Type**: Process Agent
- **Confidence**: High
- **Reasoning**:
  - 4フェーズの逐次ワークフロー（DISCOVERY → DESIGN → GENERATION → REFINEMENT）
  - 15ステージ、各ステージにチェックポイントと承認ゲート
  - 複雑な状態管理（steering-state.md, audit.md）
  - Plan → Approve → Execute → Verifyパターンに完全合致

## Core Capabilities

### Primary
- ユーザーの目的からターゲットエージェントを分析・分類
- ドメイン調査と利害関係者の特定
- ワークフローアーキテクチャの設計
- ルールファイル群の完全生成
- AI-DLC 11品質次元に基づく品質検証

### Secondary
- セッション継続（状態ファイルによる中断・再開）
- 適応的深度（Minimal/Standard/Comprehensive）
- 質問ファイルによる構造化された対話
- 矛盾・曖昧性の検出と解消

### Quality
- コンテンツバリデーション（Mermaid、ASCII、Markdown）
- 過信防止メカニズム
- 監査証跡（ISO 8601タイムスタンプ）
- 統合バリデーション（相互参照の完全性）

## Complexity Assessment
- **Overall**: Complex
- **Phases**: 4
- **Stages**: 15（14 ALWAYS + 1 CONDITIONAL）
- **Key Complexity Drivers**:
  - メタエージェント（自身がポリシーを生成するエージェント）
  - 11品質次元の品質保証
  - 4種のエージェントタイプへの適応
  - マルチセッション対応の状態管理

## Domain Profile
- **Domain**: AIエージェント向けステアリングポリシー作成
- **Familiarity**: Specialized（SPM自体がリファレンス実装）
- **Risk Level**: High（生成されたポリシーの品質がターゲットエージェント全体に伝播）

## Existing Policy Set Analysis
- **Current Files**: 26ファイル（`.steering/`配下）
  - core-workflow.md: 506行
  - common/: 10ファイル
  - discovery/: 4ファイル
  - design/: 4ファイル
  - generation/: 4ファイル
  - refinement/: 3ファイル
- **Current Coverage**: 4フェーズ × 15ステージ完全カバー

## Brush-Up Requirements (User Responses)

### 重点領域
- **全体的な品質向上**（全フェーズ・全ステージの見直し）
- **スキル化**: Claude Codeマーケットプレイス対応、自動アップデート機能

### 既知の課題
- **Inception（DISCOVERY/DESIGN）に比べてConstruction（GENERATION/REFINEMENT）の品質が低い**
- リサーチ能力の向上が必要
- 設計力・実装力の向上が必要
- 既存インストール済みスキルの品質水準を目標とする

### 期待する改善効果
- より高品質なポリシー生成（品質向上）
- より多様なエージェントタイプへの対応（対応範囲拡大）

### 追加機能
- 自動テスト・検証機能（生成ポリシーの動作検証）

### プラットフォーム
- Claude Code（現状維持）

## Key Insights
1. **品質格差の解消**: DISCOVERY/DESIGNフェーズの品質をGENERATION/REFINEMENTにも反映する必要あり
2. **スキル化**: `.steering/`ディレクトリ構造からClaude Codeスキル形式への変換が必要
3. **マーケットプレイス**: 配布・自動アップデートの仕組みが必要
4. **自動検証**: 生成したポリシーの品質を自動的にテストする仕組みが必要
5. **ベンチマーク**: 既存の高品質スキル群を参考にした品質基準の引き上げ
