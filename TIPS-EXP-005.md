# TIPS-EXP-005 — Public Dry-Run v3: Value Signals at Betfair, Exchange Prices Excluded From the Trigger

Version 1.0 · Registered 2026-09-30. This file is frozen; its SHA-256 is
published and anchored externally (GitHub + OpenTimestamps). It succeeds
TIPS-EXP-003, which is closed on the same day with 4 settled bets.

## Why a new version

On 2026-09-30 we measured, on the full history, where the closing line value of
our value flags comes from. In the reconstructed flag history (backtest, 3,199
settled flags), flags whose "best soft price" was an exchange (Betfair Exchange
or Matchbook, both stored as soft bookmakers) had CLV −0.32% (±0.65, n=550) even
before commission, while flags priced at real soft bookmakers had +2.29% (±0.30,
n=2,649). Our public flag rule is therefore changed to exclude exchanges (rule
v2, A1) and to require prices seen within 90 minutes (rule v2, B90).

TIPS-EXP-003's rule 1 ("every public value flag") would silently change its
trigger mid-experiment. Rather than amend it, we close it as recorded (4 settled
bets, too few to read) and register this experiment with the new trigger.

## Hypothesis

Value signals whose price comes from a soft bookmaker (not an exchange),
executed at Betfair under the same execution rules as TIPS-EXP-003, retain a
small positive edge after commission. Expected size: low single digits at flat
stakes.

## Rules — fixed in advance

1. SIGNALS: the public value flag computed with exchanges excluded from the
   best soft price (fair prob >= 30%, 0.5% <= edge <= 12%, best soft above the
   devigged Pinnacle fair). Unlike the public flag, NO 90-minute age gate is
   applied to the soft price: the bet is executed at the Betfair price, and the
   fair price is re-checked at execution (rule 2).
2. FRESH FAIR ONLY: the fair probability is recomputed from the live sharp
   benchmark at execution time. If the latest benchmark reading is older than
   2 hours, the signal is SKIPPED (reason `fair_stale`) and rechecked later.
3. EXECUTION WINDOW: bets are placed only within T-3h to T-10min before
   kick-off. Earlier candidates are skipped (`too_early`) and rechecked.
4. ENTRY FILTER: Betfair best available back price must grant >= 2.5% edge
   after 2% commission versus the fresh fair probability, AND available
   liquidity at that price must be >= 500 units AND >= 3x the stake.
5. STAKING: half-Kelly per bet (0.5 x edge/(odds-1)), capped at 5% of bankroll
   per bet and 20% total exposure per day. Simulated bankroll: 1,000 lei.
6. MODE: DRY-RUN. Nothing is staked. Bets are recorded at the observed price
   and settled at the real result, minus 2% commission on winnings.
7. METRICS: P&L, ROI (weighted AND flat-stake, both reported), and CLV versus
   the DEVIGGED Pinnacle close (power devig).
8. PROMOTION GATE (decided in advance): real-money testing is justified ONLY
   if 150+ recorded bets show flat-stake ROI > 0 with the 95% interval
   excluding zero, AND average devigged CLV > 0. Otherwise the strategy is
   declared not viable at the exchange and receives a public autopsy.
9. LEDGER: every decision, including skipped signals and their reasons, is
   published live under the label EXP-005 and cannot be edited or removed
   after kick-off. TIPS-EXP-003's ledger stays published under EXP-003.

## Honest caveats

- Our research found no durable edge at the exchange itself; this is a test,
  not a claim. At the historical rate of qualifying signals, reaching 150 bets
  may take several months.
- The simulation behind the trigger change used the same historical data that
  motivated it; only the live results of this experiment count as evidence.
