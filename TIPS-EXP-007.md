# TIPS-EXP-007 — Value Flags on Goal Markets at Soft-Bookmaker Prices (Paper Test)

Version 1.0 · Registered on the date of its commit in the public record
repository. This file is frozen; its SHA-256 is published and anchored
externally (GitHub + OpenTimestamps). It runs in parallel with TIPS-EXP-005 and
TIPS-EXP-006, which are not changed in any way.

## Why this experiment

Our public value flags cover only the 1X2 market. About a third of the picks
posted by the tipsters we audit are on goal markets (over/under, both teams to
score), and since October 2026 our match pages show the fair price on those
markets without any value flag. This experiment asks, before we ever show such
a flag: when a soft bookmaker prices a goal market clearly above the sharp
market's fair price, does that price beat the devigged Pinnacle close?

Nothing is staked and no flag from this experiment is shown before kick-off.

## Hypothesis

Flags defined by the rules below have a mean closing line value above zero.

## Rules — fixed in advance

1. MARKETS: full-time total goals over/under on half-goal lines (0.5, 1.5,
   2.5, ...) and both teams to score (yes / no). Whole and quarter lines, where
   part of the stake can be refunded, are excluded.
2. MATCHES: football matches in the leagues our odds feed covers
   (the-odds-api), excluding World Cup and UEFA Nations League matches, as in
   all our audited figures. Matches that share one feed event are skipped.
3. READINGS: our collector reads each match's goal-market prices from the
   feed (the-odds-api, EU region; markets totals, alternate_totals and btts)
   once in each of five windows before kick-off: 330–390, 165–195, 80–100 and
   35–55 minutes, and 3–15 minutes. Kick-off is the feed's start time from the
   previous reading (our fixture time before the first reading, or when the
   feed's time has passed and our fixture time is more than 60 minutes later).
   When the start time moves, the readings are repeated for the new time.
   Where a bookmaker quotes the same line in totals and in alternate_totals,
   the totals price is used. Flags are evaluated only at the first four
   readings.
4. FAIR PRICE: Pinnacle's two prices on the same line in the same reading,
   with the margin removed by power devig. The feed must report Pinnacle's
   market as updated at most 90 minutes before the reading.
5. FLAG: a soft bookmaker's price on that line and side, with price at most
   3.50 and gap = price × fair probability − 1 of at least 5% and at most 12%
   (larger gaps are treated as feed errors and ignored). The feed must report
   that bookmaker's market as updated at most 60 minutes before the reading.
   Soft bookmakers are all bookmakers in the feed's EU region except Pinnacle
   and this list, frozen at registration: exchanges (betfair_ex_eu, matchbook,
   betfair_ex_uk, smarkets) and UK-only bookmakers excluded from our value
   flags at registration (betfair_sb_uk, paddypower, skybet, ladbrokes_uk,
   coral, betfred_uk, betway, boylesports, casumo, grosvenor, livescorebet,
   virginbet, unibet_uk, leovegas, betano_uk).
6. ONE FLAG PER MATCH: the first reading with a qualifying price creates the
   flag; at that reading the qualifying price with the largest gap is chosen,
   across all half-goal lines, both sides and both markets (ties: the lower
   price, then the bookmaker key in alphabetical order). Later readings add no
   flag for that match. Lines and markets of the same match are correlated;
   counting them separately would overstate the evidence.
7. CLOSE AND CLV: the close is Pinnacle's pair of prices on the flag's line in
   the last reading that is after the flag and within 30 minutes before the
   kick-off time the feed gives at that reading; the feed must report
   Pinnacle's market as updated after the flag. It is recorded on the flag.
   CLV = flag price × devigged closing probability − 1. Flags without a close
   (no such reading, Pinnacle no longer quoting the line, or Pinnacle not
   updated since the flag) are counted in coverage and excluded from the
   primary measure. A flag whose match does not start within 60 minutes of the
   kick-off time recorded at its last reading is void: excluded from the
   primary measure and from coverage. A flag whose match has no recorded final
   score 96 hours after kick-off keeps its CLV and is excluded from the return
   only.
8. DECISION: if 300 flags get a close by 2027-03-31 (UTC), the decision sample
   is the first 300 of them in order of detection, and the result is final once
   no flag detected up to the 300th is still waiting for its close. Otherwise
   the sample is every flag detected by 2027-03-31 that gets a close, and the
   result is final once none of them is still waiting for its close. "Works" if
   the lower bound of the 95% interval (normal approximation, from 30 flags) is
   above zero; "fails" if the upper bound is below zero; otherwise, including
   with fewer than 30 flags, "inconclusive". There is no early stop and no flag
   is created after the 300th flag with a close or after 2027-03-31. Interim
   figures are published continuously, labelled interim.
9. SECONDARY MEASURES (reported, not decisive): coverage; flat-stake return of
   1 unit per flag (published from 100 flags with a result); CLV by market
   (over/under vs both teams to score), by gap band (5–8% vs 8–12%) and by
   bookmaker; CLV at the same bookmaker's price on the same line at our next
   reading after the flag, with the share of flags whose price was no longer
   quoted at that reading.
10. RESULT: settled on the final score we record for the match. In cup ties
    decided after extra time that score can include extra-time goals; this
    affects only the secondary return, not CLV.
11. LEDGER: every flag is recorded when it is detected, with the Pinnacle pairs
    used at the flag and at the close and their feed update times, and every
    flag is listed publicly after its match has started, with its price, fair
    price, close, CLV and result. Flags are not shown anywhere before kick-off.
12. T0 AND CHANGES: flags count from the moment this file is registered (the
    timestamp of its commit in the public record repository). Prices read
    before T0 are collected but cannot create flags. Any pause of the collector
    or change to its switches during the experiment is published in
    CORRECTIONS.md.

## Expected rate and sample size

Unknown. In the readings made on 9 October 2026 before registration (cut-off
19:05 UTC), no soft price was 5% or more above the fair price:
- 14 readings of 11 matches that requested alternate_totals and btts only:
  2–5 eligible soft bookmakers per match, 214 soft prices comparable with
  Pinnacle on the same line, highest 1.8% above the fair price;
- 3 readings of 3 matches that also requested totals (from 18:45 UTC): 10–13
  eligible soft bookmakers per match, 106 comparable prices, highest 4.2% above
  the fair price.
If flags are rare, the date limit will decide with a small sample and the
likely verdict is "inconclusive". We register that outcome in advance instead
of loosening the rule later. With a per-flag CLV standard deviation of about 8
percentage points, 300 flags would give an interval of about ±0.9 percentage
points.

## Honest caveats

- A positive result means the prices beat the sharp close on paper. It does
  not mean a bettor would be allowed to stake on them: soft bookmakers limit
  accounts that beat the close, and a price can be gone by the time a bettor
  acts (see the next-reading measure).
- Soft prices and the benchmark come from the same feed. Pinnacle's prices in
  that feed can lag Pinnacle itself; a stale benchmark creates flags that are
  not real value. Choosing the largest gap favours such moments; the close
  must be an update of Pinnacle's market after the flag, which is meant to
  expose them.
- Flags without a close are excluded from the primary measure; if missing
  closes follow how the line moved, the measure can be biased. Coverage is
  reported for that reason.
- The experiment measures the soft bookmakers in the feed's EU region, not soft
  bookmakers in general.
