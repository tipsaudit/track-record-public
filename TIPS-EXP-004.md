# TIPS-EXP-004 — Public Dry-Run: The Italian Draw, Executed at Betfair

Version 1.0 · Registered 2026-09-24. This file is frozen; its SHA-256 is
published and anchored externally (GitHub + OpenTimestamps). It runs in
parallel with TIPS-EXP-003 and does not change it.

## Why this experiment exists

Two measurements point in opposite directions.

- Our value signals (best soft price above the devigged Pinnacle fair) carry
  real value at soft bookmakers, but that value does not survive at the
  exchange: at Betfair the same selections average about −2.8% versus the
  fair price, and the controlled counterfactual is +0.6% (TIPS-EXP-002
  autopsy; TIPS-EXP-003 is still running).
- The Italian draw (TIPS-EXP-001) is different in kind. It is not a price
  discrepancy between bookmakers but a structural underpricing of the draw by
  the market itself in Serie A and Serie B. The underpricing persists to the
  close and is visible at exchange prices too. On the 2024-25 and 2025-26
  seasons (football-data.co.uk Betfair Exchange odds, draws with Pinnacle
  odds ≤ 3.50), backing the draw at the exchange with 5% commission returned
  +17.7% (Serie A, n=266) and +14.6% (Serie B, n=393).

Those two seasons belong to the period in which the anomaly was discovered, so
they are not independent confirmation. This test asks the only question that
matters for automation: does the draw bias survive real exchange execution,
forward, under rules fixed in advance?

## Hypothesis

Backing the TIPS-EXP-001 draw selections at the best available Betfair back
price, never below the market's own devigged fair price, yields a positive
flat-stake return after commission.

## Rules — fixed in advance

1. SELECTIONS: exactly the TIPS-EXP-001 picks (Serie A and Serie B, the draw,
   Pinnacle first-snapshot draw odds ≤ 3.50), restricted to matches that also
   have a row in our public forward test. The live benchmark used in rule 2 is
   refreshed only for those rows. Picks without such a row are not proposed.
2. FRESH FAIR ONLY: the fair draw probability is recomputed at execution from
   the live Pinnacle benchmark (power devig). If the latest benchmark reading
   is older than 2 hours, the pick is SKIPPED (`fair_stale`) and rechecked.
3. EXECUTION WINDOW: bets are recorded only within T-3h to T-10min before
   kick-off. Earlier candidates are skipped (`too_early`) and rechecked.
4. ENTRY FILTER: the Betfair best available back price must be at least the
   devigged fair draw odds (no additional edge threshold: the hypothesis is
   that the market itself underprices the draw). Available liquidity at that
   price must be ≥ 500 units and ≥ 3x the stake. A price implying more than
   12% edge over fair is treated as a data defect and skipped.
5. STAKING: flat 1% of a simulated 1,000-lei bankroll per bet (10 lei), with
   its own 20% daily exposure cap, separate from TIPS-EXP-003.
6. MODE: DRY-RUN. Nothing is staked. Bets are recorded at the observed price
   and settled at the real result, minus 2% commission on winnings.
7. METRICS: flat-stake ROI with its 95% interval (primary), P&L, and CLV
   versus the devigged Pinnacle close (reported, but NOT a success criterion:
   the anomaly persists to the close, so CLV is expected to be near zero).
8. EVALUATION (decided in advance): one evaluation at the end of the 2026-27
   season (2027-06-30), on every recorded bet. Real-money testing is justified
   ONLY if the flat-stake ROI is positive with the 95% interval excluding zero.
   Interim figures are published live but trigger no decision in either
   direction. If the interval includes zero, the strategy stays "unproven at
   the exchange" and receives a public write-up.
9. LEDGER: every decision, including skipped picks and their reasons, is
   recorded under the label EXP-004 and cannot be edited or removed after
   kick-off.

## Honest caveats

- The historical exchange figures are in-sample for the discovery period. The
  prior for this test is "positive but smaller than the backtest", not +15%.
- At draw odds near 3.3, a season of roughly 300 bets gives a 95% interval of
  about ±18 points. A modest true edge may therefore end the season as
  "unproven". That is the price of not stopping early when numbers look good.
- Displayed Betfair prices, even at 500+ liquidity, may not be fillable at
  size for a real account. Serie B exchange liquidity three hours before
  kick-off may be thin, and the skip reasons will show how often.

TipsAudit — measured, not opined. https://tipsaudit.com
