---
name: "breathecode-my-cohorts-read"
description: "Read the student’s 4Geeks cohorts, schedules, roles, statuses, and completion."
---

# BreatheCode My Cohorts Read — v3

## Purpose
Identify the authenticated student’s 4Geeks cohort enrollments and report cohort identity, schedule, role, educational status, and completion when available.

## Endpoint
- `GET https://breathecode.herokuapp.com/v1/admissions/user/me`
- Authentication: `Authorization: Token <TOKEN_4GEEKS>`
- Use the response’s `cohorts[]` array as the authoritative enrollment source.

## Output
For each `cohorts[]` record, return available safe fields:
- `cohort.id`, `cohort.name`, `cohort.slug`
- `cohort.kickoff_date`, `cohort.ending_date`, and schedule fields when present
- enrollment `role`
- `educational_status` (`ACTIVE`, `GRADUATED`, `SUSPENDED`, or `DROPPED`)
- completion summary only when present

Clearly separate active and graduated cohorts. List all returned records; do not infer a single cohort. If the array is empty, report that no cohort enrollments were returned.

## Credential and privacy safety
- Read `TOKEN_4GEEKS` from `~/.openclaw/.env` or approved local secret storage at execution time.
- Never print or log the token, authorization header, raw response, or unrelated profile fields.
- Return cohort data only to Joe in the private conversation.

## Scope boundary
Read-only and limited to this one profile API action. Do not call the secondary cohort endpoint, task endpoints, activity endpoints, or perform writes.

## Live evidence
The profile endpoint returned HTTP 200 and cohort records. The previous `/v1/admissions/academy/cohort/me` approach was unreliable for this account: academy=4 returned HTTP 200 with an empty list, while other profile academy IDs returned 403.
