# Domain Knowledge Base — UC Talent

Entry point cho Cursor/agent trước khi implement business logic hoặc API mới.

## Mục đích

- Tránh duplicate logic giữa use case, controller, service, frontend hook
- Xác định repo/module nào sở hữu nghiệp vụ nào
- Map endpoint → use case (schema chi tiết dùng Swagger/OpenAPI)

## Quy tắc cho agent

1. Đọc [OWNERSHIP.md](OWNERSHIP.md) khi task liên quan business logic
2. Đọc `docs/domains/<module>/` trước khi thêm use case, port, endpoint
3. Đọc `docs/contracts/` khi task cross-repo (NestJS ↔ FastAPI ↔ frontend)
4. Không copy OpenAPI schema vào doc — link tới Swagger
5. Khi đổi business rule → cập nhật domain doc (xem `_templates/`)

## Quick navigation

### Ownership & contracts

| Doc | Mô tả |
|-----|-------|
| [OWNERSHIP.md](OWNERSHIP.md) | Ai sở hữu logic nào, anti-patterns |
| [contracts/nestjs-fastapi.md](contracts/nestjs-fastapi.md) | CV, UPP queue, internal webhooks |
| [contracts/nestjs-embedding.md](contracts/nestjs-embedding.md) | Job index, semantic search |
| [contracts/frontend-api-map.md](contracts/frontend-api-map.md) | Frontend hook → backend endpoint |

### NestJS domains (uc-talent-backend)

| Module | Docs | Use cases | Repo path |
|--------|------|-----------|-----------|
| job | [domains/job/](domains/job/) | 37 | `uc-talent-backend/src/modules/job/` |
| user | [domains/user/](domains/user/) | 29 | `uc-talent-backend/src/modules/user/` |
| talent | [domains/talent/](domains/talent/) | 25 | `uc-talent-backend/src/modules/talent/` |
| upp | [domains/upp/](domains/upp/) | 23 | `uc-talent-backend/src/modules/upp/` |
| notification | [domains/notification/](domains/notification/) | 20 | `uc-talent-backend/src/modules/notification/` |
| blockchain | [domains/blockchain/](domains/blockchain/) | 19 | `uc-talent-backend/src/modules/blockchain/` |
| organization | [domains/organization/](domains/organization/) | 18 | `uc-talent-backend/src/modules/organization/` |
| social | [domains/social/](domains/social/) | 16 | `uc-talent-backend/src/modules/social/` |
| admin | [domains/admin/](domains/admin/) | 16 | `uc-talent-backend/src/modules/admin/` |
| auth | [domains/auth/](domains/auth/) | 15 | `uc-talent-backend/src/modules/auth/` |
| skill | [domains/skill/](domains/skill/) | 10 | `uc-talent-backend/src/modules/skill/` |
| partner | [domains/partner/](domains/partner/) | 10 | `uc-talent-backend/src/modules/partner/` |
| file-storage | [domains/file-storage/](domains/file-storage/) | 10 | `uc-talent-backend/src/modules/file-storage/` |
| notes | [domains/notes/](domains/notes/) | 8 | `uc-talent-backend/src/modules/notes/` |
| location | [domains/location/](domains/location/) | 7 | `uc-talent-backend/src/modules/location/` |
| payment | [domains/payment/](domains/payment/) | 6 | `uc-talent-backend/src/modules/payment/` |
| internal | [domains/internal/](domains/internal/) | 2 | `uc-talent-backend/src/modules/internal/` |
| other | [domains/other/](domains/other/) | 1 | `uc-talent-backend/src/modules/other/` |
| group | [domains/group/](domains/group/) | 1 | `uc-talent-backend/src/modules/group/` |
| auto-complete | [domains/auto-complete/](domains/auto-complete/) | 1 | `uc-talent-backend/src/modules/auto-complete/` |
| settings | [domains/settings/](domains/settings/) | 0 | `uc-talent-backend/src/modules/settings/` |

Mỗi domain folder gồm: `OVERVIEW.md`, `USE-CASES.md`. Module phức tạp thêm `API.md`, `FLOWS.md`.

### FastAPI & embedding services

| Service | Docs | Repo |
|---------|------|------|
| FastAPI (CV, UPP, AI) | [services/fastapi/](services/fastapi/) | `uc-backend-fastapi/` |
| Embedding (index, search) | [services/embedding/](services/embedding/) | `uc-embedding-fastapi/` |

### Swagger / OpenAPI (schema — không duplicate ở đây)

| Service | URL |
|---------|-----|
| NestJS | `/api` + module controllers; GraphQL playground |
| FastAPI | `http://localhost:8000/docs` |
| Embedding | service `/docs` |

### Templates

Copy từ [\_templates/](_templates/) khi tạo hoặc cập nhật domain doc.
