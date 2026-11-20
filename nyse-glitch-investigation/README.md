# NYSE opening glitch: pandas investigation

Course: Data Warehousing, Hofstra University, Spring 2026 (Prof. Tzer Hung Low). Short individual exercise.

## Question

On January 24, 2023 a fault at the NYSE open produced trades at wrong prices, and the exchange later cancelled ("busted") trades outside a price band for each stock. For UBER: which of six early trades survived, and what was a surviving trade near the edge of the band worth at the close?

## Data

- The NYSE's published list of bust prices per symbol (public Excel file on nyse.com).
- Six UBER trade records given in the exercise.
- UBER daily prices from Yahoo Finance through `yfinance`.

## Approach

Read the NYSE file with pandas and look up the UBER band. Parse the pipe-delimited trade records into a DataFrame. Mark a trade as survived only if its price is strictly inside the band. Look up the closing price for that day and compute the profit on the surviving trade.

## Result

- UBER band: trades below $28.50 or above $31.50 were busted.
- Five of the six trades (prices $25.67 to $25.87) were busted. One survived: 4,575 shares at $28.51, one cent above the lower bound.
- UBER closed at $29.93. Holding the surviving trade to the close made $1.42 per share, $6,496.50 in total.

This is consistent with someone buying just inside the band, but six trades cannot show intent.

## Files

- `nyse_glitch_investigation.ipynb`: the notebook with outputs.

## Skills

pandas (reading Excel from a URL, parsing delimited text, boolean filtering), yfinance.
