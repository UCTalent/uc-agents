---
name: implement-backend
description: Implement UC Talent NestJS backend (uc-talent-backend) — Clean Architecture, Port In/Out, TypeORM. Backend rules auto-apply via globs when editing src/modules. For FastAPI use implement-fastapi skill.
---

# Implement Backend — NestJS (uc-talent-backend)

**Path:** `uc-talent-backend/` under the parent workspace (TypeScript/NestJS).

**Not this skill:** `uc-backend-fastapi/`, `uc-embedding-fastapi/` → **`implement-fastapi`**.

## Cursor rules (auto via globs)

When editing `uc-talent-backend/src/modules/**/*.ts`, Cursor injects:

- `.cursor/rules/backend-architecture.mdc`
- `.cursor/rules/backend-coding-standards.mdc`
- `.cursor/rules/backend-project-architechture.mdc`

No need to read these rule files manually — globs handle activation. Follow them once module files are in context.

## Input

- **task_summary**, **task_description**, **acceptance_criteria** (from prompt)
- **task_description**, **affected_files**, **api_contracts** (optional)

## Workflow

0. **Domain docs** — Read `docs/OWNERSHIP.md` and `docs/domains/<module>/` (USE-CASES, API, FLOWS) before adding logic.
1. **Explore (Codegraph)** — `codegraph_explore` for Port In → UseCase → Port Out (e.g. module + symbol names). `codegraph_callers` before signature changes. Prefer `codegraph_node` over `Read` for indexed `.ts` files. Policy: `uctalent-workflow.mdc` §4.2.
2. **Plan** — Layers, Port tokens, mappers (DTO↔Command, Result↔DTO, Domain↔ORM).
3. **Implement** — Dependency rule; Port In in controllers only.
4. **Verify** — `read_lints` on changed `.ts` files.
5. **Tests** — unit + integration.
6. **Run** — `Agent(testing)` or user-approved `pnpm test`.
7. **Report** — JSON below.

## Summary

```bash
pnpm format && pnpm lint && pnpm test
```

- Presentation → Application → Domain; Infrastructure → Application → Domain
- Controller: DTO → Command → Port In → Result → Response DTO
- ORM entities only in Infrastructure

## AI assistant rules

1. Do not create `.md`/`.mdc` unless user asks.
2. Ask before `pnpm lint`, `pnpm test`, migrations.
3. `read_lints` before complete; no `console.log`.

## Completion checklist

- [ ] Backend V2 rules satisfied (via globs on changed module files)
- [ ] Port In in controller; mappers in correct layers
- [ ] `pnpm lint` clean; tests run

## Output

```json
{
  "backend_stack": "nestjs",
  "task_summary": "...",
  "files_modified": [],
  "ports_added": [],
  "endpoints_added": [],
  "database_changes": [],
  "tests_added": 0,
  "testing_agent": { "tests_pass": true },
  "lint_clean": true,
  "notes": ""
}
```
