# HEARTBEAT.md - Periodic Checks

Lightweight, read-only nudges. See `AGENTS.md` Hard Limits — no external writes on heartbeat.

## Weekdays

- [ ] **GitHub pulse** — if >24h since last check, suggest `/github` when Joe messages
- [ ] **Gateway** — alert only if gateway is down

## Never on heartbeat

- Send email, create drafts, or post to GitHub
- Restart gateway or change config unprompted
