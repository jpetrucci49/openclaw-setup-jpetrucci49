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

### Security decisions
- The student token was not requested, pasted, stored, or logged.
- No external API call was made.
- No skill was applied or installed; all proposals require separate explicit approval.
- Proposed skills must never expose credentials in chat, logs, memory, skill files, or error reports.

### Source
Attached API reference: `STUDENT_API_CALLS_REFERENCE---857517e5-7226-4163-9410-a4ea150a36a3.md`, received 2026-09-14. Base URL documented there: `https://breathecode.herokuapp.com`.
