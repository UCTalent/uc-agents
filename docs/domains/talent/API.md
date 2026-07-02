# Talent — API Map

Prefix: `/api/v2/talents` (RouterModule v2).

## REST — TalentController

| Method | Path | Use Case | Auth |
|--------|------|----------|------|
| GET | `/api/v2/talents` | `GetPublicTalentsUseCase` | Public |
| GET | `/api/v2/talents/me` | `GetTalentMeUseCase` | JWT |
| GET | `/api/v2/talents/profile/me` | `GetTalentMeUseCase` | JWT |
| GET | `/api/v2/talents/me/resumes` | `GetMyUploadedResumesUseCase` | JWT |
| GET | `/api/v2/talents/me/upload_resumes` | `ListMyResumeRecordsUseCase` | JWT |
| POST | `/api/v2/talents/me/resumes` | `UploadResumeUseCase` | JWT |
| POST/PUT/PATCH | `/api/v2/talents/me`, `/me/onboarding` | `OnboardingTalentUseCase` | JWT |
| PATCH | `/api/v2/talents/me/visibility` | `UpdateVisibilityUseCase` | JWT |
| GET | `/api/v2/talents/username/:username` | `GetPublicTalentByUsernameUseCase` | Public |
| GET | `/api/v2/talents/username/:username/preview` | `GetPublicTalentPreviewByUsernameUseCase` | Public |
| GET | `/api/v2/talents/:id` | `GetTalentUseCase` | Optional |
| GET | `/api/v2/talents/:id/profile-completion` | `GetProfileCompletionUseCase` | JWT |
| GET | `/api/v2/talents/:id/similar-talents` | `GetSimilarTalentsUseCase` | Public |
| POST/PUT/DELETE | `/api/v2/talents/:id/experiences/*` | Experience use cases | JWT |
| POST/PUT/DELETE | `/api/v2/talents/:id/educations/*` | Education use cases | JWT |
| POST | `/api/v2/talents/:id/external-links` | `AddExternalLinkUseCase` | JWT |

## GraphQL

| Operation | Resolver | Use Case |
|-----------|----------|----------|
| `v2TalentMe` | `talent.resolver` | `GetTalentMeUseCase` |

## Source

- `presentation/http/controllers/talent.controller.ts`
- `presentation/graphql/resolvers/talent.resolver.ts`
