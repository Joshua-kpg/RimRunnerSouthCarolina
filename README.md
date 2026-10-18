# RimRunnerSouthCarolina

Unofficial South Carolina HS boys basketball ratings. Not affiliated with the SCHSL, SCISA or MaxPreps.
Live site: https://rimrunnersouthcarolina.web.app/ (Firebase Hosting; backup copy: GitHub Pages).

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project (accounts, picks, follows).

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 07:30 and 15:00 UTC
(2:30 am / 10:00 am Eastern standard time) and on manual dispatch. Before the South Carolina boys season opens
(`SEASON_START` in index.html = first game on the MaxPreps schedules) the script makes no requests.
SCHSL schools' games before Fri Nov 20, 2026 (SCHSL first contest date) are ignored.
From then on it reads boys basketball finals for the last 3 days from the MaxPreps South Carolina scores-by-date page
(team-page fallback), matches them to the schedule by MaxPreps team id + date, records them in `data/results.json` and
bakes scores and W-L records into `index.html`. The page recomputes Elo from those finals. It commits only when data
changed, then deploys (index.html + logos/) to Cloudflare Pages if the `CLOUDFLARE_API_TOKEN` repository secret is set.
