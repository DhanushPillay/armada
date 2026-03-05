# Contributing to Armada

Thanks for your interest in contributing to Armada! This document outlines how to get started.

## Getting Started

1. Fork the repository
2. Clone your fork locally
3. Set up the local development environment:

```bash
# Bootstrap the database
cd bootstrap
pip install -r requirements.txt
python bootstrap_database.py
python bootstrap_secrets.py
python bootstrap_cluster_resources.py
cd ..

# Create a project
bash create-project.sh

# Start all services
cd local
docker compose up --build
```

The dashboard should be available at `http://localhost:3000`.

## Making Changes

1. Create a branch from `master` for your changes
2. Make your changes, keeping commits focused and atomic
3. Run the tests before submitting:

```bash
pytest tests/
```

4. Open a pull request against `master`

## Project Structure

Armada is a monorepo organized as follows:

### Services (`services/`)

- `services/orchestrator` — FastAPI orchestration API (merges configs, pushes to Redis, deploys K8s Jobs, dispatches via RabbitMQ)
- `services/agent` — Celery worker that loads its config from Redis and executes user-provided Python code
- `services/backend` — FastAPI monitoring backend with REST endpoints for runs, jobs, and events, plus WebSocket broadcasting
- `services/frontend` — React/TypeScript dashboard (Launch and Monitor panels)
- `services/proxy-provider` — FastAPI service for selecting, rotating and verifying proxies from SQL Server
- `services/fingerprint-provider` — FastAPI service for fetching and forging browser fingerprints (Arkose)
- `services/project` — Project template copied by `create-project.sh` when scaffolding a new project

### Libraries (`lib/`)

- `lib/fantomas` — In-house browser automation library built on nodriver (WindMouse cursor paths, xdotool mode, virtual screen)

### Infrastructure & Tooling

- `bootstrap/` — Python scripts to create SQL Server tables, Kubernetes secrets, and deploy via Helm
- `deploy/` — Helm chart with templates for all services (Redis, RabbitMQ, backend, orchestrator, etc.)
- `local/` — Docker Compose setup for local development (7 services including Redis and RabbitMQ)
- `first-try/` — Self-contained demo (Flask-based "Twittor" website + pre-configured project)
- `tools/` — Bulk data import scripts for SQL Server tables
- `tests/` — End-to-end test suites for each service and integration (database, fantomas, proxy, etc.)

## Guidelines

- Keep pull requests small and focused on a single concern
- Follow the existing code style in each service (Python for backend, TypeScript for frontend)
- Add tests for new functionality when applicable
- Update documentation if your changes affect user-facing behavior

## Reporting Issues

If you find a bug or have a feature request, please open an issue on GitHub with:

- A clear description of the problem or suggestion
- Steps to reproduce (for bugs)
- Expected vs actual behavior

## License

By contributing, you agree that your contributions will be licensed under the [GNU Affero General Public License v3](LICENSE).
