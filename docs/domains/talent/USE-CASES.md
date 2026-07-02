# Talent — Use Cases Catalog

| Use Case | Mô tả nghiệp vụ | Code |
|----------|-----------------|------|
| `GetTalentMeUseCase` | Profile talent đăng nhập | `get-talent-me.use-case.ts` |
| `GetTalentUseCase` | Talent by id | `get-talent.use-case.ts` |
| `GetTalentByUserIdUseCase` | Talent by user id | `get-talent-by-user-id.use-case.ts` |
| `EnsureTalentProfileUseCase` | Tạo/đảm bảo talent record | `ensure-talent-profile.use-case.ts` |
| `HasTalentProfileUseCase` | Kiểm tra có talent profile | `has-talent-profile.use-case.ts` |
| `OnboardingTalentUseCase` | Onboarding wizard | `onboarding-talent.use-case.ts` |
| `UpdateVisibilityUseCase` | Public/private profile | `update-visibility.use-case.ts` |
| `GetProfileCompletionUseCase` | % hoàn thành profile | `get-profile-completion.use-case.ts` |
| `UploadResumeUseCase` | Upload resume file | `upload-resume.use-case.ts` |
| `GetMyUploadedResumesUseCase` | List resumes của me | `get-my-uploaded-resumes.use-case.ts` |
| `ListMyResumeRecordsUseCase` | Resume records | `list-my-resume-records.use-case.ts` |
| `GetPublicTalentsUseCase` | Directory public talents | `get-public-talents.use-case.ts` |
| `GetPublicTalentByUsernameUseCase` | Public profile by username | `get-public-talent-by-username.use-case.ts` |
| `GetPublicTalentPreviewByUsernameUseCase` | OG preview | `get-public-talent-preview-by-username.use-case.ts` |
| `GetSimilarTalentsUseCase` | Talents tương tự | `get-similar-talents.use-case.ts` |
| `AddExperienceUseCase` | Thêm experience | `add-experience.use-case.ts` |
| `UpdateExperienceUseCase` | Sửa experience | `update-experience.use-case.ts` |
| `MergeExperiencesUseCase` | Merge duplicate experiences | `merge-experiences.use-case.ts` |
| `AddEducationUseCase` | Thêm education | `add-education.use-case.ts` |
| `UpdateEducationUseCase` | Sửa education | `update-education.use-case.ts` |
| `DeleteEducationUseCase` | Xóa education | `delete-education.use-case.ts` |
| `MergeEducationsUseCase` | Merge educations | `merge-educations.use-case.ts` |
| `AddExternalLinkUseCase` | Thêm social/link | `add-external-link.use-case.ts` |
| `PurgeTalentForUserUseCase` | GDPR purge | `purge-talent-for-user.use-case.ts` |

## Ghi chú

- `EnsureTalentProfileUseCase` là entry point khi module khác cần talent — không duplicate trong job controller
- CV parsing không thuộc talent module
