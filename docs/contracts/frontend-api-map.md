# Frontend API Map

Map consumer → backend endpoint → use case (NestJS). Schema: Swagger/GraphQL codegen.

## Transport overview

| App | Primary | NestJS | FastAPI | Other |
|-----|---------|--------|---------|-------|
| `uc-frontend-nextjs-v1` | REST hooks | `axiosNestjsService` | `fastApi()` in `apiService.ts` | GraphQL (2 ops), Strapi |
| `uc-ats-react-app` | GraphQL + REST | `graphqlClient` + Relay + `axiosBasic` | Unused (`axiosFastApi` no imports) | API gateway (Thirdweb) |

## uc-frontend-nextjs-v1 — key consumers

| Feature | Consumer | Backend | Endpoint / operation |
|---------|----------|---------|---------------------|
| Apply job | `hooks/api/mutations/useApplyJob.ts` | NestJS REST | `POST /jobs/:id/apply` → `ApplyJobUseCase` |
| Job list | `hooks/api/queries/useGetListJob.tsx` | NestJS REST | `GET /jobs` → `GetJobsUseCase` |
| Job detail | job detail hooks | NestJS REST | `GET /jobs/:id` → `GetJobUseCase` |
| Profile me | `useGetProfileMe.tsx` + GraphQL | NestJS GraphQL | `v2TalentMe` |
| Login | `useLogin.ts` | NestJS OAuth | `POST /oauth/token` |
| CV validate/extract | CV hooks | FastAPI | `/eval/extract_and_evaluate`, `/v1/cv/extract` |
| Analyze application | `useAnalyzeApplication` | FastAPI | `/v1/recommendation/analyze_application` |
| AI assistant | `features/aiAssistant/api.ts` | FastAPI | `/v1/agent/conversations` |
| Job recommendation | recommendation hooks | FastAPI | `/v1/recommendation/*` |
| UPP / referral | UPP hooks | FastAPI | `/v2/referral/*`, `/v2/upp/*` |
| CMS articles/FAQ | `services/strapi*.ts` | Strapi | content API |

Client files:
- NestJS REST: `src/services/axiosNestjsService.ts`
- FastAPI: `src/services/apiService.ts` → `fastApi()`
- GraphQL: `src/configs/graphqlClient.ts`

## uc-ats-react-app — key consumers

| Feature | Consumer | Backend | Endpoint / operation |
|---------|----------|---------|---------------------|
| Job list | `hooks/queries/job/useGetJobs.ts` | NestJS GraphQL | `v2MyJobs` → `GetJobsUseCase` |
| Job detail (kanban) | `useGetDetailJobForCandidate.ts` | NestJS GraphQL | job + applies |
| Profile | `useGetProfileMe.ts` | NestJS GraphQL | talent me |
| Update job apply | `useUpdateJobApply.ts` | NestJS GraphQL | `UpdateJobApplyUseCase` |
| HR add candidate | `services/api/job.ts` | NestJS REST | `POST /jobs/:id/hr_add_candidate` |
| Extract resume (HR) | HR mutations | NestJS REST | `POST /jobs/:id/hr_add_candidate/extract_resume` |
| Close job | GraphQL mutation | NestJS GraphQL | `CloseJobUseCase` |
| Blockchain sign | `services/api/blockchain.ts` | NestJS REST | `/blockchain/*` |
| Thirdweb auth | `useAuthenticationThirdweb.ts` | API gateway | not NestJS direct |

Client files:
- GraphQL (graphql-request): `src/configs/graphqlClient.ts`, `src/graphql-client/`
- Relay: `src/environment.ts`
- REST: `src/services/axios/axiosBasic.ts`, `src/services/api/`

## Rules for frontend work

1. Check this map before adding new API call — endpoint may already exist
2. NestJS business changes → update `docs/domains/<module>/API.md`
3. FastAPI/AI features → `docs/services/fastapi/`
4. Request/response shape → verify against backend DTO, not guess

## Env vars

| Var | Target |
|-----|--------|
| `NEXT_PUBLIC_URL_API_NESTJS` | NestJS REST |
| `NEXT_PUBLIC_URL_GRAPHQL_API` | NestJS GraphQL |
| `NEXT_PUBLIC_URL_API_FASTAPI` | FastAPI |
| `VITE_APP_API_URL` | ATS NestJS REST |
| `VITE_APP_GRAPHQL_ENDPOINT` | ATS NestJS GraphQL |
