# Symphony EA (XAUUSD) — Full Backtest Log Audit Report
## Log: "Log 26 june .txt" | Test Period: 2023-01-05 to 2024-06-25
## EA: papa.ex5 | Initial Deposit: £100,000 GBP | Final Balance: £188,757.85 GBP | Net: +£88,757 (+88.76%)

---

## STRUCTURE OF THE LOG FILE

The file contains **multiple successive test sessions**. The relevant main run is the **final complete run** (lines ~18,403–30,979):
- **EA:** `papa.ex5` (Symphony v3.0)
- **Symbol:** XAUUSD M15
- **Period:** 2023-01-05 to 2024-06-25
- **Deposit:** £100,000 GBP, leverage 1:500
- **Final balance:** £188,757.85 GBP

Earlier sections of the log contain partial/abandoned runs of `papa.ex5` and `F16.ex5` / `F16 Rasta.ex5` with different parameters.

---

## 1. TIMELINE OF ALL TRADES — OVERVIEW BY MONTH

The EA opened **797 new positions** across the test (376 LONGs, 421 SHORTs), plus 314 ladder partial-closes, 92 SYM EXIT invalidation closes, and 338 BE-stop moves.



| Month    | New LONGs | New SHORTs | Ladder Exits | SYM EXIT | BE Moves | SL Losses | SL Wins | SL BE |
|----------|-----------|------------|--------------|----------|----------|-----------|---------|-------|
| 2023-01  | 19        | 30         | 18           | 7        | 27       | 20        | 22      | 2     |
| 2023-02  | 34        | 26         | 27           | 4        | 25       | 31        | 12      | 13    |
| 2023-03  | 8         | 4          | 6            | 3        | 7        | 3         | 2       | 5     |
| 2023-04  | 25        | 23         | 19           | 7        | 19       | 28        | 15      | 5     |
| 2023-05  | 16        | 29         | 13           | 11       | 15       | 22        | 12      | 10    |
| 2023-06  | 28        | 19         | 26           | 5        | 25       | 19        | 14      | 11    |
| 2023-07  | 29        | 22         | 18           | 6        | 18       | 31        | 10      | 9     |
| 2023-08  | 25        | 17         | 21           | 5        | 21       | 11        | 11      | 20    |
| 2023-09  | 19        | 28         | 24           | 2        | 23       | 18        | 8       | 18    |
| 2023-10  | 17        | 16         | 9            | 2        | 10       | 20        | 6       | 8     |
| 2023-11  | 33        | 30         | 24           | 2        | 28       | 20        | 13      | 24    |
| **2023-12** | **9**  | **39**     | **11**       | **2**    | **13**   | **18**    | **13**  | **18** |
| **2024-01** | **35** | **19**     | **25**       | **4**    | **24**   | **32**    | **16**  | **7**  |
| 2024-02  | 15        | 38         | 20           | 9        | 23       | 19        | 7       | 24    |
| 2024-03  | 8         | 0          | 2            | 5        | 1        | 0         | 1       | 3     |
| 2024-04  | 16        | 19         | 9            | 7        | 11       | 14        | 13      | 8     |
| 2024-05  | 18        | 40         | 23           | 7        | 27       | 24        | 17      | 13    |
| 2024-06  | 24        | 23         | 19           | 4        | 21       | 22        | 16      | 7     |

**Note:** "SL Win" = stop loss triggered but SL was raised/trailed above/below entry = profitable close. "SL Loss" = genuine loss (SL below entry for long, SL above entry for short). "SL BE" = stopped at or within 2 pips of entry.



### Key Trade Sequences (Selected December 2023 Positions)

| Date       | Dir   | Lots  | Entry   | Exit    | Exit Type                  | Approx P&L |
|------------|-------|-------|---------|---------|----------------------------|------------|
| 2023-11-23 | LONG  | 18.16 | 1992.67 | ~2070   | Trailing SL (Dec 4 profit) | +£1,100+  |
| 2023-11-23 | LONG  | 15.66 | 1992.77 | ~2070   | Trailing SL (Dec 4 profit) | +£970+    |
| 2023-12-04 | SHORT | 0.80  | 2072.20 | 2049-2045 | Ladder Rung1+2           | +small    |
| 2023-12-04 | SHORT | 0.75  | 2071.12 | 2049-2045 | Ladder Rung1+2           | +small    |
| 2023-12-04 | SHORT | 1.83  | 2068.28 | 2039-2045 | Ladder + BE SL            | +small    |
| 2023-12-05 | SHORT | 0.14  | 2034.66 | 2031.76 | SL hit (genuine loss ~-3p) | -small    |
| 2023-12-05 | SHORT | 0.12  | 2034.48 | 2031.67 | SL hit (genuine loss)      | -small    |
| 2023-12-08 | SHORT | 6.82  | 2029.51 | 2029.40 | BE SL (~0 pips)            | ~£0       |
| 2023-12-11 | SHORT | 1.61  | 1998.85 | 1998.40 | BE SL (~0 pips)            | ~£0       |
| 2023-12-11 | SHORT | 2.31  | 1995.09 | 1994.14 | BE SL (~0 pips)            | ~£0       |
| 2023-12-12 | SHORT | 3.02  | 1987.64 | 1987.54 | BE SL (0.1p)               | ~£0       |
| 2023-12-12 | SHORT | 2.68  | 1987.36 | 1987.23 | BE SL (0.13p)              | ~£0       |
| 2023-12-12 | SHORT | 2.24  | 1987.00 | 1986.29 | SL hit genuine (-0.71p)    | -small    |
| 2023-12-12 | SHORT | 1.16  | 1985.46 | 1985.23 | BE SL (0.23p)              | ~£0       |
| 2023-12-12 | SHORT | 1.30  | 1984.87 | 1984.35 | SL hit genuine             | -small    |
| 2023-12-12 | SHORT | 2.41  | 1986.75 | 1989.51 | SL hit genuine (-2.76p)    | -£52      |
| 2023-12-13 | SHORT | 1.32  | 1979.24 | 1983.22 | SL hit genuine (-3.98p)    | -£41      |
| 2023-12-14 | LONG  | 5.09  | 2032.65 | 2035.44 | Trailing SL (profit)       | +£107     |
| 2023-12-14 | LONG  | 2.77  | 2034.55 | 2028.50 | SL hit genuine (-6.05p)    | -£132     |
| 2023-12-14 | SHORT | 4.51  | 2037.60 | 2041.10 | SL hit genuine (-3.50p)    | -£124     |
| 2023-12-15 | SHORT | 1.71  | 2036.41 | 2026.78 | Trailing SL (profit)       | +£130     |
| 2023-12-15 | SHORT | 1.42  | 2034.48 | 2025.81 | Trailing SL (profit)       | +£97      |
| 2023-12-18 | SHORT | 11.66 | 2023.09 | 2022.72 | BE SL (~0 pips)            | ~£0       |
| 2023-12-18 | SHORT | 3.00  | 2022.49 | 2027.96 | SL hit genuine (-5.47p)    | -£129     |
| 2023-12-18 | SHORT | 4.63  | 2024.16 | 2027.87 | SL hit genuine (-3.71p)    | -£135     |
| 2023-12-18 | SHORT | 3.92  | 2023.39 | 2027.85 | SL hit genuine (-4.46p)    | -£138     |
| 2023-12-19 | SHORT | 2.79  | 2023.94 | 2029.81 | SL hit genuine (-5.87p)    | -£129     |
| 2023-12-19 | SHORT | 2.88  | 2024.12 | 2029.82 | SL hit genuine (-5.70p)    | -£129     |
| 2023-12-19 | SHORT | 3.03  | 2024.49 | 2029.81 | SL hit genuine (-5.32p)    | -£127     |
| 2023-12-20 | SHORT | 6.13  | 2041.38 | 2037.07 | Trailing SL (profit)       | +£210     |
| 2023-12-21 | LONG  | 8.11  | 2035.55 | 2033.39 | SL hit genuine (-2.16p)    | -£138     |
| 2023-12-21 | LONG  | 6.39  | 2035.83 | 2033.40 | SL hit genuine (-2.43p)    | -£122     |
| 2023-12-22 | SHORT | 9.42  | 2049.97 | 2051.68 | Near-BE SL (-1.71p)        | -£127     |
| 2023-12-22 | SHORT | 9.46  | 2049.98 | 2051.67 | Near-BE SL (-1.69p)        | -£126     |
| 2023-12-26 | SHORT | 6.93  | 2063.70 | 2063.70 | Pure BE SL (0 pips)        | ~£0       |
| 2023-12-26 | SHORT | 9.80  | 2064.26 | 2064.26 | Pure BE SL (0 pips)        | ~£0       |
| 2023-12-26 | SHORT | 7.27  | 2063.27 | 2063.27 | Pure BE SL (0 pips)        | ~£0       |
| 2023-12-26 | SHORT | 8.70  | 2063.63 | 2063.63 | Pure BE SL (0 pips)        | ~£0       |
| 2023-12-26 | SHORT | 5.51  | 2062.54 | 2062.54 | Pure BE SL (0 pips)        | ~£0       |
| 2023-12-26 | LONG  | 8.02  | 2062.34 | 2060.21 | SL hit genuine (-2.13p)    | -£134     |
| 2023-12-27 | SHORT | 14.90 | 2065.84 | 2066.89 | SL hit genuine (-1.05p)    | -£123     |
| 2023-12-27 | SHORT | 11.08 | 2065.46 | 2066.90 | SL hit genuine (-1.44p)    | -£126     |
| 2023-12-27 | SHORT | 6.82  | 2068.69 | 2070.91 | SL hit genuine (-2.22p)    | -£120     |
| 2023-12-28 | LONG  | 5.03  | 2075.13 | 2071.94 | SL hit genuine (-3.19p)    | -£126     |
| 2023-12-28 | LONG  | 4.77  | 2075.30 | 2071.93 | SL hit genuine (-3.37p)    | -£126     |
| 2023-12-28 | LONG  | 3.15  | 2076.66 | 2072.00 | SL hit genuine (-4.66p)    | -£116     |
| 2023-12-28 | SHORT | 2.79  | 2074.73 | 2080.06 | SL hit genuine (-5.33p)    | -£117     |

*P&L estimates use £7.87/pip/lot (XAUUSD at £/USD ~1.27 at time of test)*



---

## 2. ALL LOSING TRADES — CLASSIFICATION

Total losing SL events across full backtest: **352** (out of 765 total SL events)

Breakdown by cause:
- **Genuine original SL hit** (SL never moved from initial placement): price moved directly against the position and hit the hard stop.
- **BE stop above entry for LONG / below entry for SHORT** (SL was raised but then price reversed back through): position was profitable then gave back gains.
- **Near-BE SL** (SL within 1–3 pips of entry, lost only spread+slippage).

### Complete December 2023 Losing SL Events (18 genuine losses):

| Date/Time          | Ticket | Dir   | Lots | Entry   | SL Hit  | Loss (pips) | Est. Loss |
|--------------------|--------|-------|------|---------|---------|-------------|-----------|
| 2023-12-05 11:17   | #1374  | SHORT | 0.14 | 2034.66 | 2031.76 | -2.90p      | -£3.2     |
| 2023-12-05 11:17   | #1375  | SHORT | 0.12 | 2034.48 | 2031.67 | -2.81p      | -£2.7     |
| 2023-12-12 10:15   | #1388  | SHORT | 2.24 | 1987.00 | 1986.29 | -0.71p*     | -£5       |
| 2023-12-12 11:01   | #1392  | SHORT | 1.30 | 1984.87 | 1984.35 | -0.52p*     | -£4       |
| 2023-12-12 15:05   | #1395  | SHORT | 2.41 | 1986.75 | 1989.51 | **-2.76p**  | -£52      |
| 2023-12-13 12:55   | #1397  | SHORT | 1.32 | 1979.24 | 1983.22 | **-3.98p**  | -£41      |
| 2023-12-14 15:30   | #1405  | LONG  | 2.77 | 2034.55 | 2028.50 | **-6.05p**  | -£132     |
| 2023-12-14 16:32   | #1407  | SHORT | 4.51 | 2037.60 | 2041.10 | **-3.50p**  | -£124     |
| 2023-12-18 19:48   | #1419  | SHORT | 3.00 | 2022.49 | 2027.96 | **-5.47p**  | -£129     |
| 2023-12-18 19:48   | #1420  | SHORT | 4.63 | 2024.16 | 2027.87 | **-3.71p**  | -£135     |
| 2023-12-18 19:48   | #1421  | SHORT | 3.92 | 2023.39 | 2027.85 | **-4.46p**  | -£138     |
| 2023-12-19 14:40   | #1425  | SHORT | 2.79 | 2023.94 | 2029.81 | **-5.87p**  | -£129     |
| 2023-12-19 14:40   | #1426  | SHORT | 2.88 | 2024.12 | 2029.82 | **-5.70p**  | -£129     |
| 2023-12-19 14:40   | #1427  | SHORT | 3.03 | 2024.49 | 2029.81 | **-5.32p**  | -£127     |
| 2023-12-21 12:40   | #1436  | LONG  | 8.11 | 2035.55 | 2033.39 | **-2.16p**  | -£138     |
| 2023-12-21 12:40   | #1437  | LONG  | 6.39 | 2035.83 | 2033.40 | **-2.43p**  | -£122     |
| 2023-12-26 15:50   | #1459  | LONG  | 8.02 | 2062.34 | 2060.21 | **-2.13p**  | -£134     |
| 2023-12-27 16:52   | #1465  | SHORT | 6.82 | 2068.69 | 2070.91 | **-2.22p**  | -£120     |
| 2023-12-27 10:01   | #1461  | SHORT | 14.90| 2065.84 | 2066.89 | **-1.05p**  | -£123     |
| 2023-12-27 10:01   | #1462  | SHORT | 11.08| 2065.46 | 2066.90 | **-1.44p**  | -£126     |
| 2023-12-28 15:58   | #1467  | LONG  | 5.03 | 2075.13 | 2071.94 | **-3.19p**  | -£126     |
| 2023-12-28 15:58   | #1468  | LONG  | 4.77 | 2075.30 | 2071.93 | **-3.37p**  | -£126     |
| 2023-12-28 15:58   | #1469  | LONG  | 3.15 | 2076.66 | 2072.00 | **-4.66p**  | -£116     |
| 2023-12-28 17:01   | #1473  | SHORT | 2.79 | 2074.73 | 2080.06 | **-5.33p**  | -£117     |

> *Small values near 0 are borderline BE cases; the classification threshold is ±2 pips.*

**December 2023 estimated total gross SL losses: ~-£2,100**
**December 2023 estimated total SL gains (trailing stops): ~+£570**
**December 2023 net SL balance: ~-£1,530**
*(The month was still overall profitable due to large Nov carry-over longs closed profitably in Dec)*



---

## 3. DECEMBER 2023 — DETAILED ANALYSIS

### Price Context
- **XAUUSD range in December 2023:** ~1,979 – ~2,077 (from trade entries in log)
- **Key event:** XAUUSD made an all-time high near **~2,146** on Dec 1–4, 2023 (reported externally; log shows trades clustered at 2060–2076 in late November/early December suggesting a spike and reversal)
- The EA had been carrying **large LONG positions from late November** (entries near 1992–2011) with SLs raised to 2069–2070 via the `SYM BE moved` mechanism
- After the ATH spike these trailing stops were hit **profitably** on Dec 4 — generating **the biggest single-day profits of Q4 2023**
- After the ATH, XAUUSD then spent December **choppy, oscillating 1979–2080**, generating repeated whipsaw losses for SHORT entries

### What Was Happening in December 2023?

**Phase 1 — Dec 1–4 (ATH Spike):**
- Three large LONG positions from Nov (tickets 1349, 1350, 1351, opened ~2010–2012) hit their **trailing BE stops at 2069–2070** as price peaked then pulled back from ATH
- Each closed with **+58–59 pips** profit at 0.60–0.67 lots = **+£110–125 each, ~+£350 total**
- Simultaneously, EA entered fresh SHORTs at 2068–2072 (tickets 1365, 1366, 1367) seeing the ATH reversal

**Phase 2 — Dec 4–13 (Short Bias, Choppy Market):**
- EA had SHORT bias throughout, entering at progressively lower levels (2072 → 2034 → 1987 → 1979)
- Price dropped from ~2072 to ~1979 (correct direction!) BUT the EA's SLs were extremely tight
- **Dec 5:** Two small shorts stopped immediately at -2.9p (whipsawed)
- **Dec 6–7:** `Basket ceiling reached` — EA could not add positions because risk limit already maxed at **currentRisk=5,945** vs max ~5,000
- **Dec 7:** One trailing SHORT (#1367) closed at **+28.4 pips** profit
- **Dec 8:** 6.82L SHORT opened, stopped at BE immediately (+0.11p)
- **Dec 11–12:** Multiple SHORTs at 1984–1998, all stopped at BE or tiny losses. Price too choppy
- **Dec 12:** HEAVIEST single-day loss session — 6 SHORTs opened at 1984–1987, **SLs were tiny** (0.3–2.8 pips). All stopped out
- **Dec 13:** Short at 1979 stopped out at -3.98 pips. Then at 22:58, Dec 4 SHORTs (#1365, #1366) finally stopped out at 2022–2023 (massive **BE SL profit** of +49p, having run from Dec 4's entry at 2071 all the way down to 2022, then price rose back — stopped out at massive gain)

**Phase 3 — Dec 14 (Direction Flip, Counter-Direction Trap):**
- 10:00: LONG entered (#1401, 5.09L @ 2032.65)
- 10:15–10:30: Ladder exits taken, BE moved up to 2032.65
- 11:44: **LONG stopped at +2.79 pips** (raised SL — minor profit)
- 13:15: Another LONG entered (#1405, 2.77L @ 2034.55)
- **15:30: LONG #1405 stopped at -6.05p = -£132 (genuine loss)**
- **16:30: SHORT entered (#1407, 4.51L @ 2037.60) — COUNTER DIRECTION while LONG still conceptually active**
- **16:32: SHORT #1407 stopped at -3.50p = -£124 (genuine loss — stopped in 2 minutes!)**
- 16:45: `SYM EXIT: mode-invalidation-at-peak triggered short exit phaseAtInvalid=3`
- Dec 14 alone: two genuine losses totaling **~-£256**

**Phase 4 — Dec 15–18 (Short Re-entry, Price Spikes Up):**
- Dec 15: Two SHORTs at 2034–2036 (ladder exits taken profitably, BE moved)
- Dec 18: 11.66L SHORT opened at 2023.09 — stopped at BE immediately (spread only)
- Then 3 more SHORTs at 2022–2024 with SLs at 2027–2028
- **Dec 18 19:48 — Triple simultaneous SL sweep:** All three SHORTs (#1419, #1420, #1421) hit SL at the **same second** at 2027–2028. Price spiked UP through all of them
- **Combined loss: ~-£402 in a single second**

**Phase 5 — Dec 19 (Immediate Re-entry, Immediate Re-loss):**
- Within hours of the Dec 18 losses, EA re-entered with 3 more SHORTs at 2023–2024
- `SYM: Basket ceiling reached` briefly blocked entries
- **Dec 19 14:40 — Triple simultaneous SL sweep again:** #1425, #1426, #1427 all stopped at 2029–2030
- **Combined loss: ~-£385 in one second**
- Pattern: same entry zone (2022–2024), same SL zone (2029–2030), same outcome

**Phase 6 — Dec 20–21 (Position Size Escalation):**
- Dec 20 11:30: 6.13L SHORT @ 2041 (ladder exits taken, partial profit, BE set)
- Dec 20 17:45: **2.95L LONG opened while SHORT still running** — COUNTER-DIRECTION
- Dec 20 16:00: SHORT #1431 hit trailing SL at 2037.07 (+4.31p = +£183 profit)
- Dec 21 12:00: **TWO HUGE LONGS added simultaneously** (#1436 8.11L, #1437 6.39L @ 2035–2036)
- 12:40: Both stopped within **40 seconds** at -2.16p and -2.43p = **-£260 combined**

**Phase 7 — Dec 22–29 (Holiday Period Chaos):**
- Dec 22: Two 9.4L SHORTs opened, both hit SL at 2051.68 within 42 minutes = **-£127 + -£126**
- Dec 26: 5 SHORTs entered (6.93–9.80L each), ALL hit BE stops (breakeven — spread loss only, but 5 positions × large lots = opportunity cost)
- Dec 26 also: LONG 8.02L entered and stopped at -2.13p = **-£134**
- Dec 27: Two massive SHORTs (14.90L + 11.08L!) opened — both stopped within 76 minutes = **-£249**; third SHORT 6.82L stopped same afternoon = -£120
- Dec 28: Three LONGS (5.03+4.77+3.15L) opened, ALL stopped in simultaneous SL sweep = **-£368**. Then SHORT re-entered, stopped at -5.33p = -£117
- Dec 29: `SYM EXIT: mode-invalidation triggered` — EA recognized the chaos and exited remaining short bias

### December 2023 Summary

| Metric | Value |
|--------|-------|
| New SHORT positions opened | 39 |
| New LONG positions opened | 9 |
| Genuine losing SL events | 18 |
| Breakeven SL events | 18 |
| Profitable trailing SL events | 13 |
| Days with mass SL (3+ simultaneous) | 5 (Dec 18, 19, 21, 27, 28) |
| Worst single loss event | Dec 18: -£402 (triple simultaneous SL) |
| Basket ceiling blocks | 5 separate days |
| SYM EXIT events | 2 |
| **Net estimated SL P&L** | **~-£1,500** |



---

## 4. COUNTER-DIRECTION TRADES — IDENTIFIED CASES

The EA operates in **hedge mode** (MT5 position netting off), meaning it can hold simultaneous LONG and SHORT positions. The log shows **83 calendar days** across the backtest where both new BUY and SELL positions were opened on the same calendar day.

In December 2023 and January 2024, the following counter-direction trade instances are confirmed:

### December 2023 Counter-Direction Cases

**Dec 14 2023:**
- 10:00 — LONG #1401 opened (5.09L @ 2032.65)
- 13:15 — LONG #1405 added (2.77L @ 2034.55)
- **16:30 — SHORT #1407 opened (4.51L @ 2037.60) while two LONGs still active**
- Result: SHORT stopped in 2 minutes (-£124), LONG #1405 also stopped (-£132)

**Dec 20–21 2023:**
- Dec 20 11:30 — SHORT #1431 opened (6.13L @ 2041.38)
- **Dec 20 17:45 — LONG #1435 opened (2.95L @ 2034.20) while SHORT still running**
- **Dec 21 12:00 — TWO more LONGs added (#1436 8.11L + #1437 6.39L) while SHORT #1431 still open**
- Result: SHORT #1431 eventually stopped at profit (+4.31p). LONGs #1436+#1437 stopped in 40 seconds (-£260)

**Dec 26 2023:**
- Multiple SHORTs open (26 collective lots across 5 positions)
- **15:30 — LONG #1459 opened (8.02L @ 2062.34) while SHORTs still running**
- Result: LONG stopped at -2.13p = -£134

**Dec 28 2023:**
- Three LONGs opened 14:30–15:00 (5.03+4.77+3.15L)
- **16:45 — SHORT #1473 opened (2.79L @ 2074.73) while LONGs still active**
- Result: SHORT stopped at -5.33p = -£117; LONGs all stopped earlier at -£368

### January 2024 Counter-Direction Cases

**Jan 5 2024:**
- Three LONGs opened 09:00–11:15 (6.86+6.60+5.77L)
- **16:45 — SHORT #1500 opened (3.90L @ 2044.60) while LONGs active**
- Result: SHORT stopped at -3.59p = -£111; LONGs already stopped at BE

**Jan 9 2024 (worst single day):**
- 08:00–08:45 — Three LONGs opened (6.55+6.66+3.16L @ ~2033)
- **09:00 — SHORT #1505 opened (4.38L @ 2033.14) while three LONGs still active**
- Result: SHORT stopped at -3.11p = -£107. LONGs hit BE stops (breakeven)
- Then 12:00–13:45: Three new SHORTs opened; 15:38 all stopped simultaneously = -£410
- Then LONG re-entered at 17:30, stopped 27 minutes later = -£109

**Jan 10 2024:**
- SHORT #1521 opened 10:00 (5.1L @ 2030.84), stopped at -2.60p at 10:07
- **13:45–15:15: Three LONGs opened (1.15+1.09+1.37L)**
- **`Basket ceiling reached`** — EA was at risk limit
- 19:53 — All three LONGs hit their original SLs simultaneously (-11–12p each) = -£99 combined
- **SYM EXIT** triggered next morning

**Jan 22 2024:**
- LONG #1567 opened 12:00 (3.92L @ 2021.84), ladder exits taken, BE moved
- **18:00 — SHORT #1573 opened (1.92L @ 2024.20) while LONG still active**
- Result: LONG stopped at raised BE (+3.93p = minor profit). SHORT stopped at -6.97p = -£105

**Jan 25 2024:**
- SHORT #1587 opened 09:45 (12.12L @ 2016.49), ladder exits taken, BE moved
- **14:45–15:30 — Three LONGs opened (4.48+4.64+2.48L) while SHORT still active**
- 15:30: SHORT hit BE stop (0 pips). LONGs stopped simultaneously = -£345 total

**Jan 29–30 2024:**
- Three LONGs open 11:30–13:45 (2.18+3.00+3.78L)
- **15:00 — SHORT #1602 opened (2.28L) while LONGs still active**
- SHORT stopped at -5.93p = -£141; LONGs then stopped at raised BEs (minor losses)

> **Conclusion:** Counter-direction entries are frequent (83 days across the full test) and appear to be a deliberate feature of the algorithm's mode-switching logic. However, in the December–January volatile period, they repeatedly resulted in both directions being stopped out in quick succession, doubling the loss on any given swing.



---

## 5. MASS SL EVENTS — ALL INSTANCES WHERE MULTIPLE POSITIONS STOPPED SIMULTANEOUSLY

"Mass SL event" = 3 or more stop-loss triggers on the same calendar date (unique tickets, not partial closes).

### December 2023 Mass SL Events

| Date         | # SLs | Positions Hit                                        | Combined Loss |
|--------------|-------|------------------------------------------------------|---------------|
| 2023-12-04   | 3     | #1349 (0.60L +59p), #1350 (0.67L +59p), #1351 (0.63L +59p) | **+£350 PROFIT** (trailing stops) |
| 2023-12-12   | 6     | #1385 (3.02L ~0p), #1386 (2.68L ~0p), #1388 (2.24L), #1390 (1.16L), #1392 (1.30L), #1395 (2.41L) | Mixed — mostly BE, ~-£57 |
| 2023-12-13   | 3     | #1395 (2.41L -2.76p), #1397 (1.32L -3.98p), #1365/#1366 trailing | Mixed |
| **2023-12-18** | **6** | #1415 (11.66L BE), #1410 (0.86L profit), #1409 (1.03L profit), **#1419 (3.0L -5.47p), #1420 (4.63L -3.71p), #1421 (3.92L -4.46p)** | **~-£402 net loss** |
| **2023-12-19** | **3** | **#1425 (2.79L -5.87p), #1426 (2.88L -5.70p), #1427 (3.03L -5.32p)** | **~-£385 total** |
| 2023-12-21   | 3     | #1431 (3.69L +4.31p), **#1436 (8.11L -2.16p), #1437 (6.39L -2.43p)** | ~-£210 net |
| 2023-12-26   | 6     | #1447 (6.93L BE), #1448 (9.80L BE), #1452 (7.27L BE), #1454 (8.70L BE), #1456 (5.51L BE), #1459 (8.02L loss) | ~-£134 (only 1 real loss, rest BE) |
| **2023-12-27** | **3** | **#1461 (14.90L -1.05p), #1462 (11.08L -1.44p)**, #1465 (6.82L -2.22p) | **~-£370 total** |
| **2023-12-28** | **4** | **#1467 (5.03L -3.19p), #1468 (4.77L -3.37p), #1469 (3.15L -4.66p)**, #1473 (2.79L -5.33p) | **~-£485 total** |

### January 2024 Mass SL Events

| Date         | # SLs | Key Positions                                        | Est. Loss |
|--------------|-------|------------------------------------------------------|-----------|
| 2024-01-02   | 3     | #1475 (1.82L -9.81p), #1476 (2.64L -5.64p), #1479 (3.72L -4.09p) | ~-£252 |
| 2024-01-04   | 4     | #1486 (2.13L raised), #1487 (2.96L -5.02p), #1488 (3.93L -3.92p), #1489 (3.04L -4.43p) | ~-£320 |
| **2024-01-05** | **4** | #1493 (5.49L BE), **#1496 (6.60L -2.30p), #1497 (5.77L -2.62p)**, #1500 (3.90L -3.59p) | ~-£340 |
| **2024-01-09** | **8** | #1504 (2.53L BE), #1505 (4.38L -3.11p), #1510-#1512 (3x BE), **#1513 (3.79L), #1514 (2.77L), #1515 (2.91L)**, #1519 (3.75L) | **~-£520** |
| 2024-01-10   | 4     | #1521 (5.10L -2.60p), #1523 (1.15L -11.34p), #1524 (1.09L -12.15p), #1525 (1.37L -9.58p) | ~-£255 |
| 2024-01-15   | 3     | #1537 (4.07L -3.13p), #1538 (3.80L -3.37p), #1541 (5.04L raised BE) | ~-£200 |
| 2024-01-18   | 3     | #1549 (0.93L raised), #1550 (0.82L raised), #1551 (0.69L raised) | Minor (all profit) |
| 2024-01-24   | 3     | #1575 (3.50L raised), #1576 (2.85L raised), #1577 (1.65L raised) | Minor losses |
| **2024-01-25** | **4** | #1587 (7.28L BE), **#1591 (4.48L -3.27p), #1592 (4.64L -3.38p), #1593 (2.48L -5.13p)** | **~-£345** |
| 2024-01-29   | 4     | #1599 (1.32L raised), #1600 (1.80L raised), #1601 (2.28L raised), #1602 (2.28L -5.93p) | Minor |
| 2024-01-30   | 3     | #1613 (5.97L -2.29p), #1615 (1.21L raised), #1616 (1.45L raised) | ~-£107 |

> **Jan 9, 2024 was the single worst day in the backtest**: 8 stop-loss events, 4 genuine losses + 4 breakevens, estimated combined loss **~-£520 gross** (partially offset by ladder profits earlier that morning).



---

## 6. PROFIT SUMMARY — OVERALL AND BY MONTH

### Total Backtest Result
| Metric | Value |
|--------|-------|
| Starting balance | £100,000 GBP |
| Final balance | £188,757.85 GBP |
| Net profit | **+£88,757.85 (+88.76%)** |
| Test period | 2023-01-05 to 2024-06-25 (nearly 18 months) |
| Monthly average | ~+£4,930/month |

### SL-Based Trade Outcome Summary

| Metric | Count |
|--------|-------|
| Total SL events | 765 |
| Profitable SL exits (trailing/raised stops) | **208** (27%) |
| Losing SL events | **352** (46%) |
| Breakeven SL events | **205** (27%) |

### Monthly Estimated Net P&L from SL Events Only (pips × lots)

Note: This is a **proxy metric only** — it does NOT include ladder partial-close profits (which are the EA's primary profit mechanism), spread costs, or SYM EXIT profits. Actual monthly P&L will differ significantly from this number.

| Month    | SL Win p×L | SL Loss p×L | SL Net p×L | Notes |
|----------|-----------|------------|------------|-------|
| 2023-01  | +139.7    | -213.3     | -73.5      | Mostly short bias, choppy start |
| 2023-02  | +77.0     | -346.9     | -269.9     | Heavy losing month |
| 2023-03  | +33.0     | -36.9      | -3.9       | Light activity |
| 2023-04  | +167.4    | -428.6     | -261.2     | High activity, poor |
| 2023-05  | +158.1    | -326.2     | -168.1     | Moderate loss |
| 2023-06  | +168.0    | -267.4     | -99.4      | Improving |
| **2023-07** | +173.5 | **-451.4** | **-277.9** | Worst month by SL metric |
| 2023-08  | +85.3     | -160.1     | -74.8      | Mixed |
| 2023-09  | +93.1     | -228.4     | -135.3     | Short-heavy |
| 2023-10  | +142.1    | -309.3     | -167.2     | Moderate losses |
| 2023-11  | +258.8    | -290.4     | -31.6      | Near-breakeven SL activity |
| **2023-12** | **+239.2** | -271.4 | -32.2   | Carried large Nov profits; losses from Dec shorts |
| **2024-01** | +149.7 | **-437.0** | **-287.3** | Worst month by SL metric in Jan |
| 2024-02  | +57.3     | -251.0     | -193.8     | Feb continued poorly |
| 2024-03  | +6.4      | 0.0        | +6.4       | Very light, 0 losses |
| 2024-04  | +122.5    | -282.7     | -160.2     | Moderate loss |
| **2024-05** | +230.1 | -481.6     | **-251.5** | Second worst month |
| 2024-06  | +182.3    | -421.0     | -238.7     | Poor finish |

**CRITICAL NOTE:** Despite negative SL P&L in most months, the overall test was highly profitable (+88.76%). This proves the EA generates its profits primarily through:
1. **Ladder partial-close exits** (taking profits at Rung1 and Rung2 before SL is hit)
2. **SYM EXIT invalidation closes** (profitable market closes when mode changes)
3. **Large position carry-overs** running for days/weeks and closing via trailing stops

The SL losses are the "cost of doing business" — the EA accepts frequent small-to-medium losses on stopped positions while its winners (when they run) are significantly larger.



---

## 7. KEY PATTERNS — TOP 3 RECURRING CAUSES OF LOSSES

### Pattern 1: "Triple/Quad Same-Direction Re-Entry After SL" (The Chaser Pattern)
**Frequency:** Documented ~15+ times across full backtest; heavily concentrated in Dec 2023 and Jan 2024.

**Mechanism:**
1. EA enters 1–3 SHORT (or LONG) positions in the same price zone
2. They are stopped out (original SL hit)
3. EA **immediately re-enters** the same direction within the next few bars at essentially the same entry price
4. New positions are stopped out again, often within the same session

**Most destructive instances:**
- Dec 18 → Dec 19 2023: SHORTs at 2022–2024 stopped (Dec 18 19:48), re-entered same level, stopped again (Dec 19 14:40) → **-£787 in 19 hours**
- Jan 4–5 2024: LONGs at 2043–2046 stopped (Jan 4 15:32), re-entered at 2042 (Jan 5 09:00–11:15), stopped again → **-£660 in 20 hours**
- Jan 9 2024: LONGs at 2033–2034 stopped at BE (10:06–10:10), SHORTs entered at same level (12:00–13:45), stopped again (15:38) → **~-£520 total day**

**Root cause:** The EA's mode-detection and directional hypothesis system appears to re-confirm the same hypothesis after an invalidation, rather than recognizing a ranging/non-trending environment and reducing size or pausing.

**Fix suggestion:** Add a "cooling off" rule: if the same price zone triggers a SL twice within N bars, reduce position size on the third entry or skip entirely.

---

### Pattern 2: "Basket Ceiling Capacity Crunch → Forced Tiny Lots → Immediate SL" 
**Frequency:** 327 basket ceiling events across full backtest; heavily concentrated in Nov–Dec 2023 and Feb 2024.

**Mechanism:**
1. EA accumulates a large basket of open positions across multiple sessions
2. The combined risk of the basket approaches or exceeds the `currentRisk` limit (~£4,900–£5,100 based on max figures in log)
3. `SYM: Basket ceiling scaled lots X.XX -> Y.YY dir=-1` — position sizes are dramatically reduced (e.g., 7.80L → 3.02L, or extreme: 4.46L → 0.14L)
4. These "ceiling-scaled" entries have extremely tiny SLs (because their lots are small) but the **same underlying price zones** — so they get stopped just as easily but contribute almost nothing to the next profitable ladder exit

**Most destructive instances:**
- Dec 5–6 2023: Basket ceiling blocked ALL new SHORT entries for 5 consecutive days (Dec 6–7) even as XAUUSD was moving in the right direction. The EA could not add to winning positions.
- Dec 11–12 2023: Ceiling forced positions down to 1.16–2.68L when the system wanted 7–8L; the reduced lots couldn't recoup previous losses efficiently
- Jan 17 2024: Ceiling scaled down to 0.02L (!) for one entry — a 2-pip move stopped it out for essentially zero impact

**Root cause:** The basket risk accumulation carries over from large unresolved positions in prior sessions. When these positions partially ladder out or hit BE, the freed-up capacity arrives late. The basket ceiling is doing its job of protecting capital, but it simultaneously prevents the EA from capitalizing on confirmed trends.

**Fix suggestion:** Review the risk cap logic. Consider separating "trapped positions" (those past ladder Rung1 that have BE stops set) from the basket risk calculation, since they represent locked-in risk that's no longer truly at the original SL level.

---

### Pattern 3: "Whipsaw at Mode Transition — Counter-Direction Positions Opened at Same Price Level"
**Frequency:** 83 days with counter-direction entries identified; notable in Dec 14, 20–21, 26, 28 (2023) and Jan 5, 9, 10, 22, 25, 29–31 (2024).

**Mechanism:**
1. EA is running a SHORT basket and detects bullish momentum shifting
2. `SYM EXIT: mode-invalidation-at-peak triggered` fires OR the SHORT hits its SL
3. Within the same bar or next few bars, EA enters a LONG
4. Price reverses again (the original short direction resumes)
5. LONG hits SL
6. Then either: another SHORT is entered (completing the whipsaw loss cycle), or the original loss is locked in

**Most destructive Dec–Jan instances:**
- Dec 14: SHORT stopped (-£124) → immediately followed by LONG stopped (-£132) → SYM EXIT → two losses in one afternoon
- Dec 21: LONG #1436+#1437 opened at 2035–2036 (countertrend against existing SHORT) → both stopped in 40 seconds for -£260
- Jan 10: SHORT stopped (10:07), SYM EXIT (10:15), LONGs entered (13:45–15:15), all three LONGs stopped at -£99 when XAUUSD dropped sharply from 2035 → 2023 that evening
- Jan 30: SHORT stopped (08:14), SYM EXIT (08:15), LONGs entered (11:45–12:15), LONGs hit BE stops shortly after (minor impact)

**Root cause:** The `SYM EXIT: mode-invalidation-at-peak` appears to trigger at Phase 3 of some internal wave/cycle count. The new opposing direction entry happens **before the reversal is confirmed**, not after. In choppy markets (Dec–Jan XAUUSD was oscillating within a 60–80 pip range for weeks), mode-transitions happen multiple times per week and the EA pays the entry-to-SL spread on each one.

**Fix suggestion:** After a `SYM EXIT` or mode-invalidation event, require a minimum distance/pip confirmation move before opening the counter-direction position. The current logic appears to re-enter within the same session immediately.

---

## APPENDIX: ADDITIONAL OBSERVATIONS

### BE Stop Behavior
The `SYM BE moved stop to entry=X ticket=Y` mechanism works as intended. **205 out of 765 SL events** (27%) were true breakeven closes — the position was profitable enough to move the stop to entry, then reversed. These are effectively free options: maximum downside is spread + slippage, maximum upside was already partially captured via ladder exits.

The BE stop firing was particularly intense in **Dec 26 2023** when 5 consecutive SHORTs (cumulative 38.21 lots) were all individually taken to BE and stopped — technically no real loss, but enormous opportunity cost given XAUUSD was moving in the SHORT direction.

### Basket SL Error (Dec 26 2023)
Four `SYM BE move failed ticket=1452 err=4756` errors appear on Dec 26 2023 at 13:30–14:00. Error 4756 in MT5 = "Invalid stops" — the EA attempted to set a BE stop that was within the broker's minimum stop distance from current price. This delayed BE protection for ticket #1452 and it was eventually stopped at entry 2063.27 only after the error resolved. No direct loss, but represents a risk exposure window.

### "No Real Ticks" Gaps
```
XAUUSD : 2023.12.22 23:59 - no real ticks within a day
XAUUSD : 2023.12.27 23:59 - no real ticks within a day
```
Both days were **Christmas/Boxing Day 2023** market closures. The EA was holding large SHORT positions over these holiday gaps, and when markets reopened, price had moved against the SHORTs (XAUUSD was rising from ~2050 to ~2065+ over the holiday period). This explains why Dec 26–27 saw immediately stopped positions — they opened into a gap-up environment.

### Lot Size Escalation
Position sizes grew dramatically over the test period:
- January 2023: Typical position = 0.06–0.26L
- November 2023: Positions up to 18.16L (Nov 23)
- December 2023: Positions up to 14.90L (Dec 27)
- January 2024: Positions up to 12.12L (Jan 25)

This confirms the EA uses some form of progressive lot sizing correlated with account equity growth. As the account grew from £100k to ~£160k+ through mid-2023, the lot sizes proportionally scaled up — meaning December's losses hit harder in absolute £ terms than the same pip-distance would have in January 2023.

---

*Report generated from full 30,987-line log analysis. All P&L estimates use approximate XAUUSD pip value of £7.87/pip/lot (based on GBP account, XAUUSD ~2,000–2,080, GBP/USD ~1.27). Actual realized P&L will depend on broker spread, swap rates, and exact fill prices.*
