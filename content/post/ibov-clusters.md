---
title: Clustering Stocks
date: 2025-01-24
description: Ibov stocks vs EPU, Focus, VIX
featured: true
tags:
  - Finance
  - Econometrics
  - RegressionAnalysis
  - RiskAversion
  - Clustering
  - BrazilianStocks
  - Ibovespa
  - DataVisualization
  - ARModels
  - Orthogonality
categories:
  - finance
series:
  - curiosity
aliases: 
thumbnail: images/cluster.png
---

#### Checking Betas Over (Almost) Orthogonal Factors

If we have **two uncorrelated (orthogonal) explanatory variables**, we get the same betas whether we run a **single regression** that includes both variables or **two separate regressions** for each variable.

For instance, **heavy rainfall in Australia affects gold prices**, and **U.S. monetary policy also affects gold prices**. Clearly, these factors are **uncorrelated**. Regressions of **gold prices against rainfall** and **gold prices against federal funds rates**, or a **single regression of gold prices against both rainfall and federal funds rates**, would yield **similar betas**.

Ignoring **causality tests**—to preserve my **visually appealing plots**—I built some **factors that, theoretically, should be orthogonal**. Using the **betas from individual regressions**, I **clustered Brazilian stocks** that compose the **Ibovespa Index**.

Given Brazil's **limited influence** on global financial markets, we can argue that **global shocks** are not caused by local Brazilian shocks. Furthermore, we can stretch this argument and suggest that **policy uncertainty** is not strongly related to **economic growth** (good luck to entrepreneurs trying to optimize production amidst the **constant news flow from Brasília**...).

In this context, I developed **three proxies** to represent **global risk aversion, local risk aversion,** and **local growth**. **Why only three?** To create **visually pleasing 3D plots**...

- **Global risk aversion**: Represented by the **VIX volatility index**, based on options from the **S&P 500**.
- **Local risk aversion**: Measured by the **Economic Policy Uncertainty (EPU)** index, which tracks keywords in **Brazilian news**.
- **Local growth**: Derived from **GDP growth forecasts** in the **Focus Report** collected by the **Brazilian Central Bank (BCB)**. **Quarterly forecasts** were interpolated to create a **12-month-ahead GDP forecast proxy**.

Since **price changes reflect unpredictable news**, I used the **error of an AR(1) process** as a proxy for surprises. A **volatile month predicts another volatile month**, and the same principle applies to **EPU**. We can also assume **inertia in forecast changes**.

Below is the **correlation between the constructed factors**. While the correlations are **not zero**, they are close enough to zero to support the assumption of **near-orthogonality**.

![Factors Correlation Matrix](/linearsim/post/images/ibov-clusters/file-20250124091114763.png)

Another way to visualize **pairwise correlations** is as follows:

![Pairwise Correlation](/linearsim/post/images/ibov-clusters/file-20250124091222872.png)

The **low correlations** confirm we're on the **right track**. Some **stock prices respond to global risk aversion**, while others are more **sensitive to local factors**. The **coefficients align with economic logic**: stock prices **drop when EPU and VIX rise unexpectedly**, while they **increase with positive GDP growth forecast revisions**.

The clustering results are summarized below. For example, **some banks cluster together**—this makes sense—but other combinations, like **Aviation and Healthcare**, may seem unrelated.

![Cluster Visualization](/linearsim/post/images/ibov-clusters/file-20250124093027477.png)

The plot suggests the presence of approximately **five clusters** (the **silhouette score** also supports this—**higher is better**). **ALOS3** and **BRAV3** are **outliers**, forming their own clusters. For **visual purposes**, I removed these stocks from the dataset, resulting in **three clusters**.

| Number of Clusters | Silhouette Score |
|--------------------|------------------|
| 2                  | 0.51             |
| 3                  | 0.53             |
| 4                  | 0.50             |
| 5                  | 0.48             |
| 6                  | 0.47             |
| 7                  | 0.49             |
| 8                  | 0.49             |
| 9                  | 0.51             |
| 10                 | 0.50             |

![Cluster Scores](/linearsim/post/images/ibov-clusters/file-20250124094554864.png)

![Cluster Breakdown](/linearsim/post/images/ibov-clusters/file-20250124094706535.png)

We observe that **some stocks in Cluster 2** are particularly **sensitive (relatively speaking) to local risk aversion**, while their **sensitivity to Focus and VIX factors** is comparable.

![Cluster Sensitivity](/linearsim/post/images/ibov-clusters/file-20250124095535638.png)

As mentioned earlier, I **did not assess parameter significance** or **regression goodness-of-fit**. This study simply highlights the **importance of identifying nearly orthogonal factors** that represent **different risks affecting stock prices**.

![Interval - EPU](/linearsim/post/images/ibov-clusters/file-20250124121103865.png)

![Interval - VIX](/linearsim/post/images/ibov-clusters/file-20250124121114402.png)

![Interval - Focus](/linearsim/post/images/ibov-clusters/file-20250124121121045.png)

If you want to take a step forward, you can use this type of clustering approach to enhance hedge and arbitrage strategies.

##### Appendix
**Parameters and clusters**—monthly data from **2015 to 2024**:

| Ticker  | Betas EPU | Betas VIX | Betas Focus | Cluster |
|---------|-----------|-----------|-------------|---------|
| ASAI3   | -19.97    | -0.62     | 0.17        | 0       |
| AURE3   | 94.48     | -15.85    | 0.12        | 0       |
| BRAP4   | 35.00     | -8.86     | 0.37        | 0       |
| BBAS3   | 8.25      | -17.46    | 0.49        | 0       |
| BRFS3   | -8.43     | -12.92    | 0.61        | 0       |
| CMIG4   | -8.60     | -13.71    | 0.70        | 0       |
| CPLE6   | 29.74     | -15.89    | 0.88        | 0       |
| ELET3   | -42.87    | -6.14     | 0.42        | 0       |
| ELET6   | -49.57    | -7.27     | 0.47        | 0       |
| GGBR4   | -7.52     | -15.64    | 0.15        | 0       |
| GOAU4   | 13.27     | -11.80    | 0.21        | 0       |
| GOLL4   | -47.14    | -5.48     | 0.04        | 0       |
| JBSS3   | -30.24    | -7.05     | 0.53        | 0       |
| MGLU3   | -4.44     | -4.55     | 0.40        | 0       |
| MRFG3   | 53.28     | -10.35    | 0.68        | 0       |
| PETR4   | -45.44    | -19.06    | 0.76        | 0       |
| PRIO3   | 2.20      | -14.58    | 0.46        | 0       |
| SUZB3   | -39.66    | -11.05    | 0.61        | 0       |
| TAEE11  | -25.36    | -23.08    | 0.94        | 0       |
| UGPA3   | -45.47    | -15.44    | 0.51        | 0       |
| USIM5   | 15.06     | -9.82     | 0.22        | 0       |
| VALE3   | 72.10     | -7.57     | 0.34        | 0       |
| AZUL4   | -69.07    | -9.11     | -0.26       | 1       |
| AZZA3   | -123.02   | -18.46    | 0.34        | 1       |
| B3SA3   | -90.56    | -13.64    | 0.48        | 1       |
| BBSE3   | -131.60   | -27.23    | 0.71        | 1       |
| BBDC3   | -84.53    | -15.17    | 0.31        | 1       |
| BBDC4   | -91.50    | -13.78    | 0.28        | 1       |
| BRKM5   | -130.83   | -14.33    | 0.61        | 1       |
| CCRO3   | -77.24    | -18.48    | 0.85        | 1       |
| CMIN3   | -101.81   | -4.80     | 0.07        | 1       |
| COGN3   | -116.98   | -15.16    | 0.40        | 1       |
| CRFB3   | -97.78    | -0.56     | -0.29       | 1       |
| CXSE3   | -84.65    | -1.61     | 0.37        | 1       |
| EMBR3   | -110.12   | -12.05    | -0.56       | 1       |
| ENGI11  | -155.00   | -23.48    | 1.03        | 1       |
| ENEV3   | -126.09   | -7.39     | 0.26        | 1       |
| EQTL3   | -156.84   | -21.83    | 0.49        | 1       |
| EZTC3   | -77.39    | -11.09    | -0.05       | 1       |
| FLRY3   | -133.17   | -24.33    | 0.52        | 1       |
| HAPV3   | -92.16    | -7.88     | 0.40        | 1       |
| IRBR3   | -85.58    | -13.31    | -0.20       | 1       |
| ITSA4   | -82.35    | -18.54    | 0.41        | 1       |
| ITUB4   | -76.93    | -15.19    | 0.14        | 1       |
| JHSF3   | -70.42    | -11.23    | 0.10        | 1       |
| KLBN11  | -123.25   | -23.72    | 0.53        | 1       |
| LIGT3   | -60.20    | -10.92    | 0.26        | 1       |
| LREN3   | -94.91    | -13.98    | 0.51        | 1       |
| MRVE3   | -107.47   | -12.63    | 0.18        | 1       |
| MULT3   | -94.91    | -25.04    | 0.81        | 1       |
| PCAR3   | -62.41    | -1.65     | -0.14       | 1       |
| PETR3   | -59.56    | -20.54    | 0.73        | 1       |
| QUAL3   | -104.49   | -8.13     | -0.02       | 1       |
| RAIL3   | -61.68    | -7.01     | 0.41        | 1       |
| RENT3   | -59.13    | -21.21    | 0.47        | 1       |
| SANB11  | -102.78   | -16.87    | -0.10       | 1       |
| SBSP3   | -134.70   | -15.54    | 0.54        | 1       |
| TIMS3   | -162.28   | -21.89    | 0.74        | 1       |
| VIVT3   | -91.51    | -4.58     | 0.25        | 1       |
| WEGE3   | -72.16    | -16.41    | 0.39        | 1       |
| YDUQ3   | -71.16    | -11.47    | 0.36        | 1       |
| ABEV3   | -272.94   | -23.84    | -0.25       | 2       |
| BPAC11  | -190.08   | -21.11    | 0.16        | 2       |
| EGIE3   | -195.52   | -27.75    | 1.17        | 2       |
| HYPE3   | -191.58   | -15.27    | 0.33        | 2       |
| RADL3   | -265.02   | -14.53    | 0.31        | 2       |
| TOTS3   | -205.54   | -22.62    | 0.77        | 2       |


**Check my personal dashboard with economic and finance data [here](https://lfpazevedo.pythonanywhere.com)**