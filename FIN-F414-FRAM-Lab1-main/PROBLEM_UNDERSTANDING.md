# Lab 1 — Problem Understanding

## What this lab is
Replicate selected tables from Fama & French (2012), *"Size, Value, and Momentum in
International Stock Returns"* (JFE), using current Kenneth French data library files,
restricted to **Global Developed Markets** vs **Japan**, over **Nov 1990 – Mar 2011**
(245 monthly observations). Work proceeds through 5 notebooks, in order, each building
on cleaned data produced by the first one.

Deadline: EOD 17 Sep 2026. Deliverable: 4 completed notebooks + their HTML exports +
one Word/PDF report with a table/figure and a written interpretation for every task.

## Current state of the repo
- `raw_data/` already contains all 8 required CSVs, correctly renamed:
  `developed_3_factors.csv`, `japan_3_factors.csv`, `developed_momentum.csv`,
  `japan_momentum.csv`, `developed_25_size_bm.csv`, `japan_25_size_bm.csv`,
  `developed_25_size_momentum.csv`, `japan_25_size_momentum.csv`.
- `cleaned_data/` already contains the corresponding 8 cleaned CSVs — i.e.
  `00_clean_data.ipynb` has already been run successfully. Step 1 (data prep) is done.
- `01_replicate_table1.ipynb` through `04_replicate_table6_table7.ipynb` are the
  **skeletons that still need to be completed** — every task cell currently contains
  only `# Write your code here`, and every "compare/interpret" markdown cell needs a
  written answer (destined for the report, not necessarily the notebook itself).

## Data pipeline recap (`00_clean_data.ipynb`, already run)
For each raw file it: locates the correct section (the 3/momentum factor files are
read by matching header columns; the 25-portfolio files are read by locating the
"Average Value Weighted Returns -- Monthly" section), keeps only rows with a 6-digit
`YYYYMM` date, converts dates, coerces returns to numeric, replaces French's
missing-value codes (`-99.99`, `-999`) with NaN, and restricts to Nov 1990–Mar 2011.
Each cleaned file should have 245 rows, 0 missing values, 0 duplicate dates. "Developed"
= the paper's "Global" market.

Key columns:
- `*_3_factors.csv`: `date, Mkt-RF, SMB, HML, RF`
- `*_momentum.csv`: `date, WML`
- `*_25_size_bm.csv` / `*_25_size_momentum.csv`: `date` + 25 value-weighted portfolio
  return columns (5×5 grid, columns ordered so every consecutive block of 5 is one size
  row — first 5 cols = smallest size quintile, last 5 = biggest).

All returns/RF are already in percent — never rescale or annualize them.

## Notebook 01 — Table 1 (factor summary statistics)
Merge each market's 3-factor file with its momentum file on `date`.
- **Task 1 (Global):** for `Mkt-RF, SMB, HML, WML` compute mean, std dev, and
  t-stat of the mean (t = mean / (std/√n)), rounded to 2 dp, shown as a table with
  columns Mean / Std dev / t-Mean.
- **Task 2 (Japan):** same, for Japanese factors.
- **Task 3:** written comparison to paper's Table 1 (Global vs Japan sections) —
  which premiums look statistically/economically meaningful.
- **Task 4:** one figure, 4 rows × 2 cols (rows = Mkt-RF/SMB/HML/WML, left col =
  Global, right col = Japan), each panel a time series of that factor with dates on
  the x-axis.
- **Task 5:** written interpretation of the plots, tying back to Task 1–3.

## Notebook 02 — Table 2 (25-portfolio mean/std, excess returns)
For all 4 portfolio datasets (developed/Japan × size-BM/size-momentum): subtract the
**market-specific RF** (Developed RF for Developed portfolios, Japan RF for Japan
portfolios) from every one of the 25 raw portfolio return columns, month by month, to
get excess returns — do not touch Mkt-RF.
- **Task 1:** produce the 4 excess-return DataFrames (date + 25 columns each).
- **Task 2:** for each of the 4 datasets, compute mean and std dev *per portfolio*
  (i.e., across the 245 months, separately for each of the 25 columns — never average
  across portfolios), then reshape each of the 4 results into a 5×5 table: rows =
  size (Small→Big) using the column-block order noted above, columns = B/M
  (Low→High) or momentum (Losers→Winners) depending on dataset. Round to 2 dp.
- **Task 3:** written comparison to paper's Table 2 Panel A (size-BM) and Panel B
  (size-momentum), Global and Japan sections — patterns across size/value/momentum
  and between markets.

## Notebooks 03 & 04 — Tables 3/4 and 6/7 (asset-pricing regressions)
Same methodology applied to two different portfolio sets: notebook 03 uses the
size/B-M 25 portfolios (CAPM, 3-factor, 4-factor models, all three), notebook 04 uses
the size/momentum 25 portfolios but **only the 4-factor model** (paper doesn't report
CAPM/3-factor for these).

For every "combination" (market pair) and every model:
1. Dependent variable = portfolio excess return (`portfolio − RF`, same market's RF).
2. Independent variables per model:
   - CAPM: `Mkt-RF`
   - Three-factor: `Mkt-RF, SMB, HML`
   - Four-factor: `Mkt-RF, SMB, HML, WML`
   (with a constant — the constant is alpha).
3. Run one **separate OLS regression per portfolio** (25 or 20 of them) via
   `statsmodels.api.OLS` (with `sm.add_constant`), for both the full 5×5 (25
   portfolios) and the 4×5 set that drops the 5 smallest-size columns
   (`all_portfolios[5:]`, i.e. `without_microcaps`).
4. Store every fitted regression result object (need alphas, residuals, t-stats,
   adjusted R², std errors later) — a dict keyed by portfolio name is the natural
   structure, reused across Table 3/4 (or 6/7) tasks.

Combinations to run (03 has 6 of these ×3 models; 04 has the same 6 but ×1 model):
1. Global portfolios + Global factors, 5×5 and 4×5
2. Japan portfolios + Global factors, 5×5 and 4×5
3. Japan portfolios + Japan factors, 5×5 and 4×5

**Table 3/6 statistics** (computed jointly across all regressions in a
combination, per model):
- `GRS`: the Gibbons-Ross-Shanken joint test that all alphas = 0. Needs: the alpha
  vector (N portfolios), the residual matrix (T×N) to form the residual covariance
  Σ, the factor means/covariance, N portfolios, K factors, T observations. Formula
  is in the paper (not derivable from a single regression's summary output —
  must be computed manually from the stored per-portfolio residuals/alphas).
- `|a|`: mean of |alpha| across the N portfolios.
- `Adjusted R²` (5×5 only): mean of each regression's adjusted R².
- `s(a)` (5×5 only): mean of each regression's alpha standard error.
- `SR(a)`: sqrt(a' Σ⁻¹ a) type Sharpe ratio of the alpha vector using the residual
  covariance matrix (exact formula given in the paper) — one joint number per
  model/combination, not an average of individual Sharpe ratios.
- 4×5 case reports only GRS, |a|, SR(a) (no R², no s(a)), computed the same way but
  restricted to the 20 non-microcap portfolios.

**Table 4/7 statistics** (per-portfolio, not averaged):
- `a` and `t(a)` for each of the 25 portfolios, taken straight from each stored
  regression's params/tstats, arranged as two separate 5×5 matrices (size rows ×
  B-M or momentum columns), for:
  - Notebook 03: Global portfolios/Global factors (all 3 models per paper table
    section) and Japan portfolios/Japan factors (three-factor model only, per the
    paper's reported section).
  - Notebook 04: Global portfolios/Global factors and Japan portfolios/Japan
    factors, four-factor model only.

## Cross-cutting things to get right
- **RF matching**: always use the RF from the same market as the portfolio being
  regressed/de-meaned (Japan portfolios use Japan RF even when regressed on Global
  factors) — the merge for the regression's X matrix should not re-subtract Mkt-RF.
- **Portfolio ordering**: the 25 columns are already ordered so that reshaping into
  a 5×5 (row-major, 5 columns at a time) matches size (rows) × B-M-or-momentum
  (columns) — this needs to be confirmed against the raw column headers, not assumed.
- **GRS and SR(a)** are the only genuinely non-trivial statistical formulas to
  implement from scratch (everything else is descriptive stats or comes straight out
  of `statsmodels` regression results) — get the exact GRS/SR(a) formulas from the
  Fama-French (2012) paper (or the original Gibbons-Ross-Shanken 1989 paper) before
  coding them.
- Small numeric differences from the published paper are expected/acceptable (French
  data library has been revised since 2012) — the write-up should note differences,
  not chase an exact match.
- The report (separate Word/PDF deliverable) needs, for every task in every notebook:
  the produced table/figure, and an original (not paper-copied, not AI-copied) written
  interpretation covering closeness to the paper, notable differences, patterns, and
  what they mean economically.

## Suggested order of work
1. Skim the Fama-French (2012) paper sections describing Tables 1–4, 6–7 and the GRS
   / SR(a) formulas (Step 2 in the README) before writing any regression code.
2. Notebook 01 (simplest — descriptive stats + one figure).
3. Notebook 02 (excess returns + 5×5 reshaping — establishes the reshape pattern
   reused in 03/04).
4. Notebook 03 (introduces the regression-storage pattern + GRS/SR(a) — the most
   complex notebook).
5. Notebook 04 (mostly repeats 03's machinery with the momentum portfolios and only
   the four-factor model).
6. Export all 4 notebooks to HTML, write the report, zip the 9 required files.
