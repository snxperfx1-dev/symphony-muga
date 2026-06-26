# Symphony — Corrected Monthly P&L (reconciled to actual +£88,758)

## Why earlier monthly numbers were wrong

1. **No reconciliation scale.** Gross price-based P&L (+£96,810 @ fixed 1.27 FX)
   was never adjusted for swap, commission and the live GBPUSD rate (which ranged
   1.20–1.31, not a fixed 1.27). The tester generated GBPUSD ticks and converted
   each trade at the live rate. Reconcile scale = actual/gross = **0.9168**.
2. **Attribution mixing.** "Monthly P&L" depends on whether a trade is booked to the
   month it ENTERED or the month it CLOSED. The two differ massively for a system
   that holds positions across month boundaries. Both are shown below.

Ground truth: log states **final balance 188,757.85 from 100,000 = +£88,757.85.**
Both tables below sum to that exactly after the 0.9168 reconcile.

---

## TABLE A — CLOSE-DATE attribution (how the account balance actually moved)

This is realized P&L booked in the month each close occurred.

| Month | Reconciled £ | |
|-------|-------------:|---|
| 2023-01 | +6,939 | |
| 2023-02 | +3,293 | |
| 2023-03 | +19,006 | gold surge |
| 2023-04 | +14,024 | |
| 2023-05 | -6,434 | |
| 2023-06 | +15,662 | |
| 2023-07 | **-8,326** | summer |
| 2023-08 | **-7,072** | summer |
| 2023-09 | +831 | |
| 2023-10 | +3,578 | |
| 2023-11 | +5,335 | |
| 2023-12 | **+214** | FLAT realized (drawdown was floating) |
| 2024-01 | -7,575 | |
| 2024-02 | **-16,747** | worst realized month |
| 2024-03 | +42,984 | gold ATH run |
| 2024-04 | +38,279 | |
| 2024-05 | -6,428 | |
| 2024-06 | -8,806 | |
| **TOTAL** | **+88,758** | = actual |

## TABLE B — ENTRY-DATE attribution (correct lens for "should I block this month")

P&L credited to the month a trade was OPENED, regardless of when it closed.
This is the right view for an entry-block decision, because blocking entries in
month X removes exactly the trades that ENTERED in month X.

| Month | Reconciled £ | |
|-------|-------------:|---|
| 2023-01 | +6,939 | |
| 2023-02 | +9,792 | |
| 2023-03 | +34,790 | |
| 2023-04 | -8,259 | |
| 2023-05 | -6,434 | |
| 2023-06 | +15,662 | |
| 2023-07 | **-8,326** | |
| 2023-08 | **-7,072** | |
| 2023-09 | +23,178 | |
| 2023-10 | **-18,770** | worst entry-month (n=1, Oct-2023 war-spike vol) |
| 2023-11 | +13,427 | |
| 2023-12 | **-10,241** | Dec-opened trades that bled into Jan/Feb |
| 2024-01 | -5,211 | |
| 2024-02 | -16,286 | |
| 2024-03 | +87,072 | the monster — March-opened runners |
| 2024-04 | -6,270 | |
| 2024-05 | -6,428 | |
| 2024-06 | -8,806 | |
| **TOTAL** | **+88,758** | = actual |

---

## Reconciling the two views of December

- **Realized (close-date): December was FLAT (+£214).** This is why the on-screen
  monthly summary showed Dec ≈ flat.
- **Entry-date: December was -£10,241.** Positions opened in late December closed at
  a loss in Jan/Feb (the holiday-gap drawdown).
- **Floating equity drawdown Dec-18 → Feb was ~£45k** (peak-to-trough), the figure
  the user originally observed on the equity curve.

All three are correct measures of different things. For the **entry-block decision**,
Table B is the right lens, and it confirms blocking December entries saves ~£10k.

---

## Corrected month-block assessment

| Month | Entry-attr £ | Years | Structural rationale | Block? |
|-------|------------:|-------|---------------------|--------|
| July | -8,326 | 1 | Summer illiquidity | **Yes** |
| August | -7,072 | 1 | Summer illiquidity | **Yes** |
| December | -10,241 | 1 | Holiday gaps/thin liquidity | **Yes** |
| October | **-18,770** | 1 | None obvious (2023 war-spike) | **Watch** — biggest single negative but no clean structural reason; risk of overfitting n=1 |
| February | -16,286 | 2 | None obvious | **Watch** |
| April/May/Jun | -6 to -8k each | 2 | None obvious | Monitor |

**Conclusion:** the Jul/Aug/Dec block already applied is justified on combined
structural + data grounds. **October is actually the single worst entry-month
(-£18,770)** but rests on one year with a one-off geopolitical volatility spike, so
it is a watch-list candidate, not an automatic block, until more years confirm it.

*All figures reconciled to the actual final balance (+£88,757.85) via the 0.9168
gross→net scale. Per-month values remain gross-of-exact-financing within that scale;
the relative win/lose pattern is robust.*
