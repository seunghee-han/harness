# Skill: Analyze LOTDATA Log

Use this skill when a task involves LOTDATA parsing, wafer/lot process values, cycle time, recipe data, or LOTDATA-derived dashboard behavior.

## Read First

1. `AGENTS.md`
2. `.agents/context.json`
3. `.agents/rules/LOTDATA_LOG_ANALYSIS.md`
4. Relevant parser, router, service, and test files under `backend/`
5. Relevant frontend display components only if UI behavior is affected

## Analysis Order

1. Identify the exact LOTDATA input shape and expected output.
2. Find the current parser path and downstream API response shape.
3. Check whether the change affects dashboard tables, charts, cycle time, or correlation behavior.
4. Inspect existing tests before modifying parser behavior.
5. Preserve backward compatibility unless the task explicitly changes the contract.

## Before Editing

- Confirm whether the task is a parser fix, API response change, UI display change, or test-data issue.
- Avoid rewriting unrelated parser branches.
- Note any ambiguous log format assumptions in the implementation notes or final response.

## Verification

- Run the focused LOTDATA-related backend tests first.
- If the change touches shared parser behavior, run `pytest backend/tests`.
- If the frontend consumes changed fields, run `npm --prefix frontend run test`.

## Failure Handling

If a wrong LOTDATA assumption causes a failed test or incorrect result, add a row to `FAILURE_LOG.md` with the new prevention rule.
