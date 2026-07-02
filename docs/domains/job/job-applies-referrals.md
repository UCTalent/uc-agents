# Job Applies & Job Referrals — List API

Migrated from legacy doc. V2 implementation: `GetJobAppliesUseCase`, `GetJobReferralsUseCase`.

## API v2

| Method | Path | Use Case |
|--------|------|----------|
| GET | `/api/v2/job_applies` | `GetJobAppliesUseCase` |
| GET | `/api/v2/job_referrals` | `GetJobReferralsUseCase` |

Controllers: `job-applies.controller.ts`, `job-referrals.controller.ts`.

## Quyền truy cập

- JWT required (`OAuth2Guard`)
- User phải có `Talent` hợp lệ; nếu không → lỗi tương đương `UserNotTalentError`

## Input Filters

- `status` (JobApply): `new`, `email_sent`, `under_review`, `interviewing`, `offering`, `hired`, `rejected`
- `job_status` (Job): `crawled`, `approved`, `rejected`, `closed`, `success_completed`, `fail_completed`, `published`, `expired`, `achieved`, `pending_to_review`
- `job_not_status` (Job): loại trừ một status
- `page` (default `1`), `page_size` (default `10`)

Filter trống → bỏ qua điều kiện đó.

## Business Logic

### Job Applies List

- Base: `JobApply` where `talent_id == current_talent.id`
- Eager-load: `job`, `talent.user`
- Filters: `status` on JobApply → `job_status` on Job → `job_not_status` exclude
- Pagination: `page`, `page_size`

### Job Referrals List

- Base: `JobReferral` where `referrer_id == current_user.id`, eager-load `job`
- `status` filter via joined `job_apply.status`
- Then `job_status`, `job_not_status` on Job
- Pagination: `page`, `page_size`

### Reward & wallet

- `reward_claimable`: `job_apply.status == hired` AND `job.status == success_completed`
- `wallet_address`: from talent/referrer wallet by `web3_chain_name`

## Response Envelope

```json
{
  "data": [ /* items */ ],
  "pagination": {
    "current": 1,
    "previous": null,
    "next": 2,
    "limit": 10,
    "total_pages": 3,
    "total_count": 25,
    "first_page?": true,
    "last_page?": false
  }
}
```

### JobApply item (summary)

Flat fields: `id`, `job_id`, `talent_id`, `status`, `email`, `headline`, `phone_number`, `web3_chain_name`, `web3_signature`, `rejected_note`.

Nested: `job` (index shape), `talent`, `resume.url`, `reward_claimable`, `wallet_address`, `web3meta` on job.

### JobReferral item (summary)

Flat: `id`, `candidate_email`, `candidate_name`, `candidate_phonenumber`, `candidate_introduction`, `recommendation`, `web3_signature`, `web3_chain_name`.

Nested: `job_apply` (or `{}` if missing), `referrer` (talent), `reward_claimable`, `wallet_address`.

## Implementation notes

- JobReferrals `status` filter = `JobApply.status`, not `JobReferral.status`
- Missing `job_apply` on referral → return `{}` for `job_apply`
- Avoid N+1 on nested objects
- Use enums for status values

## Related

- [USE-CASES.md](USE-CASES.md) — `GetJobAppliesUseCase`, `GetJobReferralsUseCase`
- [API.md](API.md) — endpoint map
