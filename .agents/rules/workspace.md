# Workspace Rules

These are project-specific rules for agents working in Etcher Solution. `AGENTS.md` is the top-level contract; this file adds practical workspace guidance.

## Project Context

- Etcher Solution is a semiconductor equipment log analysis, dashboard, realtime monitoring, prediction, and AI reporting system.
- Backend code lives under `backend/app` and uses FastAPI, Uvicorn, Pydantic, pandas, numpy, and Gemini integration.
- Frontend code lives under `frontend/app` and `frontend/components` and uses Next.js 16, React 19, TypeScript, Tailwind CSS, and Vitest.
- ML and dataset utilities live under `ml`.
- Local data, log roots, history, and generated analysis artifacts live under `data` and related configured paths.

## Working Rules

- Read `AGENTS.md` and `.agents/context.json` before making changes.
- Inspect the relevant code, tests, reference docs, and task-specific skill file before editing.
- Keep changes scoped to the requested task. Do not perform broad cleanup while fixing a narrow issue.
- Preserve user changes and unrelated dirty worktree state.
- Prefer existing project patterns over new abstractions.
- For frontend UI work, read `.agents/rules/frontend_design.md` and keep user-facing copy Korean-first.
- For log parsing work, read the relevant LOTDATA, InfoLog, or ErrorLog rule file before changing parser or API behavior.

## Important Areas

- `backend/app/main.py`: FastAPI app setup, middleware, exception handling, and router mounting.
- `backend/app/routers`: API route definitions.
- `backend/app/services`: dashboard, analysis, prediction, watcher, realtime, and virtual process logic.
- `backend/app/parser.py`: shared log parsing behavior.
- `backend/app/core/config.py`: settings and default data/model paths.
- `backend/tests`: backend regression tests.
- `frontend/app`: Next.js routes and API helpers.
- `frontend/components`: dashboard, layout, manual, and reusable UI components.
- `.agents/reference`: current project, runtime, and API reference docs for agents.
- `.agents/skills`: task-specific execution checklists.

## Verification Defaults

- Frontend change: run `npm --prefix frontend run test`.
- Frontend route/build-sensitive change: also run `npm --prefix frontend run build`.
- Backend change: run focused tests first, then `pytest backend/tests` for shared router, parser, or service behavior.
- API contract change: verify backend tests and inspect frontend callers in `frontend/app/api.ts` or nearby components.
- Documentation-only change: verify referenced paths exist and Markdown is readable.

## Approval Boundaries

Ask before deleting project data, generated reports, logs, model artifacts, or database-like state.

Ask before dependency upgrades, package lock rewrites, large refactors, deployments, remote pushes, release actions, credential changes, or destructive Git operations.

Normal reading, searching, focused edits, local tests, and local builds may proceed when relevant to the task.
