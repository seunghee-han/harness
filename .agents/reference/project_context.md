# Etcher Solution Project Context

This file is the harness-friendly project map. It explains what the product does, where important code lives, and which domain concepts agents should understand before making changes.

## Current Assessment

- Root legacy architecture docs such as `RealTime_Architecture_Plan.md` are useful background, but agents should prefer `.agents/reference/*` for current harness work.
- Runtime, package, port, and startup details belong in `.agents/reference/runtime_stack.md`.
- Public API details belong in `.agents/reference/api_v1_reference.md`.

## Application Shape

- Product: semiconductor equipment log analysis, dashboarding, realtime ingestion, AI reporting, and process prediction.
- Backend: FastAPI app in `backend/app`.
- Frontend: Next.js App Router app in `frontend`.
- ML utilities: training and dataset scripts in `ml`.
- Data roots: `data/lotlog` and related local data directories.
- Realtime ingest support: WebSocket endpoints under `/api/v1/realtime/...`.

## Main Domains

- LOTDATA and PROCESS logs: process values, recipes, cycle time, correlation, and dashboard charts.
- InfoLog: equipment information events and detail views.
- ErrorLog: alarm/error parsing, aggregation, daily summaries, and trend views.
- Dashboard: summary metrics, historical trends, alarms, generated reports, and log exploration.
- Realtime: edge-forwarded log lines, WebSocket broadcasting, and short in-memory history.
- Virtual process and prediction: recipe templates, synthetic process prediction, process output prediction, and wafer map prediction.

## Important Project Areas

- `backend/app/main.py`: FastAPI app setup and router mounting.
- `backend/app/routers`: API route definitions.
- `backend/app/services`: business logic for analysis, dashboard, prediction, watcher, and virtual process behavior.
- `backend/app/parser.py`: core log parser behavior.
- `backend/app/core/config.py`: settings and data/model directory defaults.
- `backend/tests`: backend regression tests.
- `frontend/app`: Next.js routes and API helper.
- `frontend/components`: dashboard, layout, manual, and UI components.
- `ml`: dataset building and model training utilities.
- `data`: local lotlog, history, and generated/derived data roots.

## Harness Navigation

- Use `.agents/rules` for detailed behavior and design rules.
- Use `.agents/skills` for task-specific execution order.
- Use `.agents/workflows` for repeated dev/build/PR procedures.
- Use `.agents/reference/api_v1_reference.md` for API routes and contracts.
- Use `.agents/reference/runtime_stack.md` for stack, ports, startup, and runtime facts.
