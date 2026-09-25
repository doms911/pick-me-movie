# PickMeMovie

**Find something to watch in seconds, not half an hour.**

PickMeMovie is an end-to-end recommendation system for movies and TV series. A user tells it what they're in the mood for with a few clicks (movie or series, a couple of titles they like) and optionally a free-text request ("something short and funny for tonight"), and gets back a handful of recommendations — each with a short explanation of why it was picked and where it's available to stream.

## Why

Choosing what to watch is a daily annoyance: endless scrolling across streaming apps, generic "trending" lists, and recommendations locked inside each platform. This project is an attempt to solve that properly, and to build a production-shaped ML system end to end — data pipelines, models, evaluation, serving, and UI.

## How it works

1. **Candidate generation** — pulls a few hundred candidates from several sources: collaborative filtering (users with similar taste), content similarity (embeddings of synopsis, genres and keywords), and popularity as a cold-start fallback.
2. **Filtering** — removes titles of the wrong type, already-seen titles, and titles not available in the user's region.
3. **Ranking** — blends candidate scores into a single ranked list.
4. **Agent** — an LLM layer interprets free-text requests, re-ranks the top candidates against them, and explains each pick. The recommender is a tool the agent calls; the agent does not invent recommendations on its own.

See [docs/architecture.md](docs/architecture.md) for diagrams and details.

## Roadmap

- [ ] **Phase 1 — Content-based MVP:** TMDB catalog ingest, text embeddings in pgvector, `POST /recommendations` endpoint, movie/series selection
- [ ] **Phase 2 — Collaborative filtering:** ALS on MovieLens, offline evaluation pipeline (recall@k, NDCG@k), hybrid CF + content baseline
- [ ] **Phase 3 — Agent:** free-text requests, re-ranking and explanations, evaluated on a synthetic query set
- [ ] **Phase 4 — Two-tower model and feedback loop:** neural retrieval compared against the baseline, logging user feedback, periodic retraining

## Tech stack

| Area | Choice |
|---|---|
| API and serving | Python, FastAPI |
| Database | PostgreSQL + pgvector |
| Models | `implicit` (ALS), sentence-transformers, PyTorch (two-tower) |
| Agent | LLM with tool calling |
| Frontend | Angular |
| Tooling | uv workspace, Docker Compose |
| Data | [TMDB API](https://developer.themoviedb.org/), [MovieLens](https://grouplens.org/datasets/movielens/) |

## Getting started

Requirements: Python 3.12+, [uv](https://docs.astral.sh/uv/), Docker.

```bash
git clone <repo-url>
cd pickmemovie
uv sync --all-packages
cp .env.example .env   # add your TMDB API key
```

Docker Compose setup and the ingest pipeline are coming in Phase 1.

## Project structure

```
pickmemovie/
├── backend/      FastAPI app: API, recommender, agent
├── pipelines/    Batch jobs: data ingest, embeddings, model training
├── frontend/     Angular UI
├── notebooks/    Exploration and EDA
└── docs/         Architecture, decision records, evaluation
```

## Documentation

- [Architecture](docs/architecture.md)
- [Architecture decision records](docs/adr/)

## Attribution

This product uses the TMDB API but is not endorsed or certified by TMDB. Rating data from MovieLens, provided by GroupLens Research.