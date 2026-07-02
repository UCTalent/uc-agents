# FastAPI — API Map

Schema source of truth: `http://localhost:8000/docs` and [API_DOCUMENTATION.md](../../../uc-backend-fastapi/API_DOCUMENTATION.md).

## Health

| Method | Path | Auth |
|--------|------|------|
| GET | `/` | None |
| GET | `/health`, `/healthz`, `/readyz` | None |

## CV (`/v1/cv`, `/eval`, `/v2/internal/cv`)

| Method | Path | Purpose | Auth |
|--------|------|---------|------|
| POST | `/v1/cv/extract` | Parse CV → talent schema | API key |
| POST | `/eval/extract_and_evaluate` | Upload + evaluate + persist | API key + JWT |
| POST | `/v2/internal/cv/extract_and_evaluate` | Internal full extract | Internal key |
| POST | `/v2/internal/cv/extract_preview` | Internal preview | Internal key |

## UPP (`/v2`)

| Method | Path | Purpose |
|--------|------|---------|
| GET | `/v2/referral/code` | Get referral code |
| POST | `/v2/referral/verify/code` | Verify code |
| PUT | `/v2/referral/update/code` | Update code |
| GET | `/v2/referral/signup/{ref_code}` | Signup redirect info |
| GET | `/v2/upp/points` | User points |
| GET | `/v2/upp/points/breakdown` | Points breakdown |
| GET | `/v2/leaderboard` | Leaderboard |
| GET | `/v2/leaderboard/current_user` | Current user rank |
| GET | `/v2/leaderboard/user` | User on leaderboard |
| GET | `/v2/leaderboard/points` | Leaderboard points |
| POST | `/v2/leaderboard/refresh` | Refresh cache |
| POST/GET/PUT/DELETE | `/v2/challenges/*` | Challenge CRUD + progress |

## Internal queue (`/v2/internal`)

| Method | Path | Purpose |
|--------|------|---------|
| POST | `/v2/internal/queue/signup` | Queue signup webhook |
| POST | `/v2/internal/queue/complete-onboarding` | Onboarding complete |

## Job recommendation (`/v1/recommendation`)

| Method | Path | Purpose |
|--------|------|---------|
| POST | `/v1/recommendation/job_search` | ES job search |
| POST | `/v1/recommendation/consult_career_path` | Career AI |
| POST | `/v1/recommendation/consult_career_path_with_cv` | Career AI + CV |
| POST | `/v1/recommendation/handler_match_jd_cv` | JD-CV match |
| POST | `/v1/recommendation/handler_upgrade_cv` | CV upgrade |
| POST | `/v1/recommendation/analyze_application` | Application analysis |
| POST | `/v1/recommendation/consult_gen_job_description` | Generate JD |
| POST | `/v1/recommendation/upload_cv` | CV upload helper |

## Talent sourcing (`/v1/talent_sourcing`)

| Method | Path | Purpose |
|--------|------|---------|
| POST | `/v1/talent_sourcing/resolve_job_lookup` | Resolve job |
| POST | `/v1/talent_sourcing/source_candidates` | Source candidates |
| POST | `/v1/talent_sourcing/deep_analyze_candidates` | Deep analysis |
| POST | `/v1/talent_sourcing/generate_outreach_drafts` | Outreach drafts |

## Chat agent (`/v1/agent`)

| Method | Path | Purpose |
|--------|------|---------|
| POST | `/v1/agent/conversations` | Create conversation |
| GET | `/v1/agent/conversations` | List conversations |
| POST | `/v1/agent/conversations/{id}/messages` | Send message |
| GET | `/v1/agent/conversations/{id}/messages` | Message history |

## Route source files

`uc-backend-fastapi/src/api/routes/`

## Contracts

- [nestjs-fastapi.md](../../contracts/nestjs-fastapi.md)
