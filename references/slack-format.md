# Slack Self-Message Template

This is the format for the quick-glance Slack version. It lands in your self-DM (channel ID `{{SLACK_DM_CHANNEL}}`).

## Delivery rules

- **Use `slack_send_message`, NOT `slack_send_message_draft`.** The draft tool strips markdown formatting and the message renders as a wall of text.
- Bold uses standard markdown: `**double asterisks**`. Italics: `_underscores_`.
- Slack does not render colors, so use emoji prefixes for priority: 🔴 P0 · 🟠 P1 · 🔵 P2.
- Keep the entire message under ~30 lines. Anything longer belongs in the email or the .docx.

## Template

```
**📋 Chief of Staff Brief — Week of {{Mon DD}}**

🔴 **P0 — Act today/tomorrow**
1. {{One-line action item}} — {{owner or deadline}}
2. {{...}}

🟠 **P1 — This week**
1. {{Action item}} — {{deadline}}
2. {{...}}

🔵 **P2 — Monitor**
• {{Status update}}
• {{...}}

**↪️ Carried forward**
• {{Still-pending item from last week}}

---
Full brief: `Chief-of-Staff-Brief-Week-{{Mon-DD-YYYY}}.docx`
Email draft: in your inbox
```

## Tone

- Strip every word that isn't load-bearing. "Need to follow up with X about Y by Friday" → "Follow up X on Y · Fri".
- Prefer verbs over nouns. "Decision: pricing" is weaker than "Decide pricing".
- If P0 is empty, write `🔴 **P0** — none this week.` rather than skipping the section. (P1 and P2 can be skipped if empty.)

## Why a self-DM?

- Pinnable — keeps the brief one click away all week.
- Searchable — Slack search remembers it; email folders forget.
- Mobile-friendly — easier to glance at than opening a Word doc.
