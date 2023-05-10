# Importing {#importing}

## Setup


```r
library(tidyverse)
#> ── Attaching core tidyverse packages ──── tidyverse 2.0.0 ──
#> ✔ dplyr     1.1.2     ✔ readr     2.1.4
#> ✔ forcats   1.0.0     ✔ stringr   1.5.0
#> ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
#> ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
#> ✔ purrr     1.0.1     
#> ── Conflicts ────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
#> ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
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

