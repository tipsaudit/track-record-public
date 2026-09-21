# TIPS-EXP-003 — Public Dry-Run v2: Value Signals at Betfair, With the Artefacts Removed

Version 1.0 · Registered 2026-09-21. This file is frozen; its SHA-256 is
published and anchored externally (GitHub + OpenTimestamps). It succeeds
TIPS-EXP-002, which concluded the same day with a public autopsy.

## Why a second version, and why not simply "tune" the first

TIPS-EXP-002 ran from 2026-07-18 to 2026-09-21 and recorded 262 settled bets.
Its autopsy (TIPS-EXP-002-AUTOPSY.md) found that three well-defined segments
lost money while the rest was positive: bets priced against a stale fair
(benchmark reading older than 2h; 77 bets, ROI −14.8%), bets placed 3–8 hours
before kick-off (59 bets, −21.0%), and bets at Betfair liquidity under 500
(115 bets, −7.0%). The remaining core (119 bets) was +15.3% weighted, but only
+2.4% at flat stakes: most of the weighted profit came from 17 large bets that
happened to win.

Re-filtering v1's ledger after the fact to show the good subset would be the
selection we criticise in others. So v1 stands as recorded, and this is a new,
separately pre-registered test of the rules v1's data suggests.

## Hypothesis

With stale fair prices, early execution and thin liquidity excluded by rule
rather than by hindsight, our value signals executed at Betfair retain a small
positive edge after commission. Expected size: low single digits at flat
stakes, consistent with v1's clean core (+2.4%) — not the +15% weighted figure.

## Rules — fixed in advance

1. SIGNALS: every value flag produced by our public methodology (fair prob
   >= 30%, edge <= 12%, best-soft above devigged Pinnacle fair). Unchanged.
2. FRESH FAIR ONLY: the fair probability is recomputed from the live sharp
   benchmark at execution time. If the latest benchmark reading is older than
   2 hours, the signal is SKIPPED (reason `fair_stale`) and rechecked later.
   There is no fallback to the fair price logged at detection.
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
   the DEVIGGED Pinnacle close (power devig, the same devig used across the
   site). v1 compared the exchange price to the raw closing price, which
   includes the bookmaker's margin and inflated CLV by ~5 percentage points;
   that error is corrected here and disclosed on v1's ledger.
8. PROMOTION GATE (decided in advance): real-money testing is justified ONLY
   if 150+ recorded bets show flat-stake ROI > 0 with the 95% interval
   excluding zero, AND average devigged CLV > 0. Otherwise the strategy is
   declared not viable at the exchange and receives a public autopsy.
9. LEDGER: every decision, including skipped signals and their reasons, is
   published live under the label EXP-003 and cannot be edited or removed
   after kick-off. v1's ledger stays published under EXP-002.

## Honest caveats

- Rules 2–4 were chosen because v1's losing segments matched them. That is a
  legitimate reason to pre-register a new test; it is not evidence the new
  rules work. Only this forward run can be.
- v1's clean core is +2.4% at flat stakes on 119 bets, an interval that
  comfortably includes zero. The prior for this test is "roughly break-even".
- Displayed Betfair prices, even at 500+ liquidity, may not be fillable at
  size for a real account; the dry-run still records the displayed price.

TipsAudit — measured, not opined. https://tipsaudit.com
