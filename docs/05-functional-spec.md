# Functional Requirements & Interaction Specification

Each section lists the interactions implemented in the prototype as real, stateful behavior (not static mockups).

## 1. Dashboard

- **KPI row**: 4 tiles — Active Projects, Projects At Risk, Portfolio Budget Variance, Tasks Overdue. Each computed live from the underlying project/task data, not hardcoded.
- **AI Portfolio Briefing card**: on load, renders a generated narrative (see §7) summarizing the top 3 at-risk projects. "Open Copilot" button expands the full panel with the same context pre-loaded.
- **Risk feed**: scrollable list of the most severe open risks across all projects, each a `RiskCard` (severity stripe, project name, reasoning, source, "Acknowledge" button that updates state — acknowledged risks move to a collapsed/resolved state immediately, no page reload).
- **Budget burn chart**: stacked/grouped bar comparing budgeted vs. actual across the 5 largest active projects (one scale, per dataviz standards — no dual axis).
- **Upcoming milestones**: next 5 milestones across all projects, sorted by date, with days-remaining and a status pill if at risk.
- **Filter**: a scope selector (All Projects / My Projects) that live-filters every section on the page.

## 2. Projects (list)

- Table columns: Project, Client, Phase, % Complete (progress bar), Schedule Health (pill), Budget Variance (±%, colored), PM, Last Activity.
- **Sort**: click any column header to sort ascending/descending (visually indicated).
- **Filter bar**: status (On Track / At Risk / Delayed / Completed), project type, search-by-name — all combine (AND) and update the table instantly.
- **Row click** → navigates to that project's Overview tab.
- Row count and active filter summary shown above the table ("14 of 18 projects").

## 3. Project Overview

- Header: project name, client, address, phase, contract value, combined `HealthBadge`.
- **AI-generated status summary**: a paragraph synthesized from the project's live data (% complete, budget variance, schedule delta, count of open risks, most urgent risk) — regenerates (deterministically, from current state) if underlying data changes, e.g. after acknowledging a risk.
- KPI strip scoped to this project (same tile component as Dashboard, project-scoped).
- Mini risk list (top 3, "View all" linking to Risks tab).
- Mini milestone timeline (horizontal, next 4 milestones with status).
- Secondary rail persists the schedule/budget health summary across all tabs (see IA doc).

## 4. Tasks (project-scoped and global "My Tasks" view)

- **View toggle**: List ↔ Board (Kanban columns: Not Started / In Progress / Blocked / Completed).
- **Board**: drag-and-drop (or click-to-advance for touch/trackpad reliability) between columns updates task status in state immediately.
- Moving a task to **Blocked** opens a required reason field; submitting it creates a new Risk record (type: schedule, severity: warning by default, escalates to critical if the task is within 3 days of its milestone dependency) that appears in the project's Risks tab and the Dashboard feed without a page reload — this is the concrete implementation of "the system flags delays automatically."
- **List view**: sortable by due date/priority/assignee; overdue tasks (`dueDate < today` and not completed) visually flagged (critical pill + icon), independent of manual status.
- **Filter**: by trade, assignee, status.
- Task detail (click-through panel): description, trade, assignee, due date, dependency note, checklist count, activity note field.
- Global **Tasks** nav item aggregates tasks across all projects for the signed-in user's view, same list component, with a Project column added.

## 5. Budget

- **Cost code table**: CSI-style codes, Budgeted / Committed / Actual / Forecast / Variance columns (tabular-nums, right-aligned), variance colored by severity threshold (≤5% = good, 5–12% = warning, >12% = critical).
- **Summary tiles**: Original Contract, Approved Change Orders, Revised Budget, Total Committed, Variance to Date.
- **Budget vs. Actual chart**: grouped bars per cost code (top 8 by $ magnitude), single scale.
- **Change Orders table**: status pill (Pending/Approved/Rejected), amount, submitted-by, reason; clicking a row expands inline detail (no navigation).
- Any cost code crossing the critical variance threshold while the project is <90% complete is what feeds the automatic "Budget Risk" flag referenced in §7.

## 6. Documents

- Category filter chips (Drawings, RFIs, Submittals, Contracts, Permits, Daily Reports, Photos, Specifications, Change Orders, Safety).
- Table: name, category, version, status (Current/Superseded/Pending Review), uploaded by, date, size.
- Search-by-name, instant filter.
- "Superseded" documents visually deemphasized (muted row) but not hidden by default — reflects real-world need to still locate prior versions.

## 7. Risks & Delays (per project) + AI Operations Copilot

### Rules engine (deterministic, transparent — described in the product as "automatic risk detection," never framed as opaque ML)
A risk record is auto-generated when any of:
1. A task's due date has passed without completion → schedule risk, severity scales with days overdue.
2. A task is moved to **Blocked** → schedule risk, per §4.
3. A cost code's actual/committed spend crosses 90% of budget while project completion is <90% → budget risk.
4. Portfolio-level: a project's combined schedule+budget signal crosses a threshold → project-level "At Risk"/"Delayed" status shown everywhere the project appears (list, dashboard, badges) — status is *derived*, not manually set, which is itself a UX statement (status is earned from data, not self-reported).

### Risk record shape
Severity (warning/serious/critical), title, one-line reasoning, **source citation** (e.g. "Daily Log — Oct 9" or "Cost Code 03 30 00"), recommended action, status (open/acknowledged/resolved). Acknowledging a risk is a real state transition (timestamps it, moves it out of the active feed).

### AI Operations Copilot (global panel)
- **Trigger**: top bar icon (badge shows count of unread/new risk flags) — opens a right-side slide-over panel, available from any screen.
- **Scope chip**: shows "Portfolio" or the current project name; user can change scope via a dropdown.
- **Suggested prompts** (scope-aware): e.g. portfolio scope shows "What needs my attention this week?", "Summarize budget health across all projects"; project scope shows "Summarize this project's status", "Why is this project at risk?", "Draft a weekly update for the owner".
- **Input**: free-text field; submitting runs the query against the deterministic response engine (intent matching over keywords: project names, "budget", "risk"/"delay", "summary", "overdue", trade names, "owner update") and composes a grounded natural-language answer pulling real numbers from state.
- **Response format**: short narrative paragraph(s) + a "Sources" strip of chips (clicking a source chip jumps to the relevant record — e.g. a cost code row, a risk card) — this sourcing behavior is the product's core trust mechanism and is implemented functionally, not decoratively.
- **Conversation persists** within the session (component state) as the user asks follow-ups; switching project scope resets suggested prompts but keeps history.
- Explicitly **does not** take autonomous action (no auto-sending emails, no auto-changing budgets) — every suggested action requires a human click, consistent with the brief's "technically plausible, not speculative" constraint.

## 8. Team

- Roster table: name, role, company, phone/email (fake data), projects assigned, avatar.
- Read-only in MVP (no invite/permission management — explicitly deferred, see MVP scope doc).

## 9. Cross-cutting

- All status/severity visuals use the fixed semantic palette (good/warning/serious/critical) — never repurposed for anything else, consistent with dataviz governance.
- Every data table supports keyboard focus and visible focus states.
- Responsive breakpoints: desktop (≥1024px, full sidebar + secondary rail), tablet (768–1023px, collapsed icon sidebar), phone (<768px, bottom tab bar, secondary rail collapses into an expandable summary strip).
