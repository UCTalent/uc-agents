# User — API Map

Prefix: `/api/v2/users`.

## REST — UserController

| Method | Path | Use Case | Auth |
|--------|------|----------|------|
| POST | `/api/v2/users` | `CreateUserUseCase` | Varies |
| POST | `/api/v2/users/check-username` | `CheckUsernameUseCase` | Public |
| PATCH | `/api/v2/users/username` | `UpdateUsernameUseCase` | JWT |
| GET | `/api/v2/users` | `FindAllUsersUseCase` | Admin |
| GET | `/api/v2/users/search` | `SearchUsersUseCase` | JWT |
| GET | `/api/v2/users/me` | `GetUserByIdUseCase` | JWT |
| DELETE | `/api/v2/users/me` | `DeleteCurrentUserCompletelyUseCase` | JWT |
| GET | `/api/v2/users/:id` | `GetUserByIdUseCase` | JWT |
| PUT | `/api/v2/users/me/profile` | `UpdateUserProfileUseCase` | JWT |
| PUT | `/api/v2/users/:id` | Admin update | JWT |
| DELETE | `/api/v2/users/:id` | `DeleteUserUseCase` | Admin |
| PATCH | `/api/v2/users/:id/soft-delete` | `SoftDeleteUserUseCase` | Admin |
| PATCH | `/api/v2/users/:id/restore` | `RestoreUserUseCase` | Admin |
| POST | `/api/v2/users/:id/profile-picture` | Upload | JWT |
| GET | `/api/v2/users/:id/profile-picture` | `GetProfilePictureUseCase` | JWT |
| DELETE | `/api/v2/users/:id/profile-picture` | `DeleteProfilePictureUseCase` | JWT |
| POST | `/api/v2/users/me/profile_picture/presigned-upload` | `GenerateMyProfilePicturePresignedUploadUseCase` | JWT |
| POST | `/api/v2/users/confirmation` | `ResendConfirmationEmailUseCase` | Public |

## GraphQL

| Operation | Resolver | Use Case |
|-----------|----------|----------|
| `v2UpdateUser` | `user.resolver` | `UpdateUserProfileUseCase` |

## Source

- `presentation/http/controllers/user.controller.ts`
- `presentation/graphql/resolvers/user.resolver.ts`
