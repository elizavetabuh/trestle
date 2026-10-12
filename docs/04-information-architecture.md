# Information Architecture & Navigation

## Product name used in the prototype
**Trestle** — "AI-powered construction operations" (a trestle is a load-bearing structural framework; the name signals infrastructure/support without being literal).

## Navigation model

Persistent **left sidebar** (icon + label, collapsible to icon-only) + **top bar** (search, current context breadcrumb, AI Copilot trigger, notifications, account). This follows the Buildertrend/Procore pattern (sidebar-driven, not top-nav-driven) because construction users operate within a small, stable set of top-level areas for long sessions — a left rail stays reachable without re-navigating.

```
Trestle
├── Dashboard                     (portfolio home)
├── Projects                      (list → detail)
│   └── [Project Name]
│       ├── Overview              (default tab — synthesized status)
│       ├── Tasks                 (list / board toggle)
│       ├── Budget                (cost codes, change orders)
│       ├── Documents             (categorized file list)
│       ├── Risks & Delays        (AI-flagged + manual)
│       └── Team                  (roster, roles, contact)
├── Tasks                         (cross-project "My Tasks" + team view)
└── AI Copilot                    (global slide-over panel, reachable from anywhere via top bar)
```

Settings/account menu exists in the top bar but is out of scope for functional depth (static).

## Navigation rules

- Sidebar always visible on desktop (≥1024px); collapses to a bottom tab bar pattern on narrow/phone widths (Dashboard, Projects, Tasks, Copilot — the four highest-frequency destinations).
- Selecting a project from **Projects** always lands on **Overview** first — this is deliberate: Overview is the synthesized-story tab, so it's the forced first impression rather than defaulting to whichever tab was last open.
- The AI Copilot is **global**, not nested per-project — invokable from the top bar anywhere. When opened from within a project, it's pre-scoped to that project's context (shown as a chip at the top of the panel) but the user can broaden back to portfolio scope.
- Breadcrumb in the top bar reflects current depth: `Projects / Riverside Medical Pavilion / Budget`.

## URL / state structure (hash-based for the prototype, mirrors what real routes would be)

```
#/dashboard
#/projects
#/projects/:projectId                     → redirects to overview
#/projects/:projectId/overview
#/projects/:projectId/tasks
#/projects/:projectId/budget
#/projects/:projectId/documents
#/projects/:projectId/risks
#/projects/:projectId/team
#/tasks
```
The AI Copilot panel is UI state (open/closed + scope), not a route — it layers over any page, matching its "always reachable" role.

## Screen-level layout pattern

Every content page shares one shell:
1. **Page header** — title, context actions (e.g. "New Task", view toggle, filter bar).
2. **Primary content** — table, board, or form-like detail grid depending on screen.
3. **Secondary rail** (project detail pages only) — compact "Risk & Health" summary card, kept visible regardless of which tab is active, so schedule/budget health is never more than a glance away even while reading Documents or Team.

## Component inventory (shared across screens)

- `StatusPill` (good / warning / critical / neutral / info — semantic color only, always paired with a label)
- `KPITile` (label, value, delta, optional sparkline)
- `DataTable` (sortable headers, row click-through, right-aligned tabular-numeral columns)
- `HealthBadge` (schedule/budget combined indicator used in list rows and headers)
- `RiskCard` (severity stripe, title, reasoning, source citation, recommended action, acknowledge action)
- `AICopilotPanel` (slide-over: scope chip, suggested prompts, message thread, sourced-answer formatting)
- `Avatar` / `AvatarStack` (team members)
- `EmptyState`, `Tabs`, `FilterBar`, `ProgressBar`, `Chart` primitives (bar, donut, sparkline, stacked budget bar)
