# Message for Claude Code

Copy and paste the message below into Claude Code after checking out this branch.

```text
You are working in the GitHub repository `Agentic-governance/ryokan-ops-demo` on branch `review/facility-os-improvement`.

Read `CLAUDE.md` first. Treat it as the product/design/engineering brief for this branch.

Goal:
Turn the current ryokan PMS screen demo into a stronger Facility Operations AI OS demo while preserving the ryokan-specific insight. The product should be shown as an operations layer that starts above existing PMS / site-controller / reservation systems, takes over the daily operating surface, and can eventually internalize or replace those legacy systems.

Important product principles:
- Existing PMS / reservation systems are initially connected, not ripped out.
- The product replaces the daily front-end / operating surface first.
- AI may read, detect, summarize, and propose.
- Important writes must require human approval and be audit-logged.
- Empty rooms are not the same as sellable stays. Emphasize `実効販売可能数`.
- External demand and risk signals must be connected to internal operational constraints.
- This is not only lodging; public facilities should be shown as a second vertical.

Start with P0 and P1 only. Do not overbuild.

P0 tasks:
1. Remove all realistic personal names from the demo. Replace with role labels like `支配人A`, `フロント担当B`, `会計担当C`, `夜勤担当D`, `予約ID 1208`, `ゲストID G-0421`.
2. Add `<meta name="robots" content="noindex, nofollow">` to every HTML file.
3. Identify external runtime dependencies such as CDN Chart.js or Google Fonts. Remove them if easy; otherwise document them in `DEMO_NOTES.md` as remaining demo risks.
4. For public-facility security concepts, do not implement face recognition, identity tracking, or persistent person tracking. Use privacy-safe event detection language only.

P1 tasks:
1. Update `index.html` so the first impression is a 3-minute product story, not only a screen list.
2. The hero should communicate: `施設運営AI OS — PMS/予約システムの上で、需要・リスク・保全・承認を統合する`.
3. Add three clear concepts near the top:
   - 外部を見る: SNS / Web / 口コミ / 天候 / 交通 / イベント / インバウンド動向
   - 内部を読む: 予約 / 客室 / 食事 / 清掃 / 設備 / 人員 / 在庫 / 会計例外
   - 承認して動かす: 価格変更 / 販売停止 / 顧客通知 / 保全依頼 / 発注 / レポート
4. Keep all existing screen links, but regroup them into clearer sections.
5. Add links/placeholders for new high-value screens:
   - `screen_external_signals.html`
   - `screen_risk_forecast.html`
   - `screen_strategy_actions.html`
   - `screen_public_facility.html`
   - `screen_audit_reports.html`
6. Create those new screens as static HTML pages using the existing visual language. Keep them lightweight but convincing.
7. Strengthen existing dashboard / maintenance / meals / approvals copy where safe:
   - dashboard: make it a command center, not just widgets
   - maintenance: asset registry, failure history, parts/procurement/vendor lead time, affected reservations, stop-sale support
   - meals: meal capacity, allergy/special meal capacity, supplier/procurement warnings, staffing load
   - approvals: price change, refund, stop-sale, procurement, maintenance dispatch, OTA/web copy update, guest notification, AI-proposed data correction

Technical constraints:
- Keep this as a static HTML/CSS/JS demo.
- Do not add a production backend.
- Do not add real PMS, OTA, SNS, camera, or payment integrations.
- Do not use real guest or staff data.
- Avoid a heavy framework.
- Prefer small, reviewable commits.
- After changes, verify that index links do not 404.

Deliverable:
Make the demo feel like a serious Facility Operations AI OS, with ryokan as the first vertical and public facilities as an obvious second vertical. Approval and auditability should feel central, not decorative.
```
