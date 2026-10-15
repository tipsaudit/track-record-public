# TipsAudit prediction dataset

`tipsaudit-predictions-all.csv` — the settled football (soccer) 1X2 predictions of the TipsAudit
public track record, one row per match, each logged before kick-off (see `logged_before_kickoff`
in `data/latest.json`), with the devigged fair
probabilities, the sharp-market (Pinnacle) price recorded for the prediction and the closing price, the best price per selection at other bookmakers (see the caveats), the prediction's closing-line
value, the 1X2 result, and the measured market margin (min/avg across bookmakers) on
each match. Regenerated monthly.

Columns: id, kickoff_utc, league, country, home, away, result, void_reason,
fair_prob_home/draw/away, pin_open_*, pin_close_*, best_soft_*, best_soft_book_*,
prediction_pick, prediction_odds_taken, prediction_closing_odds, prediction_clv_pct,
prediction_won, market_margin_min_pct, market_margin_avg_pct, books_priced, record_hash.
(`*` = `_home`, `_draw`, `_away`.)

License: CC-BY-4.0 (attribution: https://tipsaudit.com). Methodology:
https://tipsaudit.com/methodology · Calibration: https://tipsaudit.com/calibration

## Caveats

- `result` is the 1X2 outcome: `1` (home win), `X` (draw) or `2` (away win). It is not the score.
  It is empty on void rows (`void_reason`, e.g. `postponed`), where `prediction_won` is 0, so
  filter on `void_reason` first.
- `prediction_pick` is one outcome (`1`, `X`, `2`) or a double chance (`1X`, `12`, `X2`); for a
  double chance the odds are derived from the two Pinnacle prices as 1 / (1/a + 1/b).
- `prediction_clv_pct` = (`prediction_odds_taken` / `prediction_closing_odds` − 1) × 100: the raw
  Pinnacle price recorded for the prediction against the raw Pinnacle close, margin included in
  both. It is not devigged, so it is not comparable with a CLV measured against the devigged close.
- `pin_open_*` is the Pinnacle price recorded for the prediction: at logging or, if Pinnacle had no
  price then, the first price quoted after logging and before kick-off, which could be written in
  after the match. It is not necessarily Pinnacle's opening price. `fair_prob_*` removes the margin
  from these prices (proportionally for rows logged before 18 July 2026, by the power method after).
- `best_soft_*` / `best_soft_book_*` include two betting exchanges: Betfair Exchange (written
  `Betfair`) and Matchbook. The same bookmaker can appear under two spellings from two data feeds
  (e.g. `Marathonbet` and `Marathon Bet`). For rows logged before 21 September 2026 it is the highest price seen at
  any time before logging, not necessarily one available at that moment.
- `market_margin_min_pct`, `market_margin_avg_pct` and `books_priced` cover every bookmaker priced
  on the match, including Pinnacle and betting exchanges. The minimum usually comes from an exchange
  or Pinnacle and can be slightly negative; it is not the margin of the cheapest soft bookmaker.

## Verification

Each row also appears in the daily snapshots in this repository (`data/track-*.json`, SHA-256 in
`manifest.csv`, each submitted to OpenTimestamps; see the main README for how to verify).
`record_hash` fingerprints the values recorded for the prediction (id, match id, league, teams, kick-off,
model probabilities, the Pinnacle prices in `pin_open_*`, log time); it does not cover closing prices,
`best_soft_*` prices or results.
`kickoff_utc` and `record_hash` are as of the day the file was built: if a hashed field changes
later (for example a rescheduled kick-off), later snapshots carry a different hash for that row,
so compare it with the snapshot taken on the build date.
