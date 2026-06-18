# Claude Code Improvement Brief

This branch is for a serious product/design/code improvement pass on the current static HTML demo.

Repository: `Agentic-governance/ryokan-ops-demo`
Working branch: `review/facility-os-improvement`

## Product thesis

The current demo is not just a ryokan PMS demo. Treat it as the first vertical of a broader **Facility Operations AI OS**.

Core idea:

> Existing PMS / site controller / facility reservation systems are legacy record systems. This product should enter as a front-end / operations layer that integrates with them, takes over the daily operating surface, and can eventually internalize or replace the legacy system.

For lodging facilities, the system should combine:

- PMS / site controller integration
- reservations, rooms, meals, guest operations, approvals, accounting-adjacent workflows
- maintenance / preservation / procurement logistics
- SNS / web / review / search monitoring from CCDM-style external signals
- inbound and domestic guest risk forecasting
- AI-generated strategy proposals
- human approval and auditability before any write-back

For public facilities, the same OS can combine:

- facility reservation / usage schedule
- maintenance and asset management
- security-camera event integration, privacy-safe and event-based only
- shift / staffing management
- usage and revenue analysis
- audit-ready public-sector reports
- incident / complaint / disaster-response records
- AI-generated operational recommendations with human approval

The demo should make this bigger category obvious without losing the current ryokan use case.

## Current strengths to preserve

Keep the current direction. Do not rewrite into a generic SaaS dashboard.

Strong existing concepts:

- The top page says the system does not replace the core system at first; it layers field judgment, AI proposals, and approvals above it.
- The dashboard already shows AI priority actions.
- Maintenance is included as an operational constraint, not a side feature.
- Meal capacity is treated as a selling constraint, which is especially important for ryokan.
- The approval screen already contains useful hotel/ryokan control cases such as price changes, refunds, voucher/discount, no-show charging, voucher cancellation, and AI data corrections.
- The architecture screen already contains the important principle: AI can read autonomously; write-back requires human approval.

## P0 fixes: trust, privacy, and demo credibility

Implement these before adding new features.

### 1. Remove all realistic personal names

The index page says sample data and personal names are hidden, but several screens currently include Japanese personal names such as manager/front/accounting staff names. Replace all personal names with anonymized role labels.

Use patterns like:

- `支配人A`
- `フロント担当B`
- `会計担当C`
- `夜勤担当D`
- `予約ID 1208`
- `ゲストID G-0421`

Avoid real-looking full names anywhere in the demo.

### 2. Add `noindex, nofollow` to every HTML page

`index.html` has `noindex, nofollow`; child pages should also include it.

Add this to every HTML file:

```html
<meta name="robots" content="noindex, nofollow">
```

### 3. Avoid external runtime dependencies for demo reliability

Several pages load Chart.js from CDN and mobile pages import Google Fonts. For a serious sales/demo repo, avoid network fragility.

Preferred:

- local static JS for charts, or simple SVG/CSS charts
- system fonts only
- no third-party font import
- no remote network calls

If full removal is too large for the first pass, at least document remaining external dependencies in `DEMO_NOTES.md` and mark them as TODO.

### 4. Privacy-safe camera/security framing

For public-facility security concepts, do **not** implement face recognition, identity tracking, or persistent person tracking.

Use privacy-safe event detection language:

- crowding detected
- restricted-area entry
- fall-like event
- after-hours presence
- equipment-room door opened
- camera health offline

Represent camera inputs as event metadata and confirmation logs, not raw surveillance UX.

## P1: Reframe the top page into a 3-minute story

The current `index.html` is useful as a screen list, but the first impression should become a product story.

Add a top section before the screen grid:

### Suggested structure

1. Hero title
   - `施設運営AI OS — PMS/予約システムの上で、需要・リスク・保全・承認を統合する`

2. Short product promise
   - `既存PMSや予約システムはそのまま接続。現場が毎朝見る画面をAI運用レイヤーに置き換え、最終的には基幹機能も内包できる。`

3. Three-column explanation
   - `外部を見る`: SNS / Web / 口コミ / 天候 / 交通 / イベント / インバウンド動向
   - `内部を読む`: 予約 / 客室 / 食事 / 清掃 / 設備 / 人員 / 在庫 / 会計例外
   - `承認して動かす`: 価格変更 / 販売停止 / 顧客通知 / 保全依頼 / 発注 / レポート

4. Demo scenario buttons
   - `旅館 3分デモ`
   - `公共施設 3分デモ`
   - `承認・監査デモ`
   - `外部シグナル監視デモ`

5. Existing screen grid below

Do not remove the current screen links. Reorganize them into clearer groups:

- Lodging operations
- AI / strategy / external signals
- Maintenance / logistics
- Approval / audit
- Mobile field operations
- Public facility expansion
- Architecture / pitch materials

## P1: Add new high-value screens

Create new static HTML screens using the existing visual language. Keep them lightweight but convincing.

### A. `screen_external_signals.html`

Purpose: CCDM-style SNS / Web / review / search monitoring for marketing and risk.

Must show:

- inbound country/region trend cards: Taiwan, Hong Kong, Korea, US/EU, Southeast Asia
- domestic Japanese segments: family, senior, solo, corporate/offsite, repeaters
- signal sources: SNS, search, review sites, OTA reviews, local event pages, weather/traffic/news
- detected opportunity examples:
  - `繁体字SNSで「箱根 雪見温泉」言及が急増`
  - `週末イベントにより日帰り・前泊需要が上昇`
  - `競合施設の高単価客室が満室傾向`
- risk examples:
  - traffic disruption
  - weather disruption
  - review sentiment drop
  - language-support pressure
  - SNS complaint cluster
- AI proposal cards with required approval:
  - update official web copy
  - adjust OTA description
  - increase price for a target period
  - add multilingual pre-arrival note
  - prepare staffing / meal inventory

### B. `screen_risk_forecast.html`

Purpose: inbound and domestic guest risk prediction.

Must show:

- risk score by segment and arrival date
- no-show / late arrival / cancellation risk
- language support risk
- allergy / meal constraint risk
- weather / transportation disruption risk
- review or complaint risk
- over-service / special request overload risk
- recommended mitigations

Important: keep it practical. This is not “score the customer” in a creepy way. Frame it as operational risk and preparation support.

Use wording like:

- `対応準備リスク`
- `到着遅延可能性`
- `多言語案内の必要度`
- `食事制約確認`
- `事前説明推奨`

Avoid discriminatory or sensitive-attribute language.

### C. `screen_strategy_actions.html`

Purpose: AI-generated strategy actions with approval workflow.

Show grouped proposals:

- Revenue / pricing
- Web / SNS / OTA copy
- Operations / staffing
- Maintenance / room stop-sale
- Procurement / meal inventory
- Guest communications

Each card should include:

- recommendation
- expected impact
- evidence / signal basis
- operational constraint check
- required approver
- buttons: `承認`, `却下`, `上位者へ送る`, `根拠を見る`

### D. `screen_public_facility.html`

Purpose: show that this is not only lodging. It is a general facility operations OS.

Use a public facility example such as a municipal sports/culture center.

Must show:

- today’s reservations / events
- facility zones and utilization
- equipment / maintenance alerts
- staffing / shift coverage
- security-camera event summary, event-only and privacy-safe
- weather / transportation / local SNS risk
- usage fee / revenue / reduction-exemption summary
- audit-ready incident and decision log
- AI recommendations with human approval

Example AI recommendations:

- `18時の体育館利用は混雑・雨天・駐車場不足が重なるため受付を1名増員`
- `空調2号機の異常値により会議室Bの新規予約を一時停止候補`
- `夜間の restricted-area entry event を警備担当が確認済みにする`
- `減免申請3件を規定に基づきまとめて確認`

### E. `screen_audit_reports.html`

Purpose: public-sector and lodging-sector audit-ready reporting.

Must show:

- decisions made
- who approved
- AI proposal ID
- data sources used
- before / after values
- customer/guest/user notification status
- financial impact
- safety/maintenance impact
- export buttons: CSV / PDF placeholders

This screen should make the product credible for public facilities and regulated operations.

## P1: Strengthen existing lodging screens

### Dashboard

Make the dashboard less like “many widgets” and more like “today’s operating command center.”

Add or emphasize:

- `Operationally Sellable Inventory` concept in Japanese: `実効販売可能数`
- difference between empty rooms and sellable stays
- meal capacity, cleaning capacity, maintenance status, staffing, and OTA sync as constraints
- AI action cards should cite cross-domain evidence: PMS + maintenance + meals + external signals

### Maintenance

Maintenance is a differentiator. Make it more military-logistics / BPR-inspired but in hospitality language.

Add:

- asset registry
- failure history
- inspection cycle
- spare parts / procurement lead time
- vendor lead time
- affected rooms / affected reservations
- stop-sale decision support
- preventive maintenance recommendations

### Meals

Keep the concept that meal capacity constrains room sales.

Add:

- allergy and special meal capacity
- supplier/procurement warning
- meal-seat capacity by time slot
- staffing and preparation load
- recommended sales limits

### Approvals

Make approvals the heart of the product.

Add approval categories:

- price change
- refund / discount / no-show charge
- stop sale / room replacement
- procurement order
- maintenance vendor dispatch
- OTA / web copy update
- guest notification
- AI-proposed PMS data correction
- public facility reservation override / fee reduction

Every meaningful write action should pass through the approval model.

## P2: Improve technical structure

This repo appears to be static HTML pages with repeated CSS. Keep it static for now, but reduce fragility.

Recommended steps:

1. Add shared CSS file, for example `assets/styles.css`.
2. Move common design tokens and layout rules into shared CSS.
3. Avoid inline styles where practical.
4. Keep pages individually openable without a build step.
5. Add a tiny shared JS file, for example `assets/demo.js`, only for demo interactions.
6. Do not introduce a heavy framework unless explicitly requested.

### Responsive behavior

Several PC pages use fixed `width=1440` and `body { width:1440px; }`. That is acceptable for screenshots but poor for live demos.

Target:

- desktop: works at 1280px, 1440px, and projector-like widths
- mobile demo pages: keep the phone frame but ensure page is usable on small browser windows
- avoid horizontal scrolling for normal desktop screens

At minimum:

- change fixed `body` width to flexible width
- change fixed header width to `width:100%`
- set `.main` to fluid width or `max-width`
- preserve screenshot-friendly layout visually

## P2: Add lightweight demo interactions

Use simple JS. No backend required.

Add interactions where they increase believability:

- approval buttons change card status to approved/rejected/escalated
- tab switches update visible content
- search/filter hides rows
- AI proposal card expands evidence
- public facility security event can be marked confirmed
- audit log appends a visible row when a demo action is taken

Do not overbuild. This is a sales/product demo.

## Acceptance criteria

The branch should be considered improved when:

- all HTML files have `noindex, nofollow`
- no realistic personal names remain
- top page clearly communicates Facility Operations AI OS, not just PMS demo
- current ryokan demo still works
- new public facility concept is visible from index
- external signal / risk / strategy screens exist and are linked
- approval and audit concepts are visibly central
- links from index do not 404
- demo can be explained in 3 minutes
- no face recognition or intrusive surveillance claim is introduced
- important actions are framed as human-approved, audit-logged actions

## Suggested implementation order

1. P0 privacy/noindex cleanup.
2. Update `index.html` hero and grouping.
3. Add shared `assets/styles.css` only if it can be done safely without breaking visual layout.
4. Add `screen_external_signals.html`.
5. Add `screen_risk_forecast.html`.
6. Add `screen_strategy_actions.html`.
7. Add `screen_public_facility.html`.
8. Add `screen_audit_reports.html`.
9. Strengthen dashboard/maintenance/meals/approvals text and links.
10. Add lightweight interactions and audit-log simulation.
11. Verify all links.

## Non-goals for this branch

Do not build a production backend.
Do not add real surveillance, face recognition, or individual tracking.
Do not connect real PMS, OTA, camera, or SNS APIs.
Do not use real guest/staff data.
Do not reposition the product as only a generic dashboard.
Do not remove the ryokan-specific domain insight.

## Product language to keep using

Use these ideas repeatedly:

- `既存PMS/予約システムはそのまま、現場の主画面を置き換える`
- `AIは読む・提案する。書き込みは人間の承認後のみ`
- `空室ではなく、実効販売可能数を見る`
- `外部需要と内部制約を同じ画面で判断する`
- `施設を安全・効率・説明可能に運営するOS`
- `売上最大化だけでなく、事故・混乱・苦情・監査リスクを下げる`

