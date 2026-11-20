# SQL analysis of the 2008 financial crisis

Course: Data Warehousing, Hofstra University, Spring 2026 (Prof. Tzer Hung Low). Project 1.

Team project ("Team Khan"), four members. Section owners as marked in the notebook:

| Section | Owner |
|---|---|
| EDA, building the SQL database | Samantha |
| Risk analysis | Zeeshan |
| Transaction management, sector analysis | Alexander |
| Complex queries (CTE, view, subquery), JOIN showcase, interactive dashboard | Talha (me) |

The database documentation, the re-run of the regression on the estimation universe, and the early-warning bonus are not marked with an owner.

## Question

Did stock risk factors measured in December 2007 say anything about which companies and sectors fell the most in 2008?

## Data

Two Barra USE3 risk model files supplied by the instructor: December 2007 (10,214 stocks, 49 columns) and December 2008 (9,625 stocks). The data is licensed and is not included here.

## Approach

- Clean the files in pandas: replace the -999 placeholder with nulls, rename columns containing `%`, strip whitespace.
- Load both years into SQLite through SQLAlchemy, add an industry-to-sector mapping table and indexes.
- Risk statistics in DuckDB SQL, and an OLS regression of 2008 price change on six 2007 risk factors (statsmodels).
- A transaction that changes risk values for Bear Stearns and Lehman, measures the sector effect, then rolls back.
- My part: a composite risk z-score per stock within its sector, written three ways (CTE, view, subquery) and compared with `EXPLAIN QUERY PLAN`; window-function ranking of outliers; INNER, LEFT and simulated FULL OUTER joins between the two years with null handling; an ipywidgets dashboard to filter by z-score threshold and sector.

## Results

All numbers are printed in the notebook.

- 9,239 companies appear in both years. 975 of the 10,214 companies from 2007 are missing in 2008. The full outer join has 10,600 rows.
- On all stocks, the six risk factors explain almost none of the 2008 price change (R-squared 0.007, n = 9,239). On the estimation universe only (1,801 stocks, 1,729 used in the fit), R-squared is 0.162 and all six coefficients are negative.
- Correlation between the composite risk z-score and 2008 price change: 0.054 (n = 9,239).
- Every sector lost value in 2008. Consumer Services fell 52.15%, Energy 49.81%, Basic Materials 46.68%. Financials fell 38.95%, 9th of 14 sectors. The notebook notes this is affected by survivorship: firms that collapsed have no December 2008 price.
- Early-warning flag (composite z above 1.5), checked in five random folds: between 0% and 66.67% of flagged companies fell more than 50%, on 3 to 12 flagged companies per fold. The samples are too small to say much.

## Files

- `sql_2008_crisis_risk_analysis.ipynb`: the team notebook with outputs.

## Skills

SQL (CTEs, views, subqueries, window functions, joins, transactions, indexes, query plans), SQLite, SQLAlchemy, DuckDB, pandas, statsmodels OLS, matplotlib, ipywidgets.

## AI use

Several cells are marked in the notebook as written with AI help.
