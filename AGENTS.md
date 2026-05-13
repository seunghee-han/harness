# Etcher Solution Agent Harness

This file is the top-level operating contract for AI agents working in this repository. Read it at the start of every session before changing files.

## Project Context

Etcher Solution is a semiconductor equipment log analysis and AI reporting system.

- Backend: FastAPI, Python, Uvicorn, Pydantic, pandas, numpy
- Frontend: Next.js 16, React 19, TypeScript, Tailwind CSS, Vitest
- AI integration: Google Generative AI / Gemini
- Frontend port: `3003`
- Backend port: `8080`
- Main launcher: `start_app.ps1`

## Source Of Truth

Use these existing rule files as detailed references. Do not duplicate their full contents here.

- Workspace rules: `.agents/rules/workspace.md`
- Frontend design rules: `.agents/rules/frontend_design.md`
- LOTDATA log analysis: `.agents/rules/LOTDATA_LOG_ANALYSIS.md`
- InfoLog analysis: `.agents/rules/InfoLog_Analysis.md`
- Error log analysis: `.agents/rules/error_log_analysis.md`
- Project context: `.agents/reference/project_context.md`
- Runtime stack: `.agents/reference/runtime_stack.md`
- API v1 reference: `.agents/reference/api_v1_reference.md`
- Existing workflows: `.agents/workflows/`
- Long-term memory: `.agents/context.json`
- Failure prevention log: `FAILURE_LOG.md`
- Harness skills: `.agents/skills/`

## Agent Workflow

1. Read this file, then read `.agents/context.json` for current project state.
2. Inspect the relevant code, tests, and rule files before proposing or making changes.
3. Keep edits scoped to the requested task.
4. Preserve existing user changes. Do not revert unrelated modifications.
5. Prefer existing project patterns over new abstractions.
6. Run the smallest useful verification after changes.
7. If a mistake or failed assumption is discovered, add a concise entry to `FAILURE_LOG.md`.
8. Update `.agents/context.json` only with durable summaries, not full conversations.

## Coding Rules

- Backend changes should follow the existing FastAPI router/service structure under `backend/app`.
- Frontend changes should follow the existing Next.js App Router and component patterns under `frontend/app` and `frontend/components`.
- TypeScript changes should keep types explicit at public boundaries.
- Python changes should use clear function boundaries and predictable error handling for log parsing and API routes.
- Log parsing changes must preserve behavior for LOTDATA, InfoLog, and ErrorLog unless the task explicitly changes it.
- Do not introduce broad refactors when a narrow fix is enough.

## Approval Required

Ask the user before performing any of these actions:

- Deleting project data, generated reports, logs, model artifacts, or database-like state.
- Large-scale refactors across multiple subsystems.
- Dependency upgrades or package manager lockfile rewrites not directly requested.
- Deployment, production startup, remote pushes, or release actions.
- External API key changes, secret handling changes, or credential storage changes.
- Destructive Git operations such as reset, checkout, clean, or force push.

Normal reading, searching, focused edits, local tests, and local builds may proceed when they are relevant to the task.

## Sensors And Verification

- Frontend change: run `npm --prefix frontend run test`; run `npm --prefix frontend run build` when build behavior or routing may be affected.
- Backend change: run `pytest backend/tests`.
- API contract change: run relevant backend tests and inspect frontend API callers.
- Log parser change: prioritize tests covering LOTDATA, InfoLog, and ErrorLog behavior.
- Documentation-only change: verify file existence, paths, and Markdown readability.

If verification cannot be run, state why and record any residual risk in the final response.

## Reference Priority

When project facts conflict, prefer this order:

1. Current code, tests, package files, and config files.
2. `AGENTS.md`, `.agents/context.json`, and `FAILURE_LOG.md`.
3. `.agents/reference/project_context.md`, `.agents/reference/runtime_stack.md`, and `.agents/reference/api_v1_reference.md`.
4. `.agents/rules`, `.agents/skills`, and `.agents/workflows`.
5. Root legacy docs such as `RealTime_Architecture_Plan.md`.

## Skill Routing

Use `.agents/skills/` for task-specific workflows:

- LOTDATA analysis: `.agents/skills/analyze_lotdata_log.md`
- InfoLog analysis: `.agents/skills/analyze_info_log.md`
- ErrorLog analysis: `.agents/skills/analyze_error_log.md`
- Frontend changes: `.agents/skills/frontend_change.md`
- Backend API changes: `.agents/skills/backend_api_change.md`

Skills describe execution order. This file defines authority and safety boundaries.
