# Semantic search and thematic portfolios over Russell 1000 companies

Course: Data Warehousing, Hofstra University, Spring 2026 (Prof. Tzer Hung Low). Project 3.

Team project ("Team W"), four members. Section owners as marked in the notebook:

| Part | Owner |
|---|---|
| 1. Company text collection pipeline | Muhammad Talha Khan (me) |
| 2. LLM summarisation; 3. embedding pipelines | Alexander Khan |
| 3.5 to 3.9 embedding evaluation; 4. search engine | Zeeshan Khan |
| 5. Thematic portfolios | Samantha Nainan |
| Extras: system check widget, AI exposure classification | Muhammad Talha Khan (me) |

The notebook is `portfolio_semantic_search.ipynb`. See "Notebook" below.

## Question

Can short LLM-written summaries of each company support better company search and grouping than raw Wikipedia text?

## Data

- Holdings of the iShares Russell 1000 ETF (IWB) from ishares.com, 988 companies after cleaning.
- Company text from Wikipedia, with Yahoo Finance (`yfinance`) and one company website as fallbacks.
- One year of daily prices from Yahoo Finance.
- Stored in MongoDB Atlas.

## Approach

- Part 1 (mine). Load the holdings into MongoDB with a unique index. Resolve each company to a Wikipedia page in passes: the `wikipedia` library with a ticker check, a data-quality pass that removes wrong matches, a Bing search through Selenium, then `yfinance`, then the company's own site. Each pass only works on records that are still unresolved, so it can be rerun.
- Part 2. Map-reduce summarisation with a local model (gemma3n:e2b through Ollama), 1,500-word chunks, JSON output checked with Pydantic.
- Part 3. Embed raw text, summaries, and chunked text with seven model settings (23 configurations). Score by silhouette against sector labels and by overlap with stock-return correlation neighbours.
- Part 4. Keyword, stemmed keyword, vector and hybrid (reciprocal rank fusion) search in MongoDB Atlas, scored on 32 test queries.
- Part 5. An LLM filter assigns companies to 10 themes; equal-weighted theme portfolios are backtested.
- Extras (mine). A widget that checks every record is complete and runs searches. A classifier that labels each company's AI exposure, compared with a published analyst list.

## Results

All numbers are printed in the notebook.

- Part 1: all 988 companies resolved. Source: Wikipedia 88.2%, yfinance 9.8%, Bing 1.9%, company site 1 company.
- Part 2: 986 of 988 companies summarised in the main run.
- Part 3: best configuration was summaries with the nomic "classification" prefix. Silhouette 0.0818, against 0.0455 for the best raw-text setting and 0.0606 for the best chunked setting. On average 30.6% of its top 25 neighbours were also top 25 by return correlation (972 tickers). The silhouette scores are low in absolute terms; sectors overlap heavily.
- Part 4: vector search scored best. Mean precision@10 0.303 and mean reciprocal rank 0.601, against 0.256 and 0.582 for hybrid and 0.184 and 0.488 for plain keyword search. One text cell in the notebook names hybrid as the winner; the printed table does not support that.
- Part 5: 300 classifications across 10 themes, 278 marked core. The best theme portfolio in the backtest was quantum and advanced semiconductors, up 255.3%.
- AI exposure extra: the classifier agreed with the analyst list on some names (Microsoft, Amazon, Salesforce, ServiceNow, Adobe) and disagreed on others (Alphabet, Meta, HubSpot, UiPath).

## Notebook

`portfolio_semantic_search.ipynb` is the team notebook with its outputs. Database connection strings and email addresses have been replaced with placeholders, so it will not run against the original database as is.

## Skills

MongoDB (indexes, Atlas Search, vector search), web scraping (wikipedia, BeautifulSoup, Selenium), resumable multi-pass pipelines, data-quality checks, local LLMs with Ollama, Pydantic, sentence-transformers, clustering and retrieval metrics (silhouette, precision@k, reciprocal rank), yfinance.
