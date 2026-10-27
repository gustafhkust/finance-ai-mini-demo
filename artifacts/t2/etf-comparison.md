# T2 ETF Comparison: SPY, TLT, GLD — Fees and Historical Drawdown

T1 planned a comparison of SPY, TLT, and GLD on illustrative (synthetic) numbers; this T2 report upgrades that plan to the real-data inputs of the T2 ETF Data Pack, calculating maximum drawdown from the supplied daily price history and using the supplied issuer fee disclosures.

## Source details

- Input files: `data/t2/daily_prices.csv` (7,536 daily observations) and `data/t2/fund_info.csv` (three disclosed fund records); T1 context read from `artifacts/t1/project-plan.md`.
- Pack preparation date: 2026-09-25.
- Fee disclosures (`fund_info.csv`, fee_accessed_on = 2026-09-25):
  - SPY: 0.0945% — "Fund-information as-of date: 2026-09-10; fee-specific effective date not stated"
  - TLT: 0.15% — "Current prospectus; specific date not stated in the fee panel"
  - GLD: 0.4% — "Not stated in the selected field"
- Common drawdown period: 2016-09-01 through 2026-08-31, inclusive (2,512 sessions per ETF), daily frequency (US market sessions), adjustment basis: `adjusted_close` from the frozen Yahoo chart responses (provider split/dividend adjustments).
- Script: `artifacts/t2/calculate_drawdown.py`; command executed from the repository root: `python3 artifacts/t2/calculate_drawdown.py`.
- Input-check result: all checks passed — only SPY/TLT/GLD present; no duplicate ticker/date pairs; ascending dates per ticker; positive finite prices (close and adjusted_close); 2,512 rows per ETF; common first/last dates (2016-09-01 / 2026-08-31); identical date sets across tickers.
- Method: for each ticker, `high_t` = largest `adjusted_close` observed from the window start through day t; `drawdown_t_pct = (adjusted_close_t / high_t − 1) × 100`; maximum drawdown = minimum over the full window, with the earliest trough (and its earliest corresponding peak) on ties; full precision kept internally, two-decimal display only.

## 1. Comparison

| ETF | Annual expense ratio (%) | Calculated max drawdown (%) | Peak date | Trough date |
|---|---|---|---|---|
| SPY | 0.0945 | -33.72 | 2020-02-19 | 2020-03-23 |
| TLT | 0.15 | -48.35 | 2020-08-04 | 2023-10-19 |
| GLD | 0.4 | -26.40 | 2026-01-29 | 2026-07-16 |

Fees are shown with their disclosed precision (0.0945%, 0.15%, 0.4%); drawdowns are shown to two decimals (-33.72%, -48.35%, -26.40%).

## 2. Observation

GLD shows the smallest drawdown loss in this period: -26.40% (closest to zero), versus -33.72% for SPY and -48.35% for TLT; there are no ties among the displayed two-decimal results. These drawdowns were calculated from `adjusted_close` in `daily_prices.csv`, not read from a precomputed snapshot, and they are non-positive by construction — closer to zero means a smaller loss. Limitation: the measurement uses daily adjusted closes within this fixed 10-year window, so it does not capture intraday losses, all-time risk, bid/ask spreads, or other costs; likewise, the fees above are current issuer-disclosure snapshots (accessed 2026-09-25), not average fees over the ten-year price window, and the drawdowns are not all-time measures.

## 3. Agent Check

Self-check performed by the same Agent (not independent verification; the student did not check the data). ETF chosen: **SPY**.

Source rows re-read from `data/t2/daily_prices.csv` (via `grep -E '^(2020-02-19,SPY|2020-03-23,SPY),'`):
- `2020-02-19,SPY,338.3399963378906,307.6394958496094` (peak; adjusted_close 307.6394958496094)
- `2020-03-23,SPY,222.9499969482422,203.91189575195312` (trough; adjusted_close 203.91189575195312)

Check command (Python 3, stdlib, run from the repository root): a heredoc that re-reads `daily_prices.csv` for all SPY rows, recomputes `(trough / peak − 1) × 100` from the two source adjusted prices, and reviews the full series (all 2,512 SPY dates, cumulative highs from the window start, min drawdown, peak-before-trough order). Actual output:

```
(trough / peak - 1) * 100 = -33.717257210811404
full-series min drawdown = -33.717257210811404 at trough 2020-03-23
cumulative high at trough = 307.6394958496094 first achieved on 2020-02-19
peak-before-trough check: True
n SPY rows used: 2512
```

Comparison: the recomputed value (-33.717257210811404) matches the script's full-precision result exactly and rounds to the report's -33.72%; the reported peak date is the earliest date the cumulative high (307.6394958496094) was achieved, it is on/before the trough date, and the full-window minimum over all dates occurs at the reported pair. No correction or recheck was needed. Checking the observation against all three results (SPY -33.72, TLT -48.35, GLD -26.40) confirms GLD is closest to zero with no ties.
