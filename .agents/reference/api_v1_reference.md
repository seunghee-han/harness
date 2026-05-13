# Etcher Solution API v1 Reference

This reference is the harness-friendly API summary checked against the current FastAPI routers.

## Current Assessment

- The documented base URL remains correct: `http://localhost:8080/api/v1`.
- Public routes are produced by mounting routers with prefix `/api` in `backend/app/main.py`.
- This file is the current API reference for agents.
- Keep newer areas such as virtual process endpoints, realtime WebSocket/history endpoints, manual reports, and prediction endpoints represented here.
- Runtime stack, ports, and startup commands live in `.agents/reference/runtime_stack.md`.

## Error Format

FastAPI exception handlers in `backend/app/main.py` return errors in this shape:

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": []
  }
}
```

Known codes include `NOT_FOUND`, `UNAUTHORIZED`, `BAD_REQUEST`, `VALIDATION_ERROR`, and `SERVER_ERROR`.

## Logs API

Base path: `/api/v1/logs`

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/stream` | Stream a selected log file as Server-Sent Events. Requires `path`. |
| GET | `/stream/live` | Stream the latest watched log file as Server-Sent Events. |
| GET | `/files` | List log file groups under the configured lotlog root. Optional `date`. |
| GET/HEAD | `/detail` | Parse a selected log file. Requires `path`. Supports `lotdata`, `WaferData`, `Info.log`, `Error.log`, and generic PROCESS logs. |
| GET | `/status` | Parse `Status.log` next to a selected path. Requires `path`. |
| GET | `/recipe` | Parse matching `RECIPE_*` file for a PROCESS path. Requires `path`. |

Primary source: `backend/app/routers/logs.py`.

## Analysis API

Base path: `/api/v1/analysis`

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/errors/latest` | Read and parse the latest `Error.log`. |
| GET | `/errors/daily` | Return aggregated daily error summary. |
| GET | `/errors/events` | Return paginated error events. Supports `limit`, `offset`, and optional `date`. |
| POST | `/errors/rebuild` | Rebuild aggregated error summary JSON. |
| GET | `/cycle-time` | Return multi-lot cycle time analysis. Requires `path`. |
| POST | `/correlation` | Return parameter correlation for a log path and optional params list. |
| GET | `/dates` | Return available analysis dates. |
| GET | `/reports/manual` | List manual markdown reports. |
| GET | `/reports/manual/content` | Return one manual report. Requires `filepath`. |
| GET | `/prediction/metadata` | Return process prediction metadata. |
| POST | `/prediction/process` | Predict process output from recipe/equipment/process conditions. |
| POST | `/prediction/wafer-map` | Predict wafer map output from recipe/equipment/process conditions. |

Primary source: `backend/app/routers/analysis.py`.

## Dashboard API

Base path: `/api/v1/dashboard`

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/summary` | Return latest dashboard summary and AI report data. |
| GET | `/reports` | Return AI report history. |
| POST | `/reports` | Generate an AI report manually. Optional body: `date`, `force`. |
| GET | `/trends` | Return dashboard trend stats. Optional `months`, default `3`. |
| GET | `/alarms` | Return recent alarm events. Optional `limit`, `offset`. |

Primary source: `backend/app/routers/dashboard.py`.

## Virtual Process API

Base path: `/api/v1/virtual-process`

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/metadata` | Return virtual process metadata. |
| GET | `/templates` | List available recipe templates. |
| GET | `/template` | Return one recipe template. Requires `path`. |
| POST | `/predict` | Predict a virtual process from recipe name, chamber, sample rate, and steps. |

Primary source: `backend/app/routers/virtual_process.py`.

## Realtime API

Base path: `/api/v1/realtime`

| Method | Path | Purpose |
| --- | --- | --- |
| WS | `/ws/ingest` | Edge forwarder sends realtime JSON log records. |
| WS | `/ws/stream` | Frontend dashboard receives broadcast realtime records. |
| GET | `/history` | Return in-memory recent realtime history. |

Primary source: `backend/app/routers/realtime.py`.

## Agent Maintenance Rule

When API routes change, update this file and then check any matching frontend callers in `frontend/app/api.ts` or nearby components.
