# Data science and data warehousing coursework

Projects from two courses I took at Hofstra University with Prof. Tzer Hung Low: Data Warehousing (Spring 2026) and CSC 221 Data Science (Fall 2026). Most are team projects. Each project README says who did which part.

The notebooks were written and run in Google Colab and are saved with their charts. Several use data that is licensed or was supplied by the instructor, so that data is not in this repository and those notebooks cannot be rerun from here. The results quoted in each README are taken from the saved outputs.

| Project | Course | Team or solo | What it is |
|---|---|---|---|
| [mac3-factor-imputation](mac3-factor-imputation/) | Data Science, Fall 2026 | Team of 4. Mine: data preparation, EDA, validation framework, bonus | Estimate 17 equity risk-factor exposures for the 2000 and 2007 market peaks from other fields, then test them against the crashes that followed. |
| [sql-2008-crisis-risk-analysis](sql-2008-crisis-risk-analysis/) | Data Warehousing, Spring 2026 | Team of 4. Mine: complex queries, joins, dashboard | SQL analysis of 2007 and 2008 stock risk data: schema, joins, CTE vs view vs subquery, regression of 2008 losses on 2007 risk factors. |
| [portfolio-semantic-search](portfolio-semantic-search/) | Data Warehousing, Spring 2026 | Team of 4. Mine: the data collection pipeline and two extras | Collect text for 988 Russell 1000 companies, summarise with a local LLM, compare embeddings, build keyword, vector and hybrid search. |
| [nyse-glitch-investigation](nyse-glitch-investigation/) | Data Warehousing, Spring 2026 | Solo | Short pandas exercise on the trades cancelled after the NYSE opening fault of January 24, 2023. |

## Tools used across the projects

Python, pandas, NumPy, scikit-learn, XGBoost, CatBoost, statsmodels, SQL (SQLite, DuckDB), MongoDB Atlas, BeautifulSoup, Selenium, sentence-transformers, Ollama, the Gemini API, matplotlib.

## Notes

- Team projects list who did which part; I only claim the parts marked as mine.
- Two notebooks (MAC3 imputation and the SQL 2008 analysis) use licensed vendor data supplied for the course. Their printed data tables and the links to the source files have been removed; charts and code are kept, and the headline numbers are in each README.
- In the portfolio semantic search notebook, database connection strings and email addresses are replaced with placeholders.
