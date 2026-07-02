# Job — API Map

Global prefix: `/api`. Job module mounted at `v2`. Schema: NestJS Swagger.

## REST — JobController (`/api/v2/jobs`)

| Method | Path | Use Case / Port In | Auth |
|--------|------|-------------------|------|
| POST | `/api/v2/jobs` | `CreateJobUseCase` | JWT |
| GET | `/api/v2/jobs` | `GetJobsUseCase` | Optional JWT |
| POST | `/api/v2/jobs/recommendation` | `GetJobsRecommendationUseCase` | Optional |
| GET | `/api/v2/jobs/recommendation/me` | `GetMyRecommendedJobsUseCase` | JWT |
| GET | `/api/v2/jobs/:id` | `GetJobUseCase` | Optional JWT |
| GET | `/api/v2/jobs/:id/preview` | `GetJobPreviewUseCase` | Public |
| PUT | `/api/v2/jobs/:id` | `UpdateJobUseCase` | JWT |
| POST | `/api/v2/jobs/:id/close` | `CloseJobUseCase` | JWT |
| GET | `/api/v2/jobs/:id/referral_link` | `GenerateReferralLinkUseCase` | JWT |
| POST | `/api/v2/jobs/:id/referral_candidate` | `ReferCandidateUseCase` | JWT |
| GET | `/api/v2/jobs/:id/similar_jobs` | `GetSimilarJobsUseCase` | Public |
| POST | `/api/v2/jobs/:id/referral_apply` | `ReferralApplyUseCase` | JWT |
| PATCH | `/api/v2/jobs/:id/update_referral_info` | `UpdateReferralInfoUseCase` | JWT |
| GET | `/api/v2/jobs/job_referrals/:referralCode` | `GetJobByReferralLinkUseCase` | Public |
| PATCH | `/api/v2/jobs/applies/:id/status` | `UpdateApplicationStatusUseCase` | JWT |
| POST | `/api/v2/jobs/:id/apply` | `ApplyJobUseCase` | JWT |
| POST | `/api/v2/jobs/:id/share` | `ShareJobUseCase` | JWT |
| POST | `/api/v2/jobs/:id/hr_add_candidate/extract_resume` | `HrAddCandidateExtractResumeUseCase` | JWT |
| POST | `/api/v2/jobs/:id/hr_add_candidate` | `HrAddCandidateUseCase` | JWT |

## REST — JobAppliesController (`/api/v2/job_applies`)

| Method | Path | Use Case | Auth |
|--------|------|----------|------|
| GET | `/api/v2/job_applies` | `GetJobAppliesUseCase` | JWT |

## REST — JobReferralsController (`/api/v2/job_referrals`)

| Method | Path | Use Case | Auth |
|--------|------|----------|------|
| GET | `/api/v2/job_referrals` | `GetJobReferralsUseCase` | JWT |

## GraphQL (v2 schema)

| Operation | Resolver | Use Case |
|-----------|----------|----------|
| `v2JobDetail` | `job.resolver` | `GetJobUseCase` |
| `v2MyJobs` / job connection | `job.resolver` | `GetJobsForGraphqlListUseCase` |
| `v2GlobalJobApplyStats` | `job.resolver` | `GetGlobalJobApplyStatsUseCase` |
| `v2CloseJob` | `close-job.resolver` | `CloseJobUseCase` |
| `v2UpdateJobApply` | `job-apply.resolver` | `UpdateJobApplyUseCase` |
| Close job reasons | `choice-option.resolver` | `GetCloseJobReasonsUseCase` |

## Source files

- `presentation/http/controller/job.controller.ts`
- `presentation/http/controller/job-applies.controller.ts`
- `presentation/http/controller/job-referrals.controller.ts`
- `presentation/graphql/resolvers/job.resolver.ts`
- `presentation/graphql/resolvers/job-apply.resolver.ts`
- `presentation/graphql/resolvers/close-job.resolver.ts`

## Frontend consumers

Xem [contracts/frontend-api-map.md](../../contracts/frontend-api-map.md).
