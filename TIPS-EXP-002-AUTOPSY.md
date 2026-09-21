# TIPS-EXP-002 — Autopsy (concluded 2026-09-21)

Registered 2026-07-23, run 2026-07-18 → 2026-09-21, 262 settled dry-run bets.
Rule 7 of the frozen specification promised a public autopsy if the strategy was
not clearly viable. This is it. Nothing below was edited after kick-off; the full
ledger stays published under the label EXP-002.

## Headline result

| | Value |
|---|---|
| Settled bets | 262 |
| Win rate / break-even at avg odds 2.50 | 43.9% / 40.0% |
| P&L (half-Kelly, simulated 1,000) | +119.8 |
| ROI, stake-weighted | +3.4% |
| ROI, flat 1 unit per bet | **+0.8%** |
| Average execution edge (claimed) | +6.0% |
| CLV vs devigged Pinnacle close | **+2.48%** (61% of bets positive) |
| CLV as originally published (vs raw close) | +6.52% (83%) — see correction |

Against its own gate (100+ bets, edge >= 2%, positive CLV) v1 technically
passes. We are closing it anyway, because the pass is not what it looks like.

## Correction to the published CLV

Until 2026-09-21 the ledger compared the Betfair back price (an exchange price,
no margin) with Pinnacle's raw closing odds, which include the bookmaker's
margin (4.95% overround on these matches, ~1.7 points per selection). A price
without margin will "beat" a price with margin almost by construction. Against
the devigged close — the same power devig used everywhere else on the site — the
average CLV falls from +6.52% to +2.48% and the share of bets beating the close
from 83% to 61%. The rule said "same-book CLV"; same-book means same kind of
price. The corrected figure is now what the ledger shows, for every bet.

## Where the money was lost

| Segment | Bets | ROI | Note |
|---|---|---|---|
| Fair price stale (benchmark reading > 2h old) | 77 | **−14.8%** | code fell back to the fair logged at detection, often days old |
| Executed 3–8 h before kick-off | 59 | **−21.0%** | the +2% early-edge surcharge did not compensate |
| Betfair liquidity < 500 | 115 | **−7.0%** | displayed prices that are not really there; higher "CLV", lower results |
| **Clean core** (fresh fair, < 3 h, liquidity ≥ 500) | 119 | **+15.3%** weighted, **+2.4%** flat | most weighted profit came from 17 large bets that won |
| Everything else | 143 | −6.3% | |

The "edge paradox" — bets with the largest claimed edge (8–12%) lost the most
(ROI −34.9%) — is the same finding from another angle: the largest edges were
disproportionately stale, thin or early. A 10% edge at Betfair is a data
artefact, not value.

Calibration was slightly optimistic: fair probabilities averaged 46.3% where
43.9% of bets won; the 30–38% band was 5 points too confident, and draws (11
bets) won 18% against a 31% break-even.

## Why we did not just filter v1

Filtering the recorded ledger after seeing the results is exactly the
selection we criticise in tipsters. So v1 stands as it ran. The three losing
segments are instead turned into rules for a separately pre-registered
successor, TIPS-EXP-003, with its own ledger, hash and timestamp.

## What v1 leaves us with

At flat stakes the whole run is +0.8% and the clean core +2.4%, both inside an
interval that includes zero. That is consistent with what we wrote before
starting: our simulations put value-signal edge at the exchange near zero. The
honest prior for v2 is "roughly break-even, possibly slightly positive".

TipsAudit — measured, not opined. https://tipsaudit.com
