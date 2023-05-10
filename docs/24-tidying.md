# Tidying {#tidying}

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
```


## References of `tidyr`

* Textbook: [R for Data Science,Tidy Data](https://r4ds.had.co.nz/tidy-data.html#tidy-data)

### RStudio Primers: See References in Moodle at the bottom

4. Tidy Your Data -- [r4ds: Wrangle, II](https://r4ds.had.co.nz/wrangle-intro.html#wrangle-intro)
  - [Reshape Data - a bit old](https://rstudio.cloud/learn/primers/4.1)
  - [Separate and Unite](https://rstudio.cloud/learn/primers/4.2) 
  - [Join Data Sets](https://rstudio.cloud/learn/primers/4.3)

The first component, 'Reshape Data' deals with `pivot_longer` and `pivot_wider`. However, it uses an older version of these functions calls `gather` and `spread`.

## Variables, values, and observations: Definitions

* A **variable** is a quantity, quality, or property that you can measure.
* A **value** is the state of a variable when you measure it. The value of a variable may change from measurement to measurement.
* An **observation** or **case** is a set of measurements made under similar conditions (you usually make all of the measurements in an observation at the same time and on the same object). An observation will contain several values, each associated with a different variable. I’ll sometimes refer to an observation as a case or data point.
* **Tabular data** is a table of values, each associated with a variable and an observation. Tabular data is tidy if each value is placed in its own cell, each variable in its own column, and each observation in its own row.
* So far, all of the data that you’ve seen has been tidy. In real-life, most data isn’t tidy, so we’ll come back to these ideas again in Data Wrangling.



## Tidy Data

> “Data comes in many formats, but R prefers just one: tidy data.” — Garrett Grolemund

Data can come in a variety of formats, but one format is easier to use in R than the others. This format is known as tidy data. A data set is tidy if:

1. Each variable is in its own column
2. Each observation is in its own row
3. Each value is in its own cell (this follows from #1 and #2)

> “Tidy data sets are all alike; but every messy data set is messy in its own way.” — Hadley Wickham

> “all happy families are all alike; each unhappy family is unhappy in its own way” - Tolstoy's Anna Karenina



## `tidyr` Basics

Let us look at the figure in [R4DS](https://d33wubrfki0l68.cloudfront.net/6f1ddb544fc5c69a2478e444ab8112fb0eea23f8/91adc/images/tidy-1.png).

<img src="./data/tidy-1.png" width="100%" />

1. Each variable is in its own column
2. Each observation is in its own row



## Pivot data from wide to long:
[`pivot_longer()`](https://tidyr.tidyverse.org/reference/pivot_longer.html)

```
pivot_longer(data, cols = <columns to pivot into longer format>,
  names_to = <name of the new character column>, # e.g. "group", "category", "class"
  values_to = <name of the column the values of cells go to>) # e.g. "value", "n"
```

## Pivot data from long to wide: 
[`pivot_wider()`](https://tidyr.tidyverse.org/reference/pivot_wider.html)
In Console: vignette("pivot") 

```
pivot_wider(data, 
  names_from = <name of the column (or columns) to get the name of the output column>,
  values_from = <name of the column to get the value of the output>) 
```

## Filtering joins
[`anti_join()`, `semi_join()`](https://dplyr.tidyverse.org/reference/filter-joins.html)


* `anti_join(x,y, ...)`: return all rows from x without a match in y.
* `semi_join(x,y, ...)`: return all rows from x with a match in y.

Check `dplyr` cheat sheet, and Posit Primers Tidy Data.

