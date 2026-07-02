# Talent Domain — Overview

## Trách nhiệm

- Talent profile (me, public by username/id)
- Resume upload và list
- Education, experience, external links CRUD + merge
- Onboarding, visibility, profile completion
- Public talent directory và similar talents
- Ensure talent profile (cũng được gọi qua job port out)

## Không thuộc domain này

| Concern | Owner |
|---------|-------|
| CV AI parse/evaluate | `uc-backend-fastapi` |
| User account (email, username) | `user` module |
| Job apply logic | `job` module |
| File storage primitives | `file-storage` (talent dùng qua service/port) |

## Module liên quan

| Module | Quan hệ |
|--------|---------|
| job | `APPLY_JOB_TALENT_PORT_OUT` → `ensure-talent-profile` |
| user | Talent gắn `user_id` |
| file-storage | Resume upload |

## Repo path

`uc-talent-backend/src/modules/talent/`

## Docs

- [USE-CASES.md](USE-CASES.md)
- [API.md](API.md)
- [FLOWS.md](FLOWS.md)
