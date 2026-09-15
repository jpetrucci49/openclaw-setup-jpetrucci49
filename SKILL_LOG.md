# Skill Log

## 2026-09-14 — 4Geeks/BreatheCode student API integration

### Conversation context
Joe asked how to give Nova access to his 4Geeks account using a student token without writing code. Nova advised identifying the service/API, preferring read-only access, not sharing the token in chat, storing it only in a protected local secret mechanism, and testing with a read-only request.

Joe then attached `STUDENT_API_CALLS_REFERENCE---857517e5-7226-4163-9410-a4ea150a36a3.md`, a BreatheCode/4Geeks student API reference, and requested that each skill remain focused on one API action, that every created skill and conversation leading to it be documented here, and that a memory reference be created.

### Skills proposed

1. **breathecode-current-user-read**
   - Focus: `GET /v1/admissions/user/me`
   - Scope: authenticated current-student profile; read-only.
   - Proposal ID: `breathecode-current-user-read-20260914-a5719f85d6`
   - Status: pending; not installed.

2. **breathecode-my-cohorts-read**
   - Focus: `GET /v1/admissions/academy/cohort/me`
   - Scope: authenticated student cohort enrollments; read-only.
   - Proposal ID: `breathecode-my-cohorts-read-20260914-7e049d752d`
   - Status: pending; not installed.

3. **breathecode-my-tasks-read**
   - Focus: `GET /v1/assignment/user/me/task`
   - Scope: authenticated assigned tasks; read-only, paginated.
   - Proposal ID: `breathecode-my-tasks-read-20260914-9374704797`
   - Status: pending; not installed.

4. **breathecode-token-auth-check**
   - Focus: one authenticated `GET /v1/admissions/user/me` request.
   - Scope: validate token presence/acceptance only; read-only; no profile dump and no writes.
   - Proposal ID: `breathecode-token-auth-check-20260915-a582c60c7b`
   - Status: applied and tested.
   - Test result: HTTP 200 on 2026-09-15; token accepted and authenticated profile endpoint reachable.

### Security decisions
- The student token was not requested, pasted, stored, or logged.
- No external API call was made.
- No skill was applied or installed; all proposals require separate explicit approval.
- Proposed skills must never expose credentials in chat, logs, memory, skill files, or error reports.

## 2026-09-15 — Token authentication check

### Conversation context
Joe specified that `TOKEN_4GEEKS` is stored in `~/.openclaw/.env` and requested that the first skill validate the token and confirm the session is active. The API reference does not define a separate session-status endpoint, so the skill uses the authenticated current-user endpoint as the token acceptance test.

### Skill implemented

4. **breathecode-token-auth-check**
   - Focus: one authenticated `GET /v1/admissions/user/me` request.
   - Scope: validate token presence/acceptance only; read-only; no profile dump and no writes.
   - Proposal ID: `breathecode-token-auth-check-20260915-a582c60c7b`
   - Status: applied and tested.
   - Test result: HTTP 200 on 2026-09-15; token accepted and authenticated profile endpoint reachable.

### Security decisions
- `~/.openclaw/.env` was used only as the local secret source; the token value was never printed or logged.
- The test made exactly one GET request and discarded the response body.
- No write operation was performed.
- A successful result confirms current token acceptance, not a distinct server-side session.
### Source
Attached API reference: `STUDENT_API_CALLS_REFERENCE---857517e5-7226-4163-9410-a4ea150a36a3.md`.

## 2026-09-15 — Project status read skill

### Conversation context
Joe first requested a skill to get the status of active projects. The initial interpretation was pending assigned project tasks. Joe clarified that the skill must retrieve projects with their current status: pending, submitted, or graded.

### Skill implemented

5. **breathecode-active-projects-read**
   - Focus: one assigned-task API action filtered to `task_type=PROJECT`.
   - Scope: list project tasks across statuses with pagination; no details, writes, delivery, or unrelated endpoints.
   - Status mapping: `PENDING` → Pending; `DONE` → Submitted; `APPROVED` → Graded — approved; `REJECTED` → Graded — changes requested.
   - Proposal ID: `breathecode-active-projects-read-20260915-0a3b7a01c7`
   - Status: applied and tested.
   - Test result: HTTP 200; 62 project tasks returned — 12 PENDING and 50 DONE.

### Security decisions
- No external API request was made.
- No token value was read, printed, stored, or logged.
- The revised proposal must be explicitly approved before installation or execution.
- Memory search was attempted but unavailable because index metadata is missing.

## 2026-09-15 — Pending work read skill

### Conversation context
Joe requested the next skill to get pending work and list what remains to be completed. This is defined as all assigned tasks with API status `PENDING`, without restricting task type, so projects, exercises, lessons, and quizzes are included.

### Skill proposed

6. **breathecode-pending-work-read**
   - Focus: one assigned-task API action filtered to `task_status=PENDING`.
   - Scope: list outstanding work across task types with pagination; no details, writes, delivery, or unrelated endpoints.
   - Proposal ID: `breathecode-pending-work-read-20260915-984340c3df`
   - Status: pending; not installed.

### Security decisions
- No external API request was made.
- No token value was read, printed, stored, or logged.
- The proposal must be explicitly approved before installation or execution.
- Memory search was attempted but unavailable because index metadata is missing.
