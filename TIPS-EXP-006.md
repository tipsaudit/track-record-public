# TIPS-EXP-006 — Value Flags at Soft-Bookmaker Prices (Paper Test)

Version 1.0 · Registered 2026-10-05. This file is frozen; its SHA-256 is
published and anchored externally (GitHub + OpenTimestamps). It runs in
parallel with TIPS-EXP-005, which is not changed in any way.

## Why this experiment

TIPS-EXP-005 executes our value signals at Betfair Exchange prices. In its first
4.5 days it recorded 3 bets: most signals had no fresh benchmark, and when a
Betfair price was available its edge after commission averaged −2.3% (27 signals).
Our research already found that the edge we measure lives at soft bookmakers,
not at the exchange: in the reconstructed flag history, flags priced at soft
bookmakers had CLV +2.29% (±0.30, n=2,649) against the devigged Pinnacle close,
exchange-priced flags −0.32% (±0.65, n=550). Relaxing the Betfair filters would
add bets, but it would measure faster something we already expect to lose.

This experiment asks the question where the edge is supposed to be: can the
public value flags be taken at the soft bookmaker that showed the price, at a
price that beats the devigged close? Soft bookmakers cannot be bet automatically,
so this is a measurement on paper. Nothing is staked.

## Hypothesis

Value flags (rule flag-v2) taken at the first price the same soft bookmaker
quotes after the flag beat the devigged Pinnacle close on average: mean CLV > 0.

## Rules — fixed in advance

1. POPULATION: every value flag with rule `flag-v2` recorded live
   (`origin = live`) with detection time at or after T0 = 2026-10-05 07:20 UTC.
   Matches in World Cup and UEFA Nations League competitions are excluded, as
   in all audited figures. Each flag enters once.
2. ENTRY PRICE: the first price of the same bookmaker on the same selection
   (1X2 market) that we observe after the flag's detection time, within 60
   minutes and before kick-off, and that the bookmaker last updated at most 90
   minutes before we observed it. Observation time is when our collector read
   the price; an unchanged price re-read later counts.
3. NOT TAKEABLE: flags without such a price are not takeable. They are counted
   in coverage (takeable / settled flags) and excluded from returns.
4. RE-READ: to raise coverage, about 20–50 minutes after each flag we read that
   match's prices once from the same odds feed (the-odds-api, EU region, 1X2).
   This adds observations; it does not change rule 2.
5. STAKE AND RESULT: 1 unit per flag, flat. Settled on the official 1X2 result;
   void or abandoned matches are excluded.
6. PRIMARY MEASURE: mean CLV of the entry price against the devigged Pinnacle
   closing price (power devig): entry odds / fair closing odds − 1, with a 95%
   confidence interval (normal approximation), shown from 30 takeable flags.
7. DECISION: at 300 takeable settled flags or on 2026-12-31, whichever comes
   first. "Works" if the lower bound of the 95% interval is above zero;
   "fails" if the upper bound is below zero; otherwise "inconclusive". There is
   no early stop. Interim figures are published continuously, labelled interim.
8. SECONDARY MEASURES (reported, not decisive): flat-stake return (published
   from 100 takeable settled flags), CLV at the flag's own price, coverage,
   CLV by flag strength (edge of at least 3% vs below 3%) and by bookmaker.
9. LEDGER: every flag in the population is listed publicly with its flag
   price, re-observed price (or "not re-observed"), fair close, CLV and result.

## Sample size

From the backtest, the standard deviation of per-flag realizable CLV is about
8.4 percentage points. With 300 takeable flags the 95% interval is about
±1 percentage point wide on each side, enough to detect a true mean of about
+1.2%. At the recent rate of 25–45 flags a day, the decision point is expected
within roughly four weeks.

## Honest caveats

- A positive result means the flags were takeable at a price better than the
  sharp close, on paper. It does not mean a bettor would be allowed to stake
  on them: soft bookmakers limit accounts that beat the close.
- The rules were chosen after looking at the first days of live flags (CLV at
  signal +2.51%, realizable +1.20% on 14 flags). Only flags from T0 onward count.
- The re-read uses the same feed as the flags; an independent price source is
  not available for most of these bookmakers.
