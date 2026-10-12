# React Implementation Task Document

## Build note (read first)

This prototype was implemented and delivered as a **self-contained React application loaded via CDN** (React + ReactDOM UMD builds, in-browser JSX via Babel Standalone, hand-written CSS) rather than a Vite-bundled project, because the delivery sandbox for this engagement had no working Node/npm toolchain available. The component boundaries, state model, and file organization below are written as the **target architecture for a standard Vite + React + TypeScript migration** — the prototype's internal structure (see inline section comments in `prototype/index.html`) already follows this decomposition 1:1, so porting is a mechanical split into files, not a redesign.

## Target project structure

```
trestle/
├── src/
│   ├── main.tsx
│   ├── App.tsx                     # shell: sidebar + topbar + router outlet
│   ├── router.tsx                  # route table (React Router v6)
│   ├── styles/
│   │   ├── tokens.css              # color/type/spacing custom properties (light + dark)
│   │   └── base.css
│   ├── data/
│   │   ├── projects.ts             # mock project records
│   │   ├── tasks.ts
│   │   ├── budget.ts                # cost codes + change orders
│   │   ├── documents.ts
│   │   ├── team.ts
│   │   ├── dailyLogs.ts             # AI-citation source data
│   │   └── risks.ts
│   ├── lib/
│   │   ├── riskEngine.ts            # deterministic rule evaluation (see functional spec §7)
│   │   ├── copilotEngine.ts         # intent matching + grounded response composition
│   │   └── selectors.ts             # derived KPIs (budget variance %, schedule health, overdue counts)
│   ├── state/
│   │   └── store.tsx                # React context + reducer: tasks, risks, acknowledgements, copilot history
│   ├── components/
│   │   ├── primitives/              # StatusPill, KPITile, Avatar, ProgressBar, Tabs, FilterBar
│   │   ├── DataTable/
│   │   ├── charts/                  # BarChart, GroupedBarChart, Sparkline, DonutChart (hand-rolled SVG)
│   │   ├── RiskCard/
│   │   ├── HealthBadge/
│   │   └── AICopilotPanel/
│   └── pages/
│       ├── Dashboard/
│       ├── Projects/ (List + Detail)
│       │   └── tabs/ (Overview, Tasks, Budget, Documents, Risks, Team)
│       └── Tasks/ (global "my tasks" view)
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

## Task breakdown

### Phase 0 — Foundations
- [ ] T0.1 Design tokens: port the validated color system (semantic status palette, categorical chart palette, ink/surface/border scale) and type scale (display/body/mono) into `tokens.css`, light + dark mode blocks.
- [ ] T0.2 Mock data model: author realistic construction records for ~10 projects across types (commercial, healthcare, education, residential, industrial, mixed-use), each with tasks, cost codes, change orders, documents, team, daily logs, and derived risks.
- [ ] T0.3 `riskEngine.ts`: implement the 4 deterministic rules from the functional spec; pure functions, unit-testable, operate on current state → return `Risk[]`.
- [ ] T0.4 `selectors.ts`: derived values (project schedule health, budget variance %, portfolio KPIs) computed from base data, not stored redundantly.

### Phase 1 — App shell & navigation
- [ ] T1.1 Sidebar (collapsible, active-state highlighting, responsive collapse to bottom tab bar <768px).
- [ ] T1.2 Top bar: breadcrumb, search (non-functional stub acceptable for MVP), AI Copilot trigger with unread-risk badge, account menu (static).
- [ ] T1.3 Router table + hash-based (or React Router) route wiring per IA doc.

### Phase 2 — Dashboard
- [ ] T2.1 KPI tile row wired to live selectors.
- [ ] T2.2 Risk feed (top N global risks, `RiskCard`, working Acknowledge action dispatching to store).
- [ ] T2.3 Budget burn grouped bar chart (dataviz-compliant: single scale, validated palette, hover tooltip, legend).
- [ ] T2.4 Upcoming milestones list.
- [ ] T2.5 AI Portfolio Briefing card wired to `copilotEngine`.

### Phase 3 — Projects list + detail shell
- [ ] T3.1 Projects `DataTable`: sortable columns, filter bar (status/type/search), row click-through.
- [ ] T3.2 Project detail shell: header, `HealthBadge`, tab navigation, persistent secondary rail.
- [ ] T3.3 Overview tab: AI status paragraph, scoped KPI strip, mini risk list, mini milestone timeline.

### Phase 4 — Tasks
- [ ] T4.1 List view with sort/filter, overdue visual flag.
- [ ] T4.2 Board view (drag-and-drop or click-to-advance), status transition dispatch to store.
- [ ] T4.3 "Blocked" transition → required reason input → dispatch risk-creation action.
- [ ] T4.4 Task detail panel.
- [ ] T4.5 Global "My Tasks" page reusing the same components with a Project column added.

### Phase 5 — Budget
- [ ] T5.1 Summary tiles.
- [ ] T5.2 Cost code table with threshold-based variance coloring.
- [ ] T5.3 Budget-vs-actual chart.
- [ ] T5.4 Change orders table with inline expand.

### Phase 6 — Documents & Team
- [ ] T6.1 Documents table + category filter chips + search.
- [ ] T6.2 Team roster table.

### Phase 7 — Risks & Delays tab + AI Copilot
- [ ] T7.1 Full risk list (open/acknowledged, filterable by severity/type).
- [ ] T7.2 `AICopilotPanel`: slide-over shell, scope chip + switcher, suggested prompts (scope-aware).
- [ ] T7.3 `copilotEngine.ts`: keyword/intent matcher → response composer pulling live selector data → cited "Sources" chips with jump-to-record behavior.
- [ ] T7.4 Conversation state (session-scoped), follow-up handling.

### Phase 8 — Polish & QA
- [ ] T8.1 Responsive pass at 1440/1024/768/390px.
- [ ] T8.2 Keyboard focus states, color-contrast check against the validated palette.
- [ ] T8.3 Empty/edge states (project with zero risks, zero overdue tasks, etc.).
- [ ] T8.4 Walk all 5 core journeys from `02-personas-and-journeys.md` end-to-end as acceptance tests.

## Acceptance criteria (per core journey)

| Journey | Pass condition |
|---|---|
| A — Portfolio review | Dashboard KPIs and AI briefing reflect real, internally-consistent derived data; clicking a flagged project lands on Overview with matching numbers |
| B — Budget investigation | Sorting Projects by variance surfaces the correct worst project; Budget tab's flagged cost code matches the Copilot's cited reasoning |
| C — Field task + blocker | Moving a task to Blocked requires a reason and produces a visible new risk card within the same session, no reload |
| D — Owner update draft | Copilot's project-scoped summary includes progress, budget, schedule, and at least one sourced risk |
| E — Auto risk detection | An overdue task (seeded in mock data) already appears as an open risk on first load, proving the rule runs on data, not just on user action |
