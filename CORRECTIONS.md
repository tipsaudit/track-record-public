# Corrections

Changes we made on purpose to published data, newest first. Every earlier snapshot stays in the
Git history, so each change below can be checked by comparing the snapshots it names.

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
