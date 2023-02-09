--- 
title: "Data Analysis for Researchers AY2022"
author: "Hiroshi Suzuki"
date: "2023-02-10"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
url: https://icu-hsuzuki.github.io/da4r2022/
# cover-image: path to the social sharing image like images/cover.jpg
description: |
  This is a book based on the lecture given in AY2022 at International Christian University compiled  using the bookdown package and RStudio.
  The HTML output format for this example is bookdown::bs4_book,
  set in the _output.yml file.
biblio-style: apalike
csl: chicago-fullnote-bibliography.csl
---

# About {-}

This is an extended lecture note of a course jointly taught with Professor Taisei Kaizoji in the Winter term AY2022. PART I, The Introduction, basically consists of the course contents presented in class and my responses to assignments two to five. 
The first assignment is in Chapter 1, Introduction.

PART II is the basics of exploratory data analysis, abbreviated as EDA, summarized in several components. 

PART III explains the data source we mainly used in class: World Development Indicators of the World Bank, Our World in Data, OECD, and UN Data. As an introduction, we often used the package `WDI` to search and download the World Development Indicator data.

In APPENDICES, we included miscellaneous topics closely related to the contents of this lecture note.

## Setup {-}

We focused on the `tidyvese` packages, which includes `ggplot2`, `dplyr`, `tidyr`, `readr` and `tibble`, and did not use other packages unless it is crucial. As you can see below, other packages are automatically attached when we load `tidyverse` by `library(tidyverse)` However, some packages installed with `tidyverse` need to be loaded when they are necessary. The package `readxl` for Excel is one of them.


```r
library(tidyverse)
#> ── Attaching packages ─────────────────── tidyverse 1.3.2 ──
#> ✔ ggplot2 3.4.0      ✔ purrr   0.3.5 
#> ✔ tibble  3.1.8      ✔ dplyr   1.0.10
#> ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
#> ✔ readr   2.1.3      ✔ forcats 0.5.2 
#> ── Conflicts ────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
```

## `bookdown` {-}

We used the [`bookdown` package](https://cran.r-project.org/web/packages/bookdown/index.html) to create this page with [`bs4_book` style](https://pkgs.rstudio.com/bookdown/reference/bs4_book.html) of bootstrap4. See the [Bookdown book}(https://bookdown.org/yihui/bookdown/) for the detail.
