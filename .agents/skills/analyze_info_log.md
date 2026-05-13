# Skill: Analyze InfoLog

Use this skill when a task involves InfoLog parsing, equipment status events, detail modals, or InfoLog-derived frontend displays.

## Read First

1. `AGENTS.md`
2. `.agents/context.json`
3. `.agents/rules/InfoLog_Analysis.md`
4. Relevant backend parser, router, service, and tests
5. Relevant frontend components that render InfoLog details

## Analysis Order

1. Identify the InfoLog file format and the requested output.
2. Trace the data path from parser to API to frontend consumer.
3. Compare behavior with existing tests and sample data.
4. Check whether the change affects filtering, sorting, timestamps, or modal detail rendering.
5. Keep unrelated log formats unchanged.

## Before Editing

- Confirm whether timestamp handling, encoding, grouping, or UI presentation is the main issue.
- Keep public response fields stable unless the task requires an API change.
- Add or adjust tests when changing parsing rules.

## Verification

- Run focused InfoLog or parser tests when available.
- Run `pytest backend/tests` for shared parser/API changes.
- Run `npm --prefix frontend run test` if UI consumers are changed.

## Failure Handling

If an InfoLog rule is missed or a format assumption fails, add a concise entry to `FAILURE_LOG.md`.
