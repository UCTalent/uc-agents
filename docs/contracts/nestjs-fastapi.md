# NestJS ↔ FastAPI Contracts

Cross-service integration giữa `uc-talent-backend` và `uc-backend-fastapi`.

## Ownership reminder

| Concern | Owner |
|---------|-------|
| Business entities (Job, Talent, JobApply) | NestJS |
| CV parsing, AI evaluation | FastAPI |
| UPP points/leaderboard runtime | FastAPI `/v2` |
| Queue-driven UPP events | FastAPI `/v2/internal` webhooks |

## CV extract & evaluate

### Public / user-facing (Next.js → FastAPI)

| Endpoint | Purpose | Auth |
|----------|---------|------|
| `POST /eval/extract_and_evaluate` | Upload CV, extract + evaluate | API key + JWT |
| `POST /v1/cv/extract` | Parse CV → talent schema | API key |

### Internal (NestJS → FastAPI)

| Endpoint | Purpose | Auth |
|----------|---------|------|
| `POST /v2/internal/cv/extract_and_evaluate` | Full extract for services | Internal API key |
| `POST /v2/internal/cv/extract_preview` | Lightweight preview | Internal API key |

NestJS trigger sau job apply: port `JOB_APPLY_CV_EXTRACT_PORT_OUT` trong `ApplyJobUseCase` — không gọi FastAPI từ controller.

HR add candidate extract: `HrAddCandidateExtractResumeUseCase` → FastAPI internal.

## UPP queue webhooks (NestJS/other → FastAPI)

| Endpoint | Purpose |
|----------|---------|
| `POST /v2/internal/queue/signup` | Queue-driven signup points |
| `POST /v2/internal/queue/complete-onboarding` | Onboarding completion |

Auth: `verify_internal_api_key`.

## UPP user APIs (frontend → FastAPI directly)

Referral, points, leaderboard, challenges — xem [services/fastapi/API.md](../services/fastapi/API.md).

NestJS `upp` module có use cases riêng cho data purge / redirect; runtime UPP chủ yếu FastAPI.

## NestJS internal client

FastAPI persistence callbacks dùng NestJS internal API — xem `uc-backend-fastapi` NestJS internal client config.

## Khi implement

- CV logic mới → `uc-backend-fastapi`, cập nhật doc này + `services/fastapi/`
- NestJS chỉ orchestrate qua port adapter
- Không duplicate CV parsing trong NestJS service
