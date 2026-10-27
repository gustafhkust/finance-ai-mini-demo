# T3 Portfolio Research — Notes

## Agent check summary

*Prepared by: Reviewer (independent verification). Date: 2026-10-27.*

### Method

The Reviewer did not rely on the starter command's output alone. Two
independent harnesses were written from the T3 brief's formulas (no import
of `scripts/t3_portfolio.py`) and kept in `artifacts/t3/portfolio/run/`:

- `review_design.py` (pre-freeze): validates `inputs/design.json`,
  computes the equal-weight baseline and independently enumerates all **67
  design candidates** (66 long-only 10%-grid triples + exact equal thirds),
  applies the brief's selection rules (design return ≥ baseline within 1e-12;
  minimize max-drawdown loss; ties prefer baseline, then higher return, then
  ascending SPY/TLT/GLD weights; fallback retains the baseline unless the
  loss is strictly smaller beyond 1e-12) and compares with
  `baseline-design.json` / `candidate.json`.
- `review_test.py` (post-freeze): validates `inputs/test.json`, independently
  recomputes the baseline and the frozen candidate over the test window and
  compares with `evaluation.json`, including the SHA-256 handoff.
- The provided synthetic tests were run as harness validation:
  `python3 scripts/test_t3_portfolio.py` → 14 tests, all OK.
- The completed page `docs/t3/portfolio/index.html` was verified on source
  (values, curves, hashes, self-containment) and then opened in a browser
  over a local HTTP server to confirm actual rendering.

Full reproduction commands and detailed results:
`artifacts/t3/portfolio/run/t3-review-prep-run-record.md` and
`artifacts/t3/portfolio/run/t3-review-final-run-record.md`.

### Key results (all four return/drawdown pairs, full precision)

| Window | Portfolio | Total return | Max drawdown |
| --- | --- | --- | --- |
| Design (2021-09-01 → 2024-08-30, 754 obs) | Equal-weight baseline | 0.1242581855813576 | -0.2281297127716516 |
| Design | Selected candidate (SPY 0.40 / TLT 0.00 / GLD 0.60) | 0.3393694951641868 | -0.1781355498579864 |
| Test (2024-09-03 → 2026-08-31, 500 obs) | Equal-weight baseline | 0.3712933045999716 | -0.1164452985138378 |
| Test | Frozen candidate (same weights) | 0.6330111722628220 | -0.1700918558765748 |

Every pair was recomputed independently and matches the frozen files
(`baseline-design.json`, `candidate.json`, `evaluation.json`) within 1e-12,
including row-by-row curve equality (drawdown = nav / running peak − 1,
first nav = 100 in each window). The full design candidate-set check covered
all 67 candidates: 39 were return-eligible; the minimum max-drawdown loss
(0.1781355498579864) was uniquely achieved by the candidate above, so no
tie-breaker or fallback was needed; `improved_design_drawdown = true` was
confirmed. No discrepancy was found.

### Approved candidate version and freeze/evaluation sequence

- Freeze gate (`#t3-freeze`, completed 2026-10-27 15:47:21) bound the
  student's explicit approval to candidate.json bytes via SHA-256:
  `03a0ef1ac1481e75488e7e4acee6f6e26741410d91e70b8d3ab02eb804cf2095`
  for weights **SPY 0.40 / TLT 0.00 / GLD 0.60**
  (record: `run/freeze-approval.txt`).
- Evaluation ran only after the freeze, with
  `--expected-candidate-sha256 03a0ef…cf2095`; the tool verifies the hash
  before opening the test input (record: `run/evaluation-run.log`).
- Handoff verified: current candidate.json SHA-256 == evaluation.json
  `candidate_sha256` == approved fingerprint; test.json hash (2efe2dbf…5ddb1)
  and design.json hash (3e22e18f…4763) also match; candidate.json was not
  modified after evaluation (mtime before evaluation.json).
- Page built at `docs/t3/portfolio/index.html` from the three frozen files.

### Page review

The completed page: weights SPY 40% / TLT 0% / GLD 60%; design/test
comparison table exactly matching the four pairs above; a plain-language
drawdown explanation for novice readers (a fall from a peak of 100 to 90 is
a 10% drawdown); two test-period charts (portfolio value and drawdown) whose
curves reproduce the frozen evaluation data exactly (all polyline points
recomputed, 0 mismatches); a completed interpretation section; sources and
limitations, including a period-dependence condition that could change the
conclusion, the retrospective-holdout caveat, and both SHA-256 fingerprints.
The page is self-contained (no external libraries, API calls or keys) and
renders correctly in a browser (verified over local HTTP; no layout errors).

### Unresolved issues

None. No failure is invented; the student's own acceptance decisions are not
recorded here and are appended below by the Team Leader.

## My three decisions

Plan: I approved the plan because it included clear ownership, a separate Reviewer, a strict freeze gate, and my reader requirement to explain drawdown as a fall from 100 to 90.

Handoff and freeze: My prediction was correct; the test evaluation was blocked and waited for my explicit approval. I approved the candidate before testing because it met the design return requirement while minimizing the drawdown loss.

Acceptance: I accept the final result. The page accurately reflects the frozen weights, includes the required reader explanation, and the independent Reviewer provided evidence supporting the calculations.