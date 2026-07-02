# AGENTS.md — UC Talent Workspace Router

Open Cursor from **this parent folder** (`uc-talent/`), not from a child repo.

Detailed workflow, coding rules, and skills live under `.cursor/`. This file only routes agents to the right place.

---

## Workspace layout

```txt
uc-talent/                    ← workspace root
├── AGENTS.md
├── .cursor/                  ← agents, rules, skills
├── docs/                     ← domain knowledge (INDEX, OWNERSHIP, domains/)
├── uc-talent-backend/
├── uc-backend-fastapi/
├── uc-embedding-fastapi/
├── uc-frontend-nextjs-v1/
├── uc-ats-react-app/
├── ucl-web/
└── ucl-web-cms/
```

Use **workspace-root paths**: `uc-talent-backend/src/modules/...`, not bare `src/...`.

---

## Default behavior

| Trigger | Action |
| ------- | ------ |
| Bare Jira key (`UCT-123`, `UC-123`) | Treat as orchestrator ticket — fetch via Jira MCP first, then route by AC |
| Business logic / new API | Read `docs/` per `domain-knowledge` rule before coding |
| Explore symbols / call paths | **Codegraph MCP** first — `uctalent-workflow.mdc` §4.2 |
| Terminal commands | Ask user before `install`, `lint`, `test`, `build`, migrations, DDL |
| Fullstack work | Backend owns API contract; frontend consumes confirmed contract |

**Full workflow** (Jira, orchestrator, parallel policy, contract gate, verification, final report): `.cursor/rules/uctalent-workflow.mdc`. Repo-specific coding rules: `.cursor/rules/` (activated by globs + skills).

---

## Skills (`.cursor/skills/`)

| Work | Skill |
| ---- | ----- |
| NestJS Backend V2 | `implement-backend` |
| FastAPI | `implement-fastapi` |
| MUI frontend | `implement-frontend`, `mui-frontend` |
| Unchain Labs site | `implement-ucl-web` |
| Strapi CMS | `implement-strapi-cms` |
| Next.js patterns | `nextjs-best-practices` |
| PR review | `review-code` |

---

## Agents (`.cursor/agents/`)

| Role | File |
| ---- | ---- |
| Jira ticket entrypoint | `orchestrator.md` |
| Backend + frontend coordination | `fullstack-orchestrator.md` |
| Lint, tests, contract verification | `testing.md` |

---

## Docs (`docs/`)

| Entry | Purpose |
| ----- | ------- |
| `docs/INDEX.md` | Navigation map |
| `docs/OWNERSHIP.md` | Repo/module ownership |
| `docs/domains/<module>/` | Use cases, API, flows per NestJS module |
| `docs/contracts/` | Cross-service contracts |
| `docs/services/` | FastAPI and embedding service docs |

Update existing domain docs when business rules change. Do not create new markdown unless the user asks.
