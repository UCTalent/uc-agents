# User — Use Cases Catalog

| Use Case | Mô tả nghiệp vụ | Code |
|----------|-----------------|------|
| `CreateUserUseCase` | Tạo user | `create-user.use-case.ts` |
| `GetUserByIdUseCase` | User by id | `get-user-by-id.use-case.ts` |
| `GetUsersByIdsUseCase` | Batch get users | `get-users-by-ids.use-case.ts` |
| `FindAllUsersUseCase` | List users (admin) | `find-all-users.use-case.ts` |
| `SearchUsersUseCase` | Search by query | `search-users.use-case.ts` |
| `FindUserByEmailUseCase` | Lookup email | `find-user-by-email.use-case.ts` |
| `FindUsersByEmailsUseCase` | Batch email lookup | `find-users-by-emails.use-case.ts` |
| `FindUserByEmailOrThirdwebUseCase` | Auth lookup | `find-user-by-email-or-thirdweb.use-case.ts` |
| `UpdateUserProfileUseCase` | Update profile me | `update-user-profile.use-case.ts` |
| `UpdateUsernameUseCase` | Đổi username | `update-username.use-case.ts` |
| `CheckUsernameUseCase` | Username availability | `check-username.use-case.ts` |
| `DeleteUserUseCase` | Hard delete | `delete-user.use-case.ts` |
| `SoftDeleteUserUseCase` | Soft delete | `soft-delete-user.use-case.ts` |
| `RestoreUserUseCase` | Restore soft deleted | `restore-user.use-case.ts` |
| `DeleteCurrentUserCompletelyUseCase` | Self-delete account | `delete-current-user-completely.use-case.ts` |
| `DeleteAccountAndRelatedDataUseCase` | Cascade delete | `delete-account-and-related-data.use-case.ts` |
| `GetProfilePictureUseCase` | Get avatar URL | `get-profile-picture.use-case.ts` |
| `DeleteProfilePictureUseCase` | Remove avatar | `delete-profile-picture.use-case.ts` |
| `GenerateMyProfilePicturePresignedUploadUseCase` | Presigned upload | `generate-my-profile-picture-presigned-upload.use-case.ts` |
| `GetOrCreateReferralCodeUseCase` | Referral code | `get-or-create-referral-code.use-case.ts` |
| `RecordReferralUsageUseCase` | Track referral usage | `record-referral-usage.use-case.ts` |
| `ResendConfirmationEmailUseCase` | Resend confirm | `resend-confirmation-email.use-case.ts` |
| `ResetPasswordUseCase` | Reset password (user module) | `reset-password.use-case.ts` |
| `UpdateResetTokenUseCase` | Set reset token | `update-reset-token.use-case.ts` |
| `FindUserByResetTokenUseCase` | Validate reset | `find-user-by-reset-token.use-case.ts` |
| `FindUserByConfirmationTokenUseCase` | Email confirm | `find-user-by-confirmation-token.use-case.ts` |
| `UpdateSignInInfoUseCase` | Last sign-in metadata | `update-sign-in-info.use-case.ts` |
| `ResetFailedAttemptsUseCase` | Unlock account | `reset-failed-attempts.use-case.ts` |
| `ClearResetTokenUseCase` | Clear token after reset | `clear-reset-token.use-case.ts` |

## Ghi chú

- Auth flows (login token) → `auth` module, không duplicate trong user controller
- GraphQL `updateUser` mutation → user resolver
