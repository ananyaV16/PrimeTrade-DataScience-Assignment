# Bitcoin Market Sentiment vs. Hyperliquid Trader Performance

A data-science analysis of whether the Bitcoin **Fear & Greed Index** is associated
with measurable differences in **trader behavior and profitability** on the
Hyperliquid perpetuals exchange.

## 📁 Project Structure

```
.
├── Fear_Greed_vs_Hyperliquid_Trader_Performance.ipynb   # Main analysis notebook
├── report.md                                            # 2-page project report
├── requirements.txt                                     # Python dependencies
└── README.md                                            # This file
```

## 🎯 Objective

Investigate whether market sentiment (Fear vs. Greed) is linked to differences in:

- Average PnL and win rate
- Leverage usage and trade sizing
- Long/short positioning
- Coin selection and trading volume
- Trader-level and daily profitability

...and quantify these relationships with proper statistical testing rather than
relying on visual inspection alone.

## 📊 Datasets

| File | Description |
|------|-------------|
| `fear_greed_index.csv` | Daily Bitcoin Fear & Greed Index value (0–100) and classification label |
| `historical_data.csv` | Hyperliquid trade-level fills: account, coin, side, size, execution price, closed PnL, fee, leverage |

> **These CSVs are not included in this repository.** Place them in the notebook's
> working directory (or update the paths in the "Load Datasets" section) before
> running. If they are not found, the notebook automatically generates a
> **clearly-labelled synthetic dataset** with a matching schema so that every cell
> still executes for review purposes.

## 🚀 How to Run

### Option A — Google Colab (recommended)
1. Upload `Fear_Greed_vs_Hyperliquid_Trader_Performance.ipynb` to
   [Google Colab](https://colab.research.google.com).
2. Upload `fear_greed_index.csv` and `historical_data.csv` via the Colab file
   browser (or mount Google Drive and update the paths in Section 3 of the notebook).
3. Run all cells (`Runtime → Run all`).

### Option B — Local Jupyter
```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook Fear_Greed_vs_Hyperliquid_Trader_Performance.ipynb
```
Place the two CSV files in the same folder as the notebook, then run all cells.

## 🧠 Methodology Summary

1. **Load** both datasets (with automatic column-name normalization to handle
   minor schema variations across data sources).
2. **Clean** timestamps (supports epoch seconds, milliseconds, or date strings)
   and standardize sentiment labels into `Fear` / `Neutral` / `Greed` buckets.
3. **Handle missing values** contextually (drop unusable rows for performance
   metrics, impute leverage by coin median, treat missing fees as zero).
4. **Engineer features**: win/loss flag, long/short flag, PnL %, daily aggregates,
   7-day rolling averages.
5. **Merge** on calendar date to align trade-level activity with the daily
   sentiment reading.
6. **Explore** distributions and regime comparisons across 13+ visualizations,
   each followed by an auto-generated, data-driven insight.
7. **Test formally**: Welch's t-test, Mann-Whitney U, chi-square test of
   independence, and Pearson correlation with p-values — not just chart-reading.
8. **Conclude** with key findings, business recommendations, limitations, and
   future improvements.

## 📈 Key Analyses Included

- Average PnL: Fear vs. Greed
- Win rate: Fear vs. Greed
- Average leverage and trade size by sentiment
- Long vs. short positioning distribution
- Most traded coins
- Trader-level profitability (top performers, profitable share)
- Daily profitability time series overlaid with sentiment
- Correlation matrix / heatmap of daily metrics
- Boxplots, histograms, and scatter plots
- Formal statistical significance testing

## ⚠️ Limitations

See the **Limitations** section at the end of the notebook and in `report.md` for a
full discussion — in short: daily sentiment vs. intraday trades granularity
mismatch, no causal claims, BTC-specific sentiment applied to a multi-asset trading
book, and single-venue data coverage.

## 🔮 Future Work

Intraday sentiment proxies, lead-lag/Granger causality analysis, trader cohort
segmentation, volatility-adjusted (risk-adjusted) PnL metrics, and an exploratory
classification model — all detailed in the notebook's final section.

## 🛠️ Tech Stack

`pandas` · `numpy` · `matplotlib` · `seaborn` · `scipy.stats`

---
*Prepared as part of a Data Science internship application assignment.*
