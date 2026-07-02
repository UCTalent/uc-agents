---
name: GitHub PR Review — UC Talent Workspace
description: 
  Review a GitHub PR diff against project rules for UC Talent repos
  (uc-talent-backend, uc-backend-fastapi, uc-embedding-fastapi, uc-frontend-nextjs-v1, uc-ats-react-app, ucl-web, ucl-web-cms)
  and generate a complete, ready-to-paste PR description with risks identified.
---

## Purpose

When the user provides a GitHub Pull Request URL, fetch and review the PR diff, then:

1. Identify which UC Talent repo(s) are affected.
2. Apply the correct project rules for that repo.
3. Produce a clean PR description using the matching template.
4. Call out rule violations, risks, and missing coverage explicitly.

---

## Input

User provides a GitHub PR URL:

```
https://github.com/org/repo/pull/123
```

---

## Workflow

### Step 1 — Fetch the PR

Open the PR URL. Read:

- PR title and description (if any).
- Base branch (prefer actual base; fall back to `main`).
- Full diff: files changed, additions, deletions.
- Branch name and commit messages — extract Jira ticket key if visible (e.g. `UCT-123`).

### Step 2 — Identify the target repo

Map changed file paths to UC Talent repos:

| File path prefix                              | Repo                     |
| --------------------------------------------- | ------------------------ |
| `uc-talent-backend/src/modules/**`            | `uc-talent-backend`      |
| `uc-talent-backend/src/domains/**`            | `uc-talent-backend` (legacy) |
| `uc-backend-fastapi/**/*.py`                  | `uc-backend-fastapi`     |
| `uc-embedding-fastapi/**/*.py`                | `uc-embedding-fastapi`   |
| `uc-frontend-nextjs-v1/**`                    | `uc-frontend-nextjs-v1`  |
| `uc-ats-react-app/**`                         | `uc-ats-react-app`       |
| `ucl-web/**`                                  | `ucl-web`                |
| `ucl-web-cms/**`                              | `ucl-web-cms`            |

If multiple repos are touched, use the **backend PR template** and cover all repos under `Changes Made`.

### Step 3 — Classify the PR type

**Backend NestJS** (`uc-talent-backend`):
- DDD Hexagonal modules, controllers, use cases, domain entities, repositories, DTOs, port interfaces, TypeORM entities, migrations, GraphQL resolvers.

**Backend FastAPI** (`uc-backend-fastapi`, `uc-embedding-fastapi`):
- Python routes, services, schemas, ORM models, migrations, AI/GenAI, embeddings, queues.

**Frontend MUI** (`uc-frontend-nextjs-v1`, `uc-ats-react-app`):
- React/Next.js components, pages, hooks, MUI `sx` styling, API calls, forms, state management.

**UCL web** (`ucl-web`):
- Next.js App Router, Tailwind, shadcn, Strapi consumer via `cmsService`.

**Strapi CMS** (`ucl-web-cms`):
- Strapi 5 schemas, controllers, plugins; verify consumer contract if schema changes.

**Fullstack**: touches both backend and frontend → use backend template, mention both.

### Step 4 — Apply project rules

Run the compliance check for the detected repo(s) before writing the description.

---

## Project Rules Compliance Check

### uc-talent-backend — DDD Hexagonal (NestJS)

Check dependency direction. Allowed:
```
presentation → application → domain
infrastructure → application → domain
```

Flag if violated:
- Domain imports NestJS decorators (`@Entity`, `@Column`, `@Injectable`, `@Controller`, `@ObjectType`, `@Field`).
- Application imports presentation DTOs, ORM entities, controllers, or infrastructure implementations.
- Presentation accesses repositories, DataSource, or external APIs directly.
- Controller/resolver contains business logic, transaction logic, or domain rules.

Check structure:
- New V2 code must live under `src/modules/<module>/` — **not** `src/domains/*`.
- Files must follow layer-specific suffixes: `*.controller.ts`, `*.use-case.ts`, `*.command.ts`, `*.query.ts`, `*.result.ts`, `*.port.ts`, `*.entity.ts`, `*.orm-entity.ts`, `*.repository.ts`, `*.persistence.mapper.ts`, `*.presentation.mapper.ts`.
- ORM entities (`*.orm-entity.ts`) belong only in `infrastructure/persistence/typeorm/entities/`.
- Domain entities (`*.entity.ts`) must be pure TypeScript — no ORM decorators.
- Use cases must implement Port In and return application result types — not response DTOs.
- Controllers must inject Port In via `@Inject(TOKEN)`, not concrete use case classes.

Check coding standards:
- No `any` — flag occurrences.
- Explicit return types on all public methods and functions.
- Type-only imports (`import type { ... }`) where applicable.
- Alias imports over deep relative imports (check `tsconfig.json`).
- Import order: external → internal aliases → relative.
- No `console.log` — use existing logger.
- Naming: `PascalCase` + suffix for classes, `camelCase` for variables, `UPPER_SNAKE_CASE` for port tokens, `kebab-case` for files.
- Error handling: throw domain/application errors in use cases; map to HTTP/GraphQL exceptions only in presentation.

Check ORM/database:
- Explicit `snake_case` column names (`@Column({ name: 'job_number' })`).
- No migration or DDL changes unless explicitly noted as approved.

Check API contract (if frontend is affected):
- Document changed endpoint path, method, request shape, response shape, enums, pagination, auth.

---

### uc-backend-fastapi / uc-embedding-fastapi — FastAPI (Python)

Architecture: `Client → routes → Depends() → services → ORM / Redis / ES / AI`

Flag if violated:
- Business logic placed directly in routes instead of `src/services/`.
- `JWTMiddleware` used — must use `Depends(get_current_user_id)` instead.
- `HTTPException` raised inside UPP service functions (service pattern A returns `(err_str, None)`).
- `basicConfig` called at import time in new modules.
- ORM queries written directly in routes (should be in services).
- New tests placed under `src/tests/` (should go under `tests/unit/` or `tests/integration/`).
- Duplicate `Base` class in models.

Check structure:
- Routes: `src/api/routes/{domain}.py`
- Schemas (Pydantic v2): `src/schemas/{domain}_schemas.py`
- Services: `src/services/{domain}_service.py`
- ORM models: `src/models/{entity}.py` registered in `models/__init__.py`
- AI clients: `src/core/ai/`
- Migrations: `migrations/*.sql` via `make migrate-generate name=...`

Check standards:
- Import order: stdlib → third-party → `src.*` (absolute only).
- Response schemas: camelCase fields. Request schemas: snake_case fields.
- Magic numbers belong in `src/config/constants.py`, not in services.
- No hardcoded secrets or API keys — must use `get_settings()`.
- Service pattern A (UPP): `(err_str, None)` tuple on failure, no `HTTPException`.
- Service pattern B (job rec/AI): class-based, raise domain errors caught in route.
- Service pattern C (CV): async functions, `Depends(get_genai_client)`.
- Auth: `require_api_key` for CV/job rec; `get_current_user_id` for UPP JWT; `verify_internal_api_key` for `/v2/internal`.

---

### uc-frontend-nextjs-v1 / uc-ats-react-app — Frontend (React/Next.js)

Flag if violated:
- `any` used — flag occurrences.
- Missing explicit return types on exported functions, hooks, API clients.
- Deep relative imports when aliases exist.
- New UI library introduced without justification (shadcn, Tailwind-only components — MUI + `sx` is default).
- Hardcoded colors, spacing, or magic values instead of MUI theme values.
- No loading/empty/error state handling for API-driven UI.
- `console.log` or `debugger` left in code.
- Missing `queryKey` variables or unstable `queryKey` in React Query.
- `"use client"` added unnecessarily to large component trees.
- `window`, `document`, or `localStorage` accessed during SSR.
- New global state added for local UI-only state.
- Form payload does not clearly match backend DTO shape.
- API contract not verified before implementing UI.

Check:
- ATS app: no Next.js-specific patterns (no SSR assumptions).
- Next.js app: SSR/hydration safe; metadata preserved on SEO pages.
- Responsive `sx` breakpoints used instead of fixed desktop-only layouts.
- Accessibility: clickable elements labeled, buttons use `button` semantics, forms have labels, images have `alt`.
- `useMemo`/`useCallback` used only where justified (expensive computation, stable dependency).

---

## Risk Identification

After checking rules, identify risks in these categories:

**Architecture risk**
- Dependency rule violations.
- ORM entity or presentation DTO leaking across layer boundaries.
- Business logic in controllers/routes.
- Domain layer contaminated with framework decorators.

**API contract risk**
- REST/GraphQL contract changed without documentation.
- Frontend field mapping may break (`camelCase` vs `snake_case`, field renames, enum changes).
- Pagination, filter, or sort params changed.
- Auth requirement changed.

**Database/schema risk**
- Migration or DDL introduced — requires explicit approval.
- Column naming mismatch.
- ORM entity changes not paired with migration.

**Breaking change risk**
- Public API behavior changed.
- Response shape changed without version control.
- Enum values added or removed.
- Nullable field changed to required.

**Test coverage risk**
- No unit tests for new use cases or services.
- No integration tests for new endpoints.
- Tests placed in wrong location (`src/tests/` instead of `tests/`).

**Scope risk**
- Large unrelated refactor mixed into feature change.
- Multiple repos changed without clear ticket justification.
- Legacy code migrated to V2 without explicit request.
- New library or dependency added without justification.

**Security risk**
- Hardcoded secrets, tokens, or environment-specific values.
- Auth guard removed or bypassed.
- `verify_internal_api_key` missing on internal webhook routes.

**Frontend-specific risk**
- API contract consumed without verification against backend.
- Hydration mismatch risk in Next.js SSR.
- Missing error/loading/empty states for API-driven UI.
- `"use client"` added unnecessarily.

---

## Output Templates

### Backend PR Template

Use for: `uc-talent-backend`, `uc-backend-fastapi`, `uc-embedding-fastapi`, or fullstack PRs.

```md
## Summary

<!-- Briefly describe what this PR changes and why. -->

## Type of Change

- [ ] **Bug fix** — non-breaking fix
- [ ] **New feature** — non-breaking addition
- [ ] **Breaking change** — existing behavior changes
- [ ] **Refactoring**
- [ ] **Chore** — build, CI/CD, dependencies
- [ ] **Documentation update**

## Related Issue / Ticket

<!-- UCT-XXX or #issue number -->

## Repos Touched

<!-- List repos affected, e.g. uc-talent-backend, uc-backend-fastapi -->

## Changes Made

<!-- Bullet list of specific changes per repo/module.
     Cover: modules, use cases, domain entities, ports, DTOs, ORM entities, services, routes, schemas, migrations, configs. -->

## Solution

<!-- How the implementation solves the problem.
     Include: main approach, logic changes, data flow, API contract changes, error handling decisions, architectural choices. -->

## API Contract (if applicable)

<!-- If any REST or GraphQL contract changed, document it here.
     Include: path/operation, method, request, response, enums, pagination, auth, errors. -->

## Risk

<!-- Describe risk areas identified from the diff.
     Cover: architecture violations, API contract impact, DB/migration impact, breaking changes, test coverage, scope concerns, security.
     If no obvious risk: "No major risk identified from the diff. Manual verification recommended." -->

## Project Rules Compliance

<!-- Summarize compliance findings.
     Flag any violations or areas where rules could not be fully verified. -->

## Testing

- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] Manual testing performed

<!-- Add specific notes on what was tested or what still needs manual verification. -->

## Checklist

- [ ] Code follows project coding standards
- [ ] Dependency direction respected (presentation → application → domain)
- [ ] Domain layer is pure TypeScript (no NestJS/TypeORM/framework decorators)
- [ ] ORM entities stay in infrastructure
- [ ] No request/response DTOs passed into application layer
- [ ] Controllers/routes are thin (no business logic)
- [ ] No migrations/DDL without explicit approval
- [ ] No new libraries introduced without justification
- [ ] No hardcoded secrets or tokens
- [ ] No unrelated formatting changes
- [ ] API contract documented if frontend is affected
- [ ] Self-review completed
```

---

### Frontend PR Template

Use for: `uc-frontend-nextjs-v1` or `uc-ats-react-app` only (no backend changes).

```md
## Summary

<!-- Briefly summarize the UI, behavior, or integration changes. -->

## Repo Touched

<!-- uc-frontend-nextjs-v1 or uc-ats-react-app -->

## Related Issue / Ticket

<!-- UCT-XXX or #issue number -->

## Changes Made

<!-- Bullet list: components, pages, hooks, API integration, forms, state, styling. -->

## Solution

<!-- How the frontend implementation works.
     Mention: components updated, hooks/state changes, API integration, form/validation changes, UI/UX behavior, responsive/styling updates. -->

## API Contract Verification

<!-- State whether backend contract was verified.
     If verified: confirm endpoint, method, request/response shape, enums, pagination, auth.
     If not verified: state it and flag as risk. -->

## Risk

<!-- Risk areas from the diff.
     Cover: UI regression, API contract alignment, state management, form payload mismatch, SSR/hydration, missing error/loading/empty states, accessibility, performance, missing tests.
     If no obvious risk: "No major risk identified from the diff. Manual verification recommended." -->

## Project Rules Compliance

<!-- Flag any violations: any usage, missing return types, hardcoded MUI values, missing states, wrong import style, console.log, debugger, etc. -->

## Testing

- [ ] Manual testing performed
- [ ] Loading/empty/error states verified
- [ ] Responsive layout checked

## Verification Commands (not run unless approved)

```bash
cd uc-frontend-nextjs-v1 && pnpm lint && pnpm typecheck && pnpm build
# or
cd uc-ats-react-app && pnpm lint && pnpm typecheck && pnpm build
```

## Checklist

- [ ] TypeScript types are correct, no `any`
- [ ] Explicit return types on hooks, API clients, exported functions
- [ ] No `console.log` or `debugger`
- [ ] Loading/error/empty states handled for all API-driven UI
- [ ] Form payload matches backend DTO shape
- [ ] API response fields used by UI exist in backend output
- [ ] Enum/status values match backend
- [ ] MUI `sx` used for styling (no unauthorized Tailwind/shadcn)
- [ ] No hardcoded colors or magic spacing values
- [ ] No unrelated file reformatting
- [ ] Responsive layout verified
- [ ] `"use client"` added only where necessary (Next.js)
- [ ] No `window`/`document`/`localStorage` in SSR context (Next.js)
```

---

## Output Rules

- Output only the filled PR description. No extra commentary unless something is ambiguous.
- Use English.
- Use Markdown. Keep it clean and paste-ready.
- Do not invent tests, tickets, screenshots, or behaviors not visible in the diff.
- If the diff cannot be accessed, ask for the diff or list of changed files before proceeding.
- If a Jira ticket key is found in the branch name, commits, or code comments, include it under `Related Issue / Ticket`.
- If no testing evidence is found in the diff, leave test checkboxes unchecked and note what should be manually verified.
- If project rules could not be fully verified from the diff alone, write: `Project rules could not be fully verified — manual review recommended for [specific areas].`
- If a rule violation is found, state it clearly under `Risk` and `Project Rules Compliance`. Do not soften violations.
