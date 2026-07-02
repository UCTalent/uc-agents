# User Domain — Overview

## Trách nhiệm

- User CRUD, search, soft delete/restore
- Profile update (me), username check/update
- Profile picture upload/presigned URL
- Referral code (một phần overlap UPP — check ownership)
- Account deletion, confirmation email resend
- Internal lookups (by email, id, reset token)

## Không thuộc domain này

| Concern | Owner |
|---------|-------|
| Talent profile, resume | `talent` |
| Login/register/OAuth | `auth` |
| UPP points runtime | `uc-backend-fastapi` |

## Module liên quan

| Module | Quan hệ |
|--------|---------|
| auth | User entity cho login/register |
| talent | `user_id` foreign key |
| file-storage | Profile picture |

## Repo path

`uc-talent-backend/src/modules/user/`

## Docs

- [USE-CASES.md](USE-CASES.md)
- [API.md](API.md)
- [FLOWS.md](FLOWS.md)
