# 0004. No LLM-generated data for training collaborative filtering

- **Status:** Accepted
- **Date:** 2026-09-25

## Context

Generating synthetic users and ratings with an LLM was considered as a data source.

Collaborative filtering learns real, often surprising co-preferences between titles. An LLM generates stereotyped users ("a sci-fi fan likes *Interstellar*, *Inception*, *The Matrix*") with a strong bias toward popular titles. A model trained on that learns the LLM, not people. Evaluating it on synthetic data is circular: the metrics look good and mean nothing.

Real data is freely available: MovieLens has tens of millions of ratings, and TMDB provides metadata.

## Decision

Train and evaluate collaborative filtering only on real interaction data (MovieLens, and later the app's own feedback).

Synthetic data is used where no real data exists:

- **Agent evaluation:** generated free-text requests with expected constraints (type, length, genre, mood)
- **End-to-end simulation:** LLM personas clicking through onboarding, to test the flow
- **Content enrichment:** generated tags and mood descriptors from synopses, as item features

## Consequences

- Offline metrics reflect real behaviour.
- TV series have no CF signal until the app collects its own interactions. Content-based retrieval and popularity cover them until then.
- Optional experiment: train the same model on synthetic and on MovieLens data and compare both on a real test set, to measure the gap.
