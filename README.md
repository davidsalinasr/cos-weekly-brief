# Chief of Staff Weekly Brief

A Claude skill that turns scattered work signals into a single prioritized weekly brief.

It scans your **messaging, email, calendar, meeting transcripts, CRM, and project tracker**, synthesizes everything into P0/P1/P2 action items, and ships the brief through five channels at once: an HTML email draft, a Slack self-message, a Word document, an Excel tracking log, and tickets on your executive board.

Built to run every Friday at 5pm so Monday starts with a clear plan, but you can trigger it on demand any time.

## What you get

| Output | Purpose |
|---|---|
| HTML email draft | Sits in your inbox, ready to read first thing Monday |
| Slack self-message | 30-second glance version, pinnable in your DMs |
| `.docx` brief | The full version with all detail, archived in your workspace |
| `.xlsx` tracking log | One row per week — long-running record of decisions and follow-ups |
| Executive-board tickets | P0/P1 items become trackable tasks automatically (Jira, Linear, Asana, ClickUp, Monday, or Notion) |

## Zero-config setup

The skill ships with a guided onboarding flow. The first time you run it, it will:

1. Greet you and explain what it's about to do.
2. Ask for your name, title, and company.
3. Ask which tools you actually use (pick from common defaults — Slack, Gmail, Google Calendar, Fathom, HubSpot, Jira — or alternatives like MS Teams, Outlook, Linear, Asana, etc.).
4. **Auto-discover everything else from those connected tools.** Slack user ID, Jira cloud ID, HubSpot pipeline stages — all read directly from the APIs. You never paste an ID.
5. Propose sensible defaults for schedule, workspace folder, and priority rules. You confirm or adjust with one yes.
6. Save your settings to a local `config.json` (gitignored) and run your first brief.

If you don't have one of the standard tools (say, no Jira), it asks if you have an alternative and walks you through connecting it. If you don't use a category at all, the brief just skips that section.

## Prerequisites

You'll need [Claude Code](https://www.anthropic.com/claude-code) (or another Claude client that supports skills) plus at least a couple of these MCP integrations connected. The more you connect, the richer the brief:

| Category | Common providers |
|---|---|
| Messaging | Slack, MS Teams, Discord |
| Email | Gmail, Outlook / MS365 |
| Calendar | Google Calendar, Outlook Calendar |
| Meeting transcripts | Fathom, Granola, Fireflies, Otter, Zoom AI |
| CRM | HubSpot, Salesforce, Close, Pipedrive |
| Project tracker | Jira, Linear, Asana, ClickUp, Monday, Notion |

The skill only requires what you actually use — connecting one of each row is plenty.

## Quick start

1. Clone or copy this folder into your skills directory.
2. Connect the MCPs you want to scan (the skill will tell you when one is missing).
3. Trigger the skill: *"run my weekly brief"*, *"chief of staff brief"*, *"prep my Monday"*.
4. Walk through the guided setup the first time. Subsequent runs are silent.

## Reconfiguring

Say *"reconfigure the brief"* or *"change my brief settings"* any time. The skill loads your existing config, lets you change just what you want, and saves.

## Customization

The defaults are opinionated but every part is overridable in `config.json`:

- **Priority bands.** P0/P1/P2 definitions live in `SKILL.md` and can be overridden in the `priority` block of `config.json`.
- **Channels scanned.** Auto-discovered from your activity, but you can pin a specific list during onboarding.
- **CRM pipeline stages.** Pulled directly from your CRM schema — no manual entry.
- **Visual styling.** Colors and table layouts are in `references/email-template.md` and `references/docx-template.md`.
- **Cadence.** Default is Friday 5pm; pair with the `schedule` skill (or any external scheduler) to run automatically.

For a manual / advanced setup walkthrough, see [`SETUP.md`](./SETUP.md).

## Contributing

PRs welcome — especially for additional providers (Notion-as-tracker, Salesforce CRM, MS Teams messaging, etc.), alternative output channels, or non-English templates.

## License

MIT. See [`LICENSE`](./LICENSE).
