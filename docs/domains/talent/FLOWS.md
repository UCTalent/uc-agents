# Talent — Business Flows

## Flow: Ensure Talent Profile (cross-module)

Job apply và các flow khác gọi `EnsureTalentProfileUseCase` qua port out — không tạo talent inline.

```mermaid
sequenceDiagram
  participant Job as ApplyJobUseCase
  participant Port as TalentPortOut
  participant UC as EnsureTalentProfileUseCase

  Job->>Port: ensureTalentProfile(userId)
  Port->>UC: execute
  UC-->>Job: Talent entity
```

## Flow: Resume Upload

```mermaid
sequenceDiagram
  participant FE as NextJS
  participant Ctrl as TalentController
  participant UC as UploadResumeUseCase
  participant FS as FileStorage

  FE->>Ctrl: POST /talents/me/resumes
  Ctrl->>UC: execute
  UC->>FS: store file
```

## Flow: Onboarding

`OnboardingTalentUseCase` — POST/PUT `/me`, PATCH `/me/onboarding`. Cập nhật step, headline, skills sau signup.

## FastAPI boundary

CV extract/evaluate → frontend gọi FastAPI trực tiếp hoặc job apply trigger — không implement parse logic trong talent module.
