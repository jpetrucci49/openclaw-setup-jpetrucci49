# SKILLS_DESIGN.md — Skills We Are Implementing

Design-before-build reference for Nova's next two custom skills. Each entry answers:

1. **What does this skill do?** — one sentence  
2. **What input does the agent need?** — what you provide, in what format, and what Nova already knows from the five configuration files  
3. **What does a good output look like?** — format, destination, and how you'll know it worked  

---

## Why these two?

From the skill ideas list, the question is: _which would Joe actually trigger more than once?_

| Skill idea | Verdict | Reason |
|------------|---------|--------|
| Daily learning log | Pass | Useful, but sporadic — not weekly rhythm |
| Week plan | Pass | Overlaps `/week-ahead`; heavier write workflow for Mondays only |
| Meeting notes | Pass | Event-driven, not repeatable enough |
| Smarter calendar events | Consider later | High value, but lower frequency than email/GitHub for Joe |
| **Gmail drafts in your voice** | **Build** | Follow-on to `/triage` almost every time a thread needs a reply |
| **GitHub daily digest** | **Build** | Developer daily check-in; lighter than full `/week-ahead` |
| Task → Calendar | Pass | Weekly at best; scheduling prefs need more setup |
| Inbox triage → Tasks | Pass | Already covered by `/triage` (report + optional task creation) |

**Selected pair:** `github-daily-digest` + `gmail-drafts-in-your-voice` — both sharpen existing behaviors, run several times per week, and complement skills already in the workspace (`week-ahead-briefing`, `inbox-calendar-triage`).

---

## Shared context (all skills)

Nova reads these at session start — you rarely need to repeat them unless overriding a default.

| Config file | What skills inherit |
|-------------|---------------------|
| `USER.md` | Joe's name, timezone (`America/New_York`), email, GitHub handle, connected accounts, **email voice cues** (for drafts) |
| `IDENTITY.md` | Nova's role, slash commands, working agreement (bold reads, careful writes) |
| `SOUL.md` | Drafts only — never send without explicit approval; concise on Telegram |
| `MEMORY.md` | Zapier OAuth status, gateway health, registered skill names |
| `HEARTBEAT.md` | Periodic nudges (Monday `/week-ahead`, Friday `/triage` when unread >10) |

**Integrations:** GitHub and Gmail via Zapier MCP + `mcporter`. Config: `/root/.openclaw/workspace/config/mcporter.json`.

---

## 1. `github-daily-digest` · `/github`

**Category:** Sharpens existing behavior  
**Slash command:** `/github`  
**Skill path:** `workspace/skills/github-daily-digest/SKILL.md`

### 1. What does this skill do?

Reads Joe's open GitHub issues, pull requests, review requests, and recent activity, then sends a short, actionable briefing to the current session (Telegram or TUI).

### 2. What input does the agent need?

**What you give it**

| Input | Required? | Format | Example |
|-------|-----------|--------|---------|
| Trigger | Yes | Slash command or plain text | `/github`, "what's up on GitHub?", "any reviews waiting?" |
| Time window | No | Natural language | Default: last 24h for activity; all open for issues/PRs |
| Scope | No | Repo name or org | Default: all repos for `jpetrucci49` |
| Focus | No | Keyword | "reviews only", "my open PRs" |

**What Nova already knows (from the five files)**

- GitHub username `jpetrucci49` → `USER.md`
- Zapier GitHub connection is live → `MEMORY.md`
- Keep Telegram replies short; no filler → `SOUL.md`, `IDENTITY.md`
- Full week context lives in `/week-ahead` — this skill is the **fast GitHub-only** pass → design intent

**What Nova fetches (via Zapier MCP)**

- Notifications (`GitHubCLIAPI` / `notification`)
- Review requests (`review_request`)
- Open PRs and issues assigned to Joe (`repo_pull`, `repo_issue` search actions)
- Recent commits or repo events when useful (`commit`, `event`)

Use `mcporter call zapier.inspect_zapier_actions selected_api=GitHubCLIAPI` before first execute if schemas are uncertain.

### 3. What does a good output look like?

**Format**

```markdown
# GitHub Digest — {date}

## Needs your review ({N})
- owner/repo#123 — {title} ({age})

## Your open PRs ({N})
- owner/repo#456 — {title} ({status})

## Issues assigned to you ({N})
- owner/repo#789 — {title}

## Recent activity (24h)
- {commit/PR/event summary}

## Suggested next action
1. {single most important thing to do now}
```

**Destination:** Reply in the **current session** — Telegram (≤4,000 chars) or `openclaw chat` TUI. Read-only; no GitHub writes unless Joe asks separately in the same session.

**How you'll know it worked**

- [ ] Every cited item includes `owner/repo#number` or a link
- [ ] "Needs review" is separated from "waiting on others"
- [ ] Empty sections say so explicitly ("No review requests — you're clear")
- [ ] One concrete **Suggested next action**, not generic advice
- [ ] Fits one Telegram message; offer "expand" on request
- [ ] Completed in seconds — no full `/week-ahead` unless Joe asks for it

---

## 2. `gmail-drafts-in-your-voice` · `/draft`

**Category:** Sharpens existing behavior  
**Slash command:** `/draft`  
**Skill path:** `workspace/skills/gmail-drafts-in-your-voice/SKILL.md`

### 1. What does this skill do?

Drafts Gmail replies or new emails in Joe's voice — direct, professional, minimal fluff — shows the text for approval, then saves a Gmail draft (never sends automatically).

### 2. What input does the agent need?

**What you give it**

| Input | Required? | Format | Example |
|-------|-----------|--------|---------|
| Intent | Yes | Plain language | "Reply thanking them, propose a call next week" |
| Thread context | No | Thread ID, `/triage` row #, or pasted email | "draft for triage #2" |
| Tone override | No | Keyword | "more formal", "quick ack only" |
| New vs reply | No | Implicit or stated | "new email to vendor@…" vs reply |

**What Nova already knows (from the five files)**

- Email account `joseph.petrucci49@gmail.com` → `USER.md`
- **Voice cues** (tone, sign-off, patterns) → `USER.md` § Email voice
- Drafts only; never send/archive without approval → `SOUL.md`
- Prefer `draft_v2_reply` over send actions → `inbox-calendar-triage` skill, `IDENTITY.md`
- Thread may already be classified as **reply** by `/triage` → workflow handoff

**What Nova fetches (optional, via Zapier MCP)**

- Thread body via `get_conversation` when Joe gives a thread ID or triage reference but not the email text
- Inspect schema first: `GoogleMailV2CLIAPI`

### 3. What does a good output look like?

**Format — step 1 (always show in chat first)**

```markdown
**Draft — Re: {subject}**

{body paragraphs}

Best,
Joe
```

**Format — step 2 (after Joe says "save" / "looks good")**

Confirmation only:

```markdown
Draft saved in Gmail — Re: {subject}
Open Gmail → Drafts to review before sending.
```

**Destination**

| Step | Where |
|------|-------|
| Preview | Telegram or TUI (same session) |
| Saved draft | Gmail via Zapier `draft_v2_reply` (reply) or `draft_v2` (new message) |

**How you'll know it worked**

- [ ] Draft addresses every question or request in the original thread
- [ ] Tone matches `USER.md` voice cues — no "I hope this email finds you well"
- [ ] Joe saw the full text **before** anything was written to Gmail
- [ ] Gmail draft exists after approval; Nova confirms with subject line
- [ ] No email was **sent** — only draft created unless Joe gives a separate explicit "send it"
- [ ] Sensitive content was not logged to `MEMORY.md` or other persistent files

---

## Workflow: how the four skills fit together

```text
Monday     → /week-ahead     (full week digest)
Daily      → /github         (quick GitHub check)
As needed  → /triage         (inbox sort)
After triage → /draft        (reply in Joe's voice)
```

---

## Implementation checklist

- [ ] `workspace/skills/github-daily-digest/SKILL.md`
- [ ] `workspace/skills/gmail-drafts-in-your-voice/SKILL.md`
- [ ] Register both in `openclaw.json` → `agents.defaults.skills`
- [ ] Add email voice cues to `USER.md`
- [ ] Gateway restart or `/new` in chat to load skills
