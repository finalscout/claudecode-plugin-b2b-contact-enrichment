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
3. Use the waterfall single endpoint (`https://api-waterfall.finalscout.com/find/<method>/single?timeout=120`, header `Authorization: $FINALSCOUT_API_KEY`, no Bearer prefix, curl `-m 130`). On HTTP 408, resubmit or poll `/v1/find/single/status?id=<id>`.
4. Present the result: email, type, verification status, score, plus any enrichment fields. Flag `Risky`/`Unknown` emails with a caveat. If no contact/email is returned, say no email was found (no credit charged).
5. End with: credits consumed and credits remaining.

If the input is ambiguous or missing, ask the user what they have (LinkedIn URL, name + domain, or article URL).
