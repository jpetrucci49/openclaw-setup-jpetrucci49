---
name: "breathecode-pending-work-read"
description: "List the authenticated student’s pending 4Geeks work still needing completion."
---

# BreatheCode Pending Work Read

## Purpose
List all assigned 4Geeks work that remains pending and needs completion by the authenticated student.

## Endpoint
- `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`
- Authentication: `Authorization: Token <TOKEN_4GEEKS>`
- Query parameters:
  - `task_status=PENDING`
  - `limit` and `offset` for pagination

Do not restrict `task_type` by default: include pending projects, exercises, lessons, and quizzes so the result represents the student’s complete outstanding workload.

## Credential safety
- Read `TOKEN_4GEEKS` from `~/.openclaw/.env` or the approved local secret mechanism at execution time.
- Never print, persist, or log the token, authorization header, raw response, or unrelated sensitive profile data.

## Output
Return a concise checklist of pending work using safe fields available in the response, such as task id, title/description, task type, due date, cohort, and review metadata. Group or sort by task type and due date when those fields are available. Clearly state the total count. If no pending tasks are returned, report that the outstanding-work list is empty.

## Scope boundary
- Read-only and focused on one API action.
- Do not fetch task details, update submissions, deliver tasks, resolve cohorts, list completed work, or perform writes.
- Use pagination and avoid bursts of duplicate requests.
- “Still have to complete” is defined as API status `PENDING`; do not silently include `DONE`, `APPROVED`, or `REJECTED` tasks.

## Source and interpretation
Attached `STUDENT_API_CALLS_REFERENCE---857517e5-7226-4163-9410-a4ea150a36a3.md`, received 2026-09-14. This skill complements `breathecode-active-projects-read` by covering all pending task types rather than projects across multiple statuses.
