---
description: Check FinalScout account credits and rate limits
---

Check the FinalScout account status.

1. Verify `FINALSCOUT_API_KEY` is set; if not, tell the user to export it (key at https://finalscout.com/app/api/settings) and stop.
2. Run:

```bash
curl -s https://api.finalscout.com/v1/account -H "Authorization: $FINALSCOUT_API_KEY"
```

3. Report `owner_email`, `credits_available`, and the per-endpoint rate limits in a short table. Remind the user that each email successfully found costs 1 credit and that no credit is charged when no email is found.
