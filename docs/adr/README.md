# Architecture decision records

Short records of significant decisions: the context, what was decided, and the consequences. Format based on [Michael Nygard's template](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions).

To add one, copy `0000-template.md`, give it the next number, and link it below. Records are never deleted; a reversed decision gets a new record that supersedes the old one.

| # | Decision | Status |
|---|---|---|
| [0001](0001-monorepo-with-uv-workspace.md) | Monorepo with a uv workspace | Accepted |
| [0002](0002-postgres-with-pgvector.md) | PostgreSQL with pgvector as the only datastore | Accepted |
| [0003](0003-als-baseline-before-two-tower.md) | ALS baseline before a two-tower model | Accepted |
| [0004](0004-no-synthetic-data-for-cf-training.md) | No LLM-generated data for training collaborative filtering | Accepted |
