---
title: "On the quest for the Carnaval.dat file"
date: "2024-12-30"
description: "R and Python together for another adventure"
featured: true
tags:
  - IBGE
  - Seas
  - Genhol
categories:
  - economics
  - programming
series:
  - "Macroeconomist Daily Routine"
aliases:
  - ibge-seas-genhol
thumbnail: "https://png.pngtree.com/png-vector/20220610/ourmid/pngtree-holy-grail-icon-in-cartoon-style-isolated-on-white-background-png-image_4864634.png"
---

# Seasonal Adjustment (Seas)

## Moving holidays

Each holiday has a unique impact on economic activity.

For example, during Christmas, people purchase gifts before December 25th.

In China, the central bank increases the money supply ahead of the Lunar New Year holiday.

In Brazil, during Carnival, the beverage industry operates at full capacity beforehand, while the cellphone market booms after the week of festivities (both for new and "refurbished" phones).

Seasonal adjustment for monthly series is straightforward in the first example, as Christmas occurs on a fixed date within December.

However, for holidays that "move"—such as Lunar New Year (January/February) or Carnival (February/March)—we need to adjust the _factor_ applied to those months.

Each year, with the first economic activity data release, [IBGE](https://ftp.ibge.gov.br/Industrias_Extrativas_e_de_Transformacao/Pesquisa_Industrial_Mensal_Producao_Fisica/Material_de_apoio/) publishes new weights (factors) to be used throughout the year.

When Carnival falls in February, an additional weight is added to February’s collected value, accounting for the typical economic slowdown during the festivities. Conversely, this weight is negative for March in such years. Naturally, the opposite occurs when Carnival is in March.

Since IBGE releases the parameters to calculate the holiday's economic impact [begin-before=-4/end-before=-1](https://ftp.ibge.gov.br/Industrias_Extrativas_e_de_Transformacao/Pesquisa_Industrial_Mensal_Producao_Fisica/Notas_Metodologicas/notas_metodologicas_pimpf_01_2022.pdf), we can use the software [Genhol](https://www.census.gov/data/software/x13as/genhol.html) to calculate these weights whenever needed.

The official date of Carnival is a Tuesday. However, _I couldn’t match the published spreadsheet values_ available in IBGE's FTP repository using this date. Assuming IBGE truly uses the specified days before the holiday, the only plausible source of error must come from the *holiday date file Carnaval.dat*, which is missing from the repository.

After adjustments—setting the official day to Wednesday for certain years—I successfully replicated the exact values:

| Original Date    | Replacement Date |
|------------------|------------------|
| 2022-03-01       | 2022-03-02       |
| 2003-03-04       | 2003-03-05       |
| 2014-03-04       | 2014-03-05       |
| 2025-03-04       | 2025-03-05       |


### Genhol weights
![[images/file-20241230183536266.png]]
![/home/lfpazevedo/Documents/linearsim/static/images/photo.jpeg](file:///home/lfpazevedo/Documents/linearsim/static/images/photo.jpeg)

Interestingly, for Corpus Christi, another movable holiday, this issue was not encountered.

![[images/genhol-carnival/file-20241230183541742.png]]

You can find the script for this study on my [Github Repository](https://github.com/lfpazevedo/py-seas).

Feel free to send me PRs—let’s unravel the mystery of the Carnaval.dat file together!


