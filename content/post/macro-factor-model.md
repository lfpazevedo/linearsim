---
title: Statistical arbitrage with macro-factors
date: 2025-01-31
description: Statistical arbitrage with macro-factors
featured: true
tags:
  - Finance
  - Econometrics
  - BrazilianStocks
  - Risk
  - Portfolio
categories:
  - finance
series:
  - modeling
aliases: 
thumbnail: images/factor.png
---
#### How to reduce portfolio drawdowns using macroeconomic variables

As I showed in my previous [post](https://lfpazevedo.github.io/linearsim/post/ibov-clusters/), **identifying** independent (orthogonal) factors is crucial for explaining variations in asset prices.

When dealing with portfolios containing dozens or even hundreds of assets, we must understand the potential risks our capital is exposed to, so we can either avoid them or bet on a specific scenario to adjust the portfolio’s risk-return as desired.

*In this post, I'll show how adding macro-features can improve portfolio metrics, **especially** drawdowns.*

I present the following factors:
1. **Economic Policy Uncertainty (EPU)** - a local risk factor
2. **VIX** - a global risk factor
3. **GDP Forecast** - local growth risk from Focus

![Factors Correlation Matrix](/linearsim/post/images/ibov-clusters/file-20250124091114763.png)

EPU has a monthly frequency. Therefore, for this post, I used the USDBRL daily series made orthogonal to VIX (specifically, the residual from an OLS regression). If we have a global shock, USD and VIX will move in the same trend and the regression error will be small. Otherwise, a higher error implies the shock was likely local.

Below is a plot of the features used to explain the excess return of the Brazilian stocks in the current Ibovespa index composition:

![Macro Factors](/linearsim/post/images/macro-factor-model/file-20250131142226313.png)

Focus’s GDP forecast for the next 12 months shows a significant increase during the pandemic period. This does not represent an actual revision for better economic conditions; rather, it is a base adjustment due to the economic downturn from the Covid crisis—essentially a **denominator error**. We should treat this data accordingly, but, for simplicity proposes, I'll keep the variable as it is. We should treat this data accordingly, but for simplicity, I will keep the variable as it is. Focus and VIX are properly treated to capture **surprises** (you can find more details [here](https://lfpazevedo.github.io/linearsim/post/ibov-clusters/)).

Another factor used to explain the excess return of stocks is the **market risk factor**. This variable decomposes stock returns into their idiosyncratic and systematic components, allowing us to apply the macro-factor regression only on the idiosyncratic series. I could use the _BOVA11_ contract to represent the market factor, but instead, I used PCA as a proxy. I also used only the current Ibovespa composition, which may cause some optimization and risk-neutralization issues, but for the purpose of this post, I'll keep it simple.

#### Checking the data

Some sectors respond more strongly to certain factors than others. The Covid global shock affected the Industrial sector more than the Consumer Staples sector:

| Sector Cumulative Return                                                  | Sector Cumulative Return                                                  |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| ![](/linearsim/post/images/macro-factor-model/file-20250131144319565.png) | ![](/linearsim/post/images/macro-factor-model/file-20250131144347534.png) |


Real Estate experienced a significant downturn during the local news episode in May 2017, and Consumer Discretionary company prices declined during the 2022 growth revision period.

| Sector Cumulative Return                                                  | Sector Cumulative Return                                                  |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| ![](/linearsim/post/images/macro-factor-model/file-20250131144614391.png) | ![](/linearsim/post/images/macro-factor-model/file-20250131144911578.png) |

Now that I have fitted each stock using a set of features, let’s build a **statistical arbitrage model** and compare the portfolio metrics with and without macro-features.

#### Model

For the purposes of this post, I could use any statistical model. Again, I’ll keep it simple and use a model similar to moving averages. Cointegration and other econometric models would also work.

Below is an example signal for ASAI3. An open position was signaled at the beginning of 2023 and closed in June. Depending on the portfolio weights after risk-return optimization, this signal could generate substantial returns.

![Signal](/linearsim/post/images/macro-factor-model/file-20250131150122475.png)

#### Optimization

I set a constraint allowing only 10% for a long or short position in the same stock. For December 12, 2024, we have the following portfolio. On this date, the portfolio is fully short on ITSA4 and fully long on B3SA3.

![Weights on December 12, 2024](/linearsim/post/images/macro-factor-model/file-20250131150813786.png)

I also added some beta neutrality conditions, which will be discussed in the risk section.

#### Backtest

Finally, the main section of this post: **Does the inclusion of macro-features improve portfolio metrics?**

The image below shows the PnL (Profit and Loss) of three portfolios and their sector breakdown. The **“Optimal Overall”** portfolio uses only the market factor as an explanatory variable, and that factor was constrained to zero during optimization. The red line, **“Featured Optimal,”** shows the PnL of a strategy that includes macro-features in the regressions. In that strategy as well, only the market beta was neutralized. Finally, because VIX includes futures contracts, we could use them to neutralize the beta of that factor; the results are similar to those of the Featured Optimal PnL.

![PnL - Total and breakdown](/linearsim/post/images/macro-factor-model/file-20250131152048431.png)

The table below shows each portfolio’s metrics, with a highlight on drawdown. In this case, **adding macro-features** could save a hedge fund’s life!

| Portfolio         | Annualized Return | Annualized Volatility | Sharpe Ratio | Max Drawdown | 95% VaR |
|------------------|------------------|----------------------|-------------|--------------|---------|
| Optimal Overall  | 0.084            | 0.209                | 0.400       | -50.032      | -0.021  |
| Featured Overall | 0.064            | 0.209                | 0.308       | -5.670       | -0.020  |
| VIX Overall      | 0.064            | 0.208                | 0.309       | -5.596       | -0.021  |


#### Risk

Because the market factor proxy is not perfect, the optimization could not fully neutralize it. The graphs below show that the portfolios’ sensitivity to BOVA11 is far from neutral.

Here are the metrics only for the **“VIX Overall”** portfolio.

| **BOVA11**                                                                | **BOVA11**                                                                |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| ![](/linearsim/post/images/macro-factor-model/file-20250131153354059.png) | ![](/linearsim/post/images/macro-factor-model/file-20250131153342205.png) |

| **VIX**                                                                   | **VIX**                                                                   |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| ![](/linearsim/post/images/macro-factor-model/file-20250131153308795.png) | ![](/linearsim/post/images/macro-factor-model/file-20250131153315176.png) |

| **Focus**                                                                 | **Focus**                                                                 |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| ![](/linearsim/post/images/macro-factor-model/file-20250131153438931.png) | ![](/linearsim/post/images/macro-factor-model/file-20250131153448174.png) |

| **News**                                                                  | **News**                                                                  |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| ![](/linearsim/post/images/macro-factor-model/file-20250131153526472.png) | ![](/linearsim/post/images/macro-factor-model/file-20250131153513452.png) |

Risk analysis helps us identify exposures and optimize management decisions. Is it time to bet on global risks? Given the next election, should we hedge?

**IMPORTANT CONCLUSION**: Some factors (like VIX and the market factor) can be neutralized by available market instruments. **But others (like Growth and the News factor) cannot!!!** This highlights the **importance of having an economist in a quantitative fund** to predict how these factors might move.



**Check my personal dashboard with economic and finance data [here](https://lfpazevedo.pythonanywhere.com)**