# Embedding Service — Overview

Repo: `uc-embedding-fastapi/`

Vector embeddings + OpenSearch indexing. Consumed by other services, not end users directly.

## Route groups

| Group | Prefix | File | Purpose |
|-------|--------|------|---------|
| Health | `/` | `health.py` | health, healthz, readyz |
| Indexing | `/index` | `indexing.py` | Job embed + index |
| Search | `/search` | `search.py` | Semantic search |

## Docs

- [API.md](API.md)
- [contracts/nestjs-embedding.md](../../contracts/nestjs-embedding.md)

## Related docs in repo

- `uc-embedding-fastapi/docs/rag-and-reranking-pipeline.md`
- `uc-embedding-fastapi/docs/redis-queue-and-processing.md`
