---
title: "Data Release Impact: Inflation or Activity?"
date: 2025-02-09
description: EGARCH with dummies on the variance equation
featured: false
tags:
  - InterestRates
  - Econometrics
  - Finance
categories:
  - finance
  - economics
series:
  - data release
aliases: 
thumbnail: images/datarelease.png
---
#### Interest Rate Sensitivity to Economic Data Releases

The Brazilian Institute of Geography and Statistics ([IBGE](https://www.ibge.gov.br/en/)) releases monthly data on inflation and economic activity. The main inflation indicators are **IPCA** and **IPCA-15** (same methodology but different price collection), in addition to monthly surveys for **Industry, Retail,** and **Services** (PIM, PMC, PMS, respectively). IBGE also publishes monthly **Unemployment** data, but due to its relatively short time series, I will not include it in the modeling proposed in this post.

Using the **ARCH** framework to model the **fixed-for-floating swap** in Brazil (**Swap Pré-DI**), I added **dummies** for release days to the **variance equation** of an **EGARCH** model, which allows for asymmetry in interest rate variations. I modeled the mean equation as an ARMA(1,1) process and used a Student’s t-distribution for the errors.

I utilized five years of daily data (from 2012 to 2017) to estimate the parameters, and then continued collecting the estimated parameters until 2025. The distribution of these parameters provides several interesting conclusions:

1. **Swap Pré-DI volatility is higher on IPCA release days for every vertex of the curve.**
2. **Shorter vertices show a decrease in volatility on IPCA-15 release days.**
3. **Economic activity data do not show a significant impact on volatility at any vertex.**

![](/linearsim/post/images/data-release/file-20250209221939815.png)

Here are the t-statistics used to check parameter significance:

![](/linearsim/post/images/data-release/file-20250209221834065.png)

**From this analysis, one might conclude that only IPCA matters for the interest rate market in Brazil.** However, remember that we operate in a regime-changing environment. The **“market radar”** evolves over time.

So, **let's add time** as a new dimension to these plots. Below, I plot only the parameters of the equation that explains the 252-day **Pré-DI** rate, which the BCB uses in its models.

The following graph shows that from 2012 to 2017, the activity parameters were estimated as positive, whereas more recent estimates indicate they contribute to reducing volatility.

![](/linearsim/post/images/data-release/file-20250209221945233.png)

The T-statistics plot shows that PMC and PMS are closer to –2, indicating they might be nearing statistical significance.

![](/linearsim/post/images/data-release/file-20250209221837543.png)

##### Wouldn't it be great to add the economic surprise—actual released data vs. economists’ forecasts—to the mean equation? What about a dummy in the variance equation for days when Central Bank staff members appear in the media?

##### Luckily, we already have such a study! If you want more details, please check [this working paper](https://repositorio.fgv.br/items/ac47bb61-d197-47c4-9e32-402bba270a97)


**Check my personal dashboard with economic and finance data [here](https://lfpazevedo.pythonanywhere.com)**