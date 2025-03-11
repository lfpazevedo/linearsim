---
title: Building a Bond Portfolio Using PCA
date: 2025-03-11
description: PCA and Monte Carlo to simulate yield curve
featured: true
tags:
  - Econometrics
  - Finance
  - InterestRates
categories:
  - economics
series:
  - modeling
aliases: 
thumbnail: images/monte_carlo.png
---
#### Simulating Level, Slope, and Curvature for the Brazilian Yield Curve

We can explain the **difference in interest rate levels** between countries by their individual **institutional characteristics**. Countries that are better ranked in institutional indexes tend to present lower interest rates.

![](/linearsim/post/images/bond-portfolio/file-20250311094048974.png)

The **slope** of the yield curve is connected to the current path of **monetary policy**. The spread between short- and medium-term rates indicates expectations for further changes in the base rate.

![](/linearsim/post/images/bond-portfolio/file-20250311110932548.png)

The **curvature** can represent an implied risk for a country’s **fiscal policy** and the ongoing forward debt trajectory. If there is doubt about the government’s future capacity to meet its obligations, the rates for longer maturities can skyrocket.

![](/linearsim/post/images/bond-portfolio/file-20250311112639108.png)

These three components (level, slope, and curvature) are, **in order**, the main sources of **variability for the yield curve**. Since PCA captures the variability of a dataset according to its primary contributors, the connection between the yield curve and PCA is straightforward:

- **PC1** represents the level
- **PC2** the slope
- **PC3** the curvature

Here is the yield curve breakdown for the **Brazilian Swap Pré-DI** data (fixed over float, from 21 days to 7 years):

![](/linearsim/post/images/bond-portfolio/file-20250310204321074.png)

Adding these three components accounts for more than **95% of the data’s variability**, which is higher than what we usually observe in other economies (often between 80–90%).

![](/linearsim/post/images/bond-portfolio/file-20250310204310709.png)

Eigenvectors by vertices show how PC1’s **importance** is greatest and how the **impact** of PC2 and PC3 **varies across the curve**.

![](/linearsim/post/images/bond-portfolio/file-20250310204300882.png)

---

#### Simulation

Including up to the third component captures around 95% of the yield curve’s variability **without excessive noise**. Using that, **I ran a Monte Carlo simulation** to project the yield curve for the next 20 weeks.

Here is the simulated distribution of the vertices:

![](/linearsim/post/images/bond-portfolio/file-20250310204707380.png)

I simulated 1,000 paths. The following plot shows the trajectory of these 1,000 simulations over 20 steps for the one-year swap:

![](/linearsim/post/images/bond-portfolio/file-20250310204925317.png)

---

#### Portfolio

I checked the **performance** of this yield curve forecast with a **portfolio of Brazilian bonds**. Letras do Tesouro Nacional (LTN) is a zero-coupon bond with a face value of 1,000. Currently, six bonds are available to the general public:

![](/linearsim/post/images/bond-portfolio/file-20250310205522854.png)

I took the average of the simulations at each step as the forecasted yield to **predict the LTN** price over the 20-week period and to calculate the bond’s price trajectory. Then, the return rate was calculated using the current bond price.

First, I constructed an **equally weighted long-only portfolio**, which yielded the following positive Sharpe ratio:

| Constraint | M    | T   | weight_i | Exp_ret | std  | SR   |
| ---------- | ---- | --- | -------- | ------- | ---- | ---- |
| 1/6        | 1000 | 20  | 0.17     | 2.78    | 4.17 | 0.67 |

**Optimizing** under two different **constraints** resulted in:

| Constraint                  | Exp_ret | vol  | SR   | LTN_2026 | LTN_2027 | LTN_2028 | LTN_2029 | LTN_2031 | LTN_2032 |
| --------------------------- | ------- | ---- | ---- | -------- | -------- | -------- | -------- | -------- | -------- |
| Sum=1                       | 2.27    | 1.68 | 1.35 | 0.91     | 0.0      | 0.0      | 0.0      | 0.0      | 0.09     |
| Sum=1 with bounds (0.1,0.5) | 2.45    | 2.32 | 1.06 | 0.50     | 0.1      | 0.1      | 0.1      | 0.1      | 0.10     |

**For the given estimated yield curve trajectory**, we see that the shorter bond (the LTN maturing in January 2026) is the most attractive in both constrained optimizations.

**This post demonstrates how economists can guide portfolio managers by highlighting institutional differences between countries and explaining how monetary and fiscal policy can influence portfolio metrics.**


**Check my personal dashboard with economic and finance data [here](https://lfpazevedo.pythonanywhere.com)**