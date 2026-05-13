# Etcher Solution Runtime Stack

This file stores stack, version, port, startup, and verification facts for agents. Prefer current package/config files when this document conflicts with code.

## Backend Runtime

- App entrypoint: `backend/app/main.py`
- Server framework: FastAPI
- ASGI server: Uvicorn
- Language/runtime: Python 3.x, with project docs indicating Python 3.10+
- Settings: `backend/app/core/config.py`
- Default backend port: `8080`
- Router mount pattern: routers define `/v1/...` paths and `main.py` mounts them with prefix `/api`, producing public `/api/v1/...` routes.
- Dependency manifest status: `backend/requirements.txt` is referenced by `start_app.ps1` and `start_prod.ps1`, but is not present in the current workspace. Treat fresh backend environment recreation as unresolved until this is fixed.
- Middleware:
  - `GZipMiddleware` with `minimum_size=500`
  - permissive `CORSMiddleware`
- Main backend libraries:
  - Pydantic / pydantic-settings
  - pandas
  - numpy
  - requests
  - python-multipart
  - google-generativeai / Gemini integration

## Frontend Runtime

- App root: `frontend`
- Framework: Next.js `16.1.6`
- UI runtime: React `19.2.3`
- Language: TypeScript `^5`
- Styling: Tailwind CSS `^4`
- Default frontend port: `3003`
- Dev command: `npm --prefix frontend run dev`
- Build command: `npm --prefix frontend run build`
- Test command: `npm --prefix frontend run test`
- Main frontend libraries:
  - Radix UI
  - Lucide React
  - ECharts / `echarts-for-react`
  - Recharts
  - Framer Motion
  - date-fns
  - react-calendar / react-day-picker
  - react-markdown
  - clsx / tailwind-merge / class-variance-authority

## Data And Environment

- Default log data root: `data/lotlog`
- Realtime history default: `data/history` unless overridden.
- Model artifact default: `backend/model_artifacts`
- Environment file lookup is configured through `backend/app/core/config.py`.
- Gemini uses `GEMINI_API_KEY` from settings/env.

## Local Startup

- Combined local launcher: `./start_app.ps1`
- Production launcher: `./start_prod.ps1`
- Backend manual startup pattern: run Uvicorn against `backend/app/main.py` on port `8080`.
- Frontend manual startup pattern: run the Next.js dev/start scripts from `frontend/package.json` on port `3003`.
- Startup caveat: the PowerShell launchers currently attempt to install from missing `backend/requirements.txt`; existing environments may still run, but new setup is not fully documented.

## Verification Commands

- Frontend tests: `npm --prefix frontend run test`
- Frontend build: `npm --prefix frontend run build`
- Backend tests: `pytest backend/tests`
- Documentation-only changes: verify file existence, path references, and Markdown readability.

## Maintenance Rule

When package versions, ports, startup scripts, environment settings, or verification commands change, update this file and the relevant workflow or skill file.
