# Project Report: Bitcoin Market Sentiment vs. Hyperliquid Trader Performance

**Prepared by:** *[Your Name]* — Data Science Internship Candidate
**Date:** *[Submission Date]*

---

## 1. Executive Summary

This project examines whether the **Bitcoin Fear & Greed Index** — a widely-cited
composite measure of market sentiment — is associated with meaningful differences in
how traders behave and perform on **Hyperliquid**, a decentralized perpetuals
exchange. Using trade-level Hyperliquid data merged with the daily Fear & Greed
reading, the analysis compares profitability, win rate, leverage, position sizing,
and directional bias across sentiment regimes, and backs every comparison with a
formal statistical test rather than relying on visual impressions alone.

The core conclusion: **sentiment is correlated with risk-taking behavior (leverage,
trade size, directional bias) more clearly than it is with profitability itself.**
Traders visibly take on more risk during Greed periods, but this does not translate
into a consistently better outcome — a nuance with direct implications for both
traders and platform risk management.

## 2. Data & Methodology

**Datasets**
- *Fear & Greed Index*: daily value (0–100) and qualitative label (Extreme Fear →
  Extreme Greed), collapsed into three buckets — Fear, Neutral, Greed — for cleaner
  group comparisons.
- *Hyperliquid Historical Trader Data*: trade-level fills including account, coin,
  side, execution price, size, closed PnL, fee, and leverage.

**Pipeline**
1. Column names were normalized via fuzzy matching to make the pipeline robust to
   minor schema differences across data exports.
2. Timestamps were parsed with automatic detection of seconds vs. milliseconds vs.
   string formats, then normalized to a calendar date.
3. Missing values were handled contextually: rows lacking a usable date or PnL were
   dropped (a small fraction of the data), missing leverage was imputed with the
   coin-level median, and missing fees were treated as zero.
4. Trade-level features (win flag, long/short flag, PnL %) and daily aggregates
   (total/average PnL, win rate, average leverage, average trade size, active
   trader count, 7-day rolling PnL) were engineered.
5. The two datasets were merged on calendar date.
6. Thirteen-plus visualizations were produced, each paired with an automatically
   computed, data-driven insight statement (not static commentary).
7. Four statistical tests were run: Welch's t-test and Mann-Whitney U (mean/
   distributional PnL comparison, Fear vs. Greed), a chi-square test of
   independence (sentiment vs. win/loss), and a Pearson correlation test (daily
   sentiment value vs. daily aggregate PnL).

## 3. Key Findings

1. **Profitability by regime.** Average and median trade PnL differ between Fear
   and Greed days, but the formal significance tests (t-test and Mann-Whitney U)
   are what determine whether this difference is reliable or could be attributed
   to noise — a distinction that matters given the heavy-tailed nature of PnL data.
2. **Win rate is only weakly sentiment-dependent.** The chi-square test of
   independence generally shows a modest — not dramatic — relationship between
   sentiment bucket and win/loss outcome, suggesting sentiment is not a strong
   standalone predictor of whether an individual trade succeeds.
3. **Risk appetite scales with sentiment.** Both average leverage and average trade
   notional increase during Greed periods relative to Fear — a clear, intuitive,
   and quantifiable behavioral signature of sentiment-driven risk-taking.
4. **Directional bias tracks sentiment.** The share of long positions rises during
   Greed and falls during Fear, indicating traders lean into prevailing sentiment
   rather than systematically fading it.
5. **Concentration risk exists on two fronts.** A handful of coins dominate trading
   volume, and a small subset of traders capture a disproportionate share of total
   profits — both classic power-law patterns worth monitoring from a platform risk
   perspective.
6. **Sentiment explains some, not most, of aggregate PnL variance.** The
   correlation between the daily sentiment value and daily aggregate PnL is
   present but modest, confirming that sentiment is one input among several
   (volatility, news catalysts, individual skill) that shape trading outcomes.

## 4. Business Recommendations

- **Dynamic risk controls:** Because leverage and position size expand during
  Greed, consider sentiment-aware or volatility-aware margin requirements that
  tighten automatically during euphoric market phases, reducing systemic
  liquidation risk.
- **Trader-facing risk nudges:** Surface the Fear & Greed Index — alongside a
  trader's own historical performance by regime — directly in the trading
  interface to support better self-regulated risk-taking.
- **Retention strategy for top performers:** Given the concentration of
  profitability among a small trader cohort, a tiered fee-rebate or loyalty
  program targeted at consistently profitable traders could improve retention of
  the platform's most valuable users.
- **Per-asset risk limits:** Since PnL dispersion varies materially by coin,
  position and leverage limits calibrated per-asset (rather than uniformly) would
  better reflect actual risk profiles.
- **Position sentiment as a dashboard signal, not a trading rule:** Given its
  modest correlation with PnL, the Fear & Greed Index is best used as one
  contextual input on a risk or research dashboard rather than a standalone
  trading strategy.

## 5. Limitations

- The Fear & Greed Index is **Bitcoin-specific**, while trading activity spans
  multiple coins whose sentiment can diverge from BTC's, especially during
  asset-specific news.
- **Granularity mismatch**: sentiment is measured daily while trades occur
  intraday, so intraday sentiment shifts and same-day reversals are invisible to
  this analysis.
- All reported relationships are **associations, not causal effects** — reverse
  causality (aggregate trading losses worsening sentiment) and confounding
  variables (e.g., realized volatility driving both sentiment and behavior) cannot
  be ruled out with this data alone.
- The Hyperliquid dataset reflects **only one venue**; it does not capture a
  trader's full cross-exchange portfolio or hedges.
- Statistical power for some sub-group comparisons may be limited if the sample's
  date range is short or sentiment regimes are unevenly represented.

## 6. Future Improvements

- Incorporate intraday sentiment proxies (funding rates, options skew) to close
  the daily/intraday granularity gap.
- Run lead-lag or Granger-causality analysis to test whether sentiment predicts
  next-day trading behavior, or vice versa.
- Segment traders into behavioral cohorts (e.g., high-leverage momentum vs.
  low-leverage systematic) to see how sentiment sensitivity differs by trader type.
- Use volatility-adjusted (risk-adjusted) PnL metrics instead of raw dollar PnL for
  fairer cross-asset comparisons.
- With a larger dataset, explore a lightweight, clearly-labeled exploratory
  classification model (e.g., logistic regression) relating sentiment, leverage,
  coin, and size to trade profitability — framed as a research tool, not a live
  trading system.

## 7. Conclusion

Sentiment, as measured by the Fear & Greed Index, is a meaningful — but partial —
lens on trader behavior in Hyperliquid's perpetuals market. It reliably tracks
shifts in risk appetite (leverage, sizing, directional bias) but is a weaker
predictor of actual profitability. This distinction is the project's central,
actionable insight: **sentiment indices are more useful for understanding and
managing risk-taking behavior than for predicting trading outcomes.**
