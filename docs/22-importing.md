# Importing {#importing}

## Setup


```r
library(tidyverse)
#> ── Attaching packages ─────────────────── tidyverse 1.3.2 ──
#> ✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
#> ✔ tibble  3.1.8      ✔ dplyr   1.0.10
#> ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
#> ✔ readr   2.1.3      ✔ forcats 0.5.2 
#> ── Conflicts ────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
library(readxl)
library(WDI)
library(owidR)
library(wid)
```

## EDA by R Studio: Step 3 - Importing Data

Assign a name you can recall easily when you import data. You may need to reload the data with options.

3.1. Use a package:

  * WDI, wir, eurostat, etc/
  * `wdi_shortname <- WDI(indicator = "indicator's name", ... )
  * Store the data and use it: `write_csv(wdi_shortname, "./data/wdi_shortname.csv")`
  * `wdi_shortname <- read_csv("./data/wdi_shortname.csv")`
  
3.2. Use `readr` to read from `data`, your data folder

  * `df1_shortname <- read_csv("./data/file_name.csv")`


3.3. Use `readr` to read using the url of the data

  * `df2_shortname <- read_csv("url_of_the_data")`
  * Store the data and use it: `write_csv(df2_shortname, "./data/df2_shortname.csv")`
  * `df2_shortname <- read_csv("./data/df2_shortname.csv")`
  
3.5. Use `readxl` to read Excel data. Add `library(readxl)` in the setup and run.

  * `df4 <- read_excel("./data/file_name.xlsx", sheet = 1)`
  
References: Cheat Sheet - `readr`, [readr](https://readr.tidyverse.org), [readxl](https://readxl.tidyverse.org)

