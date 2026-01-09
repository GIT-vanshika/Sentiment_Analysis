# Trader Behaviour & Bitcoin Sentiment Analysis

## Overview

This project analyzes how **retail trader behaviour and performance** change with the
**Bitcoin Fear & Greed Index**. The goal is to understand:

- How PnL, win rate, trade size, and trading activity vary across sentiment regimes  
  (Extreme Fear → Extreme Greed).
- How different **types of traders** (top vs bottom performers) behave under these regimes.
- How trader performance and sentiment **evolve over time**.

All analysis is implemented in `notebook_2.ipynb`.

---

## Data

The project combines two main sources:

- **Historical trader data**  
  - Trade‑level records aggregated to **account–day** level.  
  - Key engineered metrics per account–day:
    - `total_daily_pnl`
    - `win_rate`
    - `num_trades`
    - `avg_size_usd` (average trade size in USD)
    - `avg_fee_usd` (average fee per trade)
- **Bitcoin Fear & Greed Index**
  - `value` (0–100 scale; lower = Fear, higher = Greed)
  - Mapped into discrete `sentiment_class`:
    - `Extreme Fear`, `Fear`, `Neutral`, `Greed`, `Extreme Greed`

The two datasets are merged on **date**, so each account–day is associated with
the corresponding sentiment value and class.[web:168][web:439]

---

## Methodology

All analysis is exploratory (EDA), done with pandas, NumPy, and Seaborn:

1. **Preprocessing & feature engineering**
   - Cleaned raw trade data, converted timestamps to dates.
   - Aggregated trades to account–day level and computed:
     `total_daily_pnl`, `win_rate`, `num_trades`, `avg_size_usd`, `avg_fee_usd`.
   - Joined with daily sentiment values and created `sentiment_class` bins.

2. **Sentiment‑level behaviour**
   - Grouped by `sentiment_class` and analyzed:
     - Average daily PnL, win rate.
     - Average number of trades, trade size, and fees.
   - Visualized with bar plots and error bars to show variability.

3. **Per‑account behaviour under sentiment**
   - Aggregated by `["Account", "sentiment_class"]`.
   - Ranked accounts by overall average PnL and created performance groups:
     **Top** vs **Bottom** accounts.
   - Compared:
     - Avg PnL of Top/Bottom groups across sentiment classes.
     - Changes in `avg_size_usd` and `num_trades` between Fear and Greed.

4. **Correlation analysis**
   - Computed Pearson correlations between numeric features
     (`value`, `total_daily_pnl`, `win_rate`, `num_trades`, `avg_size_usd`, etc.).
   - Visualized representative scatter plots to confirm relationships are weak.

5. **Time‑series analysis**
   - Built daily series of average PnL, trading activity, and sentiment.
   - Applied **7‑day rolling means** to smooth noise.
   - Plotted:
     - Daily PnL trend over time.
     - Daily sentiment trend over time.
     - Daily number of trades trend over time.

---

## Key Findings (high‑level)

- **Sentiment classes**
  - Extreme Fear days show **higher trading activity** and **high variability** in PnL and win rate.
  - Extreme Greed days have **fewer trades** but **strong win rates and solid PnL**, with smaller
    average trade size and fees.
  - Neutral days sit in the **middle** for most metrics.

- **Account behaviour**
  - **Top accounts** are profitable across all sentiment regimes, including Fear and Extreme Fear.
  - **Bottom accounts** lose money in all regimes, even during Greed and Extreme Greed.
  - Top accounts tend to **increase position size** (and sometimes trade frequency) on Greed days
    compared with Fear; bottom accounts stay small and relatively stable.

- **Correlations**
  - No pair of numeric features shows strong correlation; absolute coefficients stay well below 0.3.
  - Sentiment value alone does **not** strongly predict PnL or trading activity.

- **Time evolution**
  - A pronounced high‑PnL regime in early 2025 coincides with sustained higher sentiment values
    (Greed/Extreme Greed) and elevated trading activity.

These observations suggest that **trader skill and discipline** matter more than raw sentiment, but
sentiment regimes still shape how aggressively top traders deploy risk.

---

## How to Run

1. Install dependencies (example):

   ```bash
   pip install -r requirements.txt
## Access Full Summary Here
Notion Link: https://www.notion.so/Trader-Behaviour-under-Bitcoin-Fear-Greed-Sentiment-2ddbab83bfd580f4ab2cd180de6d90f9?source=copy_link