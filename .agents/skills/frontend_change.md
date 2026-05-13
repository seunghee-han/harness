# Skill: Frontend Change

Use this skill for Next.js, React, TypeScript, Tailwind, charts, layout, and frontend test work.

## Read First

1. `AGENTS.md`
2. `.agents/context.json`
3. `.agents/rules/frontend_design.md`
4. `frontend/package.json`
5. Existing page, component, hook, API client, and test files relevant to the task

## Analysis Order

1. Identify the route or component that owns the behavior.
2. Check existing design and component patterns before adding new UI structure.
3. Trace API usage through `frontend/app/api.ts` or nearby callers.
4. Inspect related tests before editing.
5. Keep layout stable across loading, empty, error, and populated states.

## Before Editing

- Confirm whether the change is visual, data-flow, routing, or test-only.
- Use existing UI primitives and chart libraries already in the project.
- Avoid broad style rewrites and unrelated component cleanup.
- Keep user-facing copy concise and product-like.

## Verification

- Run `npm --prefix frontend run test`.
- Run `npm --prefix frontend run build` when changing routes, Next.js config, dynamic imports, or build-sensitive code.
- For API contract changes, verify matching backend endpoints or tests.

## Failure Handling

If a design rule, type issue, or test failure reveals a reusable lesson, add it to `FAILURE_LOG.md`.
