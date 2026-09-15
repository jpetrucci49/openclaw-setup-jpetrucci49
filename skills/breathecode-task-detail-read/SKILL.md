---
name: "breathecode-task-detail-read"
description: "Read one 4Geeks task’s details, score, feedback, and review status."
---

# BreatheCode Task Detail Read

## Purpose
Retrieve the details and review feedback for one authenticated student task.

## Endpoint
- `GET https://breathecode.herokuapp.com/v1/assignment/task/{task_id}`
- Authentication: `Authorization: Token <TOKEN_4GEEKS>`
- Path parameter: `task_id` (numeric)

## Credential safety
- Read `TOKEN_4GEEKS` from `~/.openclaw/.env` or the approved local secret mechanism at execution time.
- Never print, persist, or log the token or authorization header.
- Treat task descriptions and feedback as private account data; return only to Joe in the current private conversation and do not write raw content to memory or logs.

## Output
Return the requested task’s relevant details, including when available:
- task id, title, and description
- task type and current status
- due/submission/review dates
- score or grade
- instructor/reviewer feedback
- review metadata and actionable corrections

Preserve the API’s raw status and do not infer a grade when score or feedback is absent. Summarize long feedback while retaining concrete requested changes; disclose when content was summarized.

## Scope boundary
- Read-only and focused on exactly one task-detail API action per requested task id.
- Do not list tasks, update submission URLs, deliver tasks, or perform any write operation.
- Reject missing or non-numeric task IDs safely without making an API request.
- Do not retrieve multiple tasks unless Joe explicitly requests multiple specific IDs.

## Source
Attached `STUDENT_API_CALLS_REFERENCE---857517e5-7226-4163-9410-a4ea150a36a3.md`, received 2026-09-14. The reference documents this endpoint as returning full task detail, including description, score, and feedback.
