---
name: github-daily-digest
description: "Read Joe's GitHub reviews, PRs, issues, and activity; deliver a concise digest styled per USER.md via Telegram or TUI."
user-invocable: true
metadata:
  {
    "openclaw":
      {
        "emoji": "🐙",
        "requires": { "bins": ["mcporter"] },
      },
  }
---

# GitHub Daily Digest · `/github`

On-demand GitHub briefing for **jpetrucci49**. Output must feel personal — not a generic API dump.

## Before you start — read config

| File | Use for |
|------|---------|
| `USER.md` | GitHub handle, daily clarity goal, Telegram as primary channel |
| `TOOLS.md` | `GitHubCLIAPI`, mcporter config path, native Telegram delivery |
| `SOUL.md` | Act first on reads; concise tone; no filler |
| `AGENTS.md` | Read-only — no GitHub writes without approval |
| `IDENTITY.md` | Name concrete items (`owner/repo#123`), not "some PRs" |

## Invoke

`/github` · "what's on GitHub?" · "any reviews waiting?"

## Phase 1 — Gather (parallel, no approval needed)

Config: `mcporter --config /root/.openclaw/workspace/config/mcporter.json`

Inspect once if needed: `mcporter call zapier.inspect_zapier_actions selected_api=GitHubCLIAPI`

```bash
# Notifications
mcporter call zapier.execute_zapier_read_action \
  selected_api=GitHubCLIAPI action=notification \
  tool_name=github_new_notification --output json

# Review requests
mcporter call zapier.execute_zapier_read_action \
  selected_api=GitHubCLIAPI action=review_request \
  tool_name=github_new_review_request --output json
```

For open PRs/issues: use `repo_pull` / `repo_issue` search actions from inspect output. Filter to Joe (`jpetrucci49`) as author, assignee, or reviewee when params allow.

Recent activity: last 24h via `commit`, `pull`, or `event` read actions — summarize, don't dump diffs.

## Phase 2 — Synthesize (Nova voice per SOUL.md)

```markdown
# GitHub Digest — {date, America/New_York}

## Needs your review ({N})
- owner/repo#123 — {title}

## Your open PRs ({N})
- owner/repo#456 — {title}

## Issues assigned to you ({N})
- owner/repo#789 — {title}

## Recent activity (24h)
- {bullets or "none"}

## Next up
→ {one concrete action — e.g. "Review owner/repo#123 first"}
```

**Personalization rules (from USER.md + IDENTITY.md):**

- Open with the single most important item if something is urgent
- Empty sections: "none — you're clear" (not omitted)
- End with **one** suggested next action, not a generic list
- Telegram: ≤4,000 chars; offer "expand" if truncated

## Phase 3 — Deliver

Send to **current session** — native Telegram to Joe (`telegram:8991213066`) or TUI. Read-only; no GitHub writes.

## Rules

- Never display raw `selected_api` IDs
- If a Zapier call fails, name the failed query and continue other sections
- Do not re-run OAuth or suggest new API connections

## Pairs with

- `gmail-drafts-in-your-voice` — when a GitHub notification needs an email reply
