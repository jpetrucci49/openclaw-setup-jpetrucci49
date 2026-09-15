---
name: "breathecode-my-cohorts-read"
description: "Identify the student’s 4Geeks cohorts, schedule, role, and enrollment status."
---

# BreatheCode My Cohorts Read

## Purpose
Identify the authenticated student’s 4Geeks cohort enrollments and report cohort identity, schedule, role, and educational status.

## Endpoint
- `GET https://breathecode.herokuapp.com/v1/admissions/academy/cohort/me`
- Authentication: `Authorization: Token <TOKEN_4GEEKS>`
- Optional query parameters:
  - `academy` (academy id)
  - `educational_status` (for targeted filtering, such as `ACTIVE` or `GRADUATED`)

## Credential safety
- Read `TOKEN_4GEEKS` from `~/.openclaw/.env` or the approved local secret mechanism at execution time.
- Never print, persist, or log the token, authorization header, raw response, or unrelated sensitive profile data.

## Output
Return each enrollment with safe fields available in the response:
- cohort id, name, and slug
- schedule
- student role
- educational status
- enrollment date when available

Clearly identify `ACTIVE` enrollments and `GRADUATED` enrollments. If multiple active cohorts exist, list them all and do not infer a single cohort. If there are no active enrollments, say so explicitly.

## Scope boundary
- Read-only and focused on one API action.
- Do not fetch tasks, activity, profile data, projects, certificates, or perform writes.
- Do not silently discard non-active enrollments; report the returned statuses.

## Source
Attached `STUDENT_API_CALLS_REFERENCE---857517e5-7226-4163-9410-a4ea150a36a3.md`, received 2026-09-14. Educational statuses include `ACTIVE`, `GRADUATED`, `SUSPENDED`, and `DROPPED`.
