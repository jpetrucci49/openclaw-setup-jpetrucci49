---
name: "breathecode-active-projects-read"
description: "List the student’s 4Geeks projects with their current submission and grading status."
---

# BreatheCode Project Status Read

## Purpose
List the authenticated student’s assigned 4Geeks projects together with their current submission and grading status.

## Endpoint
- `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task`
- Authentication: `Authorization: Token <TOKEN_4GEEKS>`
- Query parameters:
  - `task_type=PROJECT`
  - `task_status` may be requested across the relevant project states
  - `limit` and `offset` for pagination

## Status normalization
The API reference documents these task statuses:
- `PENDING` → **Pending**
- `DONE` → **Submitted** (completed by the student; awaiting or undergoing review)
- `APPROVED` → **Graded — approved**
- `REJECTED` → **Graded — changes requested**

Do not collapse `DONE`, `APPROVED`, and `REJECTED` into a generic active state. Preserve the raw API status internally and present the normalized status clearly.

## Credential safety
- Read `TOKEN_4GEEKS` from `~/.openclaw/.env` or the approved local secret mechanism at execution time.
- Never print, persist, or log the token, authorization header, raw response, or unrelated sensitive profile data.

## Output
Return a concise project list with safe fields available in the response, such as task id, title/description, normalized status, raw status when useful, due date, score, feedback summary, and review metadata. Clearly separate pending, submitted, and graded projects. If no project tasks are returned, report that clearly.

## Scope boundary
- Read-only and focused on one API action.
- Do not fetch task details, update submissions, deliver tasks, resolve cohorts, or perform writes.
- Use pagination and avoid bursts of duplicate requests.

## Source and interpretation
Attached `STUDENT_API_CALLS_REFERENCE---857517e5-7226-4163-9410-a4ea150a36a3.md`, received 2026-09-14. “Submitted” is mapped from API status `DONE`; “graded” is represented by `APPROVED` or `REJECTED`. If the live API uses different semantics, report the raw status and revise the mapping rather than guessing.
