# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.

## Composio shortcuts

For Google Calendar, Google Docs, and similar Composio tasks:

- Use the configured `composio` MCP server directly; do not run discovery searches unless a call fails.
- Default timezone: `America/New_York`.
- Typical calendar flow: create event with title, start, end, and optional description in one call.

## Zapier MCP

Primary integration path for Google Docs, Google Calendar, Gmail, Google Drive, Google Tasks, GitHub, and Telegram actions.

- MCP server name: `zapier` (OAuth via `openclaw mcp login zapier`)
- Endpoint: `https://mcp.zapier.com/api/v1/connect`
- After OAuth, enable app tools in the Zapier MCP dashboard or via the `get_zapier_skill` onboarding flow.
- Telegram is also available natively as an OpenClaw channel; use Zapier Telegram tools only when an action must go through Zapier.

## Related

- [Agent workspace](/concepts/agent-workspace)
