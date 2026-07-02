# FastAPI Service — Overview

Repo: `uc-backend-fastapi/`

AI, CV, UPP, job recommendation, talent sourcing, chat agent. NestJS giữ core business entities.

## Route groups

| Group | Prefix | File | Purpose |
|-------|--------|------|---------|
| CV extraction | `/v1/cv` | `cv_extraction.py` | Public CV parse |
| CV eval (user) | `/eval` | `cv_extract_and_evaluate.py` | Upload + evaluate + persist |
| CV internal | `/v2/internal/cv` | `cv_internal.py` | Service-to-service CV |
| UPP | `/v2` | `upp.py` | Referral, points, leaderboard, challenges |
| Internal queue | `/v2/internal` | `internal.py` | Signup/onboarding webhooks |
| Job recommendation | `/v1/recommendation` | `job_recommendation.py` | ES search, career AI, CV-JD match |
| Talent sourcing | `/v1/talent_sourcing` | `talent_sourcing.py` | Recruiter sourcing workflow |
| Chat agent | `/v1/agent` | `chat_agent.py` | Conversational AI |

## Docs

- [API.md](API.md) — endpoint map
- [contracts/nestjs-fastapi.md](../../contracts/nestjs-fastapi.md) — NestJS integration
- Existing: `uc-backend-fastapi/API_DOCUMENTATION.md`, `uc-backend-fastapi/docs/`

## Swagger

`http://localhost:8000/docs` — schema source of truth

## Auth patterns

| Pattern | Used for |
|---------|----------|
| API key (`x-api-key`) | CV, recommendation, talent sourcing |
| JWT Bearer | UPP user endpoints, chat agent, some CV routes |
| Internal API key | `/v2/internal/*` |
