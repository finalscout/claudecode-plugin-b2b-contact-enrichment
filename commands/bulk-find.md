---
description: Find emails for a list of contacts with FinalScout (bulk task with progress tracking)
argument-hint: <file.csv | pasted list of contacts>
---

Run a FinalScout bulk email-finding task for: $ARGUMENTS

Follow the finalscout b2b-contact-enrichment skill. In short:

1. Verify `FINALSCOUT_API_KEY` is set; if not, tell the user to export it (key at https://finalscout.com/app/api/settings) and stop.
2. Parse the input (CSV file, pasted list, table, or JSON) into a `persons` array. Pick the method from what each row contains: LinkedIn URLs → `linkedin`, name + domain → `professional`, article URLs → `author`. Strip empty rows.
3. Optionally check `/v1/account` first when the list is large, to confirm enough credits.
4. Submit to `https://api.finalscout.com/v1/find/<method>/bulk` (header `Authorization: $FINALSCOUT_API_KEY`) with a task `name` like "Bulk find - <date>". Save the task `id`.
   - Leave `effort` at its default (`max`) unless the user asks for a faster result — `low`/`high` return sooner but may find fewer emails, and all values cost the same.
   - For `professional` rows, pass `enable_related_domains: false` per row if the user wants the search confined to the domain they gave.
5. Poll `/v1/find/bulk/status?id=<id>` every 5 seconds, reporting progress as `finished / total`, until `Completed` or `Failed`.
6. Page through `/v1/find/bulk/dump?id=<id>&page_size=100` following `cursor` until empty.
7. Present a results table (Name | Input | Email | Type | Status | Score) and the summary line: `X / Y emails found. Z credits consumed, W credits remaining.`
8. If the user asked for a CSV, call `/v1/find/bulk/export?id=<id>`, retry until `Ready`, and share the download link with the warning that it is public and expires after 7 days.
