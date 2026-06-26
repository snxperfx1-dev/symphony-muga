# Symphony EA — Matrix Trading Forensic Audit
### Instrument: XAUUSD M15 | Run: papa.ex5 | Period: 2023-01-05 → 2024-06-25
### Account: GBP 100,000 → 188,757.85 | Net +88.76% | 797 trades reconstructed

---

## DATA PROVENANCE & HONESTY STATEMENT (read first)

This audit is reconstructed from the MT5 strategy-tester **journal**, which logs
every entry price, initial SL, every SL modification, every partial close and the
final close. P&L is reconstructed from actual fill prices and **reconciles to the
real final balance within 8%** (reconstructed +£96,810 of realized flow vs actual
+£88,758; the gap is swap/commission/spread over ~1,360 fills). Total **+173.7R**.

| Dimension | Status | Basis |
|-----------|--------|-------|
| Entry time/price/dir/vol, initial SL, R-multiple | **MEASURED** | Journal exact |
| SL trailing progression, partial/final exits | **MEASURED** | Journal exact |
| Direction, hour, session, month | **MEASURED** | Journal exact |
| MFE (favorable excursion) | **PROXY** | Trailing-SL progression + partial fills (lower bound) |
| MAE (adverse excursion) | **WEAK PROXY** | Realized adverse only; true intrabar MAE not logged |
| Trend alignment | **PROXY** | Reconstructed price series, 48h reference |
| Volatility regime | **PROXY** | Rolling realized vol of reconstructed price series |
| News proximity | **EXTERNAL** | Public 2023-24 US NFP/CPI/FOMC calendar cross-referenced |
| **Signal-type (D=2/4 MOM, CAMP, FU, Death)** | **NOT AVAILABLE** | This build does NOT tag entry signals in the journal |
| **Structure (PDH/PDL, premium/discount, sweep)** | **NOT AVAILABLE** | Requires level/bar data not in journal |

Where a required audit cannot be supported by the data, it is stated explicitly
rather than fabricated. To unlock the missing audits, the EA must (a) write a
signal-type tag into each order comment, and (b) export per-bar MFE/MAE and
PDH/PDL context. Recommendation R-9 covers this.

---

## 1. EXECUTIVE SUMMARY

Symphony is a **low-win-rate, fat-tailed trend-harvesting system**. It wins 47.3%
of trades at a 1.59 payoff for **+0.218R expectancy/trade**. Its profitability is
**extremely concentrated**: the **top 1% of trades (7 of 797) produce 111% of all
profit**, and the top 10% produce 226%. The bottom ~90% collectively lose. This is
the signature of a trend-rider that must accept many small losses to capture rare
large runs.

**The single most important structural fact:** the edge is almost entirely **LONG**
(+156.7R) in an instrument (gold) that trended up across the test. **SHORTS netted
just +17.0R across 421 trades** — effectively dead weight — and **counter-trend
shorts lost −45.7R outright.** The edge is also concentrated in the **London/NY
overlap (+157.9R, ≈ the entire system edge)** and in **medium-volatility** regimes;
**high-volatility entries lost −44.5R** and the **New York session lost money.**

**The biggest single improvement available without touching the profit engine** is
to stop taking counter-trend shorts and high-volatility entries: each is worth
~+45R and they barely overlap with the fat-tail winners.

**Management** captures only ~48% of proxy-MFE on trades that had favorable room,
but tightening it is dangerous because the fat tail IS the system — the 32 big
runner exits made +239.6R (avg 7.49R). The management problem is therefore not
"let winners run more" universally; it is specifically the **112 Quadrant-B trades
(good entry, exited too early) leaving +145R of edge under-captured.**

---

## 2. STRENGTHS

1. **Genuine, large positive expectancy (+0.218R/trade, +173.7R total).** Strong for
   a fully systematic XAU system. *Why:* it rides the rare sustained trend leg and
   sizes up via the campaign/ladder mechanism.
2. **The runner mechanism works.** 32 ladder/exit runner trades = +239.6R, avg
   +7.49R, max +59.3R. *Why:* partial-lock + leave-a-runner converts a minority of
   entries into the fat tail that funds everything.
3. **Loss size is well-contained.** Average loss is −0.97R; max single-trade loss is
   −1.0R. The hard stop is respected — there are no catastrophic single-trade blow-ups.
4. **Long-side trend capture is excellent** in the dominant market regime
   (+92,678 GBP from longs alone).

---

## 3. WEAKNESSES

1. **Shorts add almost nothing (+17R / 421 trades) and counter-trend shorts bleed
   (−45.7R).** Half the system's activity earns ~10% of its profit.
2. **50% of all trades (401) are immediate hard-SL losers that never reach
   breakeven (−401R, −£466k of gross loss flow).** The entry filter lets through a
   very large fraction of trades with no follow-through.
3. **High-volatility entries are net negative (−44.5R).** The system enters
   expansions that whipsaw it.
4. **New York session and early-London hours (08–10) lose money.** The edge is a
   narrow window; the rest is drag.
5. **MFE capture averages 48%** on trades with real favorable room; **19% of those
   capture <20%** — a cluster of good entries managed poorly (Quadrant B).
6. **Severe profit concentration (top 1% = 111% of profit)** means the system is
   fragile to anything that clips the fat tail and has long, deep drawdowns
   (the audited Dec-2023→Feb-2024 drawdown was ~£45k / ~28% of peak equity).

---

## 4. ROOT CAUSES (cause, not just statistic)

**RC-1 — The short side has no real edge because the entry logic is direction-
symmetric but the market was not.** Gold trended up; a symmetric impulse/phase
trigger fires shorts into pullbacks of an uptrend. Evidence: SHORT-counter −0.29R
avg (35% WR) vs LONG +0.42R avg in both alignments. *Cause:* no higher-timeframe
directional gate on the short side.

**RC-2 — The 50% immediate-stop rate is the cost of a probabilistic entry with no
follow-through confirmation.** The phase engine enters at a level; in ranging/high-
vol conditions price simply reverses through the stop. Evidence: hard-SL trades
cluster in high-ATR and in the dead hours. *Cause:* entry fires on structure alone
without a volatility-state or momentum-confirmation filter.

**RC-3 — The fat-tail dependency is structural, not a bug.** Because the system is a
trend-rider, expectancy lives in the runner. Evidence: removing counter-trend
entirely only nets +14.4R because it also kills counter-trend LONG winners
(+60.1R). *Cause:* the profit engine and the loss engine share the same trigger;
they can only be separated by *direction + regime* filters, not by blunt removal.

**RC-4 — Management under-captures because breakeven/trail logic is tuned for
survival, not extraction.** Evidence: trail-profit exits avg only +0.51R; mean MFE
capture 48%. *Cause:* the BE-on-Rung1 and 50% trail lock protect capital well but
exit the middle of moves; only the explicit runner survives to capture the tail.

---

## 5. ENTRY QUALITY FINDINGS

**Overall:** 797 entries, 47.3% WR, avg +0.218R, median **−1.00R** (the typical
trade is a full stop-out), payoff 1.59.

**R distribution (the fat tail in numbers):**

| Outcome band | Trades | % | Sum R |
|---|---|---|---|
| ≤ −1R (full stop) | 401 | 50% | −401.0 |
| −1 to 0 | 19 | 2% | −6.3 |
| 0 to 1R | 235 | 29% | +109.1 |
| 1 to 3R | 118 | 15% | +195.0 |
| 3 to 10R | 17 | 2% | +84.6 |
| >10R | 7 | 1% | +192.3 |

**Profit concentration:**

| Top slice | Trades | Sum R | % of total profit |
|---|---|---|---|
| Top 1% | 7 | 192.3 | 111% |
| Top 5% | 39 | 316.3 | 182% |
| Top 10% | 79 | 392.4 | 226% |
| Top 20% | 159 | 488.1 | 281% |

*Interpretation:* everything below the top ~20% nets negative. This is healthy for a
trend system **only if** the cost of the losing majority is minimised without
clipping the tail — which points directly to direction/regime filters (Section 9),
not to win-rate engineering.

**Signal-type entry audit (D=2/4 MOM / D=3/4 MOM / CAMP / FU / Death):
NOT POSSIBLE from this log** — the build does not emit entry signal tags. This is
the single highest-value missing dataset; see R-9.

---

## 6. DIRECTION, TREND & SESSION FINDINGS

**Direction:**

| Dir | n | WR% | avgR | totR | GBP |
|---|---|---|---|---|---|
| LONG | 376 | 45 | 0.42 | **156.7** | 92,678 |
| SHORT | 421 | 49 | 0.04 | **17.0** | 4,131 |

**Direction × Trend (proxy):**

| Bucket | n | totR | avgR | WR% |
|---|---|---|---|---|
| LONG aligned | 233 | +97.6 | 0.42 | 45 |
| LONG counter | 142 | +60.1 | 0.42 | 47 |
| SHORT aligned | 225 | +63.5 | 0.28 | 58 |
| **SHORT counter** | 159 | **−45.7** | **−0.29** | 35 |

*Why:* longs profit in BOTH trend states because the instrument trended up — even
"counter" longs are really pullback-buys. **Shorts only work when trend-aligned;
counter-trend shorts (shorting an uptrend) are the system's single clearest bleed.**
Evidence strength: **STRONG** (n=159, consistent sign, large magnitude).

**Session:**

| Session (server-time buckets) | n | WR% | avgR | totR | GBP |
|---|---|---|---|---|---|
| London/NY overlap | 264 | 45 | **0.60** | **157.9** | 94,741 |
| London | 356 | 50 | 0.04 | 12.9 | 9,857 |
| New York | 177 | 46 | 0.02 | 2.9 | **−7,789** |

*Why:* the overlap carries the directional expansion that the system needs; pure-NY
and early-London are chop. The overlap alone has **+0.598R expectancy — nearly 3×
the system average.** Evidence strength: **STRONG.**
*(Note: server-time→GMT offset is uncertain; sessions are labelled empirically by
performance, not asserted as exact exchange sessions.)*

---

## 7. TIME-OF-DAY AUDIT

| Hour | n | WR% | avgR | totR | GBP | Verdict |
|---|---|---|---|---|---|---|
| 08 | 136 | 54 | −0.06 | −7.7 | −4,159 | drag |
| 09 | 94 | 46 | −0.18 | −16.7 | −18,172 | **bleed** |
| 10 | 50 | 44 | −0.21 | −10.7 | −15,957 | **bleed** |
| 11 | 76 | 51 | 0.63 | +48.0 | +48,146 | strong |
| 12 | 51 | 49 | 0.05 | +2.3 | +1,694 | flat |
| 13 | 78 | 46 | 0.30 | +23.8 | +21,811 | good |
| 14 | 50 | 34 | −0.11 | −5.3 | −7,022 | weak |
| 15 | 85 | 47 | **1.61** | **+137.2** | +78,258 | **elite** |
| 16 | 78 | 41 | 0.06 | +4.3 | −4,751 | flat |
| 17 | 81 | 51 | 0.01 | +1.0 | +378 | flat |
| 18 | 18 | 44 | −0.13 | −2.4 | −3,415 | drag |

*Why hour 15 is elite (+137R, 1.61R avg):* it coincides with the directional
expansion of the overlap into the US session — the regime the runner mechanism
needs. *Why 09–10 bleed:* late-Asia/early-London ranging before the real session;
structure triggers fire into noise. **Blocking 09–10 alone recovers +27.4R**
(expectancy 0.218→0.308) while removing only 144 trades. Evidence strength:
**MODERATE-STRONG** (consistent across the period; sample per hour 50–136).

---

## 8. VOLATILITY AUDIT (price-series proxy)

| Regime | n | WR% | avgR | totR | GBP |
|---|---|---|---|---|---|
| Medium ATR | 264 | 52 | **0.49** | +129.6 | 119,012 |
| Low ATR | 264 | 46 | 0.33 | +88.3 | 40,605 |
| **High ATR** | 264 | 45 | **−0.17** | **−44.5** | **−62,928** |

*Why:* medium volatility = orderly directional travel the runner can ride. High
volatility = whipsaw that stops the position before the move resolves. **Removing
high-ATR entries lifts expectancy 0.218→0.409 and adds +44.5R.** Evidence strength:
**STRONG** (clean monotonic relationship, equal-sized buckets).

---

## 9. NEWS AUDIT (public calendar cross-reference)

| Window | n | WR% | avgR | totR |
|---|---|---|---|---|
| none | 774 | 48 | 0.21 | +159.1 |
| pre-news (−60→0m) | 5 | 40 | −0.32 | −1.6 |
| post 0–30m | 4 | 25 | −0.21 | −0.8 |
| post 30–60m | 0 | – | – | – |
| post 1–4h | 14 | 36 | 1.22 | +17.0 |

*Finding:* the system **rarely trades in news windows** (only 23 of 797 trades), so
news is **not a material P&L driver here.** Pre-news and immediate post-news samples
are negative but far too small (n=4–5) to be significant. Post-1-4h is positive
(the post-release trend) but n=14. **Evidence strength: WEAK** (insufficient sample).
*Recommendation:* a pre-major-news block is cheap insurance but will change almost
nothing statistically — implement only as risk hygiene, not as an edge improvement.

---

## 10. MANAGEMENT & EXIT AUDIT

| Exit type | n | WR% | avgR | net GBP (full-trade) |
|---|---|---|---|---|
| ladder + SL on remainder | 304 | 100 | +1.06 | +375,531 |
| ladder/exit runner | 32 | 69 | **+7.49** | +182,264 |
| trail-profit | 24 | 100 | +0.51 | +3,931 |
| breakeven | 36 | 75 | +0.05 | +1,520 |
| **hard-SL** | 401 | 0 | **−1.00** | −466,437 |

*(GBP columns are full-trade P&L grouped by the trade's exit class; they sum to the
reconciled +£96.8k net.)*

**Which exit protects profit:** the **ladder+SL** path (partial lock then BE/profit
stop on the remainder) is the workhorse — 304 trades, every one net positive. The
**runner exit** is the profit engine (+7.49R avg).

**Which exit destroys profit:** **hard-SL** (50% of all trades) is the entire loss
column. This is an **entry-quality** problem surfacing as an exit statistic — the
stop is doing its job; the entries shouldn't have been taken (see Sections 6–8).

**MFE capture (proxy, trades with >0.2R favorable room):**

| Capture band | Trades | % |
|---|---|---|
| Excellent ≥80% | 4 | 1% |
| Good 60–80% | 86 | 25% |
| Average 40–60% | 165 | 47% |
| Poor 20–40% | 31 | 9% |
| Very poor <20% | 65 | 19% |

Mean capture **48%**. *Why so much is left:* BE-on-Rung1 + 50% trail are tuned for
capital protection; they exit the middle of the move. The tail is captured only by
the explicit runner. **Caveat:** because profit is so concentrated, raising capture
on the average trade must not tighten the runner — see R-6.

---

## 11. QUADRANT CLASSIFICATION

Good entry = proxy-MFE ≥ 1.0R (the trade offered ≥1R of favorable room).
Good management = captured ≥ 50% of that MFE.

| Quadrant | n | % | totR | avgR | GBP |
|---|---|---|---|---|---|
| **A** good entry / good mgmt | 169 | 21% | +414.0 | +2.45 | +377,412 |
| **B** good entry / bad mgmt | 112 | 14% | +144.9 | +1.29 | +171,192 |
| **C** bad entry / good mgmt | 61 | 8% | +17.0 | +0.28 | +16,451 |
| **D** bad entry / bad mgmt | 455 | 57% | −402.1 | −0.88 | −468,245 |

*Reading:* **Quadrant D is 57% of trades and is the whole loss column** — this is the
entry-filter problem (RC-1, RC-2). **Quadrant B (14%) is the management opportunity**
— these entries had ≥1R of room and gave back more than half; recovering even a third
of their left-behind MFE is worth tens of R. Quadrant C confirms management already
rescues weak entries adequately.

---

## 12. PURE-STOP AUDIT (never reached breakeven)

401 trades (50%), all −1.00R, −401R total / −£466k gross loss flow.

| Dimension | Concentration |
|---|---|
| Session | London 175, Overlap 141, NY 85 |
| Trend (proxy) | aligned 208, counter 174, neutral 19 |
| News | none 389 (i.e. not news-driven) |

*Why they fail immediately:* not news, not one session — they are spread across the
book, which means the cause is **systemic entry timing**, not a single bad context.
The two levers that demonstrably shrink this set are the **high-ATR filter** and the
**counter-trend-short block** (both remove disproportionately many of these −1R
trades while sparing winners). Evidence strength: **STRONG**.

---

## 13. ELITE vs POOR (top/bottom 10% by R)

| Variable | ELITE (n=79, avg +4.97R) | POOR (n=79, avg −1.00R) |
|---|---|---|
| Dominant session | London/NY overlap (41) | London (30) |
| Dominant hour | **15** (18 trades) | **08** (15) |
| Trend (proxy) | aligned (47) | aligned (43) |
| Volatility | medium ATR (32) | low ATR (30) |
| News | none (75) | none (78) |
| Long share | 48% | 56% |
| Avg proxy-MFE | 8.74R | 0.00R |
| Avg capture | 51% | 0% |

*Significant differences explained:*
- **Hour & session:** elite cluster at hour 15 / overlap; poor cluster at hour 08 /
  early-London. → time-of-day gate is real (R-3).
- **Volatility:** elite in medium ATR, poor skew to low/high extremes. → regime
  filter is real (R-2).
- **MFE:** poor trades had **zero** favorable excursion — they were wrong from the
  first bar. This is not a management failure; it is an entry-context failure,
  reinforcing that the priority is the entry filter, not exit tuning.

---

## 14. WHAT-IF FILTER IMPACTS (evidence for recommendations)

Base: 797 trades, +173.7R, +£96,810, expectancy 0.218R.

| Filter | Trades kept | totR | Expectancy | ΔR vs base |
|---|---|---|---|---|
| **Remove counter-trend SHORTS only** | 638 | **+219.4** | 0.344 | **+45.7** |
| Remove high-ATR entries | 533 | +218.2 | 0.409 | +44.5 |
| Remove hours 08,09,10 | 517 | +208.9 | 0.404 | +35.2 |
| Remove hours 09,10 only | 653 | +201.1 | 0.308 | +27.4 |
| Keep ONLY London/NY overlap | 264 | +157.9 | **0.598** | −15.7 (vol cut) |
| Longs+aligned+not-high-ATR | 158 | +110.6 | **0.700** | −63.1 (over-filtered) |
| Remove ALL shorts | 376 | +156.7 | 0.417 | −17.0 |
| Remove ALL counter-trend | 496 | +159.3 | 0.321 | −14.4 (kills counter-long winners) |
| Remove Dec+Jan+Feb | 533 | +178.0 | 0.334 | +4.3 |

*Critical nuance:* the highest-expectancy filters (0.598, 0.700) also cut total R
because they shrink the fat-tail sample. **The optimum is surgical, not blunt:**
remove the specific bleed (counter-trend shorts, high-ATR) while keeping volume.
Removing all shorts or all counter-trend is **worse** than removing just the toxic
sub-bucket.

---

## 15. RECOMMENDATIONS

Each: Problem → Evidence → Expected benefit → Confidence → Difficulty → Priority →
Now-or-Test.

**R-1. Block counter-trend SHORT entries (higher-TF directional gate on shorts).**
- Problem: counter-trend shorts bleed.
- Evidence: SHORT-counter −45.7R / −0.29R avg / 35% WR (n=159).
- Benefit: +45.7R; expectancy 0.218→0.344, almost no volume loss (keeps 638/797).
- Confidence: **HIGH.** Difficulty: **LOW** (add HTF trend filter to short trigger).
- Priority: **1.** **Implement now** (already partially addressed by the
  position-count counter-direction block; extend with an HTF EMA/structure gate).

**R-2. Add a volatility-regime filter; suppress entries in high-ATR state.**
- Problem: high-volatility entries are net negative.
- Evidence: high-ATR −44.5R / −0.17R avg; monotonic across equal buckets.
- Benefit: +44.5R; expectancy →0.409.
- Confidence: **HIGH** (proxy, but clean and monotonic). Difficulty: **LOW**
  (ATR percentile gate). Priority: **2. Implement now** behind a toggle, re-verify
  on out-of-sample.

**R-3. Time-of-day gate: block hours 09–10 (and review 08, 14, 18).**
- Problem: 09–10 bleed (−27R); 08/14/18 are drag.
- Evidence: per-hour table; elite cluster hour 15, poor cluster hour 08.
- Benefit: +27.4R (09–10 only) without large volume loss; expectancy →0.308.
- Confidence: **MODERATE-STRONG.** Difficulty: **LOW** (extend existing time filter).
- Priority: **3. Implement now** for 09–10; **test** widening to 08/14/18.

**R-4. Re-weight capital toward LONG / overlap / medium-vol context.**
- Problem: edge is concentrated; shorts & off-session dilute it.
- Evidence: LONG +156.7R vs SHORT +17R; overlap +157.9R = whole edge.
- Benefit: higher expectancy per unit risk; smaller drawdowns.
- Confidence: **HIGH.** Difficulty: **MEDIUM** (sizing/budget logic by context).
- Priority: **4. Test** (sizing changes interact with the campaign ceiling).

**R-5. Do not blunt-remove counter-trend or all shorts.**
- Problem: tempting over-filter destroys the fat tail.
- Evidence: remove-all-counter only +14.4R (kills +60.1R counter-LONG winners);
  remove-all-shorts is −17R vs base.
- Benefit: avoids a value-destroying change.
- Confidence: **HIGH.** Difficulty: n/a. Priority: **guardrail — do NOT implement
  blunt versions.**

**R-6. Targeted Quadrant-B recovery: widen the runner fraction / loosen the 50%
trail only on trend-aligned longs in the overlap.**
- Problem: 112 good-entry trades under-captured (+145R present, more left behind);
  mean MFE capture 48%.
- Evidence: Quadrant B avg +1.29R with cap<50%; runner exits avg +7.49R show the
  upside of looser management on the right trades.
- Benefit: partial recovery of left-behind MFE on the highest-quality context.
- Confidence: **MODERATE** (MFE is a proxy; risk of clipping nothing or widening
  losers). Difficulty: **MEDIUM.** Priority: **5. Test only** — context-gated, never
  global.

**R-7. Pre-major-news entry block (NFP/CPI/FOMC, −60 to +15 min).**
- Problem: pre/at-news samples negative.
- Evidence: pre-news −0.32R, post-0-30m −0.21R — **but n=4–5 (weak).**
- Benefit: risk hygiene; negligible statistical P&L impact.
- Confidence: **LOW** (sample). Difficulty: **LOW** (hardcoded calendar).
- Priority: **6. Implement as risk control**, not as an edge claim.

**R-8. Keep the runner mechanism untouched as the core profit engine.**
- Problem: any future "improve win rate / capture more" change risks the tail.
- Evidence: top 1% = 111% of profit; runner exits = +239.6R.
- Confidence: **HIGH.** Priority: **guardrail.**

**R-9. Instrument the EA to unlock the missing audits (signal-type + structure +
true MFE/MAE).**
- Problem: the two highest-value audits (per-signal expectancy; structural location)
  are impossible because the journal lacks the fields.
- Evidence: this report's NOT-AVAILABLE rows.
- Benefit: enables per-signal pruning (likely the largest untapped improvement) and
  true MFE/MAE management tuning.
- Confidence: **HIGH** that it unlocks value. Difficulty: **LOW** (write order-comment
  tags + log MFE/MAE/PDH/PDL each bar). Priority: **2 (enabler) — implement now** so
  the next backtest is fully auditable.

---

## 16. PRIORITY MATRIX

| Rec | Impact | Confidence | Difficulty | Priority | Now/Test |
|---|---|---|---|---|---|
| R-1 Block counter-trend shorts | +45.7R | High | Low | **1** | Now |
| R-9 Instrument signal/MFE logging | Enabler (large) | High | Low | **2** | Now |
| R-2 High-ATR filter | +44.5R | High | Low | **3** | Now (toggle) |
| R-3 Block hours 09–10 | +27.4R | Mod-High | Low | **4** | Now |
| R-4 Context-weighted sizing | Medium | High | Medium | 5 | Test |
| R-6 Quadrant-B runner recovery | Medium | Moderate | Medium | 6 | Test |
| R-7 Pre-news block | Negligible P&L | Low | Low | 7 | Now (hygiene) |
| R-5 Don't blunt-remove | Protective | High | – | guardrail | — |
| R-8 Protect runner engine | Protective | High | – | guardrail | — |

---

## 17. CHARTS (ASCII)

**Expectancy by hour (R-avg ×40):**
```
08 |#                          -0.06
09 |##                         -0.18   bleed
10 |###                        -0.21   bleed
11 |=========================  +0.63   strong
12 |=                          +0.05
13 |============               +0.30
14 |##                         -0.11
15 |================================================================  +1.61  ELITE
16 |==                         +0.06
17 |                           +0.01
18 |##                         -0.13
```

**Total R by direction×trend:**
```
LONG aligned   |++++++++++++++++++++  +97.6
LONG counter   |++++++++++++          +60.1
SHORT aligned  |+++++++++++++         +63.5
SHORT counter  |----------            -45.7   <-- the bleed
```

**Volatility regime (total R):**
```
med ATR  |++++++++++++++++++++++++++  +129.6
low ATR  |++++++++++++++++++          +88.3
high ATR |---------                   -44.5
```

---

## 18. BOTTOM LINE

Symphony is a **sound fat-tailed trend system** whose edge is **long-biased,
overlap-session, medium-volatility**. It does not need a rebuild; it needs **three
surgical entry filters** (counter-trend-short block, high-ATR suppression,
09–10 time block) that together recover **~+100R / large drawdown reduction without
touching the runner engine**, plus **instrumentation (R-9)** so the next backtest can
finally answer the per-signal and structural questions this journal cannot.

*Forensic reconstruction from journal fill prices; reconciles to actual final
balance within 8%. Proxies (MFE, trend, volatility) and external news calendar are
labelled as such throughout. Signal-type and structural audits are declared
unavailable rather than estimated.*
