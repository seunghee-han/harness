---
description: Verify and build Etcher Solution
---

# Build And Verification Workflow

Use the smallest verification that matches the change. Prefer focused tests before broad checks.

## Documentation-Only Changes

1. Confirm referenced files and paths exist.
2. Read the changed Markdown to verify it is understandable.
3. Search for placeholders, encoding damage, and stale paths when relevant.

## Frontend Changes

1. Run frontend tests.
   Run `npm --prefix frontend run test`

2. Run the frontend build when routes, Next.js config, dynamic imports, or build-sensitive code changed.
   Run `npm --prefix frontend run build`

## Backend Changes

1. Run focused backend tests for the changed router, service, parser, or API behavior when available.

2. Run the full backend test suite for shared backend changes.
   Run `pytest backend/tests`

## API Contract Changes

1. Run the relevant backend tests.
2. Inspect frontend API callers in `frontend/app/api.ts` or nearby components.
3. Update `.agents/reference/api_v1_reference.md` if public routes, request bodies, response shapes, or error behavior changed.

## Runtime Note

The current workspace does not contain `backend/requirements.txt`, although `start_app.ps1` and `start_prod.ps1` reference it. Treat backend dependency recreation as unresolved until the launcher or dependency manifest is fixed.
