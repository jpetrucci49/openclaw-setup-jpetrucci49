---
name: "breathecode-progress-summary-read"
description: "Summarize course progress from the student profile’s cohort completion data."
---

# BreatheCode Progress Summary Read — v2

## Purpose
Provide a general course-progress overview from the authenticated student’s profile cohort completion data.

## Endpoint
- `GET https://breathecode.herokuapp.com/v1/admissions/user/me`
- Authentication: `Authorization: Token <TOKEN_4GEEKS>`
- Use `cohorts[].completion` as the authoritative progress source when present.

## Output
Return a concise summary including:
- active cohort names and statuses
- per-cohort completion: total, completed, percentage, and `is_complete` when provided
- required-work completion and pending required count
- missing required project slugs when useful
- an aggregate overview only when aggregation is mathematically valid; otherwise keep cohorts separate

Do not use or call `/v1/activity/me` for this skill because it returns 403 for this token/instance. Do not invent percentages. Clearly distinguish profile-provided completion from activity-based metrics, which are unavailable.

## Credential and privacy safety
- Read `TOKEN_4GEEKS` from `~/.openclaw/.env` or approved local secret storage at execution time.
- Never print or log the token, authorization header, raw response, or unrelated profile data.
- Return progress data only to Joe in the private conversation.

## Scope boundary
Read-only and limited to this one profile API action. Do not call task, project, cohort-secondary, activity, registry, certificate, or write endpoints.

## Live evidence
The profile endpoint returned HTTP 200 with authoritative cohort completion records. `/v1/activity/me` returned HTTP 403 without and with documented filters, so activity-based progress is not available from this instance/token.
