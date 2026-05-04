# HTML Email Template

This is the HTML structure for the weekly brief email draft. Send via `gmail_create_draft` with `contentType: "text/html"`.

## Design rules

- **Palette**
  - Header navy: `#1a1a2e`
  - Body text: `#333333`
  - P0 (red): `#dc3545`
  - P1 (orange): `#fd7e14`
  - P2 (teal): `#17a2b8`
  - Stale-deal row pink: `#fce4e4`
  - Subtle dividers: `#e5e5e5`
- Use **inline CSS only** — most email clients strip `<style>` blocks.
- Priority tags are colored *text* (`<span style="color:#dc3545;font-weight:bold;">[P0]</span>`), not background badges.
- Tables get `border-collapse:collapse;width:100%;font-size:14px;` and explicit `<th>` styling.
- Keep total width to ~640px max.

## Subject line

```
Chief of Staff | Week of {{Mon DD, YYYY}}
```

## Skeleton

```html
<div style="font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;color:#333;max-width:640px;margin:0 auto;padding:20px;">

  <h1 style="color:#1a1a2e;border-bottom:3px solid #1a1a2e;padding-bottom:8px;margin-bottom:20px;">
    Chief of Staff Brief — Week of {{Mon DD, YYYY}}
  </h1>

  <!-- TL;DR -->
  <h2 style="color:#1a1a2e;margin-top:24px;">TL;DR</h2>
  <ul style="line-height:1.6;">
    <li><span style="color:#dc3545;font-weight:bold;">[P0]</span> {{one-line summary of top P0 item}}</li>
    <li><span style="color:#fd7e14;font-weight:bold;">[P1]</span> {{summary}}</li>
    <li><span style="color:#17a2b8;font-weight:bold;">[P2]</span> {{summary}}</li>
  </ul>

  <!-- Decisions Needed -->
  <h2 style="color:#1a1a2e;margin-top:24px;">Decisions Needed</h2>
  <ol style="line-height:1.6;">
    <li><strong>{{Decision title}}</strong> — {{context, options, recommendation}}</li>
    <li><strong>{{Decision title}}</strong> — {{...}}</li>
  </ol>

  <!-- Sprint -->
  <h2 style="color:#1a1a2e;margin-top:24px;">Sprint</h2>
  <p><strong>In Progress:</strong> {{key-1, key-2, ...}}</p>
  <p><strong>To Do:</strong> {{key-3, key-4}}</p>
  <p><strong style="color:#dc3545;">Risk:</strong> {{capacity warning or blocker}}</p>

  <!-- Deal Pipeline -->
  <h2 style="color:#1a1a2e;margin-top:24px;">Deal Pipeline</h2>
  <table style="border-collapse:collapse;width:100%;font-size:14px;">
    <thead>
      <tr style="background:#1a1a2e;color:#fff;">
        <th style="padding:8px;text-align:left;">Deal</th>
        <th style="padding:8px;text-align:left;">Stage</th>
        <th style="padding:8px;text-align:right;">Amount</th>
        <th style="padding:8px;text-align:right;">Stale</th>
      </tr>
    </thead>
    <tbody>
      <!-- normal row -->
      <tr style="border-bottom:1px solid #e5e5e5;">
        <td style="padding:8px;">{{Deal name}}</td>
        <td style="padding:8px;">{{Stage}}</td>
        <td style="padding:8px;text-align:right;">${{amount}}</td>
        <td style="padding:8px;text-align:right;">{{N}}d</td>
      </tr>
      <!-- stale row -->
      <tr style="background:#fce4e4;border-bottom:1px solid #e5e5e5;">
        <td style="padding:8px;">{{Stale deal name}}</td>
        <td style="padding:8px;">{{Stage}}</td>
        <td style="padding:8px;text-align:right;">${{amount}}</td>
        <td style="padding:8px;text-align:right;font-weight:bold;">{{N}}d</td>
      </tr>
    </tbody>
  </table>
  <p style="font-size:13px;color:#666;margin-top:6px;">Weighted pipeline: <strong>${{weighted_total}}</strong></p>

  <!-- Key Calendar -->
  <h2 style="color:#1a1a2e;margin-top:24px;">Key Calendar</h2>
  <table style="border-collapse:collapse;width:100%;font-size:14px;">
    <thead>
      <tr style="background:#1a1a2e;color:#fff;">
        <th style="padding:8px;text-align:left;">When</th>
        <th style="padding:8px;text-align:left;">Meeting</th>
        <th style="padding:8px;text-align:left;">Prep</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom:1px solid #e5e5e5;">
        <td style="padding:8px;">{{Mon 10:00}}</td>
        <td style="padding:8px;">{{Meeting title}}</td>
        <td style="padding:8px;">{{What to prep}}</td>
      </tr>
    </tbody>
  </table>

  <!-- Email Flags -->
  <h2 style="color:#1a1a2e;margin-top:24px;">Email Flags</h2>
  <ul style="line-height:1.6;">
    <li><span style="color:#dc3545;font-weight:bold;">[P0]</span> <strong>{{Sender}}</strong> — {{subject}} → {{action}}</li>
    <li><span style="color:#fd7e14;font-weight:bold;">[P1]</span> <strong>{{Sender}}</strong> — {{subject}} → {{action}}</li>
  </ul>

  <!-- Carried Forward -->
  <h2 style="color:#1a1a2e;margin-top:24px;">Carried Forward</h2>
  <ul style="line-height:1.6;">
    <li>{{Item still pending from previous weeks}}</li>
  </ul>

  <!-- Footer -->
  <hr style="margin-top:32px;border:none;border-top:1px solid #e5e5e5;">
  <p style="font-size:12px;color:#888;">
    Full brief: <code>Chief-of-Staff-Brief-Week-{{Mon-DD-YYYY}}.docx</code><br>
    Sources: Slack · Gmail · Calendar · Fathom · HubSpot · Jira
  </p>

</div>
```

## Tips

- If a section is empty for the week, omit the heading entirely — don't print "None" placeholders.
- The TL;DR should be readable in under 10 seconds. If it's longer than 5 bullets, you're over-reporting.
- Stale deals (`>5 days` no activity) get the pink row background. Tune the threshold in the prompt if needed.
