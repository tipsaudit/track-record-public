# TIPS-EXP-002 — Public Dry-Run: Can Our Value Signals Beat Betfair?

Version 1.0 · Registered 2026-07-23. This file is frozen; its SHA-256 is
published and anchored externally (GitHub + OpenTimestamps).

## Hypothesis

Our value signals (soft-bookmaker mispricings vs the devigged sharp fair price)
retain enough edge to be profitable when executed at Betfair exchange prices,
after commission — despite our own simulations showing near-zero average edge
at the exchange. The exchange's own occasional mispricings, caught by a strict
entry filter, may be sufficient.

## Rules — fixed in advance

1. SIGNALS: every value flag produced by our public methodology (fair prob
   >= 30%, edge <= 12%, best-soft above devigged Pinnacle fair), scanned in the
   window T-6h to T-10min before kickoff.
2. EXECUTION VENUE: Betfair exchange, best available back price, at the last
   automated check before kickoff (rechecked every ~30 minutes).
3. ENTRY FILTER: bet ONLY if the Betfair price grants >= 2.5% edge after 2%
   commission versus our fair probability, AND available liquidity >= 3x stake.
4. STAKING: half-Kelly per bet (0.5 x edge/(odds-1)), capped at 5% of bankroll
   per bet and 20% total exposure per day. Simulated bankroll: 1,000 lei.
5. MODE: DRY-RUN. No real money is staked. Bets are recorded at the observed
   price and settled at the real result, minus 2% commission on winnings.
6. METRICS: P&L, ROI, and same-book CLV versus the Pinnacle close.
7. PROMOTION GATE (decided in advance): the experiment justifies real-money
   testing ONLY if 100+ recorded bets show average execution edge >= 2% AND
   positive average CLV. Otherwise the strategy is declared not viable at the
   exchange and receives a public autopsy.
8. LEDGER: every recorded bet, including skipped signals and their reasons, is
   published live and cannot be edited or removed after kickoff.

## Honest caveats

- Betfair back prices at low liquidity may not be fillable at size; the dry-run
  records the displayed price, which flatters results. The 3x liquidity filter
  mitigates but does not eliminate this.
- Our prior simulations found value-signal edge at the exchange to be ~zero on
  average; this experiment tests whether a strict entry filter changes that.
  We publish the outcome either way.

TipsAudit — measured, not opined. https://tipsaudit.com
