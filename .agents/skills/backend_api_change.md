# Skill: Backend API Change

Use this skill for FastAPI routers, services, Pydantic models, parsing endpoints, realtime streams, and backend tests.

## Read First

1. `AGENTS.md`
2. `.agents/context.json`
3. `.agents/reference/api_v1_reference.md`
4. Relevant `.agents/rules/*.md` files for the affected log type
5. `backend/app/main.py`
6. Relevant router, service, parser, config, and test files

## Analysis Order

1. Identify the endpoint, service, or parser responsible for the behavior.
2. Trace request inputs, service logic, response shape, and frontend consumers.
3. Check existing tests and API reference docs when available.
4. Keep response contracts stable unless the task explicitly changes them.
5. Add narrow validation and error handling where the API boundary needs it.

## Before Editing

- Confirm whether this is a router change, service change, parser change, or integration change.
- Avoid mixing unrelated endpoint cleanup into the same change.
- Do not change external API keys, environment variable semantics, or production startup behavior without approval.

## Verification

- Run focused backend tests for the changed area.
- Run `pytest backend/tests` for shared router, parser, or service changes.
- If frontend API callers are affected, inspect or test the corresponding frontend code.
- If public API routes or contracts change, update `.agents/reference/api_v1_reference.md`.

## Failure Handling

If an API assumption, missing test, or response contract mismatch is discovered, add a row to `FAILURE_LOG.md`.
