# Corrections

Changes we made on purpose to published data, newest first. Every earlier snapshot stays in the
Git history, so each change below can be checked by comparing the snapshots it names.

## 2026-09-30 — late predictions voided; replay flags on unplayed matches superseded

- 14 predictions were written after kick-off by pipeline errors (13 on 2026-07-25, one on
  2026-07-15; 13 of them are in this export). They are now void (`void_reason =
  logged_after_kickoff`): their result is no longer scored and they are excluded from every figure.
  The rows stay in the export with `logged_before_kickoff = 0`.
- 8 value flags reconstructed on 2026-09-26 sat on matches that had not been played yet, which
  blocked recording the live flag for those matches. They were withdrawn before kick-off (kept in
  the database as `superseded_live`, not deleted), so those matches are flagged live like any other.

## 2026-09-30 — TIPS-EXP-003: proposal step aligned with rule 1

Rule 1 of `TIPS-EXP-003.md` covers every public value flag. The step that turns flags into
proposals only picked up matches that already had a Pinnacle price when the prediction was logged.
Flags on matches where Pinnacle opened later (shown on the site from the live Pinnacle price) were
never proposed and are missing from the EXP-003 ledger — for example matches 7160 and 7648 on
28 September 2026. From 30 September 2026 they are proposed like every other flag; rule 2 (fresh
live fair price, otherwise skipped) applies to all of them. The frozen specification is unchanged,
and the missed signals are not added retroactively.

## 2026-09-29 — fields added, claims made precise

- Four fields were added at the end of every row: `value_origin` (`live` | `replay`),
  `value_recorded_at_utc`, `value_sealed_before_kickoff` and `logged_before_kickoff`.
  Existing columns keep their positions.
- The note in each snapshot now states exactly what the row `hash` covers: id, match_id, league,
  home, away, kickoff_utc, our model probabilities, the Pinnacle 1X2 prices at logging and
  logged_at. It does not cover closing prices, results or any `value_*` field.
- 13 predictions logged on 2026-07-25 were written after kick-off because of a pipeline error.
  They were in every snapshot since; they are now marked `logged_before_kickoff = 0`.
- When Pinnacle had no price at logging, the first Pinnacle price quoted after logging and before
  kick-off is filled in later, sometimes after the match. Such a row first appears in the export
  (and here) only once that price is filled in. The row `hash` includes those prices.

## 2026-09-26 — value-flag fields recomputed; flags before this date are a reconstruction

**What changed.** Between snapshots `data/track-2026-09-26T04-40-01Z.json` and
`data/track-2026-09-27T04-40-01Z.json` the `value_*` fields of 3,519 settled rows changed:
1,468 rows gained a value flag, 449 lost one, and the rest changed price, bookmaker or result.
The prediction fields and the `hash` of every row did not change, because that hash has never
covered the `value_*` fields.

**Why.** Until then, flags on settled matches were scored at the highest soft price logged for
the match (before 21 September 2026, the highest price ever seen across all bookmakers, not
necessarily one available at a given moment), and closing line value was measured against
Pinnacle's raw closing odds, margin included. That overstated the result: the site showed
+7.9% average CLV.

**What the values are now.** Every flag before 26 September 2026 was reconstructed on that day by
applying the current display rule to the prices stored before each match (`value_origin =
replay`). This is a backtest, not a record of what the site showed at the time. Its seal
(`value_commitment`) was created at reconstruction, almost always after kick-off
(`value_sealed_before_kickoff = 0`), so it only shows that the reconstruction has not changed
since it was first published (snapshot 2026-09-27T04-40-01Z). Flags recorded live, when the site
shows them, start on 26 September 2026 (`value_origin = live`): the first flag on each match is
recorded, with its seal, before kick-off (`value_recorded_at_utc`). The seal appears in the public
export once the match's prediction row is published, and this log anchors it externally only when a
daily snapshot (04:40 UTC) falls between the recording and kick-off.
