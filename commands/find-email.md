---
description: Find a verified professional email address with FinalScout (single lookup)
argument-hint: <linkedin-url | Full Name company.com | article-url>
---

Find the email address for: $ARGUMENTS

Follow the finalscout b2b-contact-enrichment skill. In short:

1. Detect the find method from the input:
   - LinkedIn profile URL (incl. Sales Navigator) → `linkedin`
   - Full name + company domain → `professional`
   - News article URL → `author`
2. Verify `FINALSCOUT_API_KEY` is set (`test -n "$FINALSCOUT_API_KEY"`). If not, tell the user to export it — key available at https://finalscout.com/app/api/settings — and stop.
3. Use the waterfall single endpoint (`https://api-waterfall.finalscout.com/find/<method>/single?timeout=120`, header `Authorization: $FINALSCOUT_API_KEY`, no Bearer prefix, curl `-m 130`). `timeout` may go up to 900. On HTTP 408, resubmit or poll `/v1/find/single/status?id=<id>`.
   - Only add `&no_charge_on_timeout=true` if the user explicitly wants "no charge unless it finds one within the window" — it discards a match found after the deadline, so it must not be combined with the 408 resubmit/poll step above.
4. Present the result: email, type, verification status, score, plus any enrichment fields. Flag `Invalid` emails clearly (re-verified as undeliverable; returned for reference only — do not send). If no contact/email is returned, say no email was found (no credit charged).
5. End with: credits consumed and credits remaining.

If the input is ambiguous or missing, ask the user what they have (LinkedIn URL, name + domain, or article URL).
