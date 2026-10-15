# TipsAudit — football predictions dataset with bookmaker odds, closing lines and margins

Free, open football (soccer) data measured from live bookmaker prices, under CC-BY-4.0:

- **[`datasets/tipsaudit-predictions-all.csv`](datasets/tipsaudit-predictions-all.csv)** — the settled
  1X2 predictions of the TipsAudit public track record, one row per match, each logged before
  kick-off (see `logged_before_kickoff` in `data/latest.json`): margin-free (devigged) fair
  probabilities, the Pinnacle price recorded for the prediction and the **closing line**, the best
  price at other bookmakers, the prediction's closing line value (CLV), the 1X2 result and the measured
  bookmaker margin on the match. The CLV column compares raw prices (it is not devigged), the
  best-soft and margin columns include betting exchanges, and the result is 1/X/2, not the score.
  Columns and caveats: [`datasets/README.md`](datasets/README.md). Regenerated monthly.
- **`data/margins-<date>.json`** — the daily bookmaker-margin index: the 1X2 overround of the
  bookmakers we measure (30-day window), overall and per league.
- **`data/track-<UTC>.json`** — daily snapshots of the public track record, described below.

Live versions: https://tipsaudit.com/data (CSV) · https://tipsaudit.com/api (JSON API, no key) ·
https://tipsaudit.com/bookmaker-margins (margin index).

## Public track record

Daily snapshots of the TipsAudit prediction track record, published once a day at 07:40
Bucharest time (04:40 UTC in summer time, 05:40 UTC in winter; each file name carries its UTC
time). A row is timestamped here before kick-off only if a snapshot was taken between its publication in
the export and the match; predictions logged on match day, or whose Pinnacle price was added after
logging, first appear after kick-off.

Each `data/track-<UTC-timestamp>.json` is the exact public export from https://tipsaudit.com at
that moment. It is written once and never overwritten, so every version stays separately
auditable. `manifest.csv` lists the SHA-256 of every snapshot, and each snapshot is submitted to
[OpenTimestamps](https://opentimestamps.org) when it is published (`.ots`). A new proof is
pending: it holds the calendar servers' receipts, not yet a Bitcoin attestation. A weekly commit
("upgrade OpenTimestamps proofs") adds the Bitcoin attestation to the proofs whose block has
confirmed, so the newest proofs in `data/` are usually still pending. The proofs next to
`TIPS-EXP-*.md` and `CORRECTIONS.md` are kept as submitted; `ots upgrade` completes them.

External independent verification has been active since 20 June 2026. Earlier predictions are
preserved in the internal ledger but were not externally timestamped before kick-off.

## Verify a snapshot

```bash
sha256sum data/track-2026-06-20T06-16-15Z.json        # must match its line in manifest.csv
ots upgrade data/track-2026-06-20T06-16-15Z.json.ots  # adds the Bitcoin attestation if this copy is still pending
ots verify data/track-2026-06-20T06-16-15Z.json.ots   # checks the file against the proof and the Bitcoin block header
```

`ots upgrade` asks the calendar servers for the attestation and rewrites the `.ots` in place
(keeping a `.bak`); right after a snapshot is published the attestation may not exist yet, so try
again a day later. `ots verify` needs a local Bitcoin Core node; without one,
`ots --no-bitcoin verify <file>.ots` prints the block height and Merkle root to check in any block
explorer. A Bitcoin attestation shows that the file existed no later than that block's time.

Timestamped snapshots are never overwritten, and the Git history keeps every earlier version of
every file, so any difference shows: if a row changes later (for example after a rescheduled
kick-off), the change is visible by comparing two snapshots. [`CORRECTIONS.md`](CORRECTIONS.md)
describes the corrections we announced.

## What is in `data/`

- `track-<UTC>.json` — the full public prediction ledger at that moment (one object per prediction:
  fixture, kick-off, the pick, its probability and the Pinnacle price recorded for the prediction
  (at logging or, if Pinnacle had no price then, the first quoted after logging and before kick-off, which can be written in after the match),
  the closing price, the prediction's closing-line value (raw Pinnacle price against the raw
  Pinnacle close, not devigged), result, units, and the value flag if there is one; a flag's CLV is
  measured against the devigged Pinnacle close). `value_origin` says whether a flag was
  recorded live when the site showed it (from 26 September 2026) or reconstructed afterwards
  (before that date; see `CORRECTIONS.md`). The row `hash` covers only the prediction fields.
  `logged_at_utc` before 2026-07-28 was stored in server time (UTC+3): subtract 3 hours for those
  rows; `logged_before_kickoff` already accounts for it.
  `latest.json` is the most recent one.
- `margins-<date>.json` — the bookmaker-margin index published that day (30-day window, per
  league). The number of bookmakers varies from day to day and includes betting exchanges; from
  5 September 2026 each entry has a `type` (`soft`, `exchange` or `etalon`), but until
  9 October 2026 the exchanges `Betfair` and `Matchbook` were typed `soft`.
- `TIPS-EXP-*.md` (+ `.ots`, in the repository root) — frozen specifications of pre-registered
  forward experiments.

## Monthly checkpoints

Once a month the current HEAD is tagged `vYYYY.MM` (see `CHANGELOG.md`). Tags are citable
pointers into the Git history; they do not change any snapshot. From `v2026.10` on, the tag is
set by the daily publication on the 1st, before that month's CSV rebuild (06:30 UTC), so a tag
holds the previous month's CSV (`v2026.10` holds the CSV added on 15 September 2026; `v2026.09`,
set on 4 September 2026, has no CSV).

## How to cite

The data is published under CC-BY-4.0. A `CITATION.cff` is included, so GitHub's
"Cite this repository" button gives you a ready-made reference. Methodology, calibration and
the live experiments: https://tipsaudit.com/methodology · https://tipsaudit.com/calibration ·
https://tipsaudit.com/strategies
