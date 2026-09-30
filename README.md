# TipsAudit — Public Track Record

Daily, append-only snapshots of the TipsAudit prediction track record, published once a day at
04:40 UTC. A row is timestamped here before kick-off only if a snapshot was taken between its
publication in the export and the match; predictions logged on match day, or whose Pinnacle price
was added after logging, first appear after kick-off.

Each `data/track-<UTC-timestamp>.json` is the exact public export from https://tipsaudit.com at
that moment — snapshots are never overwritten, so every version stays separately auditable.
`manifest.csv` lists the SHA-256 of every snapshot, and each file is anchored in the Bitcoin
blockchain via [OpenTimestamps](https://opentimestamps.org) (`.ots`).

External independent verification has been active since 20 June 2026. Earlier predictions are
preserved in the internal ledger but were not externally timestamped before kick-off.

## Verify a snapshot

```bash
sha256sum data/track-2026-06-20T06-16-15Z.json      # must match manifest.csv
ots verify data/track-2026-06-20T06-16-15Z.json.ots # confirms it existed at that time
```

Because the Git history and the OpenTimestamps proofs are external and append-only, any later
change to a published record is visible in the history. Changes we made on purpose are listed in
[`CORRECTIONS.md`](CORRECTIONS.md).

## What is in `data/`

- `track-<UTC>.json` — the full public prediction ledger at that moment (one object per prediction:
  fixture, kick-off, the pick, its probability and the Pinnacle price recorded for the prediction
  (at logging or, if Pinnacle had no price then, the first quoted after logging and before kick-off, which can be written in after the match),
  the closing price, closing-line value, result, units, and the value flag if there is one). `value_origin` says whether a flag was
  recorded live when the site showed it (from 26 September 2026) or reconstructed afterwards
  (before that date; see `CORRECTIONS.md`). The row `hash` covers only the prediction fields.
  `logged_at_utc` before 2026-07-28 was stored in server time (UTC+3): subtract 3 hours for those
  rows; `logged_before_kickoff` already accounts for it.
  `latest.json` is the most recent one.
- `margins-<date>.json` — the bookmaker-margin index measured that day (~35 bookmakers, per league).
- `TIPS-EXP-*.md` (+ `.ots`) — frozen specifications of pre-registered forward experiments.

## Monthly checkpoints

Once a month the current HEAD is tagged `vYYYY.MM` (see `CHANGELOG.md`). Tags are citable
pointers into the append-only history; they do not change any snapshot.

## How to cite

The data is published under CC-BY-4.0. A `CITATION.cff` is included, so GitHub's
"Cite this repository" button gives you a ready-made reference. Methodology, calibration and
the live experiments: https://tipsaudit.com/methodology · https://tipsaudit.com/calibration ·
https://tipsaudit.com/strategies
