# 0001. Monorepo with a uv workspace

- **Status:** Accepted
- **Date:** 2026-09-25

## Context

The system has a frontend, an online backend (API, recommender, agent) and offline batch pipelines (ingest, embeddings, training). It is built by one person, and most changes touch several parts at once, such as a new API field that needs a schema change, backend code and frontend code.

The backend should stay light for fast startup and small images, while the pipelines need heavy dependencies such as PyTorch and sentence-transformers.

## Decision

Keep everything in one repository. Python code is split into two packages, `backend` and `pipelines`, managed as members of a single [uv workspace](https://docs.astral.sh/uv/concepts/projects/workspaces/) with one shared lockfile (`uv.lock`, committed to git).

## Consequences

- One commit can change every layer. One `docker compose up` runs the whole system.
- One lockfile gives identical versions on every machine, in Docker and in CI (`uv sync --locked`).
- Each package declares its own dependencies, so the backend image does not pull in PyTorch.
- If a part ever becomes a standalone product, it has to be extracted into its own repository.
