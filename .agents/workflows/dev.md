---
description: Start Etcher Solution locally for development
---

# Development Workflow

Use this workflow when the task requires running the app locally. Do not start production services or kill existing processes unless the user asked for it or approved it.

## Preferred Startup

1. Check the current repo state.
   Run `git status --short`

2. Start the combined local launcher when a full app session is needed.
   Run `./start_app.ps1`

The launcher is intended to start the FastAPI backend on port `8080` and the Next.js frontend on port `3003`.

## Frontend Only

1. Install frontend dependencies only if `frontend/node_modules` is missing and the user approves dependency installation when needed.

2. Start the frontend dev server.
   Run `npm --prefix frontend run dev`

## Backend Only

Run Uvicorn against `backend/app/main.py` on port `8080` using the project's available Python environment.

Current note: `start_app.ps1` references `backend/requirements.txt`, but that file is not present in the current workspace. Do not assume a fresh backend environment can be recreated until that dependency source is reconciled.
