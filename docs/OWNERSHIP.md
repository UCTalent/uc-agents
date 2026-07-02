# Business Logic Ownership

Single source of truth: **ai sở hữu nghiệp vụ nào** và **không implement ở đâu**.

## Bảng ownership

| Nghiệp vụ | Owner repo / module | Không implement tại |
|-----------|---------------------|-------------------|
| Apply job, referral, close job, job list/detail | `uc-talent-backend` → `job` | FastAPI, frontend hooks |
| Job apply list, job referral list | `uc-talent-backend` → `job` (`GetJobAppliesUseCase`, `GetJobReferralsUseCase`) | Duplicate query trong controller |
| Talent profile, resume upload, education/experience | `uc-talent-backend` → `talent` | FastAPI (trừ parse CV) |
| User account, profile picture, username | `uc-talent-backend` → `user` | Frontend local state |
| Auth JWT, OAuth, Web3 login | `uc-talent-backend` → `auth` | Frontend (chỉ gọi API) |
| Organization CRUD, logo | `uc-talent-backend` → `organization` | Job module |
| Payment distribution, wallet | `uc-talent-backend` → `payment` | Job use case (dùng port out) |
| Blockchain events, sign job outcome | `uc-talent-backend` → `blockchain` | Job module (dùng port out) |
| Email/notification templates | `uc-talent-backend` → `notification` | Use case gửi mail trực tiếp |
| UPP points, leaderboard, challenges (runtime) | `uc-backend-fastapi` → `/v2` | NestJS (trừ queue webhook) |
| CV extract / evaluate / preview | `uc-backend-fastapi` | NestJS controller logic |
| Job recommendation, career AI, talent sourcing | `uc-backend-fastapi` → `/v1/recommendation`, `/v1/talent_sourcing` | NestJS duplicate AI |
| AI chat agent | `uc-backend-fastapi` → `/v1/agent` | NestJS |
| Job embedding + OpenSearch index | `uc-embedding-fastapi` → `/index` | NestJS, FastAPI |
| Semantic search (jobs/resumes) | `uc-embedding-fastapi` → `/search` | Duplicate ES query trong NestJS |
| CMS content (FAQ, articles) | `ucl-web-cms` (Strapi) | Backend NestJS |
| Public job board UI | `uc-frontend-nextjs-v1` | ATS app |
| ATS kanban, HR workflows | `uc-ats-react-app` | Next.js frontend |

## Anti-patterns (đã có trong code — không lặp lại)

### Job module

- `ApplyJobUseCase` đã gọi `talentPort.ensureTalentProfile` — **không** tạo helper tương tự ở controller/resolver khác
- `ORGANIZATION_LOGO_PORT_OUT` dùng chung bởi `GetJobsUseCase` và `GetJobUseCase` — **reuse port**, không copy enrich logo
- `WEB3_EVENTS_FOR_JOBS_PORT_OUT` và `PAYMENT_DISTRIBUTIONS_FOR_JOBS_PORT_OUT` — enrich list/detail job, không query trực tiếp trong presentation
- CV extract sau apply → `JOB_APPLY_CV_EXTRACT_PORT_OUT` — không gọi FastAPI từ controller

### Talent module

- `ensure-talent-profile` use case — entry point khi cần talent record; job apply dùng port out, không duplicate

### Auth

- Login/register/Web3 — chỉ qua `auth` use cases; frontend không implement token validation logic

### Cross-service

- NestJS gọi FastAPI CV qua internal client / port adapter — xem [contracts/nestjs-fastapi.md](contracts/nestjs-fastapi.md)
- Job index sau publish → embedding service — xem [contracts/nestjs-embedding.md](contracts/nestjs-embedding.md)

## Trước khi thêm code mới

1. Search `docs/domains/<module>/USE-CASES.md`
2. Nếu behavior đã có → extend use case hoặc thêm port out
3. Nếu cross-module → check port out đã tồn tại chưa
4. Nếu cross-repo → check [contracts/](contracts/)
