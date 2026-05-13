# Skill: Analyze ErrorLog

Use this skill when a task involves ErrorLog parsing, error aggregation, alarm analysis, daily error summaries, or error trend charts.

## Read First

1. `AGENTS.md`
2. `.agents/context.json`
3. `.agents/rules/error_log_analysis.md`
4. Backend error analysis services, routers, and tests
5. Frontend error-analysis pages and chart components if display behavior changes

## Analysis Order

1. Identify whether the task concerns raw parsing, aggregation, API shape, or visualization.
2. Trace the current flow from ErrorLog file to backend analysis result to frontend chart/table.
3. Check date, module, severity, and event grouping behavior.
4. Compare with existing tests before changing aggregation logic.
5. Preserve stable API fields unless the requested behavior requires a contract change.

## Before Editing

- Confirm the affected endpoint or component.
- Avoid changing unrelated LOTDATA or InfoLog parser behavior.
- Consider timezone and date-boundary effects when working with daily summaries.

## Verification

- Run focused error aggregation or analysis tests first.
- Run `pytest backend/tests` for shared backend changes.
- Run `npm --prefix frontend run test` if frontend error-analysis components changed.

## Failure Handling

If a missed edge case or failed aggregation assumption is found, record the prevention rule in `FAILURE_LOG.md`.
