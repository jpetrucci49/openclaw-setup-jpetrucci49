---
name: "breathecode-progress-summary-read"
description: "Summarize the authenticated student’s 4Geeks course progress from personal activity."
---

# BreatheCode Progress Summary Read

## Purpose
Provide a general overview of the authenticated student’s progress through the 4Geeks course using the student’s own learning activity.

## Endpoint
- `GET https://breathecode.herokuapp.com/v1/activity/me`
- Authentication: `Authorization: Token <TOKEN_4GEEKS>`
- Optional query parameters:
  - `cohort` (id or slug, according to the instance)
  - `date_start`
  - `date_end`

## Credential safety
- Read `TOKEN_4GEEKS` from `~/.openclaw/.env` or the approved local secret mechanism at execution time.
- Never print, persist, or log the token, authorization header, raw response, or unrelated sensitive profile data.

## Output
Return a concise overview using fields actually provided by the API, such as:
- activity period covered
- learning activity/time
- exercises or learning events recorded
- cohort context when present
- notable recent activity or inactivity

Do not invent a completion percentage, rank, or “course percentage” unless the API explicitly supplies the required denominator and numerator. If the endpoint provides activity but not total curriculum completion, say so plainly and present an activity-based progress summary instead.

## Scope boundary
- Read-only and focused on one API action.
- Do not fetch assigned tasks, project statuses, cohort enrollments, registry assets, certificates, or perform writes.
- Use the optional date and cohort filters only when needed, and avoid bursts of duplicate requests.

## Source and interpretation
Attached `STUDENT_API_CALLS_REFERENCE---857517e5-7226-4163-9410-a4ea150a36a3.md`, received 2026-09-14. The reference describes `/v1/activity/me` as user learning activity including time and exercises; it does not guarantee a curriculum-completion percentage.
