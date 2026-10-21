# Cookie Cats A/B Test: Does Moving the First Gate Hurt Retention?

**Recommendation: keep the gate at level 30.** Moving it to level 40 cut Day 7 retention by 0.82 pp (−4.3% relative) with no gain elsewhere.

## Business question
Cookie Cats is a mobile puzzle game. Players hit a "gate" that forces a wait or an in-app purchase to continue. The team tested moving the first gate from level 30 (control) to level 40 (treatment). Does a later gate keep players around longer?

## Data
90,189 players randomly assigned at install. Source: [Kaggle – Mobile Games A/B Testing: Cookie Cats](https://www.kaggle.com/datasets/mursideyarkin/mobile-games-ab-testing-cookie-cats).
Columns: `userid`, `version`, `sum_gamerounds`, `retention_1`, `retention_7`.

## Method
1. Sanity checks: sample ratio mismatch (chi-square), outlier removal (one player with 49,854 rounds)
2. Two-proportion z-tests with 95% confidence intervals on Day 1 and Day 7 retention
3. Bootstrap (2,000 resamples) of the retention difference
4. Mann-Whitney U test on game rounds played (heavily skewed distribution)

## Results

| Metric | gate_30 | gate_40 | Difference | 95% CI | p-value |
|---|---|---|---|---|---|
| Day 1 retention | 44.8% | 44.2% | −0.59 pp | [−1.24, +0.06] | 0.074 |
| Day 7 retention | 19.0% | 18.2% | −0.82 pp | [−1.33, −0.31] | 0.002 |
| Median rounds played | 17 | 16 | — | — | 0.051 |

Bootstrap: gate_30 had higher Day 7 retention in >99.9% of resamples, and higher Day 1 retention in 96.6%.

## Recommendation
- **Don't ship gate_40.** Day 7 retention drops significantly. Day 1 retention and engagement point the same way but aren't significant at 5%.
- **Investigate the test setup.** The groups are uneven (45,489 vs 44,700; SRM p = 0.009), which suggests an assignment or logging issue. The no-ship decision still holds, because the treatment failed to show any improvement, but the setup should be checked before running the next test.
- **Next test: revenue.** The gate is also a monetization point, and this dataset has no revenue. A follow-up should use ARPDAU as the primary metric, with Day 7 retention as a guardrail.

## How to run
Download `cookie_cats.csv` from Kaggle, place it next to the notebook, and run all cells. Requires pandas, numpy, scipy, statsmodels, matplotlib.
