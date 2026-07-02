# Auth — Business Flows

## Flow: Email Login

```mermaid
sequenceDiagram
  participant FE as Frontend
  participant Ctrl as AuthController
  participant UC as LoginUseCase
  participant User as UserRepository

  FE->>Ctrl: POST /auth/login
  Ctrl->>UC: execute
  UC->>User: findByEmail + verify password
  UC-->>Ctrl: tokens / session
```

## Flow: Web3 / Thirdweb Auth

`Web3AuthenticateUseCase` hoặc `Web3PayloadAuthenticateUseCase` — verify signature, find or create user, optional `ref_code` referral.

ATS: Thirdweb qua API gateway — xem [frontend-api-map.md](../../contracts/frontend-api-map.md).

## Flow: OAuth Token Lifecycle

```mermaid
sequenceDiagram
  participant Client
  participant Auth as AuthController
  participant Refresh as RefreshOAuthTokenUseCase
  participant Revoke as RevokeOAuthTokenUseCase

  Client->>Auth: POST oauth/token/refresh
  Auth->>Refresh: execute
  Client->>Auth: POST oauth/token/revoke
  Auth->>Revoke: execute
```

## Flow: Password Reset

1. `ForgotPasswordUseCase` → email with token
2. `ValidateResetTokenUseCase` → GET validate
3. `ResetPasswordUseCase` → POST reset

## JWT in other modules

Routes dùng `OAuth2Guard` + `@CurrentUserId()` — business logic nằm trong domain use cases, không trong guard.
