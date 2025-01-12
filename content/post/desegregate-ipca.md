---
title: Desegregate. Now You Can.
date: 2025-01-09
description: Different habits and climate patterns generate different seasonalities.
featured: true
tags:
  - Seas
  - IPCA
  - Forecast
  - LSTM
categories:
  - economics
series:
  - Macroeconomist Daily Routine
aliases:
  - forecast-in-natura
thumbnail: images/lettuce.webp
---
Nowadays, it’s easier and cheaper to create your own website or blog. Every corner store can share its prices with the entire planet.

For a quantitative economist, this “corner store” is like a playground of endless possibilities.

Before we had onIn the past, only a few marketplaces published food prices—like [Ceagesp](https://ceagesp.gov.br/) anb local [Ceasas](https://www.ceasa.pr.gov.br/) in major Brazilian cities—and some institutions, such as [ESALQ](https://www.cepea.esalq.usp.br/br) focused on commodity prices in selected regions. Now, we can be more ambitious and **scrape** retail or wholesale prices for any desired location worldwide.

When it comes to short-term inflation forecasting, the highest level of data **disaggregation** and localized price scraping is _the crème de la crème_. Even without collecting new price data, we can demonstrate the power of disaggregation in a simple forecasting exercise. The table below shows the regional weights of Brazil’s inflation as measured by the ([IPCA](https://www.ibge.gov.br/estatisticas/economicas/precos-e-custos/9256-indice-nacional-de-precos-ao-consumidor-amplo.html?=&t=notas-tecnicas)).

| Surveyed Areas    | INPC Weight | IPCA Weight |
|--------------------|-------------|-------------|
| **BRASIL**        | **100.00**  | **100.00**  |
| Rio Branco        | 0.72        | 0.51        |
| Belém             | 6.95        | 3.94        |
| São Luís          | 3.47        | 1.62        |
| Fortaleza         | 5.16        | 3.23        |
| Recife            | 5.60        | 3.92        |
| Aracaju           | 1.29        | 1.03        |
| Salvador          | 7.92        | 5.99        |
| Belo Horizonte    | 10.35       | 9.69        |
| Vitória           | 1.91        | 1.86        |
| Rio de Janeiro    | 9.38        | 9.43        |
| São Paulo         | 24.60       | 32.28       |
| Curitiba          | 7.37        | 8.09        |
| Porto Alegre      | 7.15        | 8.61        |
| Campo Grande      | 1.73        | 1.57        |
| Goiânia           | 4.43        | 4.17        |
| Brasília          | 1.97        | 4.06        |
**Source**: IBGE, Diretoria de Pesquisas, Coordenação de Índices de Preços.  
_Population and income data from POF 2017-2018._

In a vast and diverse country like Brazil, variations in rainfall distribution and cultural practices make seasonality highly visible in the data, especially for fresh produce.

| ![Lettuce](/linearsim/post/images/desegregate-ipca/ipca_animation_alface.gif) | ![Cilantro-Parsley](/linearsim/post/images/desegregate-ipca/ipca_animation_cheiro_verde.gif) |
| ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |

Below, you can see the inflation seasonality plots for _1105001.Lettuce_ in Curitiba (South) and Fortaleza (Northeast):

| ![Curitiba](/linearsim/post/images/desegregate-ipca/file-20250109190843874.png) | ![Fortaleza](/linearsim/post/images/desegregate-ipca/file-20250109190915357.png) |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |

You can observe that the larger temperature swings in Curitiba have a stronger effect on price variation compared to Fortaleza.

Here is the difference in consumer budget weights for the two cities:

| Location  | Regional Weight | Local Weight* | Country Weight              |
|-----------|-----------------|---------------|-----------------------------|
| **Brazil**| **100.0**       | **0.1193**    | **100% x 0.1193 = 0.1193** |
| Curitiba  | 7.37            | 0.1474        | 7.37% x 0.1474 = 0.01086338 |
| Fortaleza | 3.23            | 0.0409        | 3.23% x 0.0409 = 0.00132107 |
* Weight as on 2024-11

I ran a simple model (an LSTM with a 12-month-ahead horizon and 12 lags as features) on the IPCA category _1105.Vegetables and greens_ (since greens are perishable, non-tradeable goods, potential arbitrage between cities is reduced) for the aggregate “Brazil” and its regional composition.

Next, I forecast each sub-item within this category on a regional basis and calculated the Mean Squared Error (MSE) of each forecast:

| SNIPC Code | Subitem                 | Country Weight |
|------------|-------------------------|----------------|
| **1105**   | **Vegetables and greens** | **0.243**       |
| 1105001    | Lettuce                 | 0.1193         |
| 1105004    | Coriander               | 0.0076         |
| 1105005    | Kale                    | 0.0169         |
| 1105006    | Cauliflower             | 0.0033         |
| 1105010    | Cabbage                 | 0.0178         |
| 1105012    | Parsley                 | 0.0478         |
| 1105019    | Broccoli                | 0.0304         |

| Location                       | LSTM (Desagg.)  | LSTM (Headline)  |
|--------------------------------|-----------------|------------------|
| **Brasil (n1)**                | 13.2            | 12.3             |
| **Regions on Brasil (Desagg.)**| 11.7            | —                |
|                                |                 |                  |
| **Aracaju - SE (n6)**          | 40.6            | 37.8             |
| **Belém - PA (n7)**            | 15.0            | 16.5             |
| **Belo Horizonte - MG (n7)**   | 9.4             | 9.2              |
| **Brasília - DF (n6)**         | 7.3             | 11.8             |
| **Campo Grande - MS (n6)**     | 51.7            | 45.0             |
| **Curitiba - PR (n7)**         | 20.1            | 16.7             |
| **Fortaleza - CE (n7)**        | 6.1             | 5.6              |
| **Goiânia - GO (n6)**          | 35.4            | 29.8             |
| **Porto Alegre - RS (n7)**     | 39.4            | 79.3             |
| **Recife - PE (n7)**           | 118.7           | 109.3            |
| **Rio Branco - AC (n6)**       | 17.6            | 12.1             |
| **Rio de Janeiro - RJ (n7)**   | 17.7            | 17.5             |
| **Salvador - BA (n7)**         | 44.9            | 60.8             |
| **São Luís - MA (n6)**         | 15.3            | 12.9             |
| **São Paulo - SP (n7)**        | 30.8            | 31.6             |
| **Grande Vitória - ES (n7)**   | 5.6             | 5.5              |

By forecasting _Lettuce_ separately for Fortaleza, Curitiba, and other locations—and then summing those contributions to arrive at the national aggregate—we achieve a lower (better) error than when we only forecast the country-level headline figure.

Of course, a simple LSTM model for time series subject to so many shocks—such as weather events—can be unreliable. Adding local retail or wholesale price data helps improve these results.

**Check my personal dashboard with economic and finance data [here](https://lfpazevedo.pythonanywhere.com)**