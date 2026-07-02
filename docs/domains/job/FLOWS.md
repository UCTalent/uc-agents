# Job — Business Flows

## Flow 1: Apply Job

### Mô tả

Talent ứng tuyển job: validate talent, job, email/CV duplicate, web3 nếu có → tạo JobApply → notification → CV extract async (nếu không có recruiter evaluation).

### Sequence

```mermaid
sequenceDiagram
  participant FE as Frontend
  participant Ctrl as JobController
  participant UC as ApplyJobUseCase
  participant Talent as TalentPort
  participant Resume as ResumePort
  participant Notif as NotificationPort
  participant CV as CvExtractPort

  FE->>Ctrl: POST /jobs/:id/apply
  Ctrl->>UC: execute(command)
  UC->>Talent: ensureTalentProfile
  UC->>UC: validate job, email, CV duplicate
  UC->>Resume: getOrCreateResume
  UC->>UC: create JobApply + referral if any
  UC->>Notif: sendApplicationEmails
  UC->>CV: triggerCvExtractIfPossible
  UC-->>Ctrl: JobApply
```

### Use cases

- `ApplyJobUseCase`

### Edge cases

- Apply method `email` → status `email_sent`, trigger CV extract
- Có `fitAssessment`/`matchScore` → skip CV extract trigger
- Referral ID → tạo job referral link trước apply

---

## Flow 2: Close Job

### Mô tả

Đóng job với lý do; có thể kèm blockchain transaction qua port out.

### Sequence

```mermaid
sequenceDiagram
  participant ATS as ATS_GraphQL
  participant Resolver as CloseJobResolver
  participant UC as CloseJobUseCase
  participant Tx as CloseJobTransactionPort

  ATS->>Resolver: v2CloseJob mutation
  Resolver->>UC: execute(command)
  UC->>Tx: close with blockchain if web3 job
  UC-->>Resolver: updated Job
```

### Use cases

- `CloseJobUseCase`
- `GetCloseJobReasonsUseCase` (choice options)

---

## Flow 3: Job List/Detail Enrichment

### Mô tả

Sau khi load jobs, enrich organization logo, web3 events, payment distributions cho completed jobs.

### Sequence

```mermaid
sequenceDiagram
  participant UC as GetJobsUseCase
  participant Query as JobListQueryPort
  participant Logo as OrganizationLogoPort
  participant Web3 as Web3EventsPort
  participant Pay as PaymentDistributionsPort

  UC->>Query: execute(filters)
  UC->>Logo: attachLogoUrlsToJobs
  UC->>Web3: getByJobIds
  UC->>Pay: getByJobIds for completed only
```

### Use cases

- `GetJobsUseCase`
- `GetJobUseCase` (single job, same ports)

### Rule

Không duplicate enrich logic — dùng chung port out `ORGANIZATION_LOGO_PORT_OUT`, `WEB3_EVENTS_FOR_JOBS_PORT_OUT`, `PAYMENT_DISTRIBUTIONS_FOR_JOBS_PORT_OUT`.

---

## Flow 4: HR Add Candidate

### Mô tả

ATS thêm candidate qua REST; có bước extract resume riêng gọi FastAPI.

### Use cases

- `HrAddCandidateUseCase` — `POST /jobs/:id/hr_add_candidate`
- `HrAddCandidateExtractResumeUseCase` — `POST /jobs/:id/hr_add_candidate/extract_resume`

---

## Flow 5: Job Applies / Referrals List

Chi tiết filters, pagination, response shape: [job-applies-referrals.md](job-applies-referrals.md).
