# DEMO_NOTES — 施設運営AI OS デモ（review/facility-os-improvement）

このデモを安全に運用・更新するためのメモ。最終更新: 2026-06-19。

## 1. デプロイ設計（安全・昇格は承認制）

GitHub Pages は `main` ブランチ直下を配信している。**現行デモを壊さない**ために次の分離で運用する。

| 区分 | 場所 / URL | 方針 |
|---|---|---|
| 現行（本番・会議で使用） | `…/ryokan-ops-demo/`（main 直下） | 触らない。常に動く |
| 新版（レビュー用） | `…/ryokan-ops-demo/preview/`（main 配下の別フォルダ） | ここで目視レビュー。root は無傷 |
| 復元ポイント | git tag `demo-current-snapshot`（= 旧 main `e1f67fe`） | いつでも現行へ戻せる |
| 改善ソース | branch `review/facility-os-improvement` | 正本。レビューOK後に root へ昇格 |

- **昇格（root 更新）は人の承認後に行う。** 自動では行わない。
- 復元: `git checkout demo-current-snapshot -- .`（または該当ファイル）。

## 2. このパスで実施した改善（P0 / P1）

### P0（信頼・プライバシー・デモ信頼性）
- **個人名の匿名化**: 実名・ゲスト名・イニシャル・実在風社名を役割ラベル（支配人A / フロント担当B / 会計担当C / 夜勤担当D / 清掃担当E / 仲居担当 / 保全担当F）と ID（予約ID R-2026-xxxx / ゲスト G-xxxx）へ置換。ヘッダーの「山田太郎」→「支配人A」を全11ファイルで置換。
- **noindex 全ページ付与**: 全 HTML（31ファイル）に `<meta name="robots" content="noindex, nofollow">`。
- **外部依存**: 新規5画面はゼロ依存（後述の TODO は既存画面のみ）。
- **公共施設の防犯カメラ**: 顔認識・個人追跡・常時人物追跡は実装しない。イベント検知のみ（混雑検知 / 立入検知 / 転倒のような動き / 時間外在館 / 設備室扉の開放 / カメラ死活）＋確認ログ。

### P1（製品ストーリー・新画面・既存強化）
- **index.html**: 画面リスト → 3分間の製品ストーリーへ刷新（ヒーロー「施設運営AI OS」／外部を見る・内部を読む・承認して動かす／シナリオボタン4種／グループ分けした画面グリッド）。
- **新規5画面**（共有 `assets/styles.css` を使用・無外部依存）:
  - `screen_external_signals.html` — SNS/口コミ/天候/交通からの需要・リスク検知＋承認付き提案
  - `screen_risk_forecast.html` — 「対応準備リスク」（顧客採点ではない・センシティブ属性を扱わない）
  - `screen_strategy_actions.html` — AI提案を承認ワークフローで実行
  - `screen_public_facility.html` — 第二の現場＝公共施設（プライバシー安全なイベント運用）
  - `screen_audit_reports.html` — 誰が何をなぜ承認したかの監査ログ／CSV・PDF出力（プレースホルダ）
- **既存画面の強化**: ダッシュボード＝司令塔化＋「実効販売可能数」概念、設備保全＝資産台帳/故障履歴/部材・業者リードタイム/影響予約/販売停止支援、食事＝提供能力が客室販売を制約、承認＝価格/返金/販売停止/発注/保全派遣/OTA・Web更新/通知/データ修正/公共施設の減免まで網羅。
- `mobile_more.html` を新設（元リポジトリで参照のみ・実体なしの 404 を解消）。

## 3. 残課題（TODO・デモリスク）

### 3-1. 外部ランタイム依存（既存画面のみ・ネット断で崩れる可能性）
今回は既存画面の挙動を壊さないため撤去せず、TODO として記録する。

- **Chart.js（CDN: cdn.jsdelivr.net）** — 7ファイル:
  `market_research.html`, `screen1_dashboard.html`, `screen4_rooms.html`, `screen6_maintenance.html`, `screen7_marketing.html`, `screen8_reports.html`, `screen_meals.html`
  - 対応案: `assets/chart.min.js` としてローカル同梱、または CSS/SVG チャートへ置換。
- **Google Fonts（fonts.googleapis.com の @import）** — モバイル11ファイル（`mobile_*.html`）:
  - 対応案: `@import` を削除しシステムフォント（Hiragino/Yu Gothic）にフォールバック。表示はほぼ同等。

> 会議など回線が不安定な場でフル機能を見せる場合は、上記2点のローカル化を先に行うと安全。

### 3-2. 次パス候補（P2・本ブリーフ範囲外）
- 共有 CSS（`assets/styles.css`）への既存画面の段階移行。
- 固定幅 `width:1440px` のレスポンシブ化（プロジェクタ対応）。
- 軽量デモ操作（承認→ステータス変化／監査ログ追記）の既存画面への展開。

## 4. 検証結果（このパス）
- noindex: 31/31 HTML に付与。
- 残存実名: 高シグナル語の grep で 0。
- リンク 404: ローカル参照に壊れリンク 0。
- 新規5画面: 外部依存 0・noindex 有・非空。
