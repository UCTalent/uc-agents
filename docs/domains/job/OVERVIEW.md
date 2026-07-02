# Job Domain — Overview

## Trách nhiệm

- Đăng, cập nhật, đóng job
- Ứng tuyển (apply), HR add candidate, referral, referral apply
- Danh sách job applies / job referrals của talent
- Job recommendation enrich (gọi FastAPI/embedding qua port)
- Share job, generate referral link
- Blockchain job lifecycle hooks (created, status, claim)
- Candidate resume upload/delete cho pipeline HR

## Không thuộc domain này

| Concern | Module / repo |
|---------|---------------|
| Talent profile ensure | `talent` (port `APPLY_JOB_TALENT_PORT_OUT`) |
| CV parse/evaluate AI | `uc-backend-fastapi` |
| Email gửi apply/referral | `notification` (port out) |
| Payment distribution on-chain | `payment` (port out) |
| Web3 event storage | `blockchain` (port out) |
| Organization logo URL | `organization` (port `ORGANIZATION_LOGO_PORT_OUT`) |

## Module liên quan

| Module | Quan hệ | Port Out |
|--------|---------|----------|
| talent | Ensure profile khi apply | `APPLY_JOB_TALENT_PORT_OUT` |
| notification | Email apply, referral, close job | `APPLY_JOB_NOTIFICATION_PORT_OUT` |
| blockchain | Verify web3 apply, job events | `APPLY_JOB_BLOCKCHAIN_PORT_OUT` |
| payment | Payment distributions cho completed jobs | `PAYMENT_DISTRIBUTIONS_FOR_JOBS_PORT_OUT` |
| file-storage | Resume files | `APPLY_JOB_RESUME_PORT_OUT` |
| FastAPI | CV extract sau apply | `JOB_APPLY_CV_EXTRACT_PORT_OUT` |
| location | Validate current city apply | `REFER_CANDIDATE_LOCATION_PORT_OUT` |

## Repo path

`uc-talent-backend/src/modules/job/`

## Docs

- [USE-CASES.md](USE-CASES.md)
- [API.md](API.md)
- [FLOWS.md](FLOWS.md)
- [job-applies-referrals.md](job-applies-referrals.md) — chi tiết list applies/referrals
