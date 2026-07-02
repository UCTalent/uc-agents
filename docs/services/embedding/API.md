# Embedding Service — API Map

Schema: service `/docs` when running locally.

## Health

| Method | Path | Purpose |
|--------|------|---------|
| GET | `/health` | Basic health |
| GET | `/healthz` | Liveness |
| GET | `/readyz` | Readiness (DB, OpenSearch, Redis, queue) |

## Indexing (`/index`)

| Method | Path | Purpose | Auth |
|--------|------|---------|------|
| POST | `/index/job` | Embed job + write OpenSearch | API key |
| DELETE | `/index/job/{job_id}` | Remove job from index | API key |

## Search (`/search`)

| Method | Path | Purpose | Auth |
|--------|------|---------|------|
| POST | `/search/resumes-for-job` | Matching resumes for job | API key |
| POST | `/search/jobs-for-profile` | Matching jobs for profile | API key |
| POST | `/search/similar-jobs` | Similar jobs | API key |

## Route source files

`uc-embedding-fastapi/src/api/routes/`

## Contracts

- [nestjs-embedding.md](../../contracts/nestjs-embedding.md)

## Related repo docs

- `uc-embedding-fastapi/docs/rag-and-reranking-pipeline.md`
- `uc-embedding-fastapi/docs/redis-queue-and-processing.md`
