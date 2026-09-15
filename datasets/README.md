# TipsAudit prediction dataset

`tipsaudit-predictions-all.csv` — every football (soccer) 1X2 prediction TipsAudit has
logged before kick-off, with the devigged fair probabilities, the sharp-market (Pinnacle)
opening and closing prices, the best soft-bookmaker price per selection, the closing-line
value, the settled result, and the measured market margin (min/avg across bookmakers) on
each match. Regenerated monthly.

Columns: id, kickoff_utc, league, country, home, away, result, void_reason,
fair_prob_home/draw/away, pin_open_*, pin_close_*, best_soft_*, best_soft_book_*,
prediction_pick, prediction_odds_taken, prediction_closing_odds, prediction_clv_pct,
prediction_won, market_margin_min_pct, market_margin_avg_pct, books_priced, record_hash.

License: CC-BY-4.0 (attribution: https://tipsaudit.com). Methodology:
https://tipsaudit.com/methodology · Calibration: https://tipsaudit.com/calibration

Each row is also part of the daily, OpenTimestamps-anchored snapshots in this repository;
`record_hash` is the integrity fingerprint of the logged values.
