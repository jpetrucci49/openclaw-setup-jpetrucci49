---
name: "breathecode-my-cohorts-read"
description: "Identify the student’s 4Geeks cohort, schedule, role, and enrollment status."
---

# BreatheCode My Cohorts Read — v2

## Purpose
Identify the authenticated student’s 4Geeks cohort enrollments and report cohort identity, schedule, role, and educational status.

## Endpoint
- `GET https://breathecode.herokuapp.com/v1/admissions/academy/cohort/me?academy=<academy_id>`
- Authentication: `Authorization: Token <TOKEN_4GEEKS>`
- The `academy` query parameter is required by this instance.
- Resolve `academy_id` from the authenticated profile’s `profile_academy` relation before calling this endpoint. Do not hardcode it.
- Optional query: `educational_status` for targeted filtering such as `ACTIVE` or `GRADUATED`.

## Credential and privacy safety
- Read `TOKEN_4GEEKS` from `~/.openclaw/.env` or the approved local secret mechanism at execution time.
- Never print or log the token, authorization header, raw response, or unrelated profile data.
- Return cohort data only in Joe’s private conversation.

## Output
Return each enrollment with available safe fields: cohort id/name/slug, schedule, role, educational status, and enrollment date. Clearly distinguish `ACTIVE`, `GRADUATED`, `SUSPENDED`, and `DROPPED`. If multiple active cohorts exist, list all. If the filtered response is empty, say no enrollments were returned for the resolved academy.

## Scope boundary
Read-only. Do not fetch tasks, activity, profile data beyond the minimum academy id, projects, certificates, or perform writes.

## Test evidence
- Without `academy`: HTTP 403.
- With resolved `academy=4`: HTTP 200; empty enrollment list.
- The skill must preserve this behavior and report an empty result rather than treating it as an error.
