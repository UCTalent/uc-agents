# Job — Use Cases Catalog

Trước khi thêm logic mới → search bảng này. Prefix API: `/api/v2/`.

| Use Case | Port In | Mô tả nghiệp vụ | Port Out chính | Code |
|----------|---------|-----------------|----------------|------|
| `ApplyJobUseCase` | `IApplyJobPortIn` | Ứng tuyển job, referral link, CV extract async, notification | talent, resume, notification, blockchain, cv-extract, location | `apply-job.use-case.ts` |
| `CloseJobUseCase` | `ICloseJobPortIn` | Đóng job + blockchain transaction | close-job-transaction | `close-job.use-case.ts` |
| `CreateJobUseCase` | `ICreateJobPortIn` | Tạo job mới | job repository, enrich | `create-job.use-case.ts` |
| `UpdateJobUseCase` | `IUpdateJobPortIn` | Cập nhật job | job repository | `update-job.use-case.ts` |
| `GetJobUseCase` | `IGetJobPortIn` | Chi tiết job + enrich logo/web3/payment/job apply | enrich, organization-logo, web3-events, payment, job-apply, detail-dto | `get-job.use-case.ts` |
| `GetJobsUseCase` | `IGetJobsPortIn` | List job + pagination + enrich | job-list-query, organization-logo, web3-events, payment | `get-jobs.use-case.ts` |
| `GetJobsForGraphqlListUseCase` | — | List job cho GraphQL connection | job-list-query | `get-jobs-for-graphql-list.use-case.ts` |
| `GetJobPreviewUseCase` | — | Lightweight preview (Open Graph) | job repository | `get-job-preview.use-case.ts` |
| `GetJobAppliesUseCase` | `IGetJobAppliesPortIn` | List applies của talent hiện tại | talent, job apply query | `get-job-applies.use-case.ts` |
| `GetJobReferralsUseCase` | `IGetJobReferralsPortIn` | List referrals do user tạo | job referral query | `get-job-referrals.use-case.ts` |
| `GetJobApplyUseCase` | `IGetJobApplyPortIn` | Chi tiết một apply | job apply repository | `get-job-apply.use-case.ts` |
| `UpdateJobApplyUseCase` | `IUpdateJobApplyPortIn` | Cập nhật apply (ATS) | job apply, notification | `update-job-apply.use-case.ts` |
| `UpdateApplicationStatusUseCase` | — | Patch status apply (REST) | job apply repository | `update-application-status.use-case.ts` |
| `ReferCandidateUseCase` | `IReferCandidatePortIn` | Referrer giới thiệu ứng viên | referral, resume, notification, location | `refer-candidate.use-case.ts` |
| `ReferralApplyUseCase` | — | Apply qua referral link | apply flow | `referral-apply.use-case.ts` |
| `GenerateReferralLinkUseCase` | `IGenerateReferralLinkPortIn` | Tạo referral link + web3 address | referral repository | `generate-referral-link.use-case.ts` |
| `GetJobByReferralLinkUseCase` | `IGetJobByReferralLinkPortIn` | Resolve job từ referral code | job repository | `get-job-by-referral-link.use-case.ts` |
| `UpdateReferralInfoUseCase` | `IUpdateReferralInfoPortIn` | Cập nhật thông tin referral | job referral | `update-referral-info.use-case.ts` |
| `HrAddCandidateUseCase` | `IHrAddCandidatePortIn` | HR thêm candidate vào job | talent, resume, notification | `hr-add-candidate.use-case.ts` |
| `HrAddCandidateExtractResumeUseCase` | `IHrAddCandidateExtractResumePortIn` | HR extract resume (FastAPI) | cv-extract port | `hr-add-candidate-extract-resume.use-case.ts` |
| `UploadCandidateResumeUseCase` | `IUploadCandidateResumePortIn` | Upload resume candidate | file-storage | `upload-candidate-resume.use-case.ts` |
| `GetCandidateResumeUseCase` | `IGetCandidateResumePortIn` | Lấy resume candidate | file-storage | `get-candidate-resume.use-case.ts` |
| `DeleteCandidateResumeUseCase` | `IDeleteCandidateResumePortIn` | Xóa resume candidate | file-storage | `delete-candidate-resume.use-case.ts` |
| `ShareJobUseCase` | `IShareJobPortIn` | Chia sẻ job với member | notification, organization | `share-job.use-case.ts` |
| `CompletePendingJobShareInvitesUseCase` | — | Hoàn tất pending share invites | notification | `complete-pending-job-share-invites.use-case.ts` |
| `GetSimilarJobsUseCase` | `IGetSimilarJobsPortIn` | Jobs tương tự (embedding/ES) | similar-jobs port | `get-similar-jobs.use-case.ts` |
| `GetMyRecommendedJobsUseCase` | `IGetMyRecommendedJobsPortIn` | Semantic rec cho talent đăng nhập | recommendation enrich | `get-my-recommended-jobs.use-case.ts` |
| `GetJobsRecommendationUseCase` | — | Job rec từ FastAPI (public POST) | FastAPI client | `get-job-recommendation.use-case.ts` |
| `GetCloseJobReasonsUseCase` | `IGetCloseJobReasonsPortIn` | Lý do đóng job (choice options) | choice-option repository | `get-close-job-reasons.use-case.ts` |
| `GetGlobalJobApplyStatsUseCase` | `IGetGlobalJobApplyStatsPortIn` | Thống kê apply toàn cục (GraphQL) | stats query | `get-global-job-apply-stats.use-case.ts` |
| `JobAutocompleteUseCase` | `IJobAutocompletePortIn` | Autocomplete job title | search | `job-autocomplete.use-case.ts` |
| `UpdateTransactionDetailsUseCase` | `IUpdateTransactionDetailsPortIn` | Cập nhật tx blockchain cho job | blockchain | `update-transaction-details.use-case.ts` |
| `ApplyBlockchainJobCreatedUseCase` | `IApplyBlockchainJobCreatedPortIn` | Webhook job created on-chain | blockchain events | `apply-blockchain-job-created.use-case.ts` |
| `ApplyBlockchainJobStatusUseCase` | `IApplyBlockchainJobStatusPortIn` | Webhook job status on-chain | blockchain events | `apply-blockchain-job-status.use-case.ts` |
| `ApplyBlockchainClaimRequestedUseCase` | `IApplyBlockchainClaimRequestedPortIn` | Webhook claim requested | payment/blockchain | `apply-blockchain-claim-requested.use-case.ts` |
| `PurgeJobAppliesForTalentsUseCase` | `IPurgeJobAppliesForTalentsPortIn` | GDPR purge applies | job apply repository | `purge-job-applies-for-talents.use-case.ts` |
| `PurgeReferrerJobReferralsUseCase` | `IPurgeReferrerJobReferralsPortIn` | GDPR purge referrals | job referral repository | `purge-referrer-job-referrals.use-case.ts` |

## Ghi chú

- Controller inject Port In; một số endpoint gọi use case class trực tiếp (`ReferralApplyUseCase`, `ShareJobUseCase`) — khi extend, ưu tiên Port In pattern
- Enrich logo/web3/payment: reuse port out, không query trong presentation
- Chi tiết list applies/referrals: [job-applies-referrals.md](job-applies-referrals.md)
