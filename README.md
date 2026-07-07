# 📧 FinalScout — Claude Code Plugin

Find verified professional email addresses right from Claude Code, powered by the [FinalScout API](https://finalscout.com).

## Features

Three ways to find an email, each available as single lookup or bulk batch:

| You have | Method |
|----------|--------|
| A LinkedIn profile URL (incl. Sales Navigator) | LinkedIn find |
| A full name + company domain | Professional find |
| A news article URL (find its author) | Author find |

- **Single lookup** — one blocking call via FinalScout's waterfall endpoints (no polling), with submit-and-poll as fallback
- **Bulk lookup** — process any number of contacts from a CSV, list, table, or JSON, with progress reporting, paginated result collection, and optional CSV export
- **Verification status** — each result is marked `Valid`, `Risky`, `Invalid`, or `Unknown`, with a 0–100 deliverability score and catch-all detection
- **Enrichment** — results include title, company, location, LinkedIn URL, industry, and company details when available
- **Optional extras** — personal/generic email fallback (LinkedIn method), contact tagging, custom `meta_data` passthrough for CRM correlation, and webhook notifications
- **Account awareness** — credit balance and per-endpoint rate limits, with remaining credits shown after each operation

## What's included

| Component | Name | Purpose |
|-----------|------|---------|
| Skill | `b2b-contact-enrichment` | Triggers automatically when you ask Claude to find emails; contains the full FinalScout API playbook |
| Command | `/finalscout:find-email` | Single lookup: LinkedIn URL, name + domain, or article URL |
| Command | `/finalscout:bulk-find` | Bulk task from a CSV file or pasted list, with progress and optional CSV export |
| Command | `/finalscout:account` | Check credit balance and rate limits |

## Requirements

| Requirement | Details |
|-------------|---------|
| FinalScout account | Sign up at [finalscout.com](https://finalscout.com) |
| API key | Get yours at [finalscout.com/app/api/settings](https://finalscout.com/app/api/settings) |
| `curl` | Used for all API calls |

## Installation

### From this repository (marketplace)

In Claude Code:

```
/plugin marketplace add finalscout/claudecode-plugin-b2b-contact-enrichment
/plugin install finalscout@finalscout
```

### Local testing

```bash
claude --plugin-dir /path/to/this/repo
```

### Set your API key

```bash
export FINALSCOUT_API_KEY="your-api-key"
```

Add it to your shell profile (`~/.zshrc`, `~/.bashrc`) so it's available in every session.

## Usage

Use the slash commands:

```
/finalscout:find-email Bill Gates microsoft.com
/finalscout:find-email https://www.linkedin.com/in/satyanadella
/finalscout:bulk-find leads.csv
/finalscout:account
```

Or just ask in natural language — the skill triggers automatically:

> Find the email address of Bill Gates at microsoft.com

> Get the email for https://www.linkedin.com/in/satyanadella

> Find the author's email for this article: https://www.forbes.com/sites/...

> Find emails for all the LinkedIn URLs in leads.csv and export the results as a CSV

You can paste a CSV, a table, or JSON. Results come back as a table:

| Name | Input | Email | Type | Status | Score |
|------|-------|-------|------|--------|-------|
| Bill Gates | microsoft.com | bill@microsoft.com | Work email | Valid | 95 |

**Options** — mention them in your request and they'll be passed through:

- Include personal emails (Gmail, etc.) or generic emails (info@, support@) — LinkedIn method
- Deep-verify professional lookups when you're sure the domain is right
- Tag contacts in your FinalScout account
- Attach custom metadata (e.g. CRM ids) that comes back with each result
- Send results to a webhook instead of polling

## Pricing

Each email **successfully found** costs 1 FinalScout credit. No charge when no email is found.

## Troubleshooting

| Error | Cause | Fix |
|-------|-------|-----|
| 401 | Invalid API key | Check `FINALSCOUT_API_KEY` and the IP whitelist in API settings |
| 403 | Insufficient credits | Top up your FinalScout account |
| 405 | Account blocked | Contact dev@finalscout.com |
| 408 | Waterfall timeout | The task is still running — the plugin resubmits automatically |
| 429 | Rate limited | The plugin retries automatically |

## License

MIT
