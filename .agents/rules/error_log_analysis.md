# Error Log Analysis Rules

This document defines the rules used to parse, aggregate, and serve `Error.log` data in Etcher Solution. Read it before changing error parsing, aggregation, dashboard alarms, or error-analysis UI behavior.

## Overview

`Error.log` files can contain cumulative historical data. Aggregation must handle overlap so repeated events from different daily files do not create duplicate summaries.

## Aggregation And Storage

- Granularity: aggregate error logs by month.
- Storage format: JSON files stored under the configured lotlog aggregation area, using `aggregated_errors/YYYY_MM.json` naming.
- File naming convention: `YYYY_MM.json`, for example `2026_01.json`.
- Aggregated JSON should be the normal source for daily summaries and paginated event lists; avoid scanning raw logs for routine frontend reads.

## Deduplication Logic

Events are identified by a hash of stable event fields:

- `Timestamp`
- `Title`
- `Content`
- `Module`

Do not include `Index` in the deduplication key because the machine log index can reset across daily files.

## Event Parsing Rules

1. Standard event:
   - Format: `[Index][Date Time][Title] Content`
   - Regex: `^\[(\d+)\]\[(.*?)\]\[(.*?)\]\s*(.*)$`
2. Legacy system start event:
   - Format: `HH:mm:ss ERROR_RESULT ...`
   - Treat as unique per file date. Identical content on different dates should remain separate events.

## Required Fields

Every parsed event must include:

- `SourceFile`: absolute or relative path to the source `Error.log` file, used by the UI for filtering and traceability.
- `Level`: derived from title or content, such as `ERROR`, `WARNING`, or `INFO`.
- `Module`: module extracted from content, such as `CM1` or `PM2`, when available.

## Public API

Routers define `/v1/analysis/errors/...` paths and `backend/app/main.py` mounts them with prefix `/api`, producing these public paths:

- `GET /api/v1/analysis/errors/latest`
- `GET /api/v1/analysis/errors/daily`
- `GET /api/v1/analysis/errors/events`
- `POST /api/v1/analysis/errors/rebuild`

Frontend code should query the plural `/analysis/errors/...` paths through the configured `/api/v1` base. Do not use the old singular error-path variant.

## Rebuild Behavior

`POST /api/v1/analysis/errors/rebuild` performs a full scan of available `Error.log` files and regenerates monthly aggregation JSON. Treat this as a deliberate maintenance action, not a normal page-load operation.

## Change Safety

- Preserve deduplication behavior unless the task explicitly changes the aggregation contract.
- Consider timezone and date-boundary effects when changing daily summaries.
- Keep public response fields stable unless the task requires an API contract change.
- Run focused error aggregation or analysis tests first, then `pytest backend/tests` for shared backend changes.
