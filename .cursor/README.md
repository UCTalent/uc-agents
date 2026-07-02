# UC Talent Cursor Workflow

This workspace is designed for running Cursor from the **parent workspace folder**, where all UC Talent repositories live as sibling folders.

The goal is simple:

```txt
User provides Jira key
→ Agent fetches Jira ticket via MCP
→ Orchestrator routes work to relevant repos
→ Subagents implement focused changes
→ Testing agent verifies backend + frontend API contracts
→ Final report shows AC status and verification
```

---

## Workspace Layout

Open or run agents from the parent folder:

```txt
uc-talent/
├── AGENTS.md
├── .cursor/
├── uc-talent-backend/
├── uc-backend-fastapi/
├── uc-embedding-fastapi/
├── uc-frontend-nextjs-v1/
├── uc-ats-react-app/
├── ucl-web/              # Unchain Labs marketing site
└── ucl-web-cms/          # Strapi CMS — UCL + UCTalent content
```

Always use workspace-root paths:

```txt
uc-talent-backend/src/modules/...
uc-frontend-nextjs-v1/src/...
uc-ats-react-app/src/...
ucl-web/src/...
ucl-web-cms/src/api/...
```

Do not use bare paths like `src/...` unless the current working directory is already confirmed.

---

## Daily Usage

For normal ticket work, just provide the Jira key:

```txt
UC-123
```

or:

```txt
UCT-123
```

The agent should automatically behave as if you wrote:

```txt
Use orchestrator for UC-123.

Fetch the ticket via Jira MCP, summarize the scope, route work to relevant repos, inspect only relevant code, implement small diffs, verify frontend/backend contracts, ask before running commands, and report verification status.
```

You do **not** need to paste the full ticket unless Jira MCP fails.

---

## Frontend MUI Workflow

For UI work in these repos, invoke **`$mui-frontend`** before implementing or reviewing changes:

```txt
uc-frontend-nextjs-v1/
uc-ats-react-app/
```

For **Unchain Labs** site (`ucl-web/`), use **`$implement-ucl-web`** — Tailwind/shadcn, not MUI.

For **Strapi CMS** (`ucl-web-cms/`), use **`$implement-strapi-cms`**.

**In scope** — use the skill when the task touches:

* MUI components
* layout & spacing
* responsive UI
* dialogs / modals
* tables & lists
* forms & inputs
* cards & surfaces
* theme / `sx` styling
* design consistency with nearby screens

**Out of scope** — do **not** use this skill for:

* backend (NestJS, FastAPI)
* migrations or DDL
* infrastructure / DevOps


## Jira MCP Workflow

When a Jira key is provided, the agent must fetch the ticket via Jira MCP / Atlassian MCP.

It should read:

* summary
* description
* acceptance criteria
* relevant comments
* linked issues when relevant
* attachment list when available

If Jira MCP is unavailable, blocked, or not logged in, the agent should stop and ask for either:

```txt
- Jira MCP login/fix
- pasted ticket text as fallback
```

The agent must not guess ticket requirements when MCP fetch fails.

---

## Codegraph MCP

Server `codegraph` in `.cursor/mcp.json`. Use **before** grep/Read loops when exploring indexed source.

| Intent | Tool |
|--------|------|
| Understand area / flow / symbols to change | **`codegraph_explore`** (call FIRST) |
| Find symbol by name | `codegraph_search` |
| Callers / blast radius | `codegraph_callers` |
| One symbol or read source file | `codegraph_node` |

Order: Jira → `docs/` → **Codegraph** → implement. Full policy: `uctalent-workflow.mdc` §4.2.

---

## Attachment Policy

If the Jira ticket contains attachments:

* list attachment filenames when available
* do not guess image/screenshot/design content
* do not implement UI based on unseen screenshots
* ask the user to upload the attachment if visual inspection is required

Attachment metadata is not enough to infer visual requirements.

---

## Repo Map

| Repo                     | Purpose                                                 |
| ------------------------ | ------------------------------------------------------- |
| `uc-talent-backend/`     | Main Backend V2, NestJS, DDD Hexagonal, `src/modules/*` |
| `uc-backend-fastapi/`    | FastAPI service for UPP, CV, job recommendation, queues |
| `uc-embedding-fastapi/`  | FastAPI service for embeddings, vector search, indexing |
| `uc-frontend-nextjs-v1/` | Main Next.js frontend, MUI, `sx`                        |
| `uc-ats-react-app/`      | ATS React app, MUI, `sx`                                |
| `ucl-web/`               | Unchain Labs site — Next.js, Tailwind, shadcn           |
| `ucl-web-cms/`           | Strapi 5 CMS — FAQ, articles, insights, legal pages       |

Only inspect and modify repos related to the Jira acceptance criteria.

Do not inspect or edit every repo by default.

---

## Agent Responsibilities

### `orchestrator`

Default entrypoint for Jira tickets.

Responsibilities:

1. fetch Jira ticket via MCP
2. normalize ticket summary and acceptance criteria
3. route work to relevant repo(s)
4. decide parallel vs sequential execution
5. delegate implementation to subagents
6. verify API contract alignment
7. coordinate testing/verification
8. produce final report

### `fullstack-orchestrator`

Used when a ticket touches both frontend and backend.

Responsibilities:

1. split AC by frontend/backend
2. run backend first when frontend depends on backend contract
3. pass final backend contract to frontend
4. verify frontend/backend integration
5. coordinate backend tests and frontend checks

### `testing`

Verification agent.

Responsibilities:

1. run backend lint/test gates when approved
2. verify frontend API consumers against backend contracts
3. check frontend/backend contract alignment
4. report failures or mismatches clearly
5. never edit code directly

---

## Parallel Subagent Policy

Use parallel subagents only when work is independent.

Parallel is allowed when:

```txt
- two repos have unrelated acceptance criteria
- frontend work is UI-only and does not depend on backend contract
- FastAPI work is independent from frontend/backend changes
```

Sequential execution is required when:

```txt
- backend creates or changes an API consumed by frontend
- backend response shape affects frontend UI
- backend enum/status values affect frontend behavior
- frontend form payload depends on backend DTO/input
```

For dependent fullstack work:

```txt
1. backend subagent implements or confirms API contract
2. orchestrator extracts final backend contract
3. frontend subagent consumes that contract
4. testing agent verifies frontend API consumer alignment
5. backend tests run through testing gate
```

---

## Backend V2 Rules

For `uc-talent-backend/`, Backend V2 is the standard.

Canonical structure:

```txt
uc-talent-backend/src/modules/<module>/
├── presentation/
├── application/
├── domain/
└── infrastructure/
```

Principles:

* DDD Hexagonal
* Port In / Port Out
* Presentation → Application → Domain
* Infrastructure → Application / Domain
* Domain is pure TypeScript
* ORM entities stay in Infrastructure
* controllers/resolvers stay thin
* application owns orchestration
* backend owns API contracts for fullstack tickets

Do not add new Backend V2 code to legacy `src/domains/*`.

Do not create migrations or perform DDL unless explicitly approved.

---

## Frontend Rules

Frontend repos:

```txt
uc-frontend-nextjs-v1/
uc-ats-react-app/
ucl-web/          # Tailwind + shadcn — NOT MUI
```

Principles:

* TypeScript
* functional components
* React hooks
* MUI + `sx`
* existing repo structure
* existing API/data-fetching pattern
* responsive and accessible UI
* no shadcn unless explicitly requested or already used

For API-driven UI, verify:

* request payload matches backend DTO/input
* response fields used by UI exist in backend output
* enum/status values match backend
* pagination/filter/sort params match backend
* loading/error/empty states are handled

---

## FastAPI Rules

FastAPI repos:

```txt
uc-backend-fastapi/
uc-embedding-fastapi/
```

Use FastAPI-specific skills and rules.

Do not apply NestJS Backend V2 rules to FastAPI services.

---

## API Contract Gate

For any fullstack ticket, frontend/backend contract alignment must be verified.

Check:

* REST endpoint path or GraphQL operation name
* HTTP method
* route params
* query params
* request body/input
* required vs optional fields
* response shape
* enum/status values
* date/time format
* money/currency format
* pagination/filter/sort params
* error response shape
* auth/permission assumptions

If mismatch exists, route the fix back to the correct implementation agent.

Do not complete a fullstack ticket with known contract mismatch.

---

## Command Policy

Always ask before running terminal commands.

Before running a command, explain:

1. what command will run
2. why it is needed

Run commands from the target repo only:

```bash
cd <repo> && <command>
```

Do not automatically run:

```txt
install
lint
test
typecheck
build
migration
DDL
deploy
destructive git commands
```

without approval.

---

## Validation Commands

| Repo                     | Validation commands                         |
| ------------------------ | ------------------------------------------- |
| `uc-talent-backend/`     | `pnpm lint && pnpm test`                    |
| `uc-frontend-nextjs-v1/` | `pnpm lint && pnpm typecheck && pnpm build` |
| `uc-ats-react-app/`      | `pnpm lint && pnpm typecheck && pnpm build` |
| `uc-backend-fastapi/`    | `make lint && pytest`                       |
| `uc-embedding-fastapi/`  | `make lint && pytest`                       |
| `ucl-web/`               | `pnpm lint && pnpm build`                   |
| `ucl-web-cms/`           | `pnpm build`                                |

If commands are not run, report them as `not_run` and include the exact commands.

Do not claim lint/test/typecheck/build passed unless they actually ran and passed.

---

## File Management

Prefer editing existing files over creating new files.

Create new files only when required by:

* existing architecture
* existing repo pattern
* ticket scope

Do not create `.md`, `.mdc`, documentation, or summary files after coding unless explicitly requested.

Do not copy `.cursor/` into child repos unless per-repo config is intentionally needed.

Shared agent config belongs at the parent workspace.

---

## Scope Control

Keep changes small and acceptance-criteria focused.

Do not:

* edit all repos by default
* perform broad refactors unless required
* migrate legacy code unless requested
* introduce new libraries unless necessary and approved
* change unrelated formatting
* guess missing Jira details
* guess attachment content

Acceptance criteria are the source of truth.

---

## Final Report Format

At the end of each ticket, report:

1. Jira ticket key
2. Jira ticket summary
3. acceptance criteria checklist
4. repos touched
5. files changed
6. API contract check status
7. testing/verification status
8. commands run or not run
9. risks, assumptions, or blockers

Use clear statuses:

```txt
passed
failed
not_run
not_applicable
blocked
```

Do not claim completion if:

* Jira ticket could not be fetched
* required attachment content could not be inspected
* backend tests fail
* frontend/backend API contract mismatch remains
* required verification was skipped and the risk is significant
