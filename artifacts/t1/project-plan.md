# Project Plan — Finance AI Mini Demo

## Project Goal

Develop a clear and reproducible research workflow for comparing three familiar asset classes: `SPY` (US equities), `TLT` (long-term US Treasury bonds), and `GLD` (gold). This plan is the starting point (T1); later tutorials will design a bounded analysis task and organize a verifiable agent workflow on the same repository.

## Available Data

`data/etf_snapshot.csv` contains one row per illustrative ETF with the following columns: `ticker`, `asset_class`, `expected_return_pct`, `volatility_pct`, `max_drawdown_pct`, and `expense_ratio_pct`. The dataset is small and fixed, so no download or data-cleaning step is required. The dataset is synthetic teaching data: all numeric values are illustrative assumptions, not live or historical market observations, and must not be used as investment advice.

## Expected Final Deliverable

A concise, reproducible comparison of SPY, TLT, and GLD that documents the planned analysis (for example, expected return, volatility, maximum drawdown, and expense ratio across the three ETFs) together with a verifiable agent workflow for running and checking the analysis. The analysis itself is planned work and is not yet completed.

## Three Project Milestones

1. **Plan and setup (T1)** — Write this project plan and version it with Git; the student saves the file, while JiuWenSwarm does not commit or push.
2. **Analysis task design** — Define a bounded analysis task, such as comparing risk/return and costs across the three ETFs using the snapshot.
3. **Verified agent workflow** — Organize an agent workflow to run the planned analysis and verify the results against acceptance checks.

## One Data Limitation

The dataset is synthetic teaching data: all numeric values are illustrative assumptions, not current quotations, verified historical estimates, or forecasts. The data also omits correlations, taxes, transaction costs, liquidity, currency exposure, and investor-specific constraints, so any results are for learning only and not for real investment decisions.

## Next Action

Submit this plan for review, then have the student save the file with Git. After that, proceed to the next tutorial to design the bounded analysis task.
