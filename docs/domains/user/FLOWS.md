# User — Business Flows

## Flow: Profile Update

Frontend (Next.js GraphQL hoặc REST) → `UpdateUserProfileUseCase`. Talent-specific fields → `talent` module.

## Flow: Account Deletion

`DeleteCurrentUserCompletelyUseCase` / `DeleteAccountAndRelatedDataUseCase` — orchestrate purge across modules (talent, job, upp purge use cases).

```mermaid
sequenceDiagram
  participant User as UserController
  participant UC as DeleteAccountAndRelatedDataUseCase
  participant Talent as PurgeTalentForUser
  participant Job as PurgeJobApplies

  User->>UC: delete me
  UC->>Talent: purge
  UC->>Job: purge applies/referrals
```

## Flow: Profile Picture Presigned Upload

`GenerateMyProfilePicturePresignedUploadUseCase` → client upload direct to storage → confirm via profile picture endpoint.

## Boundary with auth

Register/login/OAuth → `auth` module only. User module handles entity persistence lookups used by auth use cases.
