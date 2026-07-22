# Workflow Builder Agent

## Workspace and boundaries

- Python 3.13 `uv` workspace. Install all workspace dependencies with `uv sync --all-packages`.
- `csa_domain` is the REST/WebSocket/LangGraph entrypoint and Temporal client; it starts and polls workflows but does not declare them.
- `csa_worker` executes Temporal workflows, signals, and agents. Keep agent-specific code in `csa_worker/agents/<name>/`; common worker code belongs above that directory.
- `csa_shared_kernel` contains only contracts shared by domain and worker (models, Temporal names, storage keys, small utilities). Do not put business logic or infrastructure there.
- Within packages, dependencies point inward: `adapters -> application -> domain`. Infrastructure configuration is in `infrastructure/config`; adapters implement application ports.

## Runtime and configuration

- The normal local setup requires the separately maintained dev-kit: copy its `dev/` directory into the repository root before using `bash dev/...` scripts. Copy each service's `.env.example` to `.env`; domain and worker must use matching database, Redis, Temporal host, and task-queue settings.
- Apply database migrations and seed data with `uv run --package csa-domain csa-domain-migrate`.
- Run services separately with `uv run --package csa-domain csa-domain` and `uv run --package csa-worker csa-worker`. Set `WORKER_AGENTS` to `all`, `core,<agent>`, or an agent list; `csa-worker-<agent>` starts only that agent's queue and needs core elsewhere.
- `graph_app.py` is the platform launcher configured by `langgraph.json`; it imports the domain graph and starts the worker as a child process. Do not use it as the normal local two-service runner.
- Deployment-varying settings belong in env-backed `*Settings`, with no business defaults; do not instantiate settings at import time. Fixed algorithmic tuning belongs in code.

## Compatibility and style

- Treat prompt YAML changes as behavior changes; do not change prompt text unless explicitly requested.
- Preserve camelCase wire fields at API/widget boundaries. Docstrings are Russian; keep log messages ASCII and shaped as `area: event key=value`.
- Temporal workflow, signal, and shared queue names are cross-service contracts: place shared names in `csa_shared_kernel` and keep actual queue names env-configured.

## Verification

- Lint: `uv run ruff check .`; format: `uvx ruff@0.15.16 format .`.
- Run an affected package: `uv run --package csa-domain pytest`, `uv run --package csa-worker pytest`, or `uv run --package csa-shared-kernel pytest`. Target one test with, for example, `uv run --package csa-domain pytest csa_domain/tests/path/to/test_file.py::test_name`.
- Integration tests require PostgreSQL, Redis, and Temporal and may skip without them. Use `pytest -m "not integration"` for the fast suite; this is also what pre-commit runs for each service package.
