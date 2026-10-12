# MVP Scope & Feature Prioritization

Method: MoSCoW, each item mapped to the persona/journey it serves (see `02-personas-and-journeys.md`).

## Must have (MVP — built in this prototype)

| Feature | Serves | Rationale |
|---|---|---|
| Portfolio Dashboard (KPI row, risk feed, budget burn snapshot, upcoming milestones) | Dana (Journey A) | The single biggest validated gap — no competitor leads with a synthesized portfolio view |
| Projects list (sortable/filterable table) | Dana, Marcus | Table-first, not card-first — matches how PMs actually scan 10–30 projects |
| Project Overview (per-project synthesized status) | Marcus, Dana | Narrows the "assemble the story yourself" gap at the project level |
| Tasks (list + board view, status transitions, trade/assignee/due date) | Priya, Marcus (Journey C) | Core operational primitive; every competitor has this, it must be solid |
| Budget tab (cost codes: budgeted/committed/actual/forecast/variance, change orders) | Marcus (Journey B) | Buildertrend proved integrated budget UX matters; we need commercial-grade depth |
| Documents tab (categorized list, versioning metadata) | Marcus, Dana | Table stakes for the category; kept intentionally lighter than Procore/ACC for MVP |
| Risks & Delays tab (AI-generated + manually flagged, with reasoning + source) | Dana, Marcus (Journey A, E) | This *is* the differentiation — make it a first-class tab, not buried |
| AI Operations Copilot (portfolio briefing + per-project Q&A, grounded with sources) | Dana, Marcus (Journey A, B, D) | The core differentiator identified in competitive research |
| Automatic risk-flag generation (rules-based: overdue tasks, budget threshold breach, blocked tasks) | all (Journey E) | Makes "AI predicts risk" concrete, explainable, and demoable without real ML infra |
| Responsive layout (desktop primary, usable down to phone width) | Priya | Field use requires it; competitors' heavy tools fail here |

## Should have (near-term post-MVP, not built in this pass)

- Plan/drawing viewer with pinned task markup (Fieldwire's strongest pattern) — high value, high build cost (PDF/image viewer + geo-pinning); deferred.
- RFI and Submittal formal workflows (multi-party routing, approval chains) — needed for subcontractor-heavy commercial GCs but adds significant workflow complexity beyond MVP's single-tenant demo.
- Client/owner-facing portal (Buildertrend's differentiator for residential) — different persona, different permission model; a distinct product surface.
- Native offline mobile app — prototype is responsive web only; true offline sync is an infrastructure investment, not a UI pattern.
- Daily log structured entry form (weather, crew count, safety) — represented as data the AI copilot cites, not yet a dedicated authoring UI.

## Could have (later)

- Schedule/Gantt with true dependencies (none of the field-first tools do this well — real opportunity, but a scheduling engine is a multi-quarter build).
- Computer-vision progress tracking (à la Buildots) — enterprise add-on territory, not core product.
- Configurable dashboards / saved views.
- Multi-company / subcontractor collaboration permissions.

## Won't have (explicitly out of scope, with rationale)

- **3D BIM model viewing/clash detection** — ACC's moat; replicating it is a different product category (design coordination, not operations).
- **Payroll, accounting sync, invoicing/AP** — financial *operations* tooling, not financial *visibility*; integration territory, not core UX.
- **Estimating/takeoff tools** — pre-construction, different buyer and workflow entirely.
- **Futuristic/speculative AI** (autonomous agents acting without approval, fully automated scheduling rewrites) — contradicts the brief's instruction to stay technically plausible; our AI explains and flags, a human still decides.

## Why this scope

The research identified one defensible, buildable wedge: **today's tools make you assemble the operational story yourself; nobody narrates it with explainable, sourced reasoning at both portfolio and project level.** Every Must-have feature above exists specifically to make that wedge real and demoable — including the supporting primitives (tasks, budget, documents) a copilot needs real data to reason over. Everything deferred is deferred because it's either (a) a different, already-dominated product category (BIM, payroll, estimating) or (b) valuable but orthogonal to proving the synthesis/AI thesis.
