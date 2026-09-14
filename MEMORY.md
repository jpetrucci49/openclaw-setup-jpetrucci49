# MEMORY.md - Long-Term Memory

_Last updated: 2026-09-14_

## People

- **Joe (Joseph)** — developer, America/New_York, joseph.petrucci49@gmail.com, GitHub `jpetrucci49`
- **Nova ⚡** — Joe's OpenClaw agent (see IDENTITY.md, SOUL.md)

## Infrastructure

| Component | Status |
|-----------|--------|
| OpenClaw gateway | ✅ systemd user service |
| Telegram | ✅ @SecondNewOpenClawHelperBot, owner `telegram:8991213066` |
| LiteLLM (4Geeks) | ✅ `gpt-5.6-luna` primary |
| Zapier MCP | ✅ Gmail, Calendar, Drive, Docs, Tasks, GitHub, Telegram |

## Custom skills (active)

| Skill | Slash | Verified |
|-------|-------|----------|
| `github-daily-digest` | `/github` | GitHub read via Zapier ✅ |
| `gmail-drafts-in-your-voice` | `/draft` | Gmail draft `1a09dfa3324c04cf` ✅ (2026-09-14) |

Removed: `week-ahead-briefing`, `inbox-calendar-triage` (replaced by focused skill pair).

## Joe's preferences (distilled)

- Act first on reads; ask before external writes
- Drafts in Joe's voice; never auto-send email
- TypeScript + Next.js; no unprompted commits

## Update log

- **2026-09-14** — Five bootstrap files configured; two skills implemented and tested
- **2026-09-13** — Zapier OAuth complete; gateway + Telegram working
