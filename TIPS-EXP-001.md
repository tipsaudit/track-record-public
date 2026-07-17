# TIPS-EXP-001 — Pre-Registered Forward Test: The Italian Draw

Version 1.0 · Registered 2026-07-17, before the start of the 2026-27 Italian season.
This file is frozen. Its SHA-256 hash is published and anchored externally
(GitHub + OpenTimestamps). Any change to this file changes the hash.

## Hypothesis

The 1X2 draw in Italian professional football (Serie A and Serie B) is
systematically underpriced by the betting market when the draw is a plausible
outcome (short draw odds).

## Rules — fixed in advance, no discretion

1. UNIVERSE: every regular-season match of Italy Serie A and Serie B in the
   2026-27 season that appears in our odds feed with Pinnacle 1X2 prices.
2. SELECTION: flag the DRAW (X) if and only if the Pinnacle opening draw odds
   in our first snapshot are less than or equal to 3.50. No other filter.
3. STAKE: flat 1 unit per selection. No skipping, no staking plans, no
   discretion of any kind.
4. PRICES: entry price = Pinnacle draw odds at our first snapshot; benchmark =
   Pinnacle closing odds (same bookmaker — clean CLV, no line shopping).
5. PRIMARY METRIC: average closing line value (CLV) versus the Pinnacle close.
   SECONDARY: flat-stake ROI at recorded entry odds.
6. DURATION: the full 2026-27 season. No early stopping on results. Technical
   outages, if any, are documented publicly and do not retroactively remove
   logged selections.
7. SUCCESS (declared in advance): season-end average CLV positive with a 95%
   bootstrap confidence interval excluding zero. Positive ROI is supportive
   evidence but is expected to be variance-dominated at the likely sample size
   (~250–450 selections).
8. FAILURE: the CI includes zero, or the point estimate is negative. In that
   case the strategy is declared dead and receives a public autopsy on our
   blog, alongside the strategies it outlived.
9. TRANSPARENCY: every selection is logged before kick-off in the public track
   record (daily snapshots on GitHub, OpenTimestamps-anchored) and settled
   automatically. Nothing can be edited or removed after kick-off.

## Historical basis (backtest — the reason this test exists)

Found in a 6-season backtest (2020-21 .. 2025-26, opening odds):
- Serie B, blind X at open: EV +7.2%, CI95 [+0.5%, +14.3%], positive in 6/6
  seasons; persists at the close (+6.5%).
- Serie A: +4.9% at open, positive in 5/6 seasons.
- Monotone structure by odds band: draw odds <= 3.5 show +9..+16% EV in both
  leagues; > 3.5 negative. Same gradient in both leagues.
- Combined pre-registered band (Italy, X <= 3.5): historical EV +13.6%
  [+7.2%, +19.9%], worst single season +6.4%.

## Honest caveats (stated before the test, not after)

- Italy emerged from a scan of 17 leagues: multiple-testing risk is real,
  mitigated but not eliminated by 6/6 season consistency, the presence of the
  effect in BOTH Italian leagues, the monotone odds-band gradient, and a
  plausible mechanism (the draw attracts no recreational volume, so its price
  receives the least correction).
- The <= 3.5 band was chosen after seeing the data (post-hoc).
- The effect may have decayed or may vanish under scrutiny. That is exactly
  why this is a forward test with pre-committed rules — not a betting
  recommendation. We publish the outcome either way.

TipsAudit — measured, not opined. https://tipsaudit.com
