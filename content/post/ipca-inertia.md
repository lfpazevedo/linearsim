---
title: "Analyzing Inflation Persistence: Long-Memory in Brazilian IPCA Subitems"
date: 2025-04-02
description: Arfima modelling
featured: true
tags:
  - ARModels
  - Econometrics
  - IBGE
  - IPCA
categories:
  - economics
series:
  - modeling
aliases: 
thumbnail: images/inertia.png
---
### **Time-varying Inflation Inertia Peaks in Brazil**

The Brazilian Institute of Geography and Statistics ([IBGE](www.ibge.gov.br)) calculates consumer inflation for 16 metropolitan regions in Brazil, each with a distinct inflation basket. As of February 2025, this detailed breakdown (subitem level) includes over **4,000 series**, totaling precisely **1,247,023 data points** available since August 1999.

| Surveyed Areas     | INPC¹ | IPCA² |
|--------------------|-------|-------|
| BRASIL             | 100.00 | 100.00 |
| Rio Branco         | 0.72  | 0.51  |
| Belém              | 6.95  | 3.94  |
| São Luís           | 3.47  | 1.62  |
| Fortaleza          | 5.16  | 3.23  |
| Recife             | 5.60  | 3.92  |
| Aracaju            | 1.29  | 1.03  |
| Salvador           | 7.92  | 5.99  |
| Belo Horizonte     | 10.35 | 9.69  |
| Vitória            | 1.91  | 1.86  |
| Rio de Janeiro     | 9.38  | 9.43  |
| São Paulo          | 24.60 | 32.28 |
| Curitiba           | 7.37  | 8.09  |
| Porto Alegre       | 7.15  | 8.61  |
| Campo Grande       | 1.73  | 1.57  |
| Goiânia            | 4.43  | 4.17  |
| Brasília           | 1.97  | 4.06  |

_Source: IBGE, Directorate of Surveys, Price Indexes Coordination._

¹ IBGE – Weight - Urban resident population (POF 2017-2018).  
² IBGE – Weight - Monthly household disposable monetary income (POF 2017-2018).


In this analysis, I've employed the **ARFIMA model** to estimate the time-varying long-memory parameter (**fractional integration parameter d**) across these series using rolling 48-month windows. This approach helps us capture shifts in inflation persistence over time and across categories and regions.

---

### **What is ARFIMA and Why Does It Matter?**

The **Fractionally Integrated AutoRegressive Moving Average (ARFIMA)** model extends the traditional ARIMA framework by allowing the integration parameter (_d_) to take fractional values. A fractional parameter between **0 and 0.5** indicates a _long-memory_ process, suggesting shocks to inflation decay slowly and continue influencing prices over extended periods.

---
### **Key Findings in Inflation Persistence**

Out of over 4,000 inflation series published in February 2025:

- Approximately **1,200 series** exhibit a fractional parameter indicating significant long-memory.
    
- The median long-memory parameter peaked towards the end of the previous presidential mandate and remains elevated.

![Time-Varying Estimated d  //  -1.0 to 0.5](/linearsim/post/images/ipca-inertia/file-20250402180934805.png)

![Time-Varying Estimated d  //  0.0 to 0.5](/linearsim/post/images/ipca-inertia/file-20250402180944245.png)

---

### **Sectoral Breakdown: Food, Services, and Industrial Goods**

Breaking down the long-memory behavior by the Brazilian Central Bank’s (BCB) special aggregates reveals distinct trends:

- **Food-at-Home and Industrial Goods**: Increasing inertia over recent years.
    
- **Administered Prices**: Growing inertia notably in the current presidential mandate.
    
- **Services**: Interestingly, reached historically low levels of inertia.

![Time-Varying Estimated d  //  0.0 to 0.5](/linearsim/post/images/ipca-inertia/file-20250402182155742.png)

#### **Detailed Breakdown (February 2025)**

| Category                | Total Series | Long-memory Series | % of Total |
| ----------------------- | ------------ | ------------------ | ---------- |
| **Food-at-Home**        | 1,373        | 404                | 29.43%     |
| **Services (Headline)** | 823          | 96                 | 11.67%     |
| **Services (Core)**     | 480          | 70                 | 14.58%     |
| **Industrial Goods**    | 1,344        | 676                | 50.30%     |

The fraction of series with significant long-memory in services is notably low, with minimal differences between core and headline components.

![Time-Varying Estimated d  //  -1.0 to 0.5](/linearsim/post/images/ipca-inertia/file-20250402184552307.png)

![Time-Varying Estimated d  //  0.0 to 0.5](/linearsim/post/images/ipca-inertia/file-20250402184603211.png)

---
### **Regional Analysis Reveals Divergent Trends**

Regionally, inertia patterns vary:

- **Services Inertia**: Generally decreasing across metropolitan regions, except for Recife, which shows a different trend.

![Time-Varying Estimated d  //  0.0 to 0.5](/linearsim/post/images/ipca-inertia/file-20250402185329512.png)

**Industrial Goods and Food-at-Home Inertia**: Exhibits a generalized and pronounced increase across most metropolitan areas.

![Time-Varying Estimated d  //  0.0 to 0.5](/linearsim/post/images/ipca-inertia/file-20250402185515642.png)

![Time-Varying Estimated d  //  0.0 to 0.5](/linearsim/post/images/ipca-inertia/file-20250402185524827.png)

---

### **Monetary Policy Implications**

This inertia analysis is particularly relevant given Brazil’s ongoing monetary tightening cycle. According to the BCB’s latest models, the peak impact of interest rate increases typically occurs within **4 quarters** for Food-at-Home and Industrial Goods but takes roughly **8 quarters** for Services. Given the current scenario of elevated long-memory parameters for Goods and Food, and lower for Services, **we may anticipate for the impulse-response function**:

- A posterior and potentially weaker reaction in Food-at-Home and Industrial Goods.
    
- A quicker-than-usual response in Services prices.

![Inflation (IPCA) response to nominal interest rate shock - Monetary Policy Report Boxes - June 2024](/linearsim/post/images/ipca-inertia/file-20250402190835952.png)


**Check my personal dashboard with economic and finance data [here](https://lfpazevedo.pythonanywhere.com)**