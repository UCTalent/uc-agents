---
name: implement-fastapi
description: Implement UC Talent FastAPI services — uc-backend-fastapi or uc-embedding-fastapi. backend-fastapi rule auto via globs when editing .py files. pytest. UPP/CV/job rec or embeddings/ML APIs.
---

# Implement FastAPI Backend

Paths: **`uc-backend-fastapi/`** or **`uc-embedding-fastapi/`** (siblings in parent workspace).

## Cursor rules (auto via globs)

When editing `uc-backend-fastapi/**/*.py` or `uc-embedding-fastapi/**/*.py`, Cursor injects:

- `.cursor/rules/backend-fastapi.mdc`

No need to read this rule file manually — globs handle activation.

Run `cd <repo> && pytest` for verification.

## Input

- **task_summary**, **task_description**, **acceptance_criteria** (pasted in prompt)
- **task_description**, **affected_files**, **domain** (upp | cv | job_recommendation | internal | queue)

## Workflow

0. **Domain docs** — Read `docs/services/fastapi/` and `docs/contracts/nestjs-fastapi.md` before adding endpoints.
1. **Explore (Codegraph)** — `codegraph_explore` for routes/services/schemas (e.g. `"get_referral_code job_recommendation"`). `codegraph_node` with `file` to read indexed `.py` instead of `Read`. Policy: `uctalent-workflow.mdc` §4.2.
2. **Plan** — Service pattern (A function / B class / C async CV); migration if DB; auth type.
3. **Implement** — Schemas → service → route → register in `main.py`.
4. **Verify** — `read_lints` on changed `.py` files.
5. **Tests** — `tests/unit/` or `tests/integration/`; `Agent(testing)` or user-approved `pytest`.
6. **Report** — JSON below.

## Pattern quick pick

| Domain | Pattern | Return shape |
|--------|---------|----------------|
| UPP, referrals, points | A — functions | `(Optional[str], result)` |
| Job search, AI orchestration | B — classes | Pydantic model or domain error |
| CV extract | C — async + GenAI dep | from route `Depends` |

## Route template (UPP)

```python
err, data = get_referral_code(db, user_id)
if err:
    raise HTTPException(status_code=HTTPStatus.BAD_REQUEST, detail=err)
return data
```

## Quality

```bash
make format && make lint    # ask user before running
pytest                      # full suite
```

- Absolute imports: `from src.services...`
- UPP responses camelCase, requests snake_case
- Constants from `src/config/constants.py`
- i18n for user-facing errors

## AI assistant rules

1. Do not create `.md`/`.mdc` unless user asks.
2. Ask before `pytest`, `make migrate`, `make lint`.
3. No business logic in new route handlers.

## Output

```json
{
  "task_summary": "...",
  "backend_stack": "fastapi",
  "files_modified": [],
  "routes_added": [],
  "migrations_added": [],
  "tests_added": 0,
  "testing": { "tests_pass": true },
  "notes": ""
}
```
