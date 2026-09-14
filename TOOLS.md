# TOOLS.md - Joe's Integration Cheat Sheet

Skills define _how_; this file defines _your_ accounts, defaults, and when to use each service.

## mcporter (MCP CLI)

| Setting | Value |
|---------|-------|
| Workspace config | `/root/.openclaw/workspace/config/mcporter.json` |
| Fallback (cwd `/root`) | `/root/config/mcporter.json` |
| List Zapier tools | `mcporter list zapier --schema` |
| Before first execute | `mcporter call zapier.inspect_zapier_actions selected_api=...` |

Always pass `--output json` for machine-readable results.

---

## Zapier MCP — when to use what

**Primary path for all Google + GitHub actions.** OAuth is complete — do not start new OAuth flows.

| Service | `selected_api` | Use when | Default account |
|---------|----------------|----------|-----------------|
| **Gmail** | `GoogleMailV2CLIAPI` | `/draft`, read inbox, search threads | joseph.petrucci49@gmail.com |
| **Google Calendar** | `GoogleCalendarCLIAPI` | Schedule events, find conflicts, busy times | joseph.petrucci49@gmail.com (primary calendar) |
| **Google Drive** | `GoogleDriveCLIAPI` | Save meeting notes, uploads | joseph.petrucci49@gmail.com |
| **Google Docs** | `GoogleDocsV2CLIAPI` | Append learning logs, formatted docs | joseph.petrucci49@gmail.com |
| **Google Tasks** | `GoogleTasksCLIAPI` | Action items, todo list | Default task list |
| **GitHub** | `GitHubCLIAPI` | `/github` digest, issues, PRs, reviews | jpetrucci49 |
| **Telegram (Zapier)** | `TelegramCLIAPI` | Only if action must go through Zapier | @SecondNewOpenClawHelperBot |

**Prefer native OpenClaw Telegram** for delivering replies to Joe (`telegram:8991213066`). Use Zapier Telegram only for Zapier-specific automations.

---

## Service defaults

### Gmail

- **Account:** joseph.petrucci49@gmail.com
- **Draft workflow:** Preview in chat → Joe approves → `gmail_create_draft` or `google_mail_create_draft_reply`
- **Sign-off:** `Best, Joe` — see `USER.md` § Email voice
- **Never** use send actions (`message`, `reply_to_message`) unless Joe says "send it"

### Google Calendar

- **Calendar:** Primary (joseph.petrucci49@gmail.com)
- **Timezone:** `America/New_York` on all events
- **Create events:** Only after Joe confirms time and title

### Google Drive / Docs

- **Account:** joseph.petrucci49@gmail.com
- **Meeting notes folder:** _(not set yet — ask Joe before first save)_
- **Learning journal doc:** _(not set yet — ask Joe before first append)_

### Google Tasks

- **List:** Default
- **Use for:** Action items Joe explicitly asks to create — not auto-created by current skills

### GitHub

- **User:** jpetrucci49
- **Read freely** for `/github`; no writes without approval per `AGENTS.md`

---

## Custom skills

| Skill | Slash | Zapier touchpoint | Output destination |
|-------|-------|-------------------|-------------------|
| `github-daily-digest` | `/github` | GitHub read actions | Telegram / TUI message |
| `gmail-drafts-in-your-voice` | `/draft` | Gmail draft write | Gmail Drafts folder |

Both skills **must** read `USER.md` (voice, accounts) and `SOUL.md` (preview-before-save) before executing.

---

## Composio (secondary — not OAuth'd)

Configured in mcporter but not authenticated. **Use Zapier first.** Fall back to Composio only if Joe completes OAuth and Zapier is unavailable.

---

## Telegram (native channel)

| Setting | Value |
|---------|-------|
| Bot | @SecondNewOpenClawHelperBot |
| Owner | `telegram:8991213066` |
| Policy | Pairing; groups require @mention |

Deliver `/github` digests here when Joe is on mobile. Keep under ~4,000 characters.

---

## Verified integrations (2026-09-14)

- **Gmail draft write:** ✅ `gmail_create_draft` — test draft id `1a09dfa3324c04cf` (subject: "OpenClaw /draft skill verification")
- **GitHub read:** ✅ `github_new_notification` — returns successfully (empty when no notifications)
- **Zapier OAuth:** ✅ All seven apps enabled per `MEMORY.md`
