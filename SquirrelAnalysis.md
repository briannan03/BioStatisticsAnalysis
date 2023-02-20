---
title: "Squirrel analysis"
author: Brianna Nason
format: html
editor: visual
execute: 
  keep-md: true
---



## Squirrel Analysis

Exploratory analysis of NYC squirrel data set.

## Running Code

When you click the **Render** button a document will be generated that includes both content and the output of embedded code. You can embed code like this:


::: {.cell}

```{.r .cell-code}
#Load the tidyverse
library(tidyverse)
```

::: {.cell-output .cell-output-stderr}
```
── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
✔ tibble  3.1.8      ✔ dplyr   1.0.10
✔ tidyr   1.2.1      ✔ stringr 1.5.0 
✔ readr   2.1.3      ✔ forcats 0.5.2 
── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
```
:::

```{.r .cell-code}
library(kableExtra)
```

::: {.cell-output .cell-output-stderr}
```

Attaching package: 'kableExtra'

The following object is masked from 'package:dplyr':

    group_rows
```
:::

```{.r .cell-code}
#Read the squirrel data file from github
nyc_squirrels <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-10-29/nyc_squirrels.csv")
```

::: {.cell-output .cell-output-stderr}
```
Rows: 3023 Columns: 36
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr (14): unique_squirrel_id, hectare, shift, age, primary_fur_color, highli...
dbl  (9): long, lat, date, hectare_squirrel_number, zip_codes, community_dis...
lgl (13): running, chasing, climbing, eating, foraging, kuks, quaas, moans, ...

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
:::

```{.r .cell-code}
#install.packages("tidymodels")
library(tidymodels)
```

::: {.cell-output .cell-output-stderr}
```
── Attaching packages ────────────────────────────────────── tidymodels 1.0.0 ──
✔ broom        1.0.2     ✔ rsample      1.1.1
✔ dials        1.1.0     ✔ tune         1.0.1
✔ infer        1.0.4     ✔ workflows    1.1.2
✔ modeldata    1.1.0     ✔ workflowsets 1.0.0
✔ parsnip      1.0.3     ✔ yardstick    1.1.0
✔ recipes      1.0.4     
── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
✖ scales::discard()        masks purrr::discard()
✖ dplyr::filter()          masks stats::filter()
✖ recipes::fixed()         masks stringr::fixed()
✖ kableExtra::group_rows() masks dplyr::group_rows()
✖ dplyr::lag()             masks stats::lag()
✖ yardstick::spec()        masks readr::spec()
✖ recipes::step()          masks stats::step()
• Learn how to get started at https://www.tidymodels.org/start/
```
:::

```{.r .cell-code}
my_data_splits <- initial_split(nyc_squirrels, prop = 0.5)

exploratory_data <- training(my_data_splits)
test_data <- testing(my_data_splits)
```
:::


You can add options to executable code like this


::: {.cell}

```{.r .cell-code}
#See the first six rows of the data we've read in to our notebook
exploratory_data %>%
  head(2) %>%
  kable() %>%
  kable_styling(c("striped", "hover"))
```

::: {.cell-output-display}

`````{=html}
<table class="table table-striped table-hover" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> long </th>
   <th style="text-align:right;"> lat </th>
   <th style="text-align:left;"> unique_squirrel_id </th>
   <th style="text-align:left;"> hectare </th>
   <th style="text-align:left;"> shift </th>
   <th style="text-align:right;"> date </th>
   <th style="text-align:right;"> hectare_squirrel_number </th>
   <th style="text-align:left;"> age </th>
   <th style="text-align:left;"> primary_fur_color </th>
   <th style="text-align:left;"> highlight_fur_color </th>
   <th style="text-align:left;"> combination_of_primary_and_highlight_color </th>
   <th style="text-align:left;"> color_notes </th>
   <th style="text-align:left;"> location </th>
   <th style="text-align:left;"> above_ground_sighter_measurement </th>
   <th style="text-align:left;"> specific_location </th>
   <th style="text-align:left;"> running </th>
   <th style="text-align:left;"> chasing </th>
   <th style="text-align:left;"> climbing </th>
   <th style="text-align:left;"> eating </th>
   <th style="text-align:left;"> foraging </th>
   <th style="text-align:left;"> other_activities </th>
   <th style="text-align:left;"> kuks </th>
   <th style="text-align:left;"> quaas </th>
   <th style="text-align:left;"> moans </th>
   <th style="text-align:left;"> tail_flags </th>
   <th style="text-align:left;"> tail_twitches </th>
   <th style="text-align:left;"> approaches </th>
   <th style="text-align:left;"> indifferent </th>
   <th style="text-align:left;"> runs_from </th>
   <th style="text-align:left;"> other_interactions </th>
   <th style="text-align:left;"> lat_long </th>
   <th style="text-align:right;"> zip_codes </th>
   <th style="text-align:right;"> community_districts </th>
   <th style="text-align:right;"> borough_boundaries </th>
   <th style="text-align:right;"> city_council_districts </th>
   <th style="text-align:right;"> police_precincts </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> -73.96927 </td>
   <td style="text-align:right;"> 40.77138 </td>
   <td style="text-align:left;"> 9H-PM-1018-03 </td>
   <td style="text-align:left;"> 09H </td>
   <td style="text-align:left;"> PM </td>
   <td style="text-align:right;"> 10182018 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> Adult </td>
   <td style="text-align:left;"> Gray </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> Gray+ </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> Ground Plane </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> POINT (-73.9692650850957 40.7713793846002) </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 13 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> -73.97323 </td>
   <td style="text-align:right;"> 40.76898 </td>
   <td style="text-align:left;"> 5F-AM-1007-01 </td>
   <td style="text-align:left;"> 05F </td>
   <td style="text-align:left;"> AM </td>
   <td style="text-align:right;"> 10072018 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> Adult </td>
   <td style="text-align:left;"> Gray </td>
   <td style="text-align:left;"> Cinnamon </td>
   <td style="text-align:left;"> Gray+Cinnamon </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> Above Ground </td>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:left;"> fence </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> POINT (-73.973230162241 40.7689824531444) </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 13 </td>
  </tr>
</tbody>
</table>

`````

:::
:::


The `echo: false` option disables the printing of code (only output is displayed).

## Questions

-   Does fur color effect its behavior?

-   Does location effect a squirrels behavior?
