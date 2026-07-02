# Auth Domain — Overview

## Trách nhiệm

- Email/password login và register
- OAuth token generate/refresh/revoke/introspect
- Web3 authenticate (wallet signature)
- Thirdweb payload authenticate
- Google OAuth
- Forgot/reset password, email confirmation
- JWKS endpoint
- Discord OAuth exchange
- Purge auth data (GDPR)

## Không thuộc domain này

| Concern | Owner |
|---------|-------|
| User profile CRUD | `user` |
| Social account linking | `social` |
| UPP signup redirect | `upp` + FastAPI |

## Module liên quan

| Module | Quan hệ |
|--------|---------|
| user | User entity, password hash |
| social | Discord exchange may link accounts |

## Repo path

`uc-talent-backend/src/modules/auth/`

## Docs

- [USE-CASES.md](USE-CASES.md)
- [API.md](API.md)
- [FLOWS.md](FLOWS.md)
