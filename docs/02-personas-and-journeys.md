# Target Personas & Core User Journeys

## Personas

### 1. Dana Whitfield — VP of Operations / Project Executive
- **Company context:** Mid-size general contractor, 12–20 active projects, $40–150M annual volume.
- **Goals:** Know which of her projects need her attention *today* without visiting each one; walk into Monday leadership meetings with accurate numbers; catch budget or schedule problems before they become change-order disputes.
- **Pain points today:** Spends Sunday night / Monday morning pulling status from 4–5 PMs by phone or email because no tool gives her a trustworthy cross-project view. Procore's portfolio dashboard exists but requires heavy configuration and still doesn't explain *why* a project is red.
- **Device context:** Desktop at her office, phone when traveling between sites.
- **Relationship to AI:** Wants a synthesized narrative ("3 projects are at risk, here's why"), not another dashboard she has to interpret herself. Trusts a number more when she can see where it came from.

### 2. Marcus Ibe — Project Manager
- **Company context:** Owns 2–4 active projects end-to-end: budget, schedule, subcontractor coordination, owner communication.
- **Goals:** Keep budget and schedule aligned, resolve blockers before they cascade, produce clean owner-facing status updates without a half-day of manual report assembly.
- **Pain points today:** Budget data lives in one tool, field progress in another (or in texts/photos from the super), documents in a shared drive or email. Reconciling "what's actually happening on site" with "what the budget says" is manual and slow in every competitor product reviewed.
- **Device context:** Laptop primarily, phone for site visits.
- **Relationship to AI:** Wants it to do the tedious synthesis (draft the weekly owner update, flag the change order that's about to blow a cost code) and always show its work.

### 3. Priya Nandakumar — Superintendent (Field Lead)
- **Company context:** On-site daily, runs the crew, logs daily progress, flags issues as they happen.
- **Goals:** Assign and close out tasks fast, log what happened today without re-typing it later for the office, escalate blockers (inspection fail, material delay, weather) immediately.
- **Pain points today:** Office-weighted tools (Procore) are too heavy to use standing on a slab; field-only tools (Fieldwire) don't let her see whether a delay she's flagging actually matters to the budget or schedule.
- **Device context:** Phone and tablet, outdoors, gloves on, spotty connectivity.
- **Relationship to AI:** Low tolerance for chat — wants fast structured input (tap, not type) and clear confirmation that what she logged reached the office.

### 4. Owner's Rep / Client stakeholder (secondary persona, informs the "explainability" requirement but is out of scope for MVP login access)
- Wants plain-language status, not raw construction data. Reinforces why AI-generated narrative summaries (not just charts) matter — the same summary a PM reads can, later, be the basis of what gets shared externally.

---

## Core User Journeys (mapped to MVP screens)

### Journey A — Dana's Monday Portfolio Review
1. Logs into **Dashboard**. Sees portfolio KPI row (active projects, at-risk count, total committed budget variance, tasks overdue this week).
2. **AI Copilot** panel is already showing a generated "Portfolio Briefing" — top 3 projects needing attention, each with a one-line reason and a link.
3. Clicks into the first flagged project → lands on **Project Overview** tab, sees the same risk restated with supporting numbers (budget variance %, schedule slip in days).
4. Opens **Risks & Delays** tab to see the full reasoning trail (which daily log entries, which cost codes).
5. Leaves a status confident she knows what to raise Monday morning — without calling anyone.

### Journey B — Marcus Investigates a Budget Variance
1. From **Projects** list, sorts by budget variance, opens the worst offender.
2. On **Budget** tab, sees cost-code-level table: budgeted vs. committed vs. actual vs. forecast, with the offending cost code highlighted.
3. Opens the AI Copilot, asks "Why is concrete over budget on this project?" — gets a grounded answer citing a specific change order and a daily log delay note.
4. Reviews the linked **Change Order** record, confirms it explains the variance, exports/acknowledges.

### Journey C — Priya Logs Field Progress & a Blocker
1. Opens **Project → Tasks** (board view) on her phone-width viewport.
2. Marks a drywall task complete; drags a blocked electrical task to "Blocked," attaches a reason ("Inspection failed — rework required").
3. Marked-blocked task immediately surfaces in the project's risk feed (deterministic rule: any task moved to Blocked auto-generates a risk flag) — closing the field-to-office gap competitors leave manual.

### Journey D — Marcus Prepares a Weekly Owner Update
1. Opens **AI Copilot**, selects the suggested prompt "Summarize this week for [Project]."
2. Copilot returns a structured narrative: progress, budget position, schedule health, open risks — each line sourced.
3. Marcus reads it, trusts it because every claim traces to a real record, and uses it as the basis for his owner email (copy is designed to be reusable, not an internal-only format).

### Journey E — New Risk Detection (system-initiated, not user-initiated)
1. A task's due date passes without being marked complete, or a cost code's actual spend crosses 90% of budget while the project is <90% complete.
2. The rules engine behind the AI Copilot auto-generates a risk record (severity, reasoning, recommended action) that appears in the Dashboard risk feed and the project's Risks tab — this is the "predictive" promise made concrete and explainable, not a black box.
