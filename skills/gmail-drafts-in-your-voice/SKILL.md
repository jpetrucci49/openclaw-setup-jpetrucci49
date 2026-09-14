---
name: gmail-drafts-in-your-voice
description: "Draft Gmail in Joe's voice using USER.md cues; preview in chat, save to Gmail Drafts only after explicit approval."
user-invocable: true
metadata:
  {
    "openclaw":
      {
        "emoji": "✉️",
        "requires": { "bins": ["mcporter"] },
      },
  }
---

# Gmail Drafts in Your Voice · `/draft`

Write email as **Joe** — not as Nova. The draft should need minimal editing in Gmail.

## Before you start — read config

| File | Use for |
|------|---------|
| `USER.md` | **Email voice** — tone, sign-off, avoid-list, technical vs client style |
| `TOOLS.md` | Gmail account, `gmail_create_draft` / `google_mail_create_draft_reply`, never send |
| `SOUL.md` | Preview → approve → save; one clarifying question max |
| `AGENTS.md` | **Stop and ask** before send; drafts OK after preview + "save" |
| `IDENTITY.md` | Chat as Nova; draft body as Joe |

## Invoke

`/draft` · "draft a reply to …" · "write an email to …"

## Joe's voice (from USER.md — do not improvise a different persona)

- Direct, professional, friendly-not-chatty
- First sentence = the answer or the ask
- No "I hope this email finds you well"
- Sign-off: `Best, Joe` (default) · `Thanks, Joe` (quick ack)
- Technical replies: bullets for multi-part answers
- Override only when Joe says "more formal", "quick ack", or "casual"

## Phase 1 — Context (read-only)

If Joe gives a subject/thread but not the email body:

```bash
mcporter call zapier.execute_zapier_read_action \
  selected_api=GoogleMailV2CLIAPI \
  action=get_conversation \
  tool_name=google_mail_get_conversation \
  --args '{"params": {"thread_id": "..."}}' --output json
```

For **new** emails: Joe must provide recipient + intent.

## Phase 2 — Preview (always — SOUL.md gray area rule)

Show full draft in chat **before** touching Gmail:

```markdown
**Draft — {Re: subject | New: subject}**

{body}

Best,
Joe
```

Ask: **"Save to Gmail Drafts?"** — wait for yes / save / looks good.

Do not save if Joe wants edits — revise preview first.

## Phase 3 — Save (after approval only)

Config: `mcporter --config /root/.openclaw/workspace/config/mcporter.json`

**New message:**

```bash
mcporter call zapier.execute_zapier_write_action \
  selected_api=GoogleMailV2CLIAPI \
  action=draft_v2 \
  tool_name=gmail_create_draft \
  --args '{"params": {"to": "...", "subject": "...", "body": "...", "body_type": "plain"}}' \
  --output json
```

**Reply to thread:**

```bash
mcporter call zapier.execute_zapier_write_action \
  selected_api=GoogleMailV2CLIAPI \
  action=draft_v2_reply \
  tool_name=google_mail_create_draft_reply \
  --args '{"params": {"thread_id": "...", "body": "..."}}' \
  --output json
```

Confirm: `Draft saved — {subject}. Check Gmail → Drafts.`

## Rules

- **Never send** — no `message` or `reply_to_message` unless Joe says "send it" in a separate message
- Don't log full email threads to `MEMORY.md`
- Don't guess dates, commitments, or recipients — ask once if unclear
- Verified tool: `gmail_create_draft` (see `TOOLS.md` verified integrations)

## Pairs with

- `github-daily-digest` — email follow-up on GitHub items
