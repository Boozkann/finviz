# Metrics & Indicator Definitions (FinViz Project)

## 🔹 Technical Indicators

**EMA (Exponential Moving Average)**
\[
EMA_t = Price_t * k + EMA_{t-1} * (1 - k)
\]
where \( k = 2 / (N + 1) \)

- EMA(20), EMA(50): short & medium trend indicators.


**RSI (Relative Strength Index)**
\[
RSI = 100 - \frac{100}{1 + RS}
\]
where \( RS = \frac{average\_gain}{average\_loss} \) (usually 14 periods)


**MACD (Moving Average Convergence Divergence)**
\[
MACD = EMA_{12} - EMA_{26}
\]
Signal line = EMA9(MACD)
Histogram = MACD - Signal


## 🔹 Fundamental Ratios

**P/E (Price to Earnings):**
\[
P/E = \frac{Share Price}{Earnings per Share}
\]

**PEG Ratio:**
\[
PEG = \frac{P/E}{EPS Growth Rate}
\]

**Debt/Equity:**
\[
D/E = \frac{Total Liabilities}{Shareholder Equity}
\]

**EBITDA Margin:**
\[
EBITDA\ Margin = \frac{EBITDA}{Revenue}
\]

**Price/Sales:**
\[
P/S = \frac{Market Cap}{Revenue}
\]


**Notes:**
- All metrics sourced from Financial Modeling Prep or calculated from base values.
- Technical indicators computed by Lambda (`ingest_prices`).
- Fundamental metrics refreshed daily via `fundamentals_loader`.
