# Imputing MAC3 factor history

Course: CSC 221 Data Science, Hofstra University, Fall 2026 (Prof. Tzer Hung Low). Project 2. Graded 100/100.

Team project, four members. The notebook labels each part with its owner:

| Part | Owner |
|---|---|
| 1. Data inventory, merging, EDA | Muhammad Talha Khan |
| 2. Baseline and validation framework | Muhammad Talha Khan |
| 3. Imputation models for 17 factors | Zain Ul Abideen |
| 4. Imputing 2000 and 2007, stress tests | Ronit Kumar Emmanuel |
| 5. Crash validation | Pratham |
| 6. Cross-crash test | Pratham |
| Bonus, reality checks, reproducibility checks | Muhammad Talha Khan |

## Question

Bloomberg's MAC3 equity factor model has no exposures for March 2000 or October 2007. Can the 17 MAC3 factor exposures be estimated for those two market peaks from fields that did exist then, and do the estimates point at the stocks that later fell the most?

## Data

Bloomberg PORT exports supplied by the instructor: two Excel files per date for seven dates (2000-03-24, 2007-10-09, November 2022 to 2025, 2026-10-09), about 1,600 US-listed stocks per date, plus two sector-fill files for the peak dates. MAC3 exposures exist only from 2022 onward. The data is licensed and is not included in this repository, so the notebook cannot be rerun from here.

## Approach

- Parts 1 and 2 (my parts). Inventoried the 14 files, merged the two files per date on ticker, stacked them into one panel (11,196 rows, 78 columns), filled missing sectors, built a crash total return per stock, and removed duplicate listings. Set up walk-forward splits (train 2022 to 2024, test 2025; train 2022 to 2025, test 2026), Spearman rank correlation as the score, and a baseline that maps each MAC3 factor to its closest factor in the older Bloomberg model.
- Part 3. OLS, Ridge, Lasso, degree-2 versions of each, XGBoost and CatBoost for every factor. Model per factor chosen by leave-one-year-out on the training years only.
- Part 4. Stress test: blank 2025 inputs to match 2000 and 2007 coverage, then compare gap fixes (twin fill of forward ratios, MICE, training on degraded copies). Final models impute all 17 factors at both peaks.
- Parts 5 and 6. Build baskets of the 30 and 50 highest-risk stocks from the imputed factors and compare their crash returns with all stocks. Train a model on one crash and test it on the other.
- Bonus (mine). A 2026 watch list by volatility rank, a check of risky stocks outside the crash's main sector, and a range for each imputed factor across four model variants.

## Results

All numbers are printed in the notebook.

- Baseline, mean Spearman over 17 factors: 0.651 (2025) and 0.540 (2026).
- Chosen models: 0.890 (2025) and 0.877 (2026). XGBoost was chosen for 13 of 17 factors. 16 of 17 factors beat the baseline in both test years; Dividend Yield tied it.
- A random 80/20 split overstated accuracy by 0.02 to 0.07 on the four factors tested, compared with the walk-forward split.
- With 2025 inputs blanked to 2000 coverage, mean Spearman fell from 0.890 to 0.806. Training on degraded copies brought it back to 0.859. With 2007 coverage it was 0.878.
- Confidence labels: 13 factors "use", 2 "use with caution" (Growth, Momentum), 2 "do not use" (Liquidity, Long-Term Reversal).
- 2000 to 2002: the top-30 basket from the analysts' hint score lost 92.8%. All stocks lost 46.1%.
- 2007 to 2009: the top-30 basket lost 80.1%. All stocks lost 57.6%. Here the same score built from the older Bloomberg factors did slightly better (83.4%), so the imputed factors added nothing over the old model in that crash.
- Cross-crash: no model trained on one crash beat a plain volatility ranking on the other by rank IC (0.328 vs best trained 0.273 on 2007; 0.649 vs 0.602 on 2000).

There is no answer key for 2000 or 2007. The basket results show the imputed factors are useful, not that each value is correct.

## Files

- `mac3_factor_imputation.ipynb`: the submitted notebook with outputs.
- `results/spearman_by_factor.csv`: Spearman score for every factor, model and test year.
- `results/crash_validation.csv`: basket returns for both crashes.
- `results/cross_crash.csv`: cross-crash model results.

The per-stock imputed factor files and the 2026 watch list are not included because they are tied to licensed Bloomberg data.

## Skills

pandas (cleaning and merging vendor Excel exports), exploratory analysis, walk-forward validation, leakage checks, scikit-learn (Ridge, Lasso, IterativeImputer), XGBoost, CatBoost, rank-based evaluation, team coordination on a shared notebook.

## AI use

The notebook ends with the team's AI disclosure statement, which lists the tools each member used and how the output was checked.
