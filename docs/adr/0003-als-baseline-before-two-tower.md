# 0003. ALS baseline before a two-tower model

- **Status:** Accepted
- **Date:** 2026-09-25

## Context

Two options for collaborative filtering:

- **ALS matrix factorization** (`implicit` library): trains in minutes and has few hyperparameters.
- **Two-tower neural model** (PyTorch): uses item and user features as well as IDs, which helps cold start, but needs a feature pipeline, negative sampling, and temperature tuning. Each of these can be wrong without any visible error.

Without a baseline there is no way to tell whether a neural model's score is good. Published results ([Rendle et al., 2020](https://arxiv.org/abs/2005.09683)) also show that well-tuned matrix factorization often matches or beats neural approaches on ID-only data. The value of a two-tower model is in the item features.

## Decision

Build in this order:

1. Content-based retrieval (text embeddings)
2. ALS on MovieLens, with the offline evaluation pipeline
3. Hybrid of ALS and content-based retrieval as the baseline
4. Two-tower model, evaluated against that baseline on the same split

## Consequences

- A working recommender early, and a trustworthy reference point for every later model.
- The evaluation pipeline is built first, so each new model becomes one more row in the results table.
- The two-tower model has a concrete job: a single model that covers cold start and TV series (which MovieLens lacks).
