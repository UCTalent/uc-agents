# Auth — Use Cases Catalog

| Use Case | Port In | Mô tả nghiệp vụ | Code |
|----------|---------|-----------------|------|
| `LoginUseCase` | `ILoginPortIn` | Email/password login | `login.use-case.ts` |
| `RegisterUseCase` | `IRegisterPortIn` | Đăng ký tài khoản | `register.use-case.ts` |
| `Web3AuthenticateUseCase` | `IWeb3AuthenticatePortIn` | Wallet auth | `web3-authenticate.use-case.ts` |
| `Web3PayloadAuthenticateUseCase` | `IWeb3PayloadAuthenticatePortIn` | Thirdweb payload | `web3-payload-authenticate.use-case.ts` |
| `GoogleAuthenticateUseCase` | `IGoogleAuthenticatePortIn` | Google OAuth | `google-authenticate.use-case.ts` |
| `ForgotPasswordUseCase` | `IForgotPasswordPortIn` | Gửi reset email | `forgot-password.use-case.ts` |
| `ResetPasswordUseCase` | `IResetPasswordPortIn` | Đặt lại mật khẩu | `reset-password.use-case.ts` |
| `ValidateResetTokenUseCase` | `IValidateResetTokenPortIn` | Kiểm tra token reset | `validate-reset-token.use-case.ts` |
| `ConfirmEmailUseCase` | `IConfirmEmailPortIn` | Xác nhận email | `confirm-email.use-case.ts` |
| `GenerateOAuthTokenUseCase` | — | OAuth token issuance | `generate-oauth-token.use-case.ts` |
| `RefreshOAuthTokenUseCase` | `IRefreshOAuthTokenPortIn` | Refresh token | `refresh-oauth-token.use-case.ts` |
| `RevokeOAuthTokenUseCase` | `IRevokeOAuthTokenPortIn` | Revoke token | `revoke-oauth-token.use-case.ts` |
| `IntrospectOAuthTokenUseCase` | `IIntrospectOAuthTokenPortIn` | Token introspection | `introspect-oauth-token.use-case.ts` |
| `GetJwksUseCase` | `IGetJwksPortIn` | JWKS public keys | `get-jwks.use-case.ts` |
| `PurgeAuthDataForUserUseCase` | — | GDPR purge tokens | `purge-auth-data-for-user.use-case.ts` |

## Ghi chú

- `OAuth2Guard` / `get_current_user_id` — presentation layer, không business logic trong guard
- Legacy paths (`/oauth/token`) vẫn có thể dùng từ frontend — map trong API.md
