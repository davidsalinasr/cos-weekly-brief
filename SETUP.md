# Setup Guide

> **You probably don't need this file.** The skill walks you through setup automatically the first time you run it — connect a few MCPs, then say *"run my weekly brief"* and follow the prompts. This guide is for advanced users who want to pre-populate `config.json`, debug a setup issue, or understand what the auto-discovery is actually doing.

---

## How auto-onboarding works

When you run the skill the first time (or any time `config.json` is missing), it does this:

1. **Greets you** and asks for your name, title, and company.
2. **Asks which tools you use** — a single multi-select question across six categories (messaging, email, calendar, transcripts, CRM, project tracker) with the most common provider as the default option.
3. **Checks which MCPs are connected.** For any tool you said yes to but haven't connected, it pauses and prompts you to connect it.
4. **Auto-discovers IDs** from each connected MCP:
   - Your Slack user ID and self-DM channel come from the Slack identity API.
   - Your email comes from the connected Gmail/Outlook account.
   - Your Google Calendar primary ID comes from `list_calendars`.
   - Your Jira cloud ID and account ID come from `getAccessibleAtlassianResources` and `atlassianUserInfo`.
   - Your HubSpot pipeline stages and probabilities are pulled directly from the `dealstage` property schema.
   - Equivalent flows exist for Linear, Asana, ClickUp, Monday, MS Teams, Outlook, Salesforce, etc.
5. **Proposes defaults** for schedule, workspace folder, stale-deal threshold, and priority rules. You confirm or adjust each.
6. **Writes everything to `config.json`** (gitignored).

You never paste a Slack user ID, a Jira cloud UUID, or a HubSpot pipeline stage. If you find yourself doing that, something has gone wrong — file an issue.

---

## Connecting MCPs

Each integration is a one-click connector in your Claude client. Open the connectors panel and search:

| Category | Default provider | Search term |
|---|---|---|
| Messaging | Slack | "Slack" |
| Email | Gmail | "Gmail" or "Google Workspace" |
| Calendar | Google Calendar | usually bundled with Gmail |
| Meeting transcripts | Fathom | "Fathom" |
| CRM | HubSpot | "HubSpot" |
| Project tracker | Jira | "Atlassian" |

If your provider isn't the default, search for it directly. The skill supports the alternatives listed in `README.md`; if yours isn't covered, it will fall back to skipping that category and you can supply data manually in chat.

---

## Manually pre-populating `config.json` (advanced)

If you'd rather skip onboarding and seed the config yourself:

1. Copy `config.example.json` to `config.json`.
2. Fill in the `user` block by hand.
3. For each MCP you've connected, run a one-liner in chat to fetch the ID. Examples:
   - *"Get my Slack user ID."*
   - *"Get my Atlassian cloud ID and account ID."*
   - *"List my Jira projects."*
   - *"List my HubSpot deal stages and their probabilities."*
4. Paste the values into the corresponding fields in `config.json`.
5. Set each `connected: true` for the categories you've populated.

The skill will skip onboarding on its next run and go straight to the brief.

---

## Customizing defaults

The values in `delivery` and `priority` blocks of `config.json` override the skill's built-in defaults:

| Field | Default | Notes |
|---|---|---|
| `delivery.schedule` | `Fri 17:00 local` | Free-text — used by the `schedule` skill or external scheduler |
| `delivery.workspace_path` | `~/Documents/Weekly-Briefs` | Created if it doesn't exist |
| `delivery.stale_deal_threshold_days` | `5` | Deals with more days since last activity get the pink-row treatment |
| `delivery.tone` | `Concise, direct, action-driven. No fluff.` | Passed to the LLM as a style constraint |
| `priority.p0` / `p1` / `p2` | (empty — uses defaults from `SKILL.md`) | Override with your own role-specific definitions |

To deeply customize the email/Word/Slack visuals, edit the templates in `references/`.

---

## Troubleshooting

- **Auto-discovery returns no Jira projects** — confirm the connected Atlassian account has access to at least one project. The skill calls `getVisibleJiraProjects`; if the list is empty, the auth is scoped too narrowly.
- **HubSpot pipeline stages came back wrong** — the skill reads from the default deal pipeline. If you have multiple pipelines, the discovery picks the one used most recently. You can override via the `crm.pipeline_stages` array in `config.json`.
- **Slack message renders as a giant text blob** — make sure the skill is using the standard send tool, not a draft variant. Drafts strip markdown formatting in some Slack MCPs.
- **Executive-board ticket creation fails with 403** — the connected account doesn't have permission to create issues in the chosen project. Either grant permission or pick a different project during reconfiguration.
- **Email draft has weird spacing** — open `references/email-template.md` and verify the inline CSS hasn't been mangled by an editor.
