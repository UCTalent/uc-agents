# NestJS / FastAPI ↔ Embedding Service

Integration với `uc-embedding-fastapi` (vector index + semantic search).

## Ownership

| Concern | Owner |
|---------|-------|
| Job document indexing (embed + OpenSearch) | `uc-embedding-fastapi` |
| Semantic search (jobs for profile, resumes for job, similar jobs) | `uc-embedding-fastapi` |
| Job CRUD, business rules | NestJS `job` module |
| ES job search (keyword/rec) | FastAPI `/v1/recommendation` |

## Indexing API

| Method | Path | Purpose |
|--------|------|---------|
| POST | `/index/job` | Embed job via Gemini, write to OpenSearch |
| DELETE | `/index/job/{job_id}` | Remove job from index |

Auth: API key.

**Khi index:** job published/updated — consumer gọi embedding service, không duplicate indexing trong NestJS.

## Search API

| Method | Path | Purpose |
|--------|------|---------|
| POST | `/search/resumes-for-job` | Matching resumes for a job |
| POST | `/search/jobs-for-profile` | Matching jobs for talent profile |
| POST | `/search/similar-jobs` | Jobs similar to reference job |

Auth: API key.

## Consumers

- FastAPI job recommendation may use ES + embedding results
- NestJS `GetSimilarJobsUseCase` — check port out adapter, không raw OpenSearch query trong use case mới

## Khi implement

- Index/search logic → `uc-embedding-fastapi` only
- Cập nhật [services/embedding/API.md](../services/embedding/API.md)
