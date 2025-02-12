---
title: Risk Aversion in Different Carry Regimes
date: 2025-02-12
description: "MTAR model: USDBRL ~ EPU + VIX + (rate diff)"
featured: true
tags:
  - ARModels
  - Econometrics
  - FX
  - InterestRates
  - RiskAversion
categories:
  - finance
series:
  - modeling
aliases: 
thumbnail: images/regime.png
---
#### *Currency sensitivity to risk factors is smaller when the interest rate differential is higher.*

**Economic fundamentals** are among the main drivers of foreign exchange price fluctuations. For long-term analysis, **interest rate differentials** can explain the overvalued BRL during the 2000s, while **purchasing power parity (PPP)** *could* be the main reason for the continuous appreciation of currencies in Eastern countries (*“could” because we may need more than 100 years of data to prove that PPP really works...*).

Since interest rate differentials (hereafter *carry*) typically display long memory—remaining in the same plateau for months at a time—it may be necessary to **split the data into regimes** when explaining short-term price changes of currencies.

**Behavioral models** can be used for this type of short-term analysis. This approach uses **Market Sentiment and Expectations** as inputs. Cognitive biases, herding, and overreaction, among others, will affect sentiment and thus the FX price.

So, in this post, I’ll present some **interesting notes about Behavioral Models** across different regimes determined by fundamental variables. **Given the BRL carry level[^1], investors will react differently to the VIX (a global shock) or to shocks in Economic Policy Uncertainty (a local shock).**

---

## The Model

\[
Y_t \sim c + Y_{t-1} + X_t + Z_t
\]

Where:
- \(Y\) is the monthly log-return of USD/BRL,  
- \(X = [\mathrm{lag\_USDBRL}, \mathrm{EPU}, \mathrm{VIX}]\), and  
- \(Z\) is the rate differential (the regime applies to \(Z\)).

As mentioned, carry doesn’t explain much of the short-term fluctuation in FX. The table below shows the **relative importance** of each variable in an OLS regression.

| Variable | Coefficient | Standardized Beta | Partial R² | Share of Forecasted Variance | Shapley R² Share |
|----------|------------|-------------------|------------|------------------------------|------------------|
| lag_usd  | 0.2695     | 0.2709           | 0.0731     | 0.3798                       | 0.3645           |
| epu      | 0.0169     | 0.2252           | 0.0501     | 0.2626                       | 0.2801           |
| vix      | 0.0476     | 0.2583           | 0.0658     | 0.3454                       | 0.3513           |
| ir_diff  | -0.0377    | -0.0302          | 0.0009     | 0.0047                       | 0.0040           |

This low explanatory power is not a problem for the current study, since the maximum \(R^2\) reached by this simple analysis is also low (as is common with FX regressions). The 3D plot below shows how the \(X\) parameters evolve over time (using a 5-year regression window). The goodness-of-fit, as measured by \(R^2\), is lower than 0.2.

![Variables correlation - 2009-25](/linearsim/post/images/fx-regime/file-20250212111416524.png)

Nevertheless, the figure above reveals some **interesting clusters**. Splitting the beta-EPU distribution in two, we see **higher BRL sensitivity to local risks from 2016 to 2018**. This period also exhibits higher betas on the lagged USD variable and **lower VIX sensitivity**. **During these years, there was also a consistent drop in the BRL carry:**

![](/linearsim/post/images/fx-regime/file-20250212104330403.png)

Given these signals, it makes sense to run regressions **for different carry regimes** to see how these parameters behave.

---

## Full Sample Regression

First, let’s examine the full sample regression, which includes observations since 2009.

The **calculated endogenous threshold** is **0.109**, yielding 145 months of low-carry \(R_1\) (low by this univariate measure) and 38 observations above it (\(R_2\)).

| Parameter  | R1 Value                 | R2 Value                 |
|------------|--------------------------|--------------------------|
| cs         | 0.0174                   | 0.0115                   |
| phi1       | 0.153                    | 0.284                    |
| beta1      | 0.00191, 0.02833         | 0.001378, -0.000409      |
| delta1     | -0.154                   | -0.0724                  |
| sigma      | 0.0906                   | 0.175                    |

In \(R_2\), **BRL inertia is higher** (coefficient \(\phi_1\)). Changes in EPU generate a similar impact (the first element of \(\beta_1\)), **while the beta for VIX is null**, signaling that global shocks only affect the currency if the carry is not high. Increases in carry appreciate the BRL (negative \(\delta_1\) coefficients), **but with a higher magnitude in the low-carry regime**.

As often noted in this blog, **parameters in financial markets change significantly over time**. Therefore, let’s check how the parameters evolved over time. In regime-switch analysis, this is crucial, because 10% carry might be considered low by today’s standards but could have been considered high a few years ago.[^2]

Using a 10-year regression window for the first estimation and then adding one month at a time (i.e., not a fixed window), the grid below shows the **evolution of the parameters** since 2019. The Bayesian estimate presented some noise, so I added a 12-month moving average to the parameter time series for visualization.

| **Betas for lag-USD**                                            | **Betas for EPU**                                                |
| ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| ![](/linearsim/post/images/fx-regime/file-20250212112701652.png) | ![](/linearsim/post/images/fx-regime/file-20250212112710470.png) |

| **Betas for VIX**                                                | **Betas for Carry**                                              |
| ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| ![](/linearsim/post/images/fx-regime/file-20250212112728294.png) | ![](/linearsim/post/images/fx-regime/file-20250212112733430.png) |

Below is a 3D visualization of the **smoothed parameters**:

![](/linearsim/post/images/fx-regime/file-20250212113417234.png)

### **Notably, EPU beta was higher for the low regime in 2023, reinforcing the conclusion that investors are more willing to tolerate risk if the carry is sufficiently attractive.**

---

[^1]: **1st footnote** – For FX behavior models, the best measure of carry is the interest rate implied by the options market. For this study, however, I’m simply using the difference between the 2-year U.S. Treasury yield and the Brazilian 504-day fixed rate (as calculated by Anbima).

[^2]: **2nd footnote** – Of course, we need to test whether the data truly exhibits a regime switch. Since this blog’s intention is only to share interesting data and thoughts, let’s *pretend* the regime-switch model is superior to a simple linear OLS.

---

**Check my personal dashboard with economic and finance data [here](https://lfpazevedo.pythonanywhere.com).**
