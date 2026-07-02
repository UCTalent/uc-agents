# Auth — API Map

Auth controller paths (v2 module). Một số legacy alias paths tồn tại song song.

## REST — AuthController

| Method | Path | Use Case | Auth |
|--------|------|----------|------|
| POST | `/api/v2/auth/login` | `LoginUseCase` | Public |
| POST | `/api/v2/auth/register` | `RegisterUseCase` | Public |
| POST | `/api/v2/auth/web3/authenticate` | `Web3AuthenticateUseCase` | Public |
| POST | `/api/v2/auth/thirdweb` | `Web3PayloadAuthenticateUseCase` | Public |
| POST | `/api/v2/auth/google` | `GoogleAuthenticateUseCase` | Public |
| POST | `/api/v2/auth/password/forgot` | `ForgotPasswordUseCase` | Public |
| POST | `/api/v2/auth/forgot-password` | `ForgotPasswordUseCase` | Public (alias) |
| POST | `/api/v2/auth/password/reset` | `ResetPasswordUseCase` | Public |
| GET | `/api/v2/auth/validate-reset-token` | `ValidateResetTokenUseCase` | Public |
| POST | `/api/v2/auth/email/confirm` | `ConfirmEmailUseCase` | Public |
| GET | `/api/v2/auth/confirm-email` | `ConfirmEmailUseCase` | Public |
| POST | `/api/v2/auth/oauth/token/refresh` | `RefreshOAuthTokenUseCase` | Public |
| POST | `/api/v2/auth/oauth/token/revoke` | `RevokeOAuthTokenUseCase` | Public |
| POST | `/api/v2/auth/oauth/token/introspect` | `IntrospectOAuthTokenUseCase` | Public |
| GET | `/api/v2/auth/jwks` | `GetJwksUseCase` | Public |
| POST | `/api/v2/auth/discord/exchange` | Social exchange | Public |
| POST | `/api/v2/auth/thirdweb/login-payload` | Thirdweb payload | Public |

## Legacy OAuth (frontend Next.js)

| Method | Path | Note |
|--------|------|------|
| POST | `/oauth/token` | `useLogin.ts` — legacy base URL |

## Source

- `presentation/http/controllers/auth.controller.ts`

## Frontend

- Next.js: `hooks/api/mutations/useLogin.ts` → `/oauth/token`
- ATS: `services/api/auth.ts` → OAuth + profile
