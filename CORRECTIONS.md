# Corrections

Corrections we announced, newest first. Timestamped snapshots are never overwritten and the Git
history shows any difference between them, so each change below can be checked by comparing the
snapshots it names.

## 2026-11-06 — wrong stored results and swapped home/away: what we repaired and what moved

27 matches held a wrong score, 4 matches decided after extra time had the 1X2 of the score after extra time, 11 predictions had home and away swapped after we logged them, and one prediction was settled on a match it was not logged for. We repaired all of it on 6 November 2026. In all, 132 matches changed their stored result: 27 with a wrong score, 4 with the 1X2 after extra time, 99 with no score, one interrupted and one not yet played; on 3 more only the team IDs changed. The return of the live value flags, our live record, goes down from +2.0% to +1.8% (95% interval [−3.5; +7.2], which includes zero): on Macarthur – Western Sydney Wanderers (31 October 2026) we had stored 0–1, the final score is 0–0, and a live flag at 2.90 counted as won is lost. All but 2 of the 21,051 results we hold since 1 June 2026 have now been compared with a second read; the feed no longer returns those 2, and nothing is settled on them. The last reads, on 5 November 2026, added 7 corrections: Macarthur – Western Sydney Wanderers (0–1 to 0–0), Partick – Stenhousemuir (4–0 to 3–0) and Atvidabergs FF – Utsikten (no result, now 1–2), and the 4 after extra time.

The backtest return goes from +2.2% to +2.1%, ±3.9, which also includes zero. 2 predictions became void: one was a loss, one a win. The prediction hit rate we show goes from 72.3% to 72.2%. Some figures get worse and some get better. All of them are below, after what happened.

**How to check this.** The last snapshot before the repair is `data/track-2026-11-06T05-40-01Z.json`. Compare it with the next day's snapshot, using only the ids in the old one: ids that are only in the new one are new predictions, and rows that were not settled in the old one are ordinary settlements. The snapshots hold `result`, `prediction_*`, `value_*` and `hash`, but no scores, raw prices or model probabilities. So we also publish [2026-11-06-results.csv](https://github.com/tipsaudit/track-record-public/blob/main/corrections/2026-11-06-results.csv), every corrected match with its fixture id, stored score, final score, half-time score and whether the league table agrees (plus the match of row 7001, which keeps its score), and [2026-11-06-hash-preimages.csv](https://github.com/tipsaudit/track-record-public/blob/main/corrections/2026-11-06-hash-preimages.csv), the text behind the old and new hash of the 4 rewritten rows. The moved prices are in `datasets/tipsaudit-predictions-all.csv` from the next snapshot on (`pin_open_*`, `pin_close_*`, `best_soft_*`). In the snapshots a void row has an empty `result` and `prediction_won` 0, but it is not a loss and it counts in no rate, return or CLV; from this repair on, a new column at the end of the file, `void_reason`, says why it is void.

Earlier snapshots keep the old values. So do the copies on Hugging Face and Kaggle until their next upload. A published Zenodo version keeps its files (DOI 10.5281/zenodo.23683502); the corrected data goes into the next version, which the concept DOI 10.5281/zenodo.23683501 will point to.

### Fault 1: wrong scores saved once and never read again

Our collector saves a score the first time the fixture feed gives the status "finished", 100 to 170 minutes after kick-off. We did not keep the feed's responses, so we infer that status from the collector, which saves only on "finished". Nothing corrected a score once it was saved: from 12 October a later read stored the final score in a separate column, on 16 of these matches, and nothing compared it with the result.

On 5 November we read again the matches that looked most at risk and the results listed under "What was read" below, and took only the status "finished". 27 held a wrong score. All 27 were saved within three hours of kick-off, so the rate that says something is among results saved within 24 hours of kick-off and then compared with a later read. 19 of the 27 kicked off in one 22-hour window, from 21 August 16:00 to 22 August 14:00 UTC: 19 of 169 results (11.2%), and all of that window has been compared. Outside it: 8 of 10,757 (0.07%), with 2 more not compared. Results saved later than 24 hours after kick-off: 0 of 10,123 compared. We do not know why that window. On 21 of the 27 the stored score was the half-time score. On the other 6 it was not (0–1 stored, half-time 0–0, final 0–0; 1–1 stored, half-time 2–0, final 2–1; 2–3 stored, half-time 1–0, final 2–2; 1–2 stored, half-time 1–1, final 2–2; 0–1 stored, half-time 0–0, final 0–0; 4–0 stored, half-time 2–0, final 3–0). On Macarthur – Western Sydney Wanderers the stored score counted a goal that VAR cancelled; the feed's own event list shows both the goal and the cancellation. On the others we cannot say why.

The league table from the same feed agrees with the corrected score for both teams on 16 of the 27. On the other 11 the table cannot say, so one reading of the feed is all we have. The last column below says which is which; it is not a second source.

14 of the 27 are in the public file. `prediction_won` and the value flag follow the new `result`; `hash`, seals and closing prices do not depend on the result and do not change.

| Row | Match | Stored | Final | `result` | `prediction_won` | Value flag | Table agrees |
|---|---|---|---|---|---|---|---|
| 2106 | Cheongju – Suwon Bluewings | 2–3 | 2–2 | 2 → X | 1 → 0 | – | yes |
| 2422 | Sonderjyske – Viborg | 0–1 | 0–0 | 2 → X | 1 → 0 | won +1.33 → lost −1.00 | yes |
| 3594 | FC Cajamarca – Atletico Grau | 1–1 | 1–3 | X → 2 | 1 → 0 | lost −1.00, unchanged | yes |
| 3627 | Nantong Haimen Codion – Shandong Taishan II | 1–0 | 2–2 | 1 → X | 1 | won +1.88 → lost −1.00 | cannot say |
| 3628 | Tai'an Tiankuang – Shanghai Second | 1–2 | 2–2 | 2 → X | 0 | lost −1.00, unchanged | cannot say |
| 3631 | Shanghai Port II – Qingdao Red Lions | 0–0 | 2–0 | X → 1 | 1 | won +2.25 → lost −1.00 | cannot say |
| 3632 | Dalian Yingbo B – BIT | 1–1 | 3–4 | X → 2 | 0 | – | cannot say |
| 3903 | Club Queretaro – Toluca | 0–0 | 1–2 | X → 2 | 1 | – | yes |
| 4081 | Leones del Norte – Independiente del Valle | 1–0 | 2–3 | 1 → 2 | 0 → 1 | – | cannot say |
| 5124 | Njardvik – Afturelding | 1–1 | 2–1 | X → 1 | 0 → 1 | lost −1.00 → won +2.10 | cannot say |
| 12829 | Macarthur – Western Sydney Wanderers | 0–1 | 0–0 | 2 → X | 1 → 0 | live, won +1.90 → lost −1.00 | yes |
| 3619 | Alianza Atletico – Sporting Cristal | 1–0 | 3–1 | 1 | 1 | lost −1.00, unchanged | yes |
| 3929 | Leon – Monterrey | 1–0 | 2–0 | 1 | 1 | lost −1.00, unchanged | cannot say |
| 5374 | Ahrobiznes Volochysk – Probiy Horodenka | 2–0 | 6–0 | 1 | 1 | – | yes |

- Net effect on the hit rate: 4 picks go from won to lost, 2 from lost to won; row 7001, below, also leaves the count as a won pick. Net effect on the backtest flags: 3 go from won to lost, 1 from lost to won, −4.36 units in all, including the void below. On the live flags: 1 from won to lost, 0 from lost to won, −2.90 units.
- **The last reads, on 5 November 2026,** added 7 matches: the 4 after extra time, below, and these three. Macarthur – Western Sydney Wanderers is in the table above and moves the live record. Partick – Stenhousemuir (24 October 2026, 4–0 stored, 3–0 final) keeps its 1X2, and its only prediction row is a display-only price, so it moves team and league statistics; its match page now lists three goals, not four. Atvidabergs FF – Utsikten (1 November 2026) had no result at all and now holds 1–2; it is among the 99 below.
- **4 matches decided after extra time** still had the 1X2 of the score after extra time, though our rule since 16 October 2026 is the score after 90 minutes. Our repair of that day did not reach them, because we did not hold their 90-minute score. The stored scores stay; only the 1X2 changes: Dinamo Tbilisi – US Mondorf-les-bains (16 July 2026, 1–1 after extra time, 0–1 after 90 minutes, 1X2 X → 2), Riga – Vardar Skopje (28 July 2026, 5–2 after extra time, 1–2 after 90 minutes, 1X2 1 → 2), Drita – Floriana (28 July 2026, 2–1 after extra time, 1–1 after 90 minutes, 1X2 1 → X) and Fylkir – HK Kopavogur (13 September 2026, 2–2 after extra time, 1–2 after 90 minutes, 1X2 X → 2). In the public file: row 6310: `result` X → 2, `prediction_won` 0, unchanged. Not in the public file, because they have no Pinnacle price: row 1657: `result` 1 → 2, `prediction_won` 1, unchanged; row 1658: `result` 1 → X, `prediction_won` 1 → 0. No value flag sits on them. Two more reads of the same kind contradicted themselves (Pafos – HNK Hajduk Split, Ilves – Stjarnan, both 30 July 2026): the score in the response does not match its own list of goals. That list agrees with what we hold, 4–0 and 2–1 after extra time, so those results stay.
- **Row 4838, Coquimbo Unido – Huachipato (16 September).** We stored 0–1. The feed now says "Match Interrupted" and gives no finished status, though the same response still shows 0–1 and Huachipato as winner. The league table from the same feed does not count this match for either team (games played: Coquimbo Unido 25 (38–35) and Huachipato 25 (34–40)); without it, our results and that table agree. We count no result without a finished status, and a prediction with no result after 7 days is void, so this row and its backtest flag are now void (`no_result`), with blank closing prices like every void row. The rule does not look at the score, but both were losses, so this helps us: counted as lost, the backtest return would be +2.04% instead of +2.07%, and the hit rate 72.231% instead of 72.240%. One row in the favourites backtest and one in each /dropping-odds count leave with this match. That favourites row was a lost pick, and it explains the whole move in that backtest below: 670 picks won, of 891 before and 890 after.
- 10 predictions on these matches are not in the public file, because they have no Pinnacle price. Their pages now show the corrected score. On 3580, 3598, 3711 and 3719 the result changes too: the pick on 3580 and 3711 goes from won to lost; on 3598 from lost to won; on 3719 it stays as it was. 3 of the 27 matches have no prediction from our model.
- **99 matches with no stored score**, in three competitions we do not forecast (Ettan Södra, Liga Pro Serie B, Victoria NPL 2), now hold their final score. No prediction or flag depended on them. On 14 of them we also took the feed's kick-off time, which differed by more than an hour.
- **43 site picks** had stayed unverifiable because the result reached us after the 6-day limit, or for lack of a score: most are on UEFA Nations League matches in September, 4 are on the matches above. They are now graded, and the counts of settled picks move (table below). 1 has a price, on API-Football, and moves its return on double chance (table below); the others have none.
- **Four tipster picks on the 1X2 market** (#188, #203, #366, #733) were still graded won on the score after extra time, though the matches were level after 90 minutes. Our repair of 16 October did not reach tipster picks. They are now lost, on the rule on /methodology; the tipsters involved are analysis-tip, analysistips and hj-rh.
- **Grasshoppers – FC Basel (28 November).** We had stored a 0–0 on 26 October for a match not yet played. It is cleared. Nothing depended on it.
- **Shakhtar Donetsk – Obolon-Brovar (30 October).** The feed still said "not started" when our 48-hour window for first reading a result closed, so this match had no result, and row 12975 and a live favourite stayed open. It is now read and settled like any new match (2–0). The figures below already include it.
- Team and league statistics are recalculated after both repairs. The repairs themselves change 21 league rows and 249 team rows.

### Fault 2: home and away swapped after we logged a prediction

The feed can swap home and away after we log a prediction. Searching by team name, we found 11 such predictions, with kick-offs from 6 August to 1 November 2026; 5 are in the public file. A prediction keeps what we logged: the team order, the model's probabilities and the log time. Prices and results had followed the feed's new order, so on some rows they sat under the wrong team name. We do not know when each swap happened.

- **Rows 3636, 4299, 4474, 6074.** Pinnacle prices, closing prices, best soft prices and `result` moved into the logged team order. Won or lost, odds taken, closing odds and CLV are unchanged. The pick is the same team at the same price, but its letter follows the swap: `prediction_pick` goes from X2 to 1X on 3636 and from 1 to 2 on 4474 and 6074 (4299 stays 12). The old letter pointed at the wrong name. `hash` changes because it covers the Pinnacle prices, and these match pages carry a dated line pointing here:
  - 3636: 20d508a30543d28e → 958256f1dbaf27e4
  - 4299: 60a6db5637dfa6a4 → 0a831a3de5a2fa0a
  - 4474: 22cd492516d85611 → 394c9725da143002
  - 6074: 2fed0182b5c6b4c0 → f3316885d9cedf4f

  We do not store which order the feed used when we read a price; our raw price captures stay as the feed sent them. On 3636 and 4299 we inferred the order from the range of prices those teams closed at, so neither is direct proof. On 4299 both prices rule out the old order: Koper at home at 3.33 is outside its home range (1.48–2.68), and Aluminij away at 2.09 is outside its away range (2.80–5.90). On 3636 only one does: Bucheon away at 2.54 is outside its away range (4.19–7.85, 7 matches), while Pohang at home at 3.09 fits its home range (2.24–4.78). The new reading fits all four ranges on both rows. 3636 is the weaker of the two.
- **Row 6074.** `best_soft_*` mixed prices from before and after the swap. It now holds the highest price seen for each team: 8.60 (Marathonbet) for Livyi Bereh, first seen on 10 September 2026, and 1.47 (Betano) for Polessya, first seen on 13 September 2026. The row was logged on 6 September 2026, so on this row, unlike other rows of that age, `best_soft_*` includes prices seen after logging.
- **Row 3636.** Its backtest flag and seal are unchanged. `value_selection` goes from 1 to 2, because the `home` in its `value_preimage` is the fixture's home team, Bucheon. That flag is a win of +2.30 units and rests on the inference above. Without it the backtest return would be +2.00% instead of +2.07%.
- **Row 4052** was already consistent in the logged order and keeps its values. One tipster's pick on that match read the other team's closing price and showed CLV −78.57%. That was wrong because the row is swapped, so we removed the value and put nothing in its place: the last Pinnacle price we hold on that match is from 12 August, nearly 12 days before kick-off and before the pick was posted. That tipster's verdict stays below market and the grade stays D. But the same problem sits on other picks of the same tipster that we did not touch: 10 more of its CLV values rest on a last price more than 24 hours before kick-off, most from 12 August. Without them its average is −2.84% [−6.20; +0.56], n=68, which is no longer below market. See "closing prices" below.
- **Rows 2468, 2475, 2916, 8482, 12842, 13561** have no Pinnacle price and are not in the public file. On the site, the result of 2468, 2475, 8482 and 13561 now reads in the logged order; the pick on 2468 shows as lost, not won. Row 12842 is SK Poltava – Viktoriya Mykolaivka, as we logged it; the fixture has them the other way. It had both faults, a half-time score and swapped teams, which cancelled out on its `result`; the score is now the final 2–1.
- **Team IDs.** On 4 matches (Yarud Mariupol' – Chernihiv, Viktoriya Mykolaivka – SK Poltava, Shakhtar Donetsk – Obolon'-Brovar, Pogoń Siedlce – Ruch Chorzów) the team IDs were swapped relative to the names. That moves only their team pages and team statistics.

We keep these 11 forecasts counted, even though the model's percentages on them were calculated with the other team at home. Voiding them now would be a choice made with their results known, and the rule below applies before the result is read. For comparison: under that rule, the 5 rows in the public file would be void, with a hit rate of 72.248% instead of 72.240% and a backtest return of +2.00% instead of +2.07%. On their own these rows leave hit rate and CLV unchanged and improve the model's /calibration Brier from 0.6138 to 0.6136 and log-loss from 1.0236 to 1.0233. A new column at the end of the public file, `teams_reversed_after_logging`, marks the 5 rows. It is documented on /data and in the README of the public repository.

### One prediction settled on a match it was not logged for

**Row 7001** was logged on 17 September 2026 as Deportivo Maldonado – Juventud. The fixture data then changed the opponent, the match became Deportivo Maldonado – Cerro Largo, and on 20 September 2026 the row was settled on that match, 2–0, as a won pick. Juventud played another match the next day (row 7003). The row has no value flag, and nothing else depends on it, though it leaves /calibration and the favourites backtest count.

It is now void (`opponent_changed`), with blank closing prices like every void row. The match keeps its result, which is right for Cerro Largo. This goes against us: it removes a won pick. Kept, the hit rate would be 72.244% (5,838 of 8,081) instead of 72.240% (5,837 of 8,080); prediction CLV moves by less than 0.001 points.

We decided this with the result known, which is what we refused to do for the 11 swapped forecasts above. The difference: those forecast the match that was played, with the two teams in the other order. This one names a team that was not in the match.

### What moved

Figures from our data at 2026-11-06 05:52 UTC, at the precision the site shows. "Before" is the snapshot named above.

| Figure | Before | After |
|---|---|---|
| Predictions settled | 8,082 | 8,080 |
| Predictions void | 42 | 44 |
| Prediction hit rate | 72.3% (5,840 won) | 72.2% (5,837 won) |
| Prediction CLV, raw | −0.9% (−0.949) | −1.0% (−0.951) |
| Backtest value flags | 3,085 | 3,084 |
| Backtest flags, return | +2.2% ±3.9 (+68.30 units) | +2.1% ±3.9 (+63.94 units) |
| Backtest flags, CLV at the flag | +1.9% ±0.3 (+1.860) | +1.9% ±0.3 (+1.857) |
| Backtest flags re-quoted within 60 minutes | 28% (879 of 3,085) | 29% (879 of 3,084) |
| Live value flags | 1,582 | 1,582 |
| Live flags, won | 51% | 51% |
| Live flags, return | +2.0% [−3.4; +7.4] (+2.033) | +1.8% [−3.5; +7.2] (+1.850) |
| Live flags, CLV at the flag | +1.6% ±0.5 | +1.6% ±0.5 |
| Live flags, realizable CLV | +1.0% on 1,319 | +1.0% on 1,319 |
| /experiments/soft-value, flat-stake return on 1,295 flags (secondary measure) | +0.6% (+0.577) | +0.4% (+0.353) |
| Favourites backtest, matches | 6,575 | 6,573 |
| Favourites backtest, picks | 891 | 890 |
| Favourites backtest, hit rate | 75.2% | 75.3% |
| Favourites backtest, return | −0.3% [−4.1; +3.5] | −0.2% [−4.0; +3.6] |
| /calibration, matches | 8,082 | 8,080 |
| /calibration, market Brier / log-loss | 0.597 / 0.999 (0.5965 / 0.9991) | 0.596 / 0.999 (0.5964 / 0.9990) |
| /calibration, model Brier / log-loss | 0.614 / 1.024 (0.6138 / 1.0236) | 0.613 / 1.023 (0.6134 / 1.0230) |
| API-Football, settled picks | 2,349 (1,678 won) | 2,350 (1,679 won) |
| API-Football, return on double chance | −3.1% | −3.0% |
| Statarea, settled picks | 1,769 (923 won) | 1,805 (944 won) |
| Zulubet, settled picks | 293 (153 won) | 300 (159 won) |
| Tipster hj-rh, average CLV on 1X2 | −4.54% [−8.28; −1.10], n=79 | −3.59% [−6.79; −0.48], n=78 |
| Tipster hj-rh, trust score | 50 D | 52 D |
| Tipster hj-rh, picks won of 302 | 217 | 216 |
| Tipster hj-rh, return | −0.96% | −1.74% |
| Tipster analysistips, picks won of 100 | 76 | 74 |
| Tipster analysistips, return | +14.82% | +12.09% |
| Tipster analysis-tip, picks won of 49 | 42 | 41 |
| /dropping-odds, shortened prices (rolling 90 days) | 1,046, won 49.9% | 1,045, won 49.9% |
| /dropping-odds, drifted prices (rolling 90 days) | 980, won 35.0% | 979, won 34.9% |

The two /dropping-odds rows sit on a window that moves every day, so their totals will not reproduce on another day. The effect of the repair is one row less on each.

On /track-record, these rows of the breakdown move. Return is shown from 100 flags, with its 95% interval. The first 2 rows are live and backtest apart; every other row combines the two.

| Row | Flags before | after | Win before | after | Return before | after | CLV before | after |
|---|---|---|---|---|---|---|---|---|
| recorded: Live | 1,582 | 1,582 | 51% | 51% | +2.0% ±5.4 | +1.8% ±5.4 | +1.6% ±0.5 | +1.6% ±0.5 |
| recorded: Backtest | 3,085 | 3,084 | 50% | 50% | +2.2% ±3.9 | +2.1% ±3.9 | +1.9% ±0.3 | +1.9% ±0.3 |
| strength: Strong value | 1,445 | 1,445 | 47% | 47% | +5.8% ±6.2 | +5.4% ±6.2 | +3.7% ±0.5 | +3.7% ±0.5 |
| strength: Value | 1,283 | 1,283 | 51% | 51% | +3.6% ±5.9 | +3.4% ±5.9 | +1.5% ±0.5 | +1.5% ±0.5 |
| strength: Micro-edge | 1,939 | 1,938 | 52% | 52% | −1.5% ±4.5 | −1.5% ±4.5 | +0.5% ±0.3 | +0.5% ±0.3 |
| odds: < 1.80 | 1,395 | 1,394 | 69% | 69% | +3.8% ±3.7 | +3.9% ±3.7 | +2.0% ±0.3 | +2.0% ±0.3 |
| odds: 1.80 – 2.49 | 1,826 | 1,826 | 49% | 49% | +1.6% ±4.8 | +1.5% ±4.8 | +1.9% ±0.4 | +1.9% ±0.4 |
| odds: 2.50 – 3.99 | 1,446 | 1,446 | 34% | 34% | +1.2% ±7.3 | +0.8% ±7.3 | +1.4% ±0.5 | +1.4% ±0.5 |
| month: 2026-08 | 1,034 | 1,034 | 51% | 51% | +5.3% ±6.8 | +4.4% ±6.8 | +2.0% ±0.4 | +2.0% ±0.4 |
| month: 2026-09 | 1,410 | 1,409 | 48% | 48% | −2.4% ±5.7 | −2.1% ±5.7 | +1.8% ±0.5 | +1.8% ±0.5 |
| month: 2026-10 | 1,366 | 1,366 | 50% | 50% | +1.1% ±5.8 | +0.9% ±5.8 | +1.6% ±0.5 | +1.6% ±0.5 |
| league: Primera Division Chile | 55 | 54 | 53% | 54% | — | — | +3.9% ±2.6 | +3.8% ±2.6 |
| league: Danish Superliga | 51 | 51 | 51% | 49% | — | — | +0.9% ±2.2 | +0.9% ±2.2 |
| league: 1. Deild | 48 | 48 | 65% | 67% | — | — | +1.8% ±1.8 | +1.8% ±1.8 |
| league: China League Two | 41 | 41 | 41% | 37% | — | — | +4.4% ±1.6 | +4.4% ±1.6 |
| league: A-League | 15 | 15 | 27% | 20% | — | — | +4.1% | +4.1% |

On /calibration, the observed share moves in these bands: market 10–20%: 17.1% → 17.0%; market 50–60%: 55.7% → 55.8%; market 60–70%: 64.6% → 64.5%; model 10–20%: 15.9% → 15.8%; model 50–60%: 55.7% → 55.8%; model 60–70%: 63.2% → 63.5%. The favourites backtest band 65–70% goes from 350 matches at 67.1% to 349 at 67.3%.

Not moved, on the reads we have: backtest realizable CLV (+0.8%), the live favourites record, API-Football's rate on priced picks (70.2%) and CLV (−2.58%), Statarea's rate on priced picks (52.7%) and CLV (−2.01%), Zulubet's rate on priced picks (51.6%) and CLV (+0.33%), the other prediction sites, the other tipsters and the Value Alerts register. No repaired match carries any pick of the Italian draw ledger, any TIPS-EXP-007 flag, any Beat the Close pick or any Loto Foot row. AutoBet bet #40612 (TIPS-EXP-005, row 12829: selection 1 at 3.25, stake 14.95) sits on Macarthur – Western Sydney Wanderers. This is its dated entry. The ledger row stays as settled: lost, −14.95, on the stored 0–1. The real result is 0–0, on which it would also be lost. The live flag on Macarthur – Western Sydney Wanderers counts in the population of TIPS-EXP-006. Its primary measure is CLV, which does not depend on the result, and its decision was already recorded, so neither changes; its secondary flat-stake return goes from +0.6% to +0.4%. TIPS-EXP-007 rule 10, settlement on the final score we record, is unchanged.

### What changes from today

- A prediction with a Pinnacle price that is found swapped when it is settled is void (`teams_swapped`). That decision looks at team names only, never at the result.
- A prediction with no price that is found swapped gets its result in the logged order and the marker.
- A value flag on a match found swapped is void too (`teams_swapped`).
- No new price is written on a swapped row.
- A prediction whose opponent changes in the fixture data after logging is not settled on the new match. It is void 7 days after kick-off (`opponent_changed`). A value flag or a favourite on such a match is void when it would settle. Row 7001, above, was settled before this rule.
- We read saved results again, 2 to 30 hours and 24 to 48 hours after saving them, or after kick-off if they were saved before it. We also read matches that have started, have no result and have something open on them. A reading that differs changes nothing until a second reading, at least 30 minutes later, says the same. Then one code path corrects the score, settles again what depends on it and alerts us. If more than 10 readings disagree at once, nothing changes and we are alerted, until a person has checked the feed.
- If the feed no longer gives a final result, also read twice, the result is cleared and what depended on it is reopened. It is void if there is still no result 7 days after kick-off.
- These corrections rewrite settled rows of the public file by themselves. They show up first as a difference between two daily snapshots. We add the ones that touch the public file to this page by hand, so an entry can come later than the change.
- These corrections do not change two pre-registered ledgers: the Italian draw (TIPS-EXP-001) and AutoBet (TIPS-EXP-005 and the AutoBet experiments before it). Their specifications forbid editing or removing a row after kick-off (rule 9 of TIPS-EXP-001 and TIPS-EXP-005), so a row stays as it was settled. If a correction touches one of their rows, we publish a dated entry here with the old and the real result, and the final evaluation uses the real result. The rules above do not close their rows either: a bet on a prediction found swapped stays and is settled at the real result, with no CLV because the row has no close, and a bet on a prediction whose opponent changed stays open and gets a dated entry here.
- TIPS-EXP-007 says something else. Rule 10: "settled on the final score we record for the match". No rule forbids that score from being corrected, so its secondary return follows a correction by itself; its CLV and its decision do not depend on the result.

These rules are also on /methodology.

### What this does not cover

- **What was read.** We hold 21,051 results since 1 June 2026. 21,049 have been compared with a later read: 8,472 when another of our collectors stored the 90-minute score, 7,353 in one read on 5 November 2026 (368 requests, 1 difference: Partick – Stenhousemuir, stored 4–0, final 3–0), 4,618 in a second read the same day, after we found that the first list had missed them (232 requests, 6 differences: the 4 after extra time and the 2 that contradicted themselves), and 606 read in smaller lots that day, among them every result under a live flag, a live favourite or a Value Alert.
- **What was not read: 2 results.** The feed no longer returns these fixtures (Trujillanos FC – Rayo Zuliano and Portuguesa FC – Estudiantes de Merida FC, Venezuela, 16 September 2026, stored 2–4 and 2–1). No prediction, flag or pick rests on them.
- **A second read is not proof.** Macarthur – Western Sydney Wanderers had been read again on 1 November and the feed still said 0–1; we found it by comparing our results with the league table, and the feed changed to 0–0 later. So "not moved" above means not moved on one more read of the same feed. Results saved after those reads are covered by the new rule above.
- **Matches with no result.** 456 matches that kicked off since 1 June hold no result at all: 151 are postponed or cancelled, and most of the rest are in competitions we do not forecast. The reads above cover the results we hold, not these. No settled prediction or figure rests on them.
- **Closing prices.** On 394 rows of the public file, the last Pinnacle price we hold is more than 24 hours before kick-off (340 between 1 and 7 days, 54 more than 7 days; 371 kick off in August, most of them in the time our odds feed ran out of provider credits, 12 August to 1 September). They stay counted, row 4052 among them, with that price as their close. 158 of them have a backtest flag. Without them, backtest flag CLV would be +1.83% instead of +1.86% (shown +1.8% instead of +1.9%) and prediction CLV −0.98% instead of −0.95%. Whether a price that old can count as a close is a decision we have not taken here.
- **Row 5223**, Utrecht – GO Ahead Eagles, has closing prices of 19.15 / 7.33 / 1.16 from 5 September 2026, three days before kick-off, which gives the largest negative CLV in the file (−75.67%). They look wrong, but we hold no better price, so the row stays as it is. Here the error goes against us: without it, prediction CLV would be −0.94% instead of −0.95%.
- We looked for swaps by team name. A swap visible only in the prices would not show up that way. 4 predictions look like candidates (rows 4899, 5223, 5986, 10878), and none is proven.
- Results from before 1 June 2026 were not read again in bulk. No prediction is settled on them. Among those we did read, two matches from 2022 (Eibar – Girona, Sligo Rovers – Dundalk) differ from the feed; we left them. A friendly from 2026, Finland U18 – Norway U18, keeps its score, because the feed now returns another match under that fixture id.
- All scores come from one feed, API-Football.
- A match awarded by a competition keeps the score played on the pitch, so our table differs from the official one there. We know of three: two in Chile in February 2026, with no prediction on them, and Altay – Kyzyl-Zhar on 8 August, stored 2–2 where the table counts 3–0, with one prediction that is not in the public file.

## 2026-11-06 — TIPS-EXP-006: decided, inconclusive

TIPS-EXP-006 (`TIPS-EXP-006.md`, SHA-256 7a5b74695fe5...) has reached its decision point of 300 takeable settled flags. The result is inconclusive under the pre-registered rule, on all three readings below: every 95% interval includes zero, so it shows neither that the flags beat the devigged close at the bookmaker's price nor that they do not. The decision was recorded on 24 October 2026 and is shown on /experiments/soft-value.

- **The flags as they stood when 300 was first reached, on 17 October 2026 at 01:25 UTC.** This is the literal reading of the specification. Average closing line value at the bookmaker's own price: +0.49%, 95% interval −0.55% to +1.53%.
- **The first 300 flags after a later exclusion, a point reached on 17 October 2026 at 08:25 UTC.** On 18 October, after that point, we voided 84 flags whose benchmark was an exchange and not Pinnacle, 8 of them in the sample above. Rule v2 was meant to require a Pinnacle benchmark, but the specification has no such exclusion, so this reading is ours and not the specification's. It is the one the page shows: +0.57%, 95% interval −0.50% to +1.64%.
- **The same exclusion, the first 300 flags in order of detection, not of settlement:** +0.26%, 95% interval −0.84% to +1.36%.

**How to check the first two.** Take the flags with `value_origin` = live, `value_recorded_at_utc` from 2026-10-05 07:20:00 UTC on and a `value_realizable_clv_pct`, order them by `kickoff_utc`, keep the first 300 and average that column. `data/track-2026-10-18T04-40-01Z.json` gives +0.50%, ±1.04; `data/track-2026-11-06T05-40-01Z.json` gives +0.57%, ±1.07. The file rounds each value, so the last digit can differ from the figures above.

Rule 9 asks that every flag be listed publicly. The page lists the latest 100; all of them are in the daily snapshots, in the `value_*` columns. Flags settled after the decision point are descriptive and do not change the decision. The result of a flag does not enter this measure, so the corrections in the entry above do not change it.

## 2026-10-27 — "Betfair" prices from two feeds (repaired on 2026-10-15)

Until 15 October 2026 one of our odds feeds stored Betfair Sportsbook prices under our Betfair
Exchange entry (named `Betfair` in the margin files). On that day we moved them (103,298 price rows on 4,159 matches) to a separate
Betfair Sportsbook entry and recomputed the bookmaker margins of the matches involved. What it
changes in material we had published:
- In the flag rule v2 note (2026-09-30, below), 28 of the 550 backtest flags counted as
  exchange-priced were Betfair Sportsbook prices. Without them, exchange-priced flags had CLV
  −0.36% (±0.67, n=522), against −0.32% (±0.65, n=550) as published. The conclusion is unchanged.
- In the monthly CSV, `best_soft_*` values named `Betfair` mix Betfair Exchange and Betfair
  Sportsbook prices for rows logged up to 15 October 2026 (see `datasets/README.md`).
- In the daily margin files up to 15 October 2026, the `Betfair` entry (later
  `Betfair Exchange (EU)`) held both, so its margin (between 5.4% and 9.0%, depending on the day) was not an exchange's. From
  16 October 2026 it holds exchange prices only.

## 2026-10-09 — TIPS-EXP-007 registered

**TIPS-EXP-007 registered** (`TIPS-EXP-007.md`, SHA-256
fcc30e8b5f02a236a588dee42e858023c274420a8c4a3d27104c2c33771675e2). A paper test of value flags on
goal markets (over/under on half-goal lines and both teams to score): soft-bookmaker prices 5% to
12% above Pinnacle's devigged price on the same line, one flag per match, measured by closing line
value against the devigged Pinnacle close. Flags count from the timestamp of this commit; decided
on the first 300 flags with a close or on 2027-03-31. Flags are listed only after kick-off. It runs
in parallel with TIPS-EXP-005 and TIPS-EXP-006, which are unchanged.

## 2026-10-05 — TIPS-EXP-006 registered; how TIPS-EXP-005 actually executes

**TIPS-EXP-006 registered** (`TIPS-EXP-006.md`, SHA-256
7a5b74695fe5fdb0a5b838cdf707df6e4f8ecc354dcabd813c0cdec2ec85e492). A paper test of the public
value flags (rule flag-v2) at the soft bookmaker's own price, from 2026-10-05 07:20 UTC, decided at
300 takeable flags or on 2026-12-31. It runs in parallel with TIPS-EXP-005, which is unchanged.

**TIPS-EXP-005, two executor behaviours its spec does not state.** Both have applied since its first
day and stay unchanged until it ends, so its rules do not change mid-test:
- between 2.5 and 3 hours before kick-off the executor requires 2 percentage points more edge than
  rule 4 states (a setting inherited from TIPS-EXP-003's executor);
- the half-Kelly stake (rule 5) is computed from the soft-bookmaker gap and price that triggered the
  signal, not from the Betfair edge at execution.

**Daily exposure cap.** The 20%-per-day cap compared days in server time (UTC+3) instead of UTC
until today; it now uses UTC days. No TIPS-EXP-005 bet was affected: its largest daily exposure so
far is 22 units against a cap of 200.

**Strategy Board.** From 30 September to 5 October the public Strategy Board and the admin summary
still showed TIPS-EXP-003 instead of the running TIPS-EXP-005. Fixed; the AutoBet page itself was
correct. Returns there are now shown only from 100 settled bets, as everywhere else.

## 2026-09-30 — backtest repaired, realizable CLV published, flag rule v2, TIPS-EXP-005

**Backtest repaired.** The reconstruction of value flags before 26 September 2026 had two
fidelity errors. It evaluated prices when a quote arrived but stamped the flag at the next
30-minute refresh, ignoring quotes that arrived in between. It also read `logged_at` before
2026-07-28 as UTC, although it was server time (UTC+3). Both are fixed, and the backtest was
re-run over the same matches: of 3,220 backtest flags, 2,919 are unchanged, 276 changed
(price, bookmaker, selection or time), 25 were withdrawn (kept in the database as
`flag-v1-old`, `replay_corrected`) and 3 were added. Changed rows have new seals; the previous
values remain in earlier snapshots.

**Realizable CLV.** A flag's CLV compares its price with the devigged Pinnacle close. That price
could be hours old when the flag appeared. Next to it we now publish realizable CLV: the first
price the same bookmaker quoted on the same selection after the flag was recorded, within 60
minutes and before kick-off, against the same close. Flags with no such quote are left out and
the coverage is reported. After the repair (95% intervals):
- backtest flags: CLV at signal +1.85% (±0.27, n=3,199); realizable +0.79% (±0.54), coverage
  932 flags (29%);
- live flags since 26 September: +0.50% at signal (n=16); realizable −0.07% (n=6) — too few to read;
- live Value Alerts (all candidates): +1.68% at signal (n=54); realizable +0.48% (n=21).
Most of the backtest's CLV at signal came from prices that did not survive the next quote.

**Flag rule v2** (from 2026-09-30 18:09 UTC). Exchange prices (Betfair Exchange, Matchbook) are
no longer treated as a soft bookmaker's best price: in the backtest they had CLV −0.32% (±0.65,
n=550) even before commission, against +2.29% (±0.30, n=2,649) at soft bookmakers. A flag now
needs a soft price and a Pinnacle price seen within the last 90 minutes. Flags recorded earlier
keep their rule (`flag-v1`); new ones are `flag-v2`. Value Alerts become `va-v2` (Betfair
Exchange also excluded). A gap seen only on older prices is shown as "not re-checked", not as
value and not as expired.

**AutoBet.** TIPS-EXP-003 is closed as recorded (4 settled bets): the rule change would have
altered its trigger mid-test. TIPS-EXP-005 is registered with the new trigger (`TIPS-EXP-005.md`,
SHA-256 a1ece343cecdf81b055a34e385faf401603a82a2f0bf33f40155baa13950fb05).

## 2026-09-30 (erratum) — the 14 "late" predictions were logged before kick-off; void withdrawn

The entry below was wrong about the 14 predictions. Until 27 July 2026 our pipeline stored
`logged_at` in server time (UTC+3), not UTC. Read correctly, all 14 were logged 36 to 156 minutes
before kick-off. Their void was withdrawn the same day and their results count again, as before.
The 29 September note that called 13 of them "logged after kick-off" was wrong for the same reason.
For every row logged before 2026-07-28, `logged_at_utc` is server time: subtract 3 hours.
`logged_before_kickoff` now accounts for this. The part of the entry below about the 8 replay
flags stands.

## 2026-09-30 — late predictions voided; replay flags on unplayed matches superseded

- 14 predictions were written after kick-off by pipeline errors (13 on 2026-07-25, one on
  2026-07-15; 13 of them are in this export). They are now void (`void_reason =
  logged_after_kickoff`): their result is no longer scored and they are excluded from every figure.
  The rows stay in the export with `logged_before_kickoff = 0`.
- 8 value flags reconstructed on 2026-09-26 sat on matches that had not been played yet, which
  blocked recording the live flag for those matches. They were withdrawn before kick-off (kept in
  the database as `superseded_live`, not deleted), so those matches are flagged live like any other.

## 2026-09-30 — TIPS-EXP-003: proposal step aligned with rule 1

Rule 1 of `TIPS-EXP-003.md` covers every public value flag. The step that turns flags into
proposals only picked up matches that already had a Pinnacle price when the prediction was logged.
Flags on matches where Pinnacle opened later (shown on the site from the live Pinnacle price) were
never proposed and are missing from the EXP-003 ledger — for example matches 7160 and 7648 on
28 September 2026. From 30 September 2026 they are proposed like every other flag; rule 2 (fresh
live fair price, otherwise skipped) applies to all of them. The frozen specification is unchanged,
and the missed signals are not added retroactively.

## 2026-09-29 — fields added, claims made precise

- Four fields were added at the end of every row: `value_origin` (`live` | `replay`),
  `value_recorded_at_utc`, `value_sealed_before_kickoff` and `logged_before_kickoff`.
  Existing columns keep their positions.
- The note in each snapshot now states exactly what the row `hash` covers: id, match_id, league,
  home, away, kickoff_utc, our model probabilities, the Pinnacle 1X2 prices at logging and
  logged_at. It does not cover closing prices, results or any `value_*` field.
- 13 predictions logged on 2026-07-25 were written after kick-off because of a pipeline error.
  They were in every snapshot since; they are now marked `logged_before_kickoff = 0`.
- When Pinnacle had no price at logging, the first Pinnacle price quoted after logging and before
  kick-off is filled in later, sometimes after the match. Such a row first appears in the export
  (and here) only once that price is filled in. The row `hash` includes those prices.

## 2026-09-26 — value-flag fields recomputed; flags before this date are a reconstruction

**What changed.** Between snapshots `data/track-2026-09-26T04-40-01Z.json` and
`data/track-2026-09-27T04-40-01Z.json` the `value_*` fields of 3,519 settled rows changed:
1,468 rows gained a value flag, 449 lost one, and the rest changed price, bookmaker or result.
The prediction fields and the `hash` of every row did not change, because that hash has never
covered the `value_*` fields.

**Why.** Until then, flags on settled matches were scored at the highest soft price logged for
the match (before 21 September 2026, the highest price ever seen across all bookmakers, not
necessarily one available at a given moment), and closing line value was measured against
Pinnacle's raw closing odds, margin included. That overstated the result: the site showed
+7.9% average CLV.

**What the values are now.** Every flag before 26 September 2026 was reconstructed on that day by
applying the current display rule to the prices stored before each match (`value_origin =
replay`). This is a backtest, not a record of what the site showed at the time. Its seal
(`value_commitment`) was created at reconstruction, almost always after kick-off
(`value_sealed_before_kickoff = 0`), so it only shows that the reconstruction has not changed
since it was first published (snapshot 2026-09-27T04-40-01Z). Flags recorded live, when the site
shows them, start on 26 September 2026 (`value_origin = live`): the first flag on each match is
recorded, with its seal, before kick-off (`value_recorded_at_utc`). The seal appears in the public
export once the match's prediction row is published, and this log timestamps it externally only when
a daily snapshot (07:40 Bucharest time: 04:40 UTC in summer time, 05:40 UTC in winter) falls between
the recording and kick-off.
