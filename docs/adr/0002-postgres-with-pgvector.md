# 0002. PostgreSQL with pgvector as the only datastore

- **Status:** Accepted
- **Date:** 2026-09-25

## Context

The system stores a title catalog, users and interactions (relational data), and item embeddings that need fast nearest-neighbour search. The catalog is in the tens of thousands of titles, not millions.

A dedicated vector database (Qdrant, Pinecone, Weaviate) is an option, but it adds a second datastore to run, back up and keep in sync.

## Decision

Use PostgreSQL with the [pgvector](https://github.com/pgvector/pgvector) extension for everything. Embeddings live in `vector(n)` columns with an HNSW index, next to the catalog rows they describe.

## Consequences

- One service to run. Filters (type, region, already seen) and vector search go into the same SQL query, with no cross-system joins.
- Transactions cover both catalog and vector updates, so the two never drift apart.
- pgvector is slower than dedicated engines at very large scale. At this catalog size that doesn't matter; revisit if the catalog grows by orders of magnitude.
