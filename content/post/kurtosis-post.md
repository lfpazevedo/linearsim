---
title: Add Volatility to Protect Your Portfolio From Extreme Events
date: 2025-02-25
description: We can go beyond the traditional covariance matrix when building a portfolio.
featured: false
tags:
  - Finance
  - Portfolio
  - Risk
categories:
  - finance
series:
  - modeling
aliases: 
thumbnail: images/kurtosis.png
---
#### We can go beyond the traditional covariance matrix when building a portfolio.

Financial price return distributions are not Gaussian. Extreme events occur far more often than predicted by the normal curve, and human behavior (particularly panic selling) amplifies downside moves, making the distribution of returns left-skewed.

During these extreme events, volatility measures spike. Therefore, adding volatility derivatives (futures, swaps, or even straddles and strangles) to your portfolio can help capture diversification benefits in the higher moments of the distribution.

In this post, I’ll assume we have these instruments available for both the USA and Brazil, and add volatility strategies to a portfolio of bonds and equities. In reality, volatility derivatives are scarce and/or very expensive, often undermining or even making such strategies infeasible.

I will add two measures to the bond and equity portfolio: implied volatility and the volatility risk premium (the difference between implied and realized volatility over a certain period). For the **Implied** measure (**Long Volatility – LV**), I will assume it can be purchased as an index and then we observe the PnL after a set period. The PnL for the **Volatility Risk Premium** (**VRP**) will be the difference between LV and realized volatility over one month.

Because implied volatility is forward-looking—traders might add a premium to option prices ahead of an election, for example—it is considered a better predictor of future volatility than realized volatility, at least outside of crisis periods ([check this comparison between implied and realized volatility during crises](https://ideas.repec.org/a/brf/journl/v13y2015i4p571-630.html)). The PnL comes from the difference between these two measures.

For bond indexes, I used the **S&P U.S. Treasury Bond Index** (calculated by S&P) and the **IMA** (calculated by Anbima). For equity, I used the **S&P 500** and the **IBOV** indexes. For implied volatility, I used the **VIX** for the USA and the **S&P/B3 Ibovespa VIX** for Brazil. Realized volatility was calculated using daily data over a one-month period. Finally, I included the **30-Day Average SOFR** and **CDI** as risk-free rates.

Here is the plot of the series used in this post (weekly frequency):

![](/linearsim/post/images/kurtosis-post/file-20250225181822574.png)

To build the return of the volatility strategies, I leveraged the notional so that the Modified Value-at-Risk (ModVaR) of the volatility strategies would match the ModVaR of the equity strategy. Modified VaR is the traditional VaR expanded via a Taylor series to include up to the fourth moment of the distribution.

Below is the correlation matrix:

| USA                                                                  | Brazil                                                               |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| ![](/linearsim/post/images/kurtosis-post/file-20250225105309633.png) | ![](/linearsim/post/images/kurtosis-post/file-20250225105423907.png) |

The IMA and IBOV show a high weekly correlation, which contrasts with the U.S. market. In both markets, we see a strong negative correlation between LV and equity, while VRP has a smaller correlation.

Here are the descriptive statistics for each individual return series:

## USA

| Asset Class      | Geometric Mean | Annualized Geometric Mean | Median | Minimum | Maximum | Annualized Standard Deviation | Skewness | Kurtosis | Annualized Downside Deviation | Modified VaR | Sharpe Ratio | Success Rate |
|-----------------|---------------|---------------------------|--------|---------|---------|-------------------------------|----------|----------|------------------------------|--------------|--------------|--------------|
| Bond           | 0.02%         | 1.13%                     | 0.06%  | -1.89%  | 2.22%   | 4.56%                         | 0.19     | 0.90     | 3.19%                        | 0.97%        | 0.34         | 52.82%       |
| Equity         | 0.20%         | 10.99%                    | 0.51%  | -16.23% | 11.42%  | 18.69%                         | -0.89    | 7.10     | 13.49%                       | 4.27%        | 0.81         | 57.91%       |
| Long Volatility | 0.33%         | 18.53%                    | -0.44% | -17.50% | 62.11%  | 53.70%                         | 2.23     | 13.62    | 27.92%                       | 4.25%        | 0.66         | 42.94%       |
| VRP           | 0.56%         | 33.68%                    | 1.03%  | -25.37% | 4.76%   | 18.91%                         | -5.12    | 38.21    | 16.25%                       | 4.24%        | 2.07         | 79.66%       |

---

## Brazil

| Asset Class      | Geometric Mean | Annualized Geometric Mean | Median | Minimum | Maximum | Annualized Standard Deviation | Skewness | Kurtosis | Annualized Downside Deviation | Modified VaR | Sharpe Ratio | Success Rate |
|-----------------|---------------|---------------------------|--------|---------|---------|-------------------------------|----------|----------|------------------------------|--------------|--------------|--------------|
| Bond           | 0.14%         | 7.82%                     | 0.18%  | -2.04%  | 1.69%   | 3.48%                         | -0.74    | 3.36     | 2.81%                        | 0.71%        | 2.71         | 67.18%       |
| Equity         | -0.01%        | -0.51%                    | 0.08%  | -7.56%  | 6.78%   | 16.75%                         | 0.03     | 0.44     | 12.50%                       | 3.75%        | -0.06        | 50.77%       |
| Long Volatility | 0.20%         | 11.09%                    | 0.20%  | -7.05%  | 10.78%  | 18.56%                         | 0.47     | 1.46     | 12.30%                       | 3.57%        | 0.89         | 52.82%       |
| VRP           | 3.83%         | 605.09%                   | 4.20%  | -9.49%  | 12.32%  | 28.92%                         | -0.96    | 1.72     | 13.18%                       | 3.55%        | 45.90        | 88.72%       |

Because the ModVaR of equity determined the leverage for the volatility strategies, the risk measure is similar across them.

Even though LV’s Sharpe ratio is lower, it can still help with portfolio diversification due to its positive skewness.

In Brazil’s sample, there is no clear risk–reward trade-off between bonds and equities—bonds have both a lower return and lower volatility than equities.

#### Higher-Moment Correlations

Now let’s check the correlations in the higher moments.

#### USA

- **Coskewness**

|        | Bond² | Equity² | LV²   | VRP²  | Bond*Equity | Bond*LV | Equity*LV |
| ------ | ----- | ------- | ----- | ----- | ----------- | ------- | --------- |
| Bond   | 0.19  | 0.33    | 0.77  | -0.04 |             |         |           |
| Equity | -0.21 | -0.89   | -1.29 | 0.45  |             |         |           |
| LV     | 0.38  | 0.80    | 2.22  | -0.07 | -0.41       |         |           |
| VRP    | -0.36 | -1.82   | -0.43 | -5.10 | -0.06       | -0.03   | 0.47      |
- **Cokurtosis**

|        | Bond³ | Equity³ | LV³   | VRP³  | Bond*Equity² | Bond*LV² | Bond*VRP² | Equity*LV² | Equity*VRP² | Bond²*Equity | Bond²*LV | Equity²*VRP | LV²*VRP | Equity*LV*VRP |
|--------|-------|---------|-------|-------|--------------|----------|-----------|------------|-------------|--------------|----------|-------------|---------|---------------|
| Bond   | 3.87  | -1.07   | 5.44  | 1.26  | 2.18         | 2.80     | 2.14      |            |             |              |          |             |         | 0.41          |
| Equity | 0.15  | 9.98    | -9.67 | -7.04 | -1.07        | -3.02    | 0.10      | 6.20       | 12.64       |              |          |             |         |               |
| LV     | 0.99  | -5.01   | 16.41 | 0.95  | 1.65         |          |           |            | -2.78       | -1.87        |          | -1.90       |         |               |
| VRP    | -0.16 | 3.08    | -3.49 | 40.66 | -0.81        | -1.08    |           | 2.42       |             | 0.54         | -0.82    |             | 2.02    |               |

#### Brazil

- **Coskewness**

|        | Bond² | Equity² | LV²   | VRP²  | Bond*Equity | Bond*LV | Equity*LV |
| ------ | ----- | ------- | ----- | ----- | ----------- | ------- | --------- |
| Bond   | -0.74 | -0.13   | -0.50 | -0.26 |             |         |           |
| Equity | -0.40 | 0.03    | -0.36 | -0.12 |             |         |           |
| LV     | 0.60  | 0.27    | 0.47  | 0.19  | 0.32        |         |           |
| VRP    | -0.55 | -0.22   | -0.22 | -0.95 | -0.33       | 0.33    | 0.15      |
- **Cokurtosis**

|        | Bond³ | Equity³ | LV³   | VRP³  | Bond*Equity² | Bond*LV² | Bond*VRP² | Equity*LV² | Equity*VRP² | Bond²*Equity | Bond²*LV | Equity²*VRP | LV²*VRP | Equity*LV*VRP |
| ------ | ----- | ------- | ----- | ----- | ------------ | -------- | --------- | ---------- | ----------- | ------------ | -------- | ----------- | ------- | ------------- |
| Bond   | 6.25  | 2.17    | -2.42 | 0.56  | 2.61         | 3.58     | 2.06      |            |             |              |          |             |         | -1.02         |
| Equity | 3.61  | 3.40    | -2.28 | 0.35  | 2.17         | 2.18     | 1.00      | 2.06       | 1.03        |              |          |             |         | -0.71         |
| LV     | -3.66 | -1.79   | 4.39  | -0.77 | -1.77        |          |           |            | -0.61       | -2.52        |          | -0.71       | -1.70   | 0.93          |
| VRP    | 1.94  | 0.40    | -1.70 | 4.64  | 0.69         | 1.28     |           | 0.93       |             | 1.14         | -1.55    |             | 1.52    | -0.61         |

A positive coskewness between assets \[i; i; j\] (i.e., twice i, once j) means that asset j has a high return when the volatility of asset i is high. In other words, j is a good hedge against spikes in i. For both the USA and Brazil, the LV strategy is a good hedge when the other assets are volatile.

For cokurtosis, positive values for \[i; i; i; j\] indicate a more negatively skewed distribution for asset i when asset j’s return is lower. This holds for VRP versus equity in the USA, and for bonds and equity in Brazil.

Positive values in \[i; i; j; k\] signal that the covariance between j and k increases when the volatility of i increases. In the USA, the covariance between equities and VRP increases when LV volatility is high. In Brazil, equity and VRP covariance is higher in periods of high bond volatility.

Finally, \[i; i; j; j\] > 0 means the volatility of i and j increases together. We observe this with bonds and LV in both countries.

#### Efficient Frontiers (Mean–ModVaR)

I built the efficient frontier in terms of mean versus Modified VaR (returns vs. VaR). I considered four possible portfolios:

| Portfolio          | Assets Included                       |
| ------------------ | ------------------------------------- |
| Bond+Equity        | ret_bond, ret_equity                  |
| Bond+Equity+LV     | ret_bond, ret_equity, ret_lv          |
| Bond+Equity+VRP    | ret_bond, ret_equity, ret_vrp         |
| Bond+Equity+LV+VRP | ret_bond, ret_equity, ret_lv, ret_vrp |

- **No Short-Selling**: In Brazil, under this constraint, equities often end up with zero weight for any given risk level.

| USA                                                                  | Brazil                                                               |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| ![](/linearsim/post/images/kurtosis-post/file-20250225111654630.png) | ![](/linearsim/post/images/kurtosis-post/file-20250225111702265.png) |

When we add volatility strategies (still without short-selling), returns jump at the same level of risk:

| USA                                                                  | Brazil                                                               |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| ![](/linearsim/post/images/kurtosis-post/file-20250225111522167.png) | ![](/linearsim/post/images/kurtosis-post/file-20250225111526231.png) |

- **Allowing Short-Selling**:

| USA                                                                  | Brazil                                                               |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| ![](/linearsim/post/images/kurtosis-post/file-20250225112352881.png) | ![](/linearsim/post/images/kurtosis-post/file-20250225112404999.png) |

#### Optimal Portfolio Weights and Performance Metrics

- **USA (no short-selling):**

| Metric               | Bond+Equity | Bond+Equity+LV | Bond+Equity+VRP | Bond+Equity+LV+VRP |
|----------------------|-------------|----------------|-----------------|--------------------|
| Mean Ann. Return    | 0.0538      | 0.1677         | 0.1512          | 0.2100             |
| Ann. Std. Dev.      | 0.0764      | 0.1051         | 0.0857          | 0.0906             |
| Skewness            | -0.4565     | -0.4044        | -4.2346         | -3.6782            |
| Kurtosis            | 4.7170      | 11.6919        | 27.6714         | 27.5831            |
| Min Monthly Loss    | -0.0569     | -0.1052        | -0.0943         | -0.1126            |
| Max Monthly Gain    | 0.0472      | 0.0730         | 0.0301          | 0.0420             |
| Mod. VaR (95%)      | 0.0167      | 0.0189         | 0.0203          | 0.0195             |
| Sharpe Ratio        | 0.7043      | 1.5949         | 1.7653          | 2.3178             |
| Success Rate        | 0.5847      | 0.5876         | 0.7599          | 0.7514             |
|=====================|=============|================|=================|====================|
| Bond                | 0.6215      | 0.0000         | 0.4557          | 0.0642             |
| Equity              | 0.3785      | 0.7464         | 0.1242          | 0.4247             |
| LV                  | NaN         | 0.2536         | NaN             | 0.1597             |
| VRP                 | NaN         | NaN            | 0.4201          | 0.3514             |


- **Brazil (no short-selling):**

| Metric               | Bond+Equity | Bond+Equity+LV | Bond+Equity+VRP | Bond+Equity+LV+VRP |
|----------------------|-------------|----------------|-----------------|--------------------|
| Mean Ann. Return    | 0.0759      | 0.0806         | 0.7374          | 0.4993             |
| Ann. Std. Dev.      | 0.0348      | 0.0301         | 0.1023          | 0.0642             |
| Skewness            | -0.7358     | -0.1117        | -1.0796         | -0.9015            |
| Kurtosis            | 3.247       | 1.2338         | 1.996           | 1.9371             |
| Min Monthly Loss    | -0.0204     | -0.0132        | -0.0388         | -0.0285            |
| Max Monthly Gain    | 0.0169      | 0.0159         | 0.0433          | 0.0291             |
| Mod. VaR (95%)      | 0.0071      | 0.0053         | 0.0126          | 0.0068             |
| Sharpe Ratio        | 2.1839      | 2.6752         | 7.2069          | 7.7722             |
| Success Rate        | 0.6718      | 0.6667         | 0.8872          | 0.8923             |
|=====================|=============|================|=================|====================|
| Bond                | 1.0         | 0.8996         | 0.6618          | 0.6454             |
| Equity              | 0.0         | 0.0            | 0.0             | 0.0                |
| LV                  | NaN         | 0.1004         | NaN             | 0.1414             |
| VRP                 | NaN         | NaN            | 0.3382          | 0.2132             |


- **USA (allowing short-selling):**

| Metric               | Bond+Equity | Bond+Equity+LV | Bond+Equity+VRP | Bond+Equity+LV+VRP |
|----------------------|-------------|----------------|-----------------|--------------------|
| Mean Ann. Return    | 0.0538      | 0.1880         | 0.1512          | 0.2102             |
| Ann. Std. Dev.      | 0.0764      | 0.1177         | 0.0857          | 0.0907             |
| Skewness            | -0.4565     | -0.4808        | -4.2346         | -3.6814            |
| Kurtosis            | 4.7170      | 12.1683        | 27.6714         | 27.6217            |
| Min Monthly Loss    | -0.0569     | -0.1197        | -0.0943         | -0.1128            |
| Max Monthly Gain    | 0.0472      | 0.0829         | 0.0301          | 0.0420             |
| Mod. VaR (95%)      | 0.0167      | 0.0214         | 0.0203          | 0.0196             |
| Sharpe Ratio        | 0.7043      | 1.5969         | 1.7653          | 2.3178             |
| Success Rate        | 0.5847      | 0.5819         | 0.7599          | 0.7514             |
|=====================|=============|================|=================|====================|
| Bond                | 0.6215      | -0.1300        | 0.4557          | 0.0629             |
| Equity              | 0.3785      | 0.8429         | 0.1242          | 0.4256             |
| LV                  | NaN         | 0.2870         | NaN             | 0.1598             |
| VRP                 | NaN         | NaN            | 0.4201          | 0.3517             |


- **Brazil (allowing short-selling):**

| Metric               | Bond+Equity | Bond+Equity+LV | Bond+Equity+VRP | Bond+Equity+LV+VRP |
|----------------------|-------------|----------------|-----------------|--------------------|
| Mean Ann. Return    | 0.0847      | 0.0844         | 0.6056          | 0.4904             |
| Ann. Std. Dev.      | 0.0322      | 0.0298         | 0.0823          | 0.0630             |
| Skewness            | -0.3579     | -0.0561        | -1.0309         | -0.8903            |
| Kurtosis            | 1.1747      | 0.9275         | 1.8786          | 1.8895             |
| Min Monthly Loss    | -0.0152     | -0.0110        | -0.0321         | -0.0280            |
| Max Monthly Gain    | 0.0156      | 0.0155         | 0.0351          | 0.0283             |
| Mod. VaR (95%)      | 0.0060      | 0.0051         | 0.0098          | 0.0067             |
| Sharpe Ratio        | 2.6258      | 2.8345         | 7.3588          | 7.7816             |
| Success Rate        | 0.6872      | 0.6718         | 0.8821          | 0.8872             |
|=====================|=============|================|=================|====================|
| Bond                | 1.1303      | 1.0075         | 0.8543          | 0.6857             |
| Equity              | -0.1303     | -0.0775        | -0.1210         | -0.0245            |
| LV                  | NaN         | 0.0700         | NaN             | 0.1308             |
| VRP                 | NaN         | NaN            | 0.2667          | 0.2080             |


###### **Of course, a Sharpe ratio of 7 (as shown for Brazil) is unrealistically high. However, the purpose of this post is simply to illustrate how higher moments of the distribution can be incorporated into portfolio construction to hedge against extreme events.**


**Check my personal dashboard with economic and finance data [here](https://lfpazevedo.pythonanywhere.com)**