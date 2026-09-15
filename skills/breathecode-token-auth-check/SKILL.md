---
name: "breathecode-token-auth-check"
description: "Validate TOKEN_4GEEKS with one authenticated, read-only BreatheCode profile request."
---

# BreatheCode Token Authentication Check

## Purpose
Validate that the configured 4Geeks student token is present and currently accepted by the BreatheCode API.

## Credential
- Read `TOKEN_4GEEKS` from the approved local environment/secret mechanism at execution time.
- Never read the token into chat output, logs, memory, skill files, proposal content, shell history, or error messages.
- Fail safely with a generic configuration message if the secret is absent.

## Check
Make exactly one read-only request:
- `GET https://breathecode.herokuapp.com/v1/admissions/user/me`
- Header: `Authorization: Token <TOKEN_4GEEKS>`
- No query or body.

## Result handling
- HTTP 200: report that the token was accepted and the authenticated profile endpoint is reachable. Return only a minimal non-sensitive confirmation; do not dump the profile.
- HTTP 401/403: report that authentication failed or access was denied; do not expose request headers or token data.
- Other HTTP errors or timeout: report that validation could not be completed and include only safe status/category details.

## Scope boundary
- This skill validates token acceptance only.
- It does not list cohorts, tasks, certificates, activity, assets, events, or perform any write operation.
- The API reference does not define a separate session endpoint, so do not claim to validate a server-side session. A successful authenticated request confirms current token acceptance.

## Source
Attached `STUDENT_API_CALLS_REFERENCE---857517e5-7226-4163-9410-a4ea150a36a3.md`, received 2026-09-14.
