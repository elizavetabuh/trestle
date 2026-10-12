# Trestle — AI-Powered Construction Operations Platform

A fully functional product prototype for a construction operations SaaS platform: portfolio dashboard, project tracking, task management, budget monitoring, document management, and an AI operations copilot that summarizes project status and flags risks — grounded in real (mock) project data, not canned text.

## What's here

- **[`docs/`](docs/)** — the full product spec: competitor/UX research, personas & journeys, MVP scope, information architecture, functional requirements, and a React implementation task document.
- **[`prototype/index.html`](prototype/index.html)** — the working prototype. A self-contained React app (React + Babel Standalone via CDN, no build step) with realistic construction data, a deterministic risk-detection engine, and a grounded AI copilot.

## Running the prototype

No build step required — it's a static file that loads React from a CDN.

```bash
cd prototype
python3 -m http.server 4173
```

Then open `http://localhost:4173`.

> **Note:** this was built as a CDN-loaded React app (not a Vite bundle) because the delivery environment had no working Node/npm toolchain. See [`docs/06-task-document.md`](docs/06-task-document.md) for the target Vite + TypeScript architecture this maps to 1:1 — the component boundaries in `index.html` already follow that decomposition, so migrating is a mechanical file split, not a redesign.

## Highlights

- 10 realistic construction projects (healthcare, industrial, education, residential, mixed-use, hospitality) with cost codes, change orders, tasks, documents, daily logs, and milestones — deterministically generated so the numbers stay internally consistent across every screen.
- A rules-based risk engine (overdue tasks, blocked tasks, budget threshold breaches, recurring weather/inspection delays) that auto-generates explainable risk records with a source citation — no black box.
- An AI Operations Copilot (portfolio + per-project scope) that answers questions about budget, schedule, and risk by reasoning over the live data, with clickable source citations back to the underlying record.
- Monochromatic UI with color reserved for status/risk signals, built on a validated, colorblind-safe palette.
