# Responses to Assignment Five {#a5resp}

## Assignment Five: The Week Six Assignment {.unnumbered}

* Choose a public data. Clearly state how you obtained the data. Even if you are able to give the URL to download the data, explain the steps you reached and obtained the data. 
* Create an R Notebook of a Data Analysis containing the following and submit the rendered HTML file (eg. `a5_123456.nb.html`  by replacing 123456 with your ID), and a PDF (or MS Word File).
  1. create an R Notebook using the R Notebook Template in Moodle,  save as `a3_123456.Rmd`, 
  2. write your name and ID and the contents, 
  3. run each code block, 
  4. preview to create `a5_123456.nb.html`,
  5. render (or knit) PDF, or Word (and then PDF)
  6. submit  `a5_123456.nb.html` and PDF (or Word) to Moodle.

1. Choose a data with at least two numerical variables. One of them can be the year.

    - Information of the data
    - Explain why you chose the data
    - List questions you want to study

2. Explore the data using visualization using `ggplot2`

    - Create various charts, and write observed comments
    - Apply a (linear regression) model, and draw a regression line to at least one chart, and write your conclusion based on the model using the slope value and R squared (and/or adjusted R squared). 

3. Observations based on your data visualization, and difficulties and questions encountered if any.

**Due:** 2023-01-30 23:59:00. Submit your R Notebook file, and a PDF file (or a MS Word file) in Moodle (The Fifth Assignment). Due on Monday!


# Set up

It is better to use the following, because you can search by error message when you get an error. Error messages are important. If you get used to it, you can correct most of the errors. You can use the information given by `message` as well.


```r
Sys.setenv(LANG = "en")
```



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
library(readxl) # for excel files
library(WDI)
```

## World Development Indicator - WDI

The following is useful when you use WDI.


```r
wdi_cache <- WDIcache()
```

### Creating a vector with `iso2` codes

It is convenient to have a vector with `iso2c` codes when you import data from WDI.


```r
asean <- c("Brunei Darussalam", "Cambodia", "Lao PDR", "Myanmar", 
           "Philippines", "Indonesia", "Malaysia", "Singapore")
brics <- c("Brazil", "Russian Federation", "India", "China")
g7 <- c("Canada", "France", "Italy", "Japan", "Germany", "United Kingdom", "United States")
income_levels <- c("High income","Upper middle income", "Middle income", 
                   "Lower middle income","Low income")
```


```r
ASEAN <- wdi_cache$country %>% filter(country %in% asean) %>% 
  distinct(iso2c) %>% pull()
ASEAN
#> [1] "BN" "ID" "KH" "LA" "MM" "MY" "PH" "SG"
```


```r
BRICs <- wdi_cache$country %>% filter(country %in% brics) %>% 
  distinct(iso2c) %>% pull()
BRICs
#> [1] "BR" "CN" "IN" "RU"
```


```r
G7 <- wdi_cache$country %>% filter(country %in% g7) %>% 
  distinct(iso2c) %>% pull()
G7
#> [1] "CA" "DE" "FR" "GB" "IT" "JP" "US"
```




```r
INCOME_LEVELS <- wdi_cache$country %>% filter(country %in% income_levels) %>%
  distinct(iso2c) %>% pull()
INCOME_LEVELS
#> [1] "XD" "XM" "XN" "XP" "XT"
```



* In the following code chunk, we import only the G7 countries.


```r
wdi_cache$series %>% filter(indicator %in% c("SG.GEN.PARL.ZS","SI.POV.GINI"))
#>        indicator
#> 1 SG.GEN.PARL.ZS
#> 2    SI.POV.GINI
#>                                                            name
#> 1 Proportion of seats held by women in national parliaments (%)
#> 2                                                    Gini index
#>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            description
#> 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Women in parliaments are the percentage of parliamentary seats in a single or lower chamber held by women.
#> 2 Gini index measures the extent to which the distribution of income (or, in some cases, consumption expenditure) among individuals or households within an economy deviates from a perfectly equal distribution. A Lorenz curve plots the cumulative percentages of total income received against the cumulative number of recipients, starting with the poorest individual or household. The Gini index measures the area between the Lorenz curve and a hypothetical line of absolute equality, expressed as a percentage of the maximum area under the line. Thus a Gini index of 0 represents perfect equality, while an index of 100 implies perfect inequality.
#>                 sourceDatabase
#> 1 World Development Indicators
#> 2 World Development Indicators
#>                                                                                                                                                                                                                                                                                                                               sourceOrganization
#> 1                                                                                                                                                                                                                                       Inter-Parliamentary Union (IPU) (www.ipu.org).  For the year of 1998, the data is as of August 10, 1998.
#> 2 World Bank, Poverty and Inequality Platform. Data are based on primary household survey data obtained from government statistical agencies and World Bank country departments. Data for high-income economies are mostly from the Luxembourg Income Study database. For more information and methodology, please see http://pip.worldbank.org.
```


```r
df1 <- WDI(country = G7, 
           indicator=c(parl = "SG.GEN.PARL.ZS", gini = "SI.POV.GINI"),
           extra=TRUE, cache=wdi_cache)
df1  # for R Notebook use this line, for PDF delete by adding #
#>            country iso2c iso3c year status lastupdated
#> 1           Canada    CA   CAN 1960         2022-12-22
#> 2           Canada    CA   CAN 1961         2022-12-22
#> 3           Canada    CA   CAN 1962         2022-12-22
#> 4           Canada    CA   CAN 1963         2022-12-22
#> 5           Canada    CA   CAN 1964         2022-12-22
#> 6           Canada    CA   CAN 1965         2022-12-22
#> 7           Canada    CA   CAN 1966         2022-12-22
#> 8           Canada    CA   CAN 1967         2022-12-22
#> 9           Canada    CA   CAN 1968         2022-12-22
#> 10          Canada    CA   CAN 1969         2022-12-22
#> 11          Canada    CA   CAN 1970         2022-12-22
#> 12          Canada    CA   CAN 1971         2022-12-22
#> 13          Canada    CA   CAN 1972         2022-12-22
#> 14          Canada    CA   CAN 1973         2022-12-22
#> 15          Canada    CA   CAN 1974         2022-12-22
#> 16          Canada    CA   CAN 1975         2022-12-22
#> 17          Canada    CA   CAN 1976         2022-12-22
#> 18          Canada    CA   CAN 1977         2022-12-22
#> 19          Canada    CA   CAN 1978         2022-12-22
#> 20          Canada    CA   CAN 1979         2022-12-22
#> 21          Canada    CA   CAN 1980         2022-12-22
#> 22          Canada    CA   CAN 1981         2022-12-22
#> 23          Canada    CA   CAN 1982         2022-12-22
#> 24          Canada    CA   CAN 1983         2022-12-22
#> 25          Canada    CA   CAN 1984         2022-12-22
#> 26          Canada    CA   CAN 1985         2022-12-22
#> 27          Canada    CA   CAN 1986         2022-12-22
#> 28          Canada    CA   CAN 1987         2022-12-22
#> 29          Canada    CA   CAN 1988         2022-12-22
#> 30          Canada    CA   CAN 1989         2022-12-22
#> 31          Canada    CA   CAN 1990         2022-12-22
#> 32          Canada    CA   CAN 1991         2022-12-22
#> 33          Canada    CA   CAN 1992         2022-12-22
#> 34          Canada    CA   CAN 1993         2022-12-22
#> 35          Canada    CA   CAN 1994         2022-12-22
#> 36          Canada    CA   CAN 1995         2022-12-22
#> 37          Canada    CA   CAN 1996         2022-12-22
#> 38          Canada    CA   CAN 1997         2022-12-22
#> 39          Canada    CA   CAN 1998         2022-12-22
#> 40          Canada    CA   CAN 1999         2022-12-22
#> 41          Canada    CA   CAN 2000         2022-12-22
#> 42          Canada    CA   CAN 2001         2022-12-22
#> 43          Canada    CA   CAN 2002         2022-12-22
#> 44          Canada    CA   CAN 2003         2022-12-22
#> 45          Canada    CA   CAN 2004         2022-12-22
#> 46          Canada    CA   CAN 2005         2022-12-22
#> 47          Canada    CA   CAN 2006         2022-12-22
#> 48          Canada    CA   CAN 2007         2022-12-22
#> 49          Canada    CA   CAN 2008         2022-12-22
#> 50          Canada    CA   CAN 2009         2022-12-22
#> 51          Canada    CA   CAN 2010         2022-12-22
#> 52          Canada    CA   CAN 2011         2022-12-22
#> 53          Canada    CA   CAN 2012         2022-12-22
#> 54          Canada    CA   CAN 2013         2022-12-22
#> 55          Canada    CA   CAN 2014         2022-12-22
#> 56          Canada    CA   CAN 2015         2022-12-22
#> 57          Canada    CA   CAN 2016         2022-12-22
#> 58          Canada    CA   CAN 2017         2022-12-22
#> 59          Canada    CA   CAN 2018         2022-12-22
#> 60          Canada    CA   CAN 2019         2022-12-22
#> 61          Canada    CA   CAN 2020         2022-12-22
#> 62          Canada    CA   CAN 2021         2022-12-22
#> 63          France    FR   FRA 1960         2022-12-22
#> 64          France    FR   FRA 1961         2022-12-22
#> 65          France    FR   FRA 1962         2022-12-22
#> 66          France    FR   FRA 1963         2022-12-22
#> 67          France    FR   FRA 1964         2022-12-22
#> 68          France    FR   FRA 1965         2022-12-22
#> 69          France    FR   FRA 1966         2022-12-22
#> 70          France    FR   FRA 1967         2022-12-22
#> 71          France    FR   FRA 1968         2022-12-22
#> 72          France    FR   FRA 1969         2022-12-22
#> 73          France    FR   FRA 1970         2022-12-22
#> 74          France    FR   FRA 1971         2022-12-22
#> 75          France    FR   FRA 1972         2022-12-22
#> 76          France    FR   FRA 1973         2022-12-22
#> 77          France    FR   FRA 1974         2022-12-22
#> 78          France    FR   FRA 1975         2022-12-22
#> 79          France    FR   FRA 1976         2022-12-22
#> 80          France    FR   FRA 1977         2022-12-22
#> 81          France    FR   FRA 1978         2022-12-22
#> 82          France    FR   FRA 1979         2022-12-22
#> 83          France    FR   FRA 1980         2022-12-22
#> 84          France    FR   FRA 1981         2022-12-22
#> 85          France    FR   FRA 1982         2022-12-22
#> 86          France    FR   FRA 1983         2022-12-22
#> 87          France    FR   FRA 1984         2022-12-22
#> 88          France    FR   FRA 1985         2022-12-22
#> 89          France    FR   FRA 1986         2022-12-22
#> 90          France    FR   FRA 1987         2022-12-22
#> 91          France    FR   FRA 1988         2022-12-22
#> 92          France    FR   FRA 1989         2022-12-22
#> 93          France    FR   FRA 1990         2022-12-22
#> 94          France    FR   FRA 1991         2022-12-22
#> 95          France    FR   FRA 1992         2022-12-22
#> 96          France    FR   FRA 1993         2022-12-22
#> 97          France    FR   FRA 1994         2022-12-22
#> 98          France    FR   FRA 1995         2022-12-22
#> 99          France    FR   FRA 1996         2022-12-22
#> 100         France    FR   FRA 1997         2022-12-22
#> 101         France    FR   FRA 1998         2022-12-22
#> 102         France    FR   FRA 1999         2022-12-22
#> 103         France    FR   FRA 2000         2022-12-22
#> 104         France    FR   FRA 2001         2022-12-22
#> 105         France    FR   FRA 2002         2022-12-22
#> 106         France    FR   FRA 2003         2022-12-22
#> 107         France    FR   FRA 2004         2022-12-22
#> 108         France    FR   FRA 2005         2022-12-22
#> 109         France    FR   FRA 2006         2022-12-22
#> 110         France    FR   FRA 2007         2022-12-22
#> 111         France    FR   FRA 2008         2022-12-22
#> 112         France    FR   FRA 2009         2022-12-22
#> 113         France    FR   FRA 2010         2022-12-22
#> 114         France    FR   FRA 2011         2022-12-22
#> 115         France    FR   FRA 2012         2022-12-22
#> 116         France    FR   FRA 2013         2022-12-22
#> 117         France    FR   FRA 2014         2022-12-22
#> 118         France    FR   FRA 2015         2022-12-22
#> 119         France    FR   FRA 2016         2022-12-22
#> 120         France    FR   FRA 2017         2022-12-22
#> 121         France    FR   FRA 2018         2022-12-22
#> 122         France    FR   FRA 2019         2022-12-22
#> 123         France    FR   FRA 2020         2022-12-22
#> 124         France    FR   FRA 2021         2022-12-22
#> 125        Germany    DE   DEU 1960         2022-12-22
#> 126        Germany    DE   DEU 1961         2022-12-22
#> 127        Germany    DE   DEU 1962         2022-12-22
#> 128        Germany    DE   DEU 1963         2022-12-22
#> 129        Germany    DE   DEU 1964         2022-12-22
#> 130        Germany    DE   DEU 1965         2022-12-22
#> 131        Germany    DE   DEU 1966         2022-12-22
#> 132        Germany    DE   DEU 1967         2022-12-22
#> 133        Germany    DE   DEU 1968         2022-12-22
#> 134        Germany    DE   DEU 1969         2022-12-22
#> 135        Germany    DE   DEU 1970         2022-12-22
#> 136        Germany    DE   DEU 1971         2022-12-22
#> 137        Germany    DE   DEU 1972         2022-12-22
#> 138        Germany    DE   DEU 1973         2022-12-22
#> 139        Germany    DE   DEU 1974         2022-12-22
#> 140        Germany    DE   DEU 1975         2022-12-22
#> 141        Germany    DE   DEU 1976         2022-12-22
#> 142        Germany    DE   DEU 1977         2022-12-22
#> 143        Germany    DE   DEU 1978         2022-12-22
#> 144        Germany    DE   DEU 1979         2022-12-22
#> 145        Germany    DE   DEU 1980         2022-12-22
#> 146        Germany    DE   DEU 1981         2022-12-22
#> 147        Germany    DE   DEU 1982         2022-12-22
#> 148        Germany    DE   DEU 1983         2022-12-22
#> 149        Germany    DE   DEU 1984         2022-12-22
#> 150        Germany    DE   DEU 1985         2022-12-22
#> 151        Germany    DE   DEU 1986         2022-12-22
#> 152        Germany    DE   DEU 1987         2022-12-22
#> 153        Germany    DE   DEU 1988         2022-12-22
#> 154        Germany    DE   DEU 1989         2022-12-22
#> 155        Germany    DE   DEU 1990         2022-12-22
#> 156        Germany    DE   DEU 1991         2022-12-22
#> 157        Germany    DE   DEU 1992         2022-12-22
#> 158        Germany    DE   DEU 1993         2022-12-22
#> 159        Germany    DE   DEU 1994         2022-12-22
#> 160        Germany    DE   DEU 1995         2022-12-22
#> 161        Germany    DE   DEU 1996         2022-12-22
#> 162        Germany    DE   DEU 1997         2022-12-22
#> 163        Germany    DE   DEU 1998         2022-12-22
#> 164        Germany    DE   DEU 1999         2022-12-22
#> 165        Germany    DE   DEU 2000         2022-12-22
#> 166        Germany    DE   DEU 2001         2022-12-22
#> 167        Germany    DE   DEU 2002         2022-12-22
#> 168        Germany    DE   DEU 2003         2022-12-22
#> 169        Germany    DE   DEU 2004         2022-12-22
#> 170        Germany    DE   DEU 2005         2022-12-22
#> 171        Germany    DE   DEU 2006         2022-12-22
#> 172        Germany    DE   DEU 2007         2022-12-22
#> 173        Germany    DE   DEU 2008         2022-12-22
#> 174        Germany    DE   DEU 2009         2022-12-22
#> 175        Germany    DE   DEU 2010         2022-12-22
#> 176        Germany    DE   DEU 2011         2022-12-22
#> 177        Germany    DE   DEU 2012         2022-12-22
#> 178        Germany    DE   DEU 2013         2022-12-22
#> 179        Germany    DE   DEU 2014         2022-12-22
#> 180        Germany    DE   DEU 2015         2022-12-22
#> 181        Germany    DE   DEU 2016         2022-12-22
#> 182        Germany    DE   DEU 2017         2022-12-22
#> 183        Germany    DE   DEU 2018         2022-12-22
#> 184        Germany    DE   DEU 2019         2022-12-22
#> 185        Germany    DE   DEU 2020         2022-12-22
#> 186        Germany    DE   DEU 2021         2022-12-22
#> 187          Italy    IT   ITA 1960         2022-12-22
#> 188          Italy    IT   ITA 1961         2022-12-22
#> 189          Italy    IT   ITA 1962         2022-12-22
#> 190          Italy    IT   ITA 1963         2022-12-22
#> 191          Italy    IT   ITA 1964         2022-12-22
#> 192          Italy    IT   ITA 1965         2022-12-22
#> 193          Italy    IT   ITA 1966         2022-12-22
#> 194          Italy    IT   ITA 1967         2022-12-22
#> 195          Italy    IT   ITA 1968         2022-12-22
#> 196          Italy    IT   ITA 1969         2022-12-22
#> 197          Italy    IT   ITA 1970         2022-12-22
#> 198          Italy    IT   ITA 1971         2022-12-22
#> 199          Italy    IT   ITA 1972         2022-12-22
#> 200          Italy    IT   ITA 1973         2022-12-22
#> 201          Italy    IT   ITA 1974         2022-12-22
#> 202          Italy    IT   ITA 1975         2022-12-22
#> 203          Italy    IT   ITA 1976         2022-12-22
#> 204          Italy    IT   ITA 1977         2022-12-22
#> 205          Italy    IT   ITA 1978         2022-12-22
#> 206          Italy    IT   ITA 1979         2022-12-22
#> 207          Italy    IT   ITA 1980         2022-12-22
#> 208          Italy    IT   ITA 1981         2022-12-22
#> 209          Italy    IT   ITA 1982         2022-12-22
#> 210          Italy    IT   ITA 1983         2022-12-22
#> 211          Italy    IT   ITA 1984         2022-12-22
#> 212          Italy    IT   ITA 1985         2022-12-22
#> 213          Italy    IT   ITA 1986         2022-12-22
#> 214          Italy    IT   ITA 1987         2022-12-22
#> 215          Italy    IT   ITA 1988         2022-12-22
#> 216          Italy    IT   ITA 1989         2022-12-22
#> 217          Italy    IT   ITA 1990         2022-12-22
#> 218          Italy    IT   ITA 1991         2022-12-22
#> 219          Italy    IT   ITA 1992         2022-12-22
#> 220          Italy    IT   ITA 1993         2022-12-22
#> 221          Italy    IT   ITA 1994         2022-12-22
#> 222          Italy    IT   ITA 1995         2022-12-22
#> 223          Italy    IT   ITA 1996         2022-12-22
#> 224          Italy    IT   ITA 1997         2022-12-22
#> 225          Italy    IT   ITA 1998         2022-12-22
#> 226          Italy    IT   ITA 1999         2022-12-22
#> 227          Italy    IT   ITA 2000         2022-12-22
#> 228          Italy    IT   ITA 2001         2022-12-22
#> 229          Italy    IT   ITA 2002         2022-12-22
#> 230          Italy    IT   ITA 2003         2022-12-22
#> 231          Italy    IT   ITA 2004         2022-12-22
#> 232          Italy    IT   ITA 2005         2022-12-22
#> 233          Italy    IT   ITA 2006         2022-12-22
#> 234          Italy    IT   ITA 2007         2022-12-22
#> 235          Italy    IT   ITA 2008         2022-12-22
#> 236          Italy    IT   ITA 2009         2022-12-22
#> 237          Italy    IT   ITA 2010         2022-12-22
#> 238          Italy    IT   ITA 2011         2022-12-22
#> 239          Italy    IT   ITA 2012         2022-12-22
#> 240          Italy    IT   ITA 2013         2022-12-22
#> 241          Italy    IT   ITA 2014         2022-12-22
#> 242          Italy    IT   ITA 2015         2022-12-22
#> 243          Italy    IT   ITA 2016         2022-12-22
#> 244          Italy    IT   ITA 2017         2022-12-22
#> 245          Italy    IT   ITA 2018         2022-12-22
#> 246          Italy    IT   ITA 2019         2022-12-22
#> 247          Italy    IT   ITA 2020         2022-12-22
#> 248          Italy    IT   ITA 2021         2022-12-22
#> 249          Japan    JP   JPN 1960         2022-12-22
#> 250          Japan    JP   JPN 1961         2022-12-22
#> 251          Japan    JP   JPN 1962         2022-12-22
#> 252          Japan    JP   JPN 1963         2022-12-22
#> 253          Japan    JP   JPN 1964         2022-12-22
#> 254          Japan    JP   JPN 1965         2022-12-22
#> 255          Japan    JP   JPN 1966         2022-12-22
#> 256          Japan    JP   JPN 1967         2022-12-22
#> 257          Japan    JP   JPN 1968         2022-12-22
#> 258          Japan    JP   JPN 1969         2022-12-22
#> 259          Japan    JP   JPN 1970         2022-12-22
#> 260          Japan    JP   JPN 1971         2022-12-22
#> 261          Japan    JP   JPN 1972         2022-12-22
#> 262          Japan    JP   JPN 1973         2022-12-22
#> 263          Japan    JP   JPN 1974         2022-12-22
#> 264          Japan    JP   JPN 1975         2022-12-22
#> 265          Japan    JP   JPN 1976         2022-12-22
#> 266          Japan    JP   JPN 1977         2022-12-22
#> 267          Japan    JP   JPN 1978         2022-12-22
#> 268          Japan    JP   JPN 1979         2022-12-22
#> 269          Japan    JP   JPN 1980         2022-12-22
#> 270          Japan    JP   JPN 1981         2022-12-22
#> 271          Japan    JP   JPN 1982         2022-12-22
#> 272          Japan    JP   JPN 1983         2022-12-22
#> 273          Japan    JP   JPN 1984         2022-12-22
#> 274          Japan    JP   JPN 1985         2022-12-22
#> 275          Japan    JP   JPN 1986         2022-12-22
#> 276          Japan    JP   JPN 1987         2022-12-22
#> 277          Japan    JP   JPN 1988         2022-12-22
#> 278          Japan    JP   JPN 1989         2022-12-22
#> 279          Japan    JP   JPN 1990         2022-12-22
#> 280          Japan    JP   JPN 1991         2022-12-22
#> 281          Japan    JP   JPN 1992         2022-12-22
#> 282          Japan    JP   JPN 1993         2022-12-22
#> 283          Japan    JP   JPN 1994         2022-12-22
#> 284          Japan    JP   JPN 1995         2022-12-22
#> 285          Japan    JP   JPN 1996         2022-12-22
#> 286          Japan    JP   JPN 1997         2022-12-22
#> 287          Japan    JP   JPN 1998         2022-12-22
#> 288          Japan    JP   JPN 1999         2022-12-22
#> 289          Japan    JP   JPN 2000         2022-12-22
#> 290          Japan    JP   JPN 2001         2022-12-22
#> 291          Japan    JP   JPN 2002         2022-12-22
#> 292          Japan    JP   JPN 2003         2022-12-22
#> 293          Japan    JP   JPN 2004         2022-12-22
#> 294          Japan    JP   JPN 2005         2022-12-22
#> 295          Japan    JP   JPN 2006         2022-12-22
#> 296          Japan    JP   JPN 2007         2022-12-22
#> 297          Japan    JP   JPN 2008         2022-12-22
#> 298          Japan    JP   JPN 2009         2022-12-22
#> 299          Japan    JP   JPN 2010         2022-12-22
#> 300          Japan    JP   JPN 2011         2022-12-22
#> 301          Japan    JP   JPN 2012         2022-12-22
#> 302          Japan    JP   JPN 2013         2022-12-22
#> 303          Japan    JP   JPN 2014         2022-12-22
#> 304          Japan    JP   JPN 2015         2022-12-22
#> 305          Japan    JP   JPN 2016         2022-12-22
#> 306          Japan    JP   JPN 2017         2022-12-22
#> 307          Japan    JP   JPN 2018         2022-12-22
#> 308          Japan    JP   JPN 2019         2022-12-22
#> 309          Japan    JP   JPN 2020         2022-12-22
#> 310          Japan    JP   JPN 2021         2022-12-22
#> 311 United Kingdom    GB   GBR 1960         2022-12-22
#> 312 United Kingdom    GB   GBR 1961         2022-12-22
#> 313 United Kingdom    GB   GBR 1962         2022-12-22
#> 314 United Kingdom    GB   GBR 1963         2022-12-22
#> 315 United Kingdom    GB   GBR 1964         2022-12-22
#> 316 United Kingdom    GB   GBR 1965         2022-12-22
#> 317 United Kingdom    GB   GBR 1966         2022-12-22
#> 318 United Kingdom    GB   GBR 1967         2022-12-22
#> 319 United Kingdom    GB   GBR 1968         2022-12-22
#> 320 United Kingdom    GB   GBR 1969         2022-12-22
#> 321 United Kingdom    GB   GBR 1970         2022-12-22
#> 322 United Kingdom    GB   GBR 1971         2022-12-22
#> 323 United Kingdom    GB   GBR 1972         2022-12-22
#> 324 United Kingdom    GB   GBR 1973         2022-12-22
#> 325 United Kingdom    GB   GBR 1974         2022-12-22
#> 326 United Kingdom    GB   GBR 1975         2022-12-22
#> 327 United Kingdom    GB   GBR 1976         2022-12-22
#> 328 United Kingdom    GB   GBR 1977         2022-12-22
#> 329 United Kingdom    GB   GBR 1978         2022-12-22
#> 330 United Kingdom    GB   GBR 1979         2022-12-22
#> 331 United Kingdom    GB   GBR 1980         2022-12-22
#> 332 United Kingdom    GB   GBR 1981         2022-12-22
#> 333 United Kingdom    GB   GBR 1982         2022-12-22
#> 334 United Kingdom    GB   GBR 1983         2022-12-22
#> 335 United Kingdom    GB   GBR 1984         2022-12-22
#> 336 United Kingdom    GB   GBR 1985         2022-12-22
#> 337 United Kingdom    GB   GBR 1986         2022-12-22
#> 338 United Kingdom    GB   GBR 1987         2022-12-22
#> 339 United Kingdom    GB   GBR 1988         2022-12-22
#> 340 United Kingdom    GB   GBR 1989         2022-12-22
#> 341 United Kingdom    GB   GBR 1990         2022-12-22
#> 342 United Kingdom    GB   GBR 1991         2022-12-22
#> 343 United Kingdom    GB   GBR 1992         2022-12-22
#> 344 United Kingdom    GB   GBR 1993         2022-12-22
#> 345 United Kingdom    GB   GBR 1994         2022-12-22
#> 346 United Kingdom    GB   GBR 1995         2022-12-22
#> 347 United Kingdom    GB   GBR 1996         2022-12-22
#> 348 United Kingdom    GB   GBR 1997         2022-12-22
#> 349 United Kingdom    GB   GBR 1998         2022-12-22
#> 350 United Kingdom    GB   GBR 1999         2022-12-22
#> 351 United Kingdom    GB   GBR 2000         2022-12-22
#> 352 United Kingdom    GB   GBR 2001         2022-12-22
#> 353 United Kingdom    GB   GBR 2002         2022-12-22
#> 354 United Kingdom    GB   GBR 2003         2022-12-22
#> 355 United Kingdom    GB   GBR 2004         2022-12-22
#> 356 United Kingdom    GB   GBR 2005         2022-12-22
#> 357 United Kingdom    GB   GBR 2006         2022-12-22
#> 358 United Kingdom    GB   GBR 2007         2022-12-22
#> 359 United Kingdom    GB   GBR 2008         2022-12-22
#> 360 United Kingdom    GB   GBR 2009         2022-12-22
#> 361 United Kingdom    GB   GBR 2010         2022-12-22
#> 362 United Kingdom    GB   GBR 2011         2022-12-22
#> 363 United Kingdom    GB   GBR 2012         2022-12-22
#> 364 United Kingdom    GB   GBR 2013         2022-12-22
#> 365 United Kingdom    GB   GBR 2014         2022-12-22
#> 366 United Kingdom    GB   GBR 2015         2022-12-22
#> 367 United Kingdom    GB   GBR 2016         2022-12-22
#> 368 United Kingdom    GB   GBR 2017         2022-12-22
#> 369 United Kingdom    GB   GBR 2018         2022-12-22
#> 370 United Kingdom    GB   GBR 2019         2022-12-22
#> 371 United Kingdom    GB   GBR 2020         2022-12-22
#> 372 United Kingdom    GB   GBR 2021         2022-12-22
#> 373  United States    US   USA 1960         2022-12-22
#> 374  United States    US   USA 1961         2022-12-22
#> 375  United States    US   USA 1962         2022-12-22
#> 376  United States    US   USA 1963         2022-12-22
#> 377  United States    US   USA 1964         2022-12-22
#> 378  United States    US   USA 1965         2022-12-22
#> 379  United States    US   USA 1966         2022-12-22
#> 380  United States    US   USA 1967         2022-12-22
#> 381  United States    US   USA 1968         2022-12-22
#> 382  United States    US   USA 1969         2022-12-22
#> 383  United States    US   USA 1970         2022-12-22
#> 384  United States    US   USA 1971         2022-12-22
#> 385  United States    US   USA 1972         2022-12-22
#> 386  United States    US   USA 1973         2022-12-22
#> 387  United States    US   USA 1974         2022-12-22
#> 388  United States    US   USA 1975         2022-12-22
#> 389  United States    US   USA 1976         2022-12-22
#> 390  United States    US   USA 1977         2022-12-22
#> 391  United States    US   USA 1978         2022-12-22
#> 392  United States    US   USA 1979         2022-12-22
#> 393  United States    US   USA 1980         2022-12-22
#> 394  United States    US   USA 1981         2022-12-22
#> 395  United States    US   USA 1982         2022-12-22
#> 396  United States    US   USA 1983         2022-12-22
#> 397  United States    US   USA 1984         2022-12-22
#> 398  United States    US   USA 1985         2022-12-22
#> 399  United States    US   USA 1986         2022-12-22
#> 400  United States    US   USA 1987         2022-12-22
#> 401  United States    US   USA 1988         2022-12-22
#> 402  United States    US   USA 1989         2022-12-22
#> 403  United States    US   USA 1990         2022-12-22
#> 404  United States    US   USA 1991         2022-12-22
#> 405  United States    US   USA 1992         2022-12-22
#> 406  United States    US   USA 1993         2022-12-22
#> 407  United States    US   USA 1994         2022-12-22
#> 408  United States    US   USA 1995         2022-12-22
#> 409  United States    US   USA 1996         2022-12-22
#> 410  United States    US   USA 1997         2022-12-22
#> 411  United States    US   USA 1998         2022-12-22
#> 412  United States    US   USA 1999         2022-12-22
#> 413  United States    US   USA 2000         2022-12-22
#> 414  United States    US   USA 2001         2022-12-22
#> 415  United States    US   USA 2002         2022-12-22
#> 416  United States    US   USA 2003         2022-12-22
#> 417  United States    US   USA 2004         2022-12-22
#> 418  United States    US   USA 2005         2022-12-22
#> 419  United States    US   USA 2006         2022-12-22
#> 420  United States    US   USA 2007         2022-12-22
#> 421  United States    US   USA 2008         2022-12-22
#> 422  United States    US   USA 2009         2022-12-22
#> 423  United States    US   USA 2010         2022-12-22
#> 424  United States    US   USA 2011         2022-12-22
#> 425  United States    US   USA 2012         2022-12-22
#> 426  United States    US   USA 2013         2022-12-22
#> 427  United States    US   USA 2014         2022-12-22
#> 428  United States    US   USA 2015         2022-12-22
#> 429  United States    US   USA 2016         2022-12-22
#> 430  United States    US   USA 2017         2022-12-22
#> 431  United States    US   USA 2018         2022-12-22
#> 432  United States    US   USA 2019         2022-12-22
#> 433  United States    US   USA 2020         2022-12-22
#> 434  United States    US   USA 2021         2022-12-22
#>          parl gini                region         capital
#> 1          NA   NA         North America          Ottawa
#> 2          NA   NA         North America          Ottawa
#> 3          NA   NA         North America          Ottawa
#> 4          NA   NA         North America          Ottawa
#> 5          NA   NA         North America          Ottawa
#> 6          NA   NA         North America          Ottawa
#> 7          NA   NA         North America          Ottawa
#> 8          NA   NA         North America          Ottawa
#> 9          NA   NA         North America          Ottawa
#> 10         NA   NA         North America          Ottawa
#> 11         NA   NA         North America          Ottawa
#> 12         NA 37.3         North America          Ottawa
#> 13         NA   NA         North America          Ottawa
#> 14         NA   NA         North America          Ottawa
#> 15         NA   NA         North America          Ottawa
#> 16         NA 33.3         North America          Ottawa
#> 17         NA   NA         North America          Ottawa
#> 18         NA   NA         North America          Ottawa
#> 19         NA   NA         North America          Ottawa
#> 20         NA   NA         North America          Ottawa
#> 21         NA   NA         North America          Ottawa
#> 22         NA 32.4         North America          Ottawa
#> 23         NA   NA         North America          Ottawa
#> 24         NA   NA         North America          Ottawa
#> 25         NA   NA         North America          Ottawa
#> 26         NA   NA         North America          Ottawa
#> 27         NA   NA         North America          Ottawa
#> 28         NA 31.5         North America          Ottawa
#> 29         NA   NA         North America          Ottawa
#> 30         NA   NA         North America          Ottawa
#> 31         NA   NA         North America          Ottawa
#> 32         NA 31.0         North America          Ottawa
#> 33         NA   NA         North America          Ottawa
#> 34         NA   NA         North America          Ottawa
#> 35         NA 31.3         North America          Ottawa
#> 36         NA   NA         North America          Ottawa
#> 37         NA   NA         North America          Ottawa
#> 38  20.598007 31.6         North America          Ottawa
#> 39  20.598007 33.2         North America          Ottawa
#> 40  20.598007   NA         North America          Ottawa
#> 41         NA 33.3         North America          Ottawa
#> 42  20.598007   NA         North America          Ottawa
#> 43  20.598007   NA         North America          Ottawa
#> 44  20.598007   NA         North America          Ottawa
#> 45  21.103896 33.7         North America          Ottawa
#> 46  21.103896   NA         North America          Ottawa
#> 47  20.779221   NA         North America          Ottawa
#> 48  21.311475 33.8         North America          Ottawa
#> 49  22.077922   NA         North America          Ottawa
#> 50  22.077922   NA         North America          Ottawa
#> 51  22.077922 33.6         North America          Ottawa
#> 52  24.755700   NA         North America          Ottawa
#> 53  24.675325 33.5         North America          Ottawa
#> 54  24.675325 33.8         North America          Ottawa
#> 55  25.081433 33.2         North America          Ottawa
#> 56  26.035503 33.7         North America          Ottawa
#> 57  26.035503 32.7         North America          Ottawa
#> 58  26.268657 33.3         North America          Ottawa
#> 59  26.946108   NA         North America          Ottawa
#> 60  28.994083   NA         North America          Ottawa
#> 61  28.994083   NA         North America          Ottawa
#> 62  30.473373   NA         North America          Ottawa
#> 63         NA   NA Europe & Central Asia           Paris
#> 64         NA   NA Europe & Central Asia           Paris
#> 65         NA   NA Europe & Central Asia           Paris
#> 66         NA   NA Europe & Central Asia           Paris
#> 67         NA   NA Europe & Central Asia           Paris
#> 68         NA   NA Europe & Central Asia           Paris
#> 69         NA   NA Europe & Central Asia           Paris
#> 70         NA   NA Europe & Central Asia           Paris
#> 71         NA   NA Europe & Central Asia           Paris
#> 72         NA   NA Europe & Central Asia           Paris
#> 73         NA   NA Europe & Central Asia           Paris
#> 74         NA   NA Europe & Central Asia           Paris
#> 75         NA   NA Europe & Central Asia           Paris
#> 76         NA   NA Europe & Central Asia           Paris
#> 77         NA   NA Europe & Central Asia           Paris
#> 78         NA   NA Europe & Central Asia           Paris
#> 79         NA   NA Europe & Central Asia           Paris
#> 80         NA   NA Europe & Central Asia           Paris
#> 81         NA 35.2 Europe & Central Asia           Paris
#> 82         NA   NA Europe & Central Asia           Paris
#> 83         NA   NA Europe & Central Asia           Paris
#> 84         NA   NA Europe & Central Asia           Paris
#> 85         NA   NA Europe & Central Asia           Paris
#> 86         NA   NA Europe & Central Asia           Paris
#> 87         NA 36.9 Europe & Central Asia           Paris
#> 88         NA   NA Europe & Central Asia           Paris
#> 89         NA   NA Europe & Central Asia           Paris
#> 90         NA   NA Europe & Central Asia           Paris
#> 91         NA   NA Europe & Central Asia           Paris
#> 92         NA 32.2 Europe & Central Asia           Paris
#> 93         NA   NA Europe & Central Asia           Paris
#> 94         NA   NA Europe & Central Asia           Paris
#> 95         NA   NA Europe & Central Asia           Paris
#> 96         NA   NA Europe & Central Asia           Paris
#> 97         NA 32.3 Europe & Central Asia           Paris
#> 98         NA   NA Europe & Central Asia           Paris
#> 99         NA   NA Europe & Central Asia           Paris
#> 100 10.918544   NA Europe & Central Asia           Paris
#> 101 10.918544   NA Europe & Central Asia           Paris
#> 102 10.918544   NA Europe & Central Asia           Paris
#> 103 10.918544 31.1 Europe & Central Asia           Paris
#> 104 10.918544   NA Europe & Central Asia           Paris
#> 105 12.131716   NA Europe & Central Asia           Paris
#> 106 12.195122 31.4 Europe & Central Asia           Paris
#> 107 12.195122 30.6 Europe & Central Asia           Paris
#> 108 12.195122 29.8 Europe & Central Asia           Paris
#> 109 12.195122 29.7 Europe & Central Asia           Paris
#> 110 18.197574 32.4 Europe & Central Asia           Paris
#> 111 18.197574 33.0 Europe & Central Asia           Paris
#> 112 18.890815 32.7 Europe & Central Asia           Paris
#> 113 18.890815 33.7 Europe & Central Asia           Paris
#> 114 18.890815 33.3 Europe & Central Asia           Paris
#> 115 26.863085 33.1 Europe & Central Asia           Paris
#> 116 26.863085 32.5 Europe & Central Asia           Paris
#> 117 26.169844 32.3 Europe & Central Asia           Paris
#> 118 26.169844 32.7 Europe & Central Asia           Paris
#> 119 26.169844 31.9 Europe & Central Asia           Paris
#> 120 38.994801 31.6 Europe & Central Asia           Paris
#> 121 39.583333 32.4 Europe & Central Asia           Paris
#> 122 39.688042   NA Europe & Central Asia           Paris
#> 123 39.514731   NA Europe & Central Asia           Paris
#> 124 39.514731   NA Europe & Central Asia           Paris
#> 125        NA   NA Europe & Central Asia          Berlin
#> 126        NA   NA Europe & Central Asia          Berlin
#> 127        NA   NA Europe & Central Asia          Berlin
#> 128        NA   NA Europe & Central Asia          Berlin
#> 129        NA   NA Europe & Central Asia          Berlin
#> 130        NA   NA Europe & Central Asia          Berlin
#> 131        NA   NA Europe & Central Asia          Berlin
#> 132        NA   NA Europe & Central Asia          Berlin
#> 133        NA   NA Europe & Central Asia          Berlin
#> 134        NA   NA Europe & Central Asia          Berlin
#> 135        NA   NA Europe & Central Asia          Berlin
#> 136        NA   NA Europe & Central Asia          Berlin
#> 137        NA   NA Europe & Central Asia          Berlin
#> 138        NA   NA Europe & Central Asia          Berlin
#> 139        NA   NA Europe & Central Asia          Berlin
#> 140        NA   NA Europe & Central Asia          Berlin
#> 141        NA   NA Europe & Central Asia          Berlin
#> 142        NA   NA Europe & Central Asia          Berlin
#> 143        NA   NA Europe & Central Asia          Berlin
#> 144        NA   NA Europe & Central Asia          Berlin
#> 145        NA   NA Europe & Central Asia          Berlin
#> 146        NA   NA Europe & Central Asia          Berlin
#> 147        NA   NA Europe & Central Asia          Berlin
#> 148        NA   NA Europe & Central Asia          Berlin
#> 149        NA   NA Europe & Central Asia          Berlin
#> 150        NA   NA Europe & Central Asia          Berlin
#> 151        NA   NA Europe & Central Asia          Berlin
#> 152        NA   NA Europe & Central Asia          Berlin
#> 153        NA   NA Europe & Central Asia          Berlin
#> 154        NA   NA Europe & Central Asia          Berlin
#> 155        NA   NA Europe & Central Asia          Berlin
#> 156        NA 29.4 Europe & Central Asia          Berlin
#> 157        NA 29.2 Europe & Central Asia          Berlin
#> 158        NA 28.7 Europe & Central Asia          Berlin
#> 159        NA 29.2 Europe & Central Asia          Berlin
#> 160        NA 29.0 Europe & Central Asia          Berlin
#> 161        NA 28.4 Europe & Central Asia          Berlin
#> 162 26.190476 28.3 Europe & Central Asia          Berlin
#> 163 26.190476 28.3 Europe & Central Asia          Berlin
#> 164 30.941704 29.1 Europe & Central Asia          Berlin
#> 165 30.941704 28.9 Europe & Central Asia          Berlin
#> 166 31.081081 30.1 Europe & Central Asia          Berlin
#> 167 32.172471 29.9 Europe & Central Asia          Berlin
#> 168 32.172471 30.1 Europe & Central Asia          Berlin
#> 169 32.750000 30.3 Europe & Central Asia          Berlin
#> 170 31.758958 31.8 Europe & Central Asia          Berlin
#> 171 31.596091 31.2 Europe & Central Asia          Berlin
#> 172 31.647635 31.4 Europe & Central Asia          Berlin
#> 173 32.189542 30.9 Europe & Central Asia          Berlin
#> 174 32.797428 30.5 Europe & Central Asia          Berlin
#> 175 32.797428 30.3 Europe & Central Asia          Berlin
#> 176 32.903226 30.8 Europe & Central Asia          Berlin
#> 177 32.903226 31.1 Europe & Central Asia          Berlin
#> 178 36.450079 31.5 Europe & Central Asia          Berlin
#> 179 36.450079 30.9 Europe & Central Asia          Berlin
#> 180 36.450079 31.6 Europe & Central Asia          Berlin
#> 181 36.450079 31.6 Europe & Central Asia          Berlin
#> 182 30.747532 31.2 Europe & Central Asia          Berlin
#> 183 30.747532 31.7 Europe & Central Asia          Berlin
#> 184 30.888575   NA Europe & Central Asia          Berlin
#> 185 31.170663   NA Europe & Central Asia          Berlin
#> 186 34.918478   NA Europe & Central Asia          Berlin
#> 187        NA   NA Europe & Central Asia            Rome
#> 188        NA   NA Europe & Central Asia            Rome
#> 189        NA   NA Europe & Central Asia            Rome
#> 190        NA   NA Europe & Central Asia            Rome
#> 191        NA   NA Europe & Central Asia            Rome
#> 192        NA   NA Europe & Central Asia            Rome
#> 193        NA   NA Europe & Central Asia            Rome
#> 194        NA   NA Europe & Central Asia            Rome
#> 195        NA   NA Europe & Central Asia            Rome
#> 196        NA   NA Europe & Central Asia            Rome
#> 197        NA   NA Europe & Central Asia            Rome
#> 198        NA   NA Europe & Central Asia            Rome
#> 199        NA   NA Europe & Central Asia            Rome
#> 200        NA   NA Europe & Central Asia            Rome
#> 201        NA   NA Europe & Central Asia            Rome
#> 202        NA   NA Europe & Central Asia            Rome
#> 203        NA   NA Europe & Central Asia            Rome
#> 204        NA   NA Europe & Central Asia            Rome
#> 205        NA   NA Europe & Central Asia            Rome
#> 206        NA   NA Europe & Central Asia            Rome
#> 207        NA   NA Europe & Central Asia            Rome
#> 208        NA   NA Europe & Central Asia            Rome
#> 209        NA   NA Europe & Central Asia            Rome
#> 210        NA   NA Europe & Central Asia            Rome
#> 211        NA   NA Europe & Central Asia            Rome
#> 212        NA   NA Europe & Central Asia            Rome
#> 213        NA 32.5 Europe & Central Asia            Rome
#> 214        NA 34.5 Europe & Central Asia            Rome
#> 215        NA   NA Europe & Central Asia            Rome
#> 216        NA 32.6 Europe & Central Asia            Rome
#> 217        NA   NA Europe & Central Asia            Rome
#> 218        NA 31.5 Europe & Central Asia            Rome
#> 219        NA   NA Europe & Central Asia            Rome
#> 220        NA 35.5 Europe & Central Asia            Rome
#> 221        NA   NA Europe & Central Asia            Rome
#> 222        NA 35.2 Europe & Central Asia            Rome
#> 223        NA   NA Europe & Central Asia            Rome
#> 224 11.111111   NA Europe & Central Asia            Rome
#> 225 11.111111 36.7 Europe & Central Asia            Rome
#> 226 11.111111   NA Europe & Central Asia            Rome
#> 227 11.111111 35.3 Europe & Central Asia            Rome
#> 228  9.841270   NA Europe & Central Asia            Rome
#> 229  9.841270   NA Europe & Central Asia            Rome
#> 230 11.488673 34.9 Europe & Central Asia            Rome
#> 231 11.525974 34.3 Europe & Central Asia            Rome
#> 232 11.525974 33.8 Europe & Central Asia            Rome
#> 233 17.301587 33.7 Europe & Central Asia            Rome
#> 234 17.301587 32.9 Europe & Central Asia            Rome
#> 235 21.269841 33.8 Europe & Central Asia            Rome
#> 236 21.269841 33.8 Europe & Central Asia            Rome
#> 237 21.269841 34.7 Europe & Central Asia            Rome
#> 238 21.587302 35.1 Europe & Central Asia            Rome
#> 239 21.428571 35.2 Europe & Central Asia            Rome
#> 240 31.428571 34.9 Europe & Central Asia            Rome
#> 241 31.428571 34.7 Europe & Central Asia            Rome
#> 242 30.952381 35.4 Europe & Central Asia            Rome
#> 243 30.952381 35.2 Europe & Central Asia            Rome
#> 244 30.952381 35.9 Europe & Central Asia            Rome
#> 245 35.714286 35.2 Europe & Central Asia            Rome
#> 246 35.714286   NA Europe & Central Asia            Rome
#> 247 35.714286   NA Europe & Central Asia            Rome
#> 248 35.714286   NA Europe & Central Asia            Rome
#> 249        NA   NA   East Asia & Pacific           Tokyo
#> 250        NA   NA   East Asia & Pacific           Tokyo
#> 251        NA   NA   East Asia & Pacific           Tokyo
#> 252        NA   NA   East Asia & Pacific           Tokyo
#> 253        NA   NA   East Asia & Pacific           Tokyo
#> 254        NA   NA   East Asia & Pacific           Tokyo
#> 255        NA   NA   East Asia & Pacific           Tokyo
#> 256        NA   NA   East Asia & Pacific           Tokyo
#> 257        NA   NA   East Asia & Pacific           Tokyo
#> 258        NA   NA   East Asia & Pacific           Tokyo
#> 259        NA   NA   East Asia & Pacific           Tokyo
#> 260        NA   NA   East Asia & Pacific           Tokyo
#> 261        NA   NA   East Asia & Pacific           Tokyo
#> 262        NA   NA   East Asia & Pacific           Tokyo
#> 263        NA   NA   East Asia & Pacific           Tokyo
#> 264        NA   NA   East Asia & Pacific           Tokyo
#> 265        NA   NA   East Asia & Pacific           Tokyo
#> 266        NA   NA   East Asia & Pacific           Tokyo
#> 267        NA   NA   East Asia & Pacific           Tokyo
#> 268        NA   NA   East Asia & Pacific           Tokyo
#> 269        NA   NA   East Asia & Pacific           Tokyo
#> 270        NA   NA   East Asia & Pacific           Tokyo
#> 271        NA   NA   East Asia & Pacific           Tokyo
#> 272        NA   NA   East Asia & Pacific           Tokyo
#> 273        NA   NA   East Asia & Pacific           Tokyo
#> 274        NA   NA   East Asia & Pacific           Tokyo
#> 275        NA   NA   East Asia & Pacific           Tokyo
#> 276        NA   NA   East Asia & Pacific           Tokyo
#> 277        NA   NA   East Asia & Pacific           Tokyo
#> 278        NA   NA   East Asia & Pacific           Tokyo
#> 279        NA   NA   East Asia & Pacific           Tokyo
#> 280        NA   NA   East Asia & Pacific           Tokyo
#> 281        NA   NA   East Asia & Pacific           Tokyo
#> 282        NA   NA   East Asia & Pacific           Tokyo
#> 283        NA   NA   East Asia & Pacific           Tokyo
#> 284        NA   NA   East Asia & Pacific           Tokyo
#> 285        NA   NA   East Asia & Pacific           Tokyo
#> 286  4.600000   NA   East Asia & Pacific           Tokyo
#> 287  4.600000   NA   East Asia & Pacific           Tokyo
#> 288  4.600000   NA   East Asia & Pacific           Tokyo
#> 289  7.291667   NA   East Asia & Pacific           Tokyo
#> 290  7.291667   NA   East Asia & Pacific           Tokyo
#> 291  7.291667   NA   East Asia & Pacific           Tokyo
#> 292  7.083333   NA   East Asia & Pacific           Tokyo
#> 293  7.083333   NA   East Asia & Pacific           Tokyo
#> 294  8.958333   NA   East Asia & Pacific           Tokyo
#> 295  9.375000   NA   East Asia & Pacific           Tokyo
#> 296  9.375000   NA   East Asia & Pacific           Tokyo
#> 297  9.375000 34.8   East Asia & Pacific           Tokyo
#> 298 11.250000   NA   East Asia & Pacific           Tokyo
#> 299 11.250000 32.1   East Asia & Pacific           Tokyo
#> 300 10.833333   NA   East Asia & Pacific           Tokyo
#> 301  7.916667   NA   East Asia & Pacific           Tokyo
#> 302  8.125000 32.9   East Asia & Pacific           Tokyo
#> 303  8.125000   NA   East Asia & Pacific           Tokyo
#> 304  9.473684   NA   East Asia & Pacific           Tokyo
#> 305  9.473684   NA   East Asia & Pacific           Tokyo
#> 306 10.107527   NA   East Asia & Pacific           Tokyo
#> 307 10.107527   NA   East Asia & Pacific           Tokyo
#> 308 10.107527   NA   East Asia & Pacific           Tokyo
#> 309  9.892473   NA   East Asia & Pacific           Tokyo
#> 310  9.677419   NA   East Asia & Pacific           Tokyo
#> 311        NA   NA Europe & Central Asia          London
#> 312        NA   NA Europe & Central Asia          London
#> 313        NA   NA Europe & Central Asia          London
#> 314        NA   NA Europe & Central Asia          London
#> 315        NA   NA Europe & Central Asia          London
#> 316        NA   NA Europe & Central Asia          London
#> 317        NA   NA Europe & Central Asia          London
#> 318        NA   NA Europe & Central Asia          London
#> 319        NA   NA Europe & Central Asia          London
#> 320        NA 33.7 Europe & Central Asia          London
#> 321        NA   NA Europe & Central Asia          London
#> 322        NA   NA Europe & Central Asia          London
#> 323        NA   NA Europe & Central Asia          London
#> 324        NA   NA Europe & Central Asia          London
#> 325        NA 30.0 Europe & Central Asia          London
#> 326        NA   NA Europe & Central Asia          London
#> 327        NA   NA Europe & Central Asia          London
#> 328        NA   NA Europe & Central Asia          London
#> 329        NA   NA Europe & Central Asia          London
#> 330        NA 28.4 Europe & Central Asia          London
#> 331        NA   NA Europe & Central Asia          London
#> 332        NA   NA Europe & Central Asia          London
#> 333        NA   NA Europe & Central Asia          London
#> 334        NA   NA Europe & Central Asia          London
#> 335        NA   NA Europe & Central Asia          London
#> 336        NA   NA Europe & Central Asia          London
#> 337        NA 31.9 Europe & Central Asia          London
#> 338        NA   NA Europe & Central Asia          London
#> 339        NA   NA Europe & Central Asia          London
#> 340        NA   NA Europe & Central Asia          London
#> 341        NA   NA Europe & Central Asia          London
#> 342        NA 35.9 Europe & Central Asia          London
#> 343        NA   NA Europe & Central Asia          London
#> 344        NA   NA Europe & Central Asia          London
#> 345        NA 36.0 Europe & Central Asia          London
#> 346        NA 35.5 Europe & Central Asia          London
#> 347        NA 35.3 Europe & Central Asia          London
#> 348 18.209408 35.7 Europe & Central Asia          London
#> 349 18.209408 36.6 Europe & Central Asia          London
#> 350 18.361153 36.8 Europe & Central Asia          London
#> 351 18.361153 39.6 Europe & Central Asia          London
#> 352 17.905918 37.9 Europe & Central Asia          London
#> 353 17.905918 35.7 Europe & Central Asia          London
#> 354 17.905918 35.5 Europe & Central Asia          London
#> 355 18.057663 36.0 Europe & Central Asia          London
#> 356 19.659443 34.3 Europe & Central Asia          London
#> 357 19.659443 34.6 Europe & Central Asia          London
#> 358 19.504644 35.7 Europe & Central Asia          London
#> 359 19.504644 34.1 Europe & Central Asia          London
#> 360 19.504644 34.3 Europe & Central Asia          London
#> 361 22.000000 34.4 Europe & Central Asia          London
#> 362 22.307692 33.2 Europe & Central Asia          London
#> 363 22.461538 32.3 Europe & Central Asia          London
#> 364 22.461538 33.2 Europe & Central Asia          London
#> 365 22.615385 34.0 Europe & Central Asia          London
#> 366 29.384615 33.2 Europe & Central Asia          London
#> 367 29.583975 34.8 Europe & Central Asia          London
#> 368 32.000000 35.1 Europe & Central Asia          London
#> 369 32.153846   NA Europe & Central Asia          London
#> 370 32.000000   NA Europe & Central Asia          London
#> 371 33.846154   NA Europe & Central Asia          London
#> 372 34.259259   NA Europe & Central Asia          London
#> 373        NA   NA         North America Washington D.C.
#> 374        NA   NA         North America Washington D.C.
#> 375        NA   NA         North America Washington D.C.
#> 376        NA   NA         North America Washington D.C.
#> 377        NA   NA         North America Washington D.C.
#> 378        NA   NA         North America Washington D.C.
#> 379        NA   NA         North America Washington D.C.
#> 380        NA   NA         North America Washington D.C.
#> 381        NA   NA         North America Washington D.C.
#> 382        NA   NA         North America Washington D.C.
#> 383        NA   NA         North America Washington D.C.
#> 384        NA   NA         North America Washington D.C.
#> 385        NA   NA         North America Washington D.C.
#> 386        NA   NA         North America Washington D.C.
#> 387        NA 35.3         North America Washington D.C.
#> 388        NA   NA         North America Washington D.C.
#> 389        NA   NA         North America Washington D.C.
#> 390        NA   NA         North America Washington D.C.
#> 391        NA   NA         North America Washington D.C.
#> 392        NA 34.5         North America Washington D.C.
#> 393        NA   NA         North America Washington D.C.
#> 394        NA   NA         North America Washington D.C.
#> 395        NA   NA         North America Washington D.C.
#> 396        NA   NA         North America Washington D.C.
#> 397        NA   NA         North America Washington D.C.
#> 398        NA   NA         North America Washington D.C.
#> 399        NA 37.4         North America Washington D.C.
#> 400        NA   NA         North America Washington D.C.
#> 401        NA   NA         North America Washington D.C.
#> 402        NA   NA         North America Washington D.C.
#> 403        NA   NA         North America Washington D.C.
#> 404        NA 38.0         North America Washington D.C.
#> 405        NA 38.4         North America Washington D.C.
#> 406        NA 40.4         North America Washington D.C.
#> 407        NA 40.0         North America Washington D.C.
#> 408        NA 39.9         North America Washington D.C.
#> 409        NA 40.3         North America Washington D.C.
#> 410 11.724138 40.5         North America Washington D.C.
#> 411 11.724138 40.0         North America Washington D.C.
#> 412 13.333333 40.0         North America Washington D.C.
#> 413        NA 40.1         North America Washington D.C.
#> 414 14.022989 40.6         North America Washington D.C.
#> 415 13.793103 40.4         North America Washington D.C.
#> 416 14.252874 40.8         North America Washington D.C.
#> 417 14.942529 40.3         North America Washington D.C.
#> 418 15.172414 41.0         North America Washington D.C.
#> 419 16.321839 41.4         North America Washington D.C.
#> 420 16.781609 40.8         North America Washington D.C.
#> 421 17.011494 40.8         North America Washington D.C.
#> 422 16.781609 40.6         North America Washington D.C.
#> 423 16.781609 40.0         North America Washington D.C.
#> 424 16.820276 40.9         North America Washington D.C.
#> 425 17.972350 40.9         North America Washington D.C.
#> 426 17.824074 40.7         North America Washington D.C.
#> 427 19.310345 41.5         North America Washington D.C.
#> 428 19.354839 41.2         North America Washington D.C.
#> 429 19.168591 41.1         North America Washington D.C.
#> 430 19.354839 41.2         North America Washington D.C.
#> 431 23.502304 41.4         North America Washington D.C.
#> 432 23.433875 41.5         North America Washington D.C.
#> 433 27.464789   NA         North America Washington D.C.
#> 434 27.649770   NA         North America Washington D.C.
#>     longitude latitude      income        lending
#> 1    -75.6919  45.4215 High income Not classified
#> 2    -75.6919  45.4215 High income Not classified
#> 3    -75.6919  45.4215 High income Not classified
#> 4    -75.6919  45.4215 High income Not classified
#> 5    -75.6919  45.4215 High income Not classified
#> 6    -75.6919  45.4215 High income Not classified
#> 7    -75.6919  45.4215 High income Not classified
#> 8    -75.6919  45.4215 High income Not classified
#> 9    -75.6919  45.4215 High income Not classified
#> 10   -75.6919  45.4215 High income Not classified
#> 11   -75.6919  45.4215 High income Not classified
#> 12   -75.6919  45.4215 High income Not classified
#> 13   -75.6919  45.4215 High income Not classified
#> 14   -75.6919  45.4215 High income Not classified
#> 15   -75.6919  45.4215 High income Not classified
#> 16   -75.6919  45.4215 High income Not classified
#> 17   -75.6919  45.4215 High income Not classified
#> 18   -75.6919  45.4215 High income Not classified
#> 19   -75.6919  45.4215 High income Not classified
#> 20   -75.6919  45.4215 High income Not classified
#> 21   -75.6919  45.4215 High income Not classified
#> 22   -75.6919  45.4215 High income Not classified
#> 23   -75.6919  45.4215 High income Not classified
#> 24   -75.6919  45.4215 High income Not classified
#> 25   -75.6919  45.4215 High income Not classified
#> 26   -75.6919  45.4215 High income Not classified
#> 27   -75.6919  45.4215 High income Not classified
#> 28   -75.6919  45.4215 High income Not classified
#> 29   -75.6919  45.4215 High income Not classified
#> 30   -75.6919  45.4215 High income Not classified
#> 31   -75.6919  45.4215 High income Not classified
#> 32   -75.6919  45.4215 High income Not classified
#> 33   -75.6919  45.4215 High income Not classified
#> 34   -75.6919  45.4215 High income Not classified
#> 35   -75.6919  45.4215 High income Not classified
#> 36   -75.6919  45.4215 High income Not classified
#> 37   -75.6919  45.4215 High income Not classified
#> 38   -75.6919  45.4215 High income Not classified
#> 39   -75.6919  45.4215 High income Not classified
#> 40   -75.6919  45.4215 High income Not classified
#> 41   -75.6919  45.4215 High income Not classified
#> 42   -75.6919  45.4215 High income Not classified
#> 43   -75.6919  45.4215 High income Not classified
#> 44   -75.6919  45.4215 High income Not classified
#> 45   -75.6919  45.4215 High income Not classified
#> 46   -75.6919  45.4215 High income Not classified
#> 47   -75.6919  45.4215 High income Not classified
#> 48   -75.6919  45.4215 High income Not classified
#> 49   -75.6919  45.4215 High income Not classified
#> 50   -75.6919  45.4215 High income Not classified
#> 51   -75.6919  45.4215 High income Not classified
#> 52   -75.6919  45.4215 High income Not classified
#> 53   -75.6919  45.4215 High income Not classified
#> 54   -75.6919  45.4215 High income Not classified
#> 55   -75.6919  45.4215 High income Not classified
#> 56   -75.6919  45.4215 High income Not classified
#> 57   -75.6919  45.4215 High income Not classified
#> 58   -75.6919  45.4215 High income Not classified
#> 59   -75.6919  45.4215 High income Not classified
#> 60   -75.6919  45.4215 High income Not classified
#> 61   -75.6919  45.4215 High income Not classified
#> 62   -75.6919  45.4215 High income Not classified
#> 63    2.35097  48.8566 High income Not classified
#> 64    2.35097  48.8566 High income Not classified
#> 65    2.35097  48.8566 High income Not classified
#> 66    2.35097  48.8566 High income Not classified
#> 67    2.35097  48.8566 High income Not classified
#> 68    2.35097  48.8566 High income Not classified
#> 69    2.35097  48.8566 High income Not classified
#> 70    2.35097  48.8566 High income Not classified
#> 71    2.35097  48.8566 High income Not classified
#> 72    2.35097  48.8566 High income Not classified
#> 73    2.35097  48.8566 High income Not classified
#> 74    2.35097  48.8566 High income Not classified
#> 75    2.35097  48.8566 High income Not classified
#> 76    2.35097  48.8566 High income Not classified
#> 77    2.35097  48.8566 High income Not classified
#> 78    2.35097  48.8566 High income Not classified
#> 79    2.35097  48.8566 High income Not classified
#> 80    2.35097  48.8566 High income Not classified
#> 81    2.35097  48.8566 High income Not classified
#> 82    2.35097  48.8566 High income Not classified
#> 83    2.35097  48.8566 High income Not classified
#> 84    2.35097  48.8566 High income Not classified
#> 85    2.35097  48.8566 High income Not classified
#> 86    2.35097  48.8566 High income Not classified
#> 87    2.35097  48.8566 High income Not classified
#> 88    2.35097  48.8566 High income Not classified
#> 89    2.35097  48.8566 High income Not classified
#> 90    2.35097  48.8566 High income Not classified
#> 91    2.35097  48.8566 High income Not classified
#> 92    2.35097  48.8566 High income Not classified
#> 93    2.35097  48.8566 High income Not classified
#> 94    2.35097  48.8566 High income Not classified
#> 95    2.35097  48.8566 High income Not classified
#> 96    2.35097  48.8566 High income Not classified
#> 97    2.35097  48.8566 High income Not classified
#> 98    2.35097  48.8566 High income Not classified
#> 99    2.35097  48.8566 High income Not classified
#> 100   2.35097  48.8566 High income Not classified
#> 101   2.35097  48.8566 High income Not classified
#> 102   2.35097  48.8566 High income Not classified
#> 103   2.35097  48.8566 High income Not classified
#> 104   2.35097  48.8566 High income Not classified
#> 105   2.35097  48.8566 High income Not classified
#> 106   2.35097  48.8566 High income Not classified
#> 107   2.35097  48.8566 High income Not classified
#> 108   2.35097  48.8566 High income Not classified
#> 109   2.35097  48.8566 High income Not classified
#> 110   2.35097  48.8566 High income Not classified
#> 111   2.35097  48.8566 High income Not classified
#> 112   2.35097  48.8566 High income Not classified
#> 113   2.35097  48.8566 High income Not classified
#> 114   2.35097  48.8566 High income Not classified
#> 115   2.35097  48.8566 High income Not classified
#> 116   2.35097  48.8566 High income Not classified
#> 117   2.35097  48.8566 High income Not classified
#> 118   2.35097  48.8566 High income Not classified
#> 119   2.35097  48.8566 High income Not classified
#> 120   2.35097  48.8566 High income Not classified
#> 121   2.35097  48.8566 High income Not classified
#> 122   2.35097  48.8566 High income Not classified
#> 123   2.35097  48.8566 High income Not classified
#> 124   2.35097  48.8566 High income Not classified
#> 125   13.4115  52.5235 High income Not classified
#> 126   13.4115  52.5235 High income Not classified
#> 127   13.4115  52.5235 High income Not classified
#> 128   13.4115  52.5235 High income Not classified
#> 129   13.4115  52.5235 High income Not classified
#> 130   13.4115  52.5235 High income Not classified
#> 131   13.4115  52.5235 High income Not classified
#> 132   13.4115  52.5235 High income Not classified
#> 133   13.4115  52.5235 High income Not classified
#> 134   13.4115  52.5235 High income Not classified
#> 135   13.4115  52.5235 High income Not classified
#> 136   13.4115  52.5235 High income Not classified
#> 137   13.4115  52.5235 High income Not classified
#> 138   13.4115  52.5235 High income Not classified
#> 139   13.4115  52.5235 High income Not classified
#> 140   13.4115  52.5235 High income Not classified
#> 141   13.4115  52.5235 High income Not classified
#> 142   13.4115  52.5235 High income Not classified
#> 143   13.4115  52.5235 High income Not classified
#> 144   13.4115  52.5235 High income Not classified
#> 145   13.4115  52.5235 High income Not classified
#> 146   13.4115  52.5235 High income Not classified
#> 147   13.4115  52.5235 High income Not classified
#> 148   13.4115  52.5235 High income Not classified
#> 149   13.4115  52.5235 High income Not classified
#> 150   13.4115  52.5235 High income Not classified
#> 151   13.4115  52.5235 High income Not classified
#> 152   13.4115  52.5235 High income Not classified
#> 153   13.4115  52.5235 High income Not classified
#> 154   13.4115  52.5235 High income Not classified
#> 155   13.4115  52.5235 High income Not classified
#> 156   13.4115  52.5235 High income Not classified
#> 157   13.4115  52.5235 High income Not classified
#> 158   13.4115  52.5235 High income Not classified
#> 159   13.4115  52.5235 High income Not classified
#> 160   13.4115  52.5235 High income Not classified
#> 161   13.4115  52.5235 High income Not classified
#> 162   13.4115  52.5235 High income Not classified
#> 163   13.4115  52.5235 High income Not classified
#> 164   13.4115  52.5235 High income Not classified
#> 165   13.4115  52.5235 High income Not classified
#> 166   13.4115  52.5235 High income Not classified
#> 167   13.4115  52.5235 High income Not classified
#> 168   13.4115  52.5235 High income Not classified
#> 169   13.4115  52.5235 High income Not classified
#> 170   13.4115  52.5235 High income Not classified
#> 171   13.4115  52.5235 High income Not classified
#> 172   13.4115  52.5235 High income Not classified
#> 173   13.4115  52.5235 High income Not classified
#> 174   13.4115  52.5235 High income Not classified
#> 175   13.4115  52.5235 High income Not classified
#> 176   13.4115  52.5235 High income Not classified
#> 177   13.4115  52.5235 High income Not classified
#> 178   13.4115  52.5235 High income Not classified
#> 179   13.4115  52.5235 High income Not classified
#> 180   13.4115  52.5235 High income Not classified
#> 181   13.4115  52.5235 High income Not classified
#> 182   13.4115  52.5235 High income Not classified
#> 183   13.4115  52.5235 High income Not classified
#> 184   13.4115  52.5235 High income Not classified
#> 185   13.4115  52.5235 High income Not classified
#> 186   13.4115  52.5235 High income Not classified
#> 187   12.4823  41.8955 High income Not classified
#> 188   12.4823  41.8955 High income Not classified
#> 189   12.4823  41.8955 High income Not classified
#> 190   12.4823  41.8955 High income Not classified
#> 191   12.4823  41.8955 High income Not classified
#> 192   12.4823  41.8955 High income Not classified
#> 193   12.4823  41.8955 High income Not classified
#> 194   12.4823  41.8955 High income Not classified
#> 195   12.4823  41.8955 High income Not classified
#> 196   12.4823  41.8955 High income Not classified
#> 197   12.4823  41.8955 High income Not classified
#> 198   12.4823  41.8955 High income Not classified
#> 199   12.4823  41.8955 High income Not classified
#> 200   12.4823  41.8955 High income Not classified
#> 201   12.4823  41.8955 High income Not classified
#> 202   12.4823  41.8955 High income Not classified
#> 203   12.4823  41.8955 High income Not classified
#> 204   12.4823  41.8955 High income Not classified
#> 205   12.4823  41.8955 High income Not classified
#> 206   12.4823  41.8955 High income Not classified
#> 207   12.4823  41.8955 High income Not classified
#> 208   12.4823  41.8955 High income Not classified
#> 209   12.4823  41.8955 High income Not classified
#> 210   12.4823  41.8955 High income Not classified
#> 211   12.4823  41.8955 High income Not classified
#> 212   12.4823  41.8955 High income Not classified
#> 213   12.4823  41.8955 High income Not classified
#> 214   12.4823  41.8955 High income Not classified
#> 215   12.4823  41.8955 High income Not classified
#> 216   12.4823  41.8955 High income Not classified
#> 217   12.4823  41.8955 High income Not classified
#> 218   12.4823  41.8955 High income Not classified
#> 219   12.4823  41.8955 High income Not classified
#> 220   12.4823  41.8955 High income Not classified
#> 221   12.4823  41.8955 High income Not classified
#> 222   12.4823  41.8955 High income Not classified
#> 223   12.4823  41.8955 High income Not classified
#> 224   12.4823  41.8955 High income Not classified
#> 225   12.4823  41.8955 High income Not classified
#> 226   12.4823  41.8955 High income Not classified
#> 227   12.4823  41.8955 High income Not classified
#> 228   12.4823  41.8955 High income Not classified
#> 229   12.4823  41.8955 High income Not classified
#> 230   12.4823  41.8955 High income Not classified
#> 231   12.4823  41.8955 High income Not classified
#> 232   12.4823  41.8955 High income Not classified
#> 233   12.4823  41.8955 High income Not classified
#> 234   12.4823  41.8955 High income Not classified
#> 235   12.4823  41.8955 High income Not classified
#> 236   12.4823  41.8955 High income Not classified
#> 237   12.4823  41.8955 High income Not classified
#> 238   12.4823  41.8955 High income Not classified
#> 239   12.4823  41.8955 High income Not classified
#> 240   12.4823  41.8955 High income Not classified
#> 241   12.4823  41.8955 High income Not classified
#> 242   12.4823  41.8955 High income Not classified
#> 243   12.4823  41.8955 High income Not classified
#> 244   12.4823  41.8955 High income Not classified
#> 245   12.4823  41.8955 High income Not classified
#> 246   12.4823  41.8955 High income Not classified
#> 247   12.4823  41.8955 High income Not classified
#> 248   12.4823  41.8955 High income Not classified
#> 249    139.77    35.67 High income Not classified
#> 250    139.77    35.67 High income Not classified
#> 251    139.77    35.67 High income Not classified
#> 252    139.77    35.67 High income Not classified
#> 253    139.77    35.67 High income Not classified
#> 254    139.77    35.67 High income Not classified
#> 255    139.77    35.67 High income Not classified
#> 256    139.77    35.67 High income Not classified
#> 257    139.77    35.67 High income Not classified
#> 258    139.77    35.67 High income Not classified
#> 259    139.77    35.67 High income Not classified
#> 260    139.77    35.67 High income Not classified
#> 261    139.77    35.67 High income Not classified
#> 262    139.77    35.67 High income Not classified
#> 263    139.77    35.67 High income Not classified
#> 264    139.77    35.67 High income Not classified
#> 265    139.77    35.67 High income Not classified
#> 266    139.77    35.67 High income Not classified
#> 267    139.77    35.67 High income Not classified
#> 268    139.77    35.67 High income Not classified
#> 269    139.77    35.67 High income Not classified
#> 270    139.77    35.67 High income Not classified
#> 271    139.77    35.67 High income Not classified
#> 272    139.77    35.67 High income Not classified
#> 273    139.77    35.67 High income Not classified
#> 274    139.77    35.67 High income Not classified
#> 275    139.77    35.67 High income Not classified
#> 276    139.77    35.67 High income Not classified
#> 277    139.77    35.67 High income Not classified
#> 278    139.77    35.67 High income Not classified
#> 279    139.77    35.67 High income Not classified
#> 280    139.77    35.67 High income Not classified
#> 281    139.77    35.67 High income Not classified
#> 282    139.77    35.67 High income Not classified
#> 283    139.77    35.67 High income Not classified
#> 284    139.77    35.67 High income Not classified
#> 285    139.77    35.67 High income Not classified
#> 286    139.77    35.67 High income Not classified
#> 287    139.77    35.67 High income Not classified
#> 288    139.77    35.67 High income Not classified
#> 289    139.77    35.67 High income Not classified
#> 290    139.77    35.67 High income Not classified
#> 291    139.77    35.67 High income Not classified
#> 292    139.77    35.67 High income Not classified
#> 293    139.77    35.67 High income Not classified
#> 294    139.77    35.67 High income Not classified
#> 295    139.77    35.67 High income Not classified
#> 296    139.77    35.67 High income Not classified
#> 297    139.77    35.67 High income Not classified
#> 298    139.77    35.67 High income Not classified
#> 299    139.77    35.67 High income Not classified
#> 300    139.77    35.67 High income Not classified
#> 301    139.77    35.67 High income Not classified
#> 302    139.77    35.67 High income Not classified
#> 303    139.77    35.67 High income Not classified
#> 304    139.77    35.67 High income Not classified
#> 305    139.77    35.67 High income Not classified
#> 306    139.77    35.67 High income Not classified
#> 307    139.77    35.67 High income Not classified
#> 308    139.77    35.67 High income Not classified
#> 309    139.77    35.67 High income Not classified
#> 310    139.77    35.67 High income Not classified
#> 311 -0.126236  51.5002 High income Not classified
#> 312 -0.126236  51.5002 High income Not classified
#> 313 -0.126236  51.5002 High income Not classified
#> 314 -0.126236  51.5002 High income Not classified
#> 315 -0.126236  51.5002 High income Not classified
#> 316 -0.126236  51.5002 High income Not classified
#> 317 -0.126236  51.5002 High income Not classified
#> 318 -0.126236  51.5002 High income Not classified
#> 319 -0.126236  51.5002 High income Not classified
#> 320 -0.126236  51.5002 High income Not classified
#> 321 -0.126236  51.5002 High income Not classified
#> 322 -0.126236  51.5002 High income Not classified
#> 323 -0.126236  51.5002 High income Not classified
#> 324 -0.126236  51.5002 High income Not classified
#> 325 -0.126236  51.5002 High income Not classified
#> 326 -0.126236  51.5002 High income Not classified
#> 327 -0.126236  51.5002 High income Not classified
#> 328 -0.126236  51.5002 High income Not classified
#> 329 -0.126236  51.5002 High income Not classified
#> 330 -0.126236  51.5002 High income Not classified
#> 331 -0.126236  51.5002 High income Not classified
#> 332 -0.126236  51.5002 High income Not classified
#> 333 -0.126236  51.5002 High income Not classified
#> 334 -0.126236  51.5002 High income Not classified
#> 335 -0.126236  51.5002 High income Not classified
#> 336 -0.126236  51.5002 High income Not classified
#> 337 -0.126236  51.5002 High income Not classified
#> 338 -0.126236  51.5002 High income Not classified
#> 339 -0.126236  51.5002 High income Not classified
#> 340 -0.126236  51.5002 High income Not classified
#> 341 -0.126236  51.5002 High income Not classified
#> 342 -0.126236  51.5002 High income Not classified
#> 343 -0.126236  51.5002 High income Not classified
#> 344 -0.126236  51.5002 High income Not classified
#> 345 -0.126236  51.5002 High income Not classified
#> 346 -0.126236  51.5002 High income Not classified
#> 347 -0.126236  51.5002 High income Not classified
#> 348 -0.126236  51.5002 High income Not classified
#> 349 -0.126236  51.5002 High income Not classified
#> 350 -0.126236  51.5002 High income Not classified
#> 351 -0.126236  51.5002 High income Not classified
#> 352 -0.126236  51.5002 High income Not classified
#> 353 -0.126236  51.5002 High income Not classified
#> 354 -0.126236  51.5002 High income Not classified
#> 355 -0.126236  51.5002 High income Not classified
#> 356 -0.126236  51.5002 High income Not classified
#> 357 -0.126236  51.5002 High income Not classified
#> 358 -0.126236  51.5002 High income Not classified
#> 359 -0.126236  51.5002 High income Not classified
#> 360 -0.126236  51.5002 High income Not classified
#> 361 -0.126236  51.5002 High income Not classified
#> 362 -0.126236  51.5002 High income Not classified
#> 363 -0.126236  51.5002 High income Not classified
#> 364 -0.126236  51.5002 High income Not classified
#> 365 -0.126236  51.5002 High income Not classified
#> 366 -0.126236  51.5002 High income Not classified
#> 367 -0.126236  51.5002 High income Not classified
#> 368 -0.126236  51.5002 High income Not classified
#> 369 -0.126236  51.5002 High income Not classified
#> 370 -0.126236  51.5002 High income Not classified
#> 371 -0.126236  51.5002 High income Not classified
#> 372 -0.126236  51.5002 High income Not classified
#> 373   -77.032  38.8895 High income Not classified
#> 374   -77.032  38.8895 High income Not classified
#> 375   -77.032  38.8895 High income Not classified
#> 376   -77.032  38.8895 High income Not classified
#> 377   -77.032  38.8895 High income Not classified
#> 378   -77.032  38.8895 High income Not classified
#> 379   -77.032  38.8895 High income Not classified
#> 380   -77.032  38.8895 High income Not classified
#> 381   -77.032  38.8895 High income Not classified
#> 382   -77.032  38.8895 High income Not classified
#> 383   -77.032  38.8895 High income Not classified
#> 384   -77.032  38.8895 High income Not classified
#> 385   -77.032  38.8895 High income Not classified
#> 386   -77.032  38.8895 High income Not classified
#> 387   -77.032  38.8895 High income Not classified
#> 388   -77.032  38.8895 High income Not classified
#> 389   -77.032  38.8895 High income Not classified
#> 390   -77.032  38.8895 High income Not classified
#> 391   -77.032  38.8895 High income Not classified
#> 392   -77.032  38.8895 High income Not classified
#> 393   -77.032  38.8895 High income Not classified
#> 394   -77.032  38.8895 High income Not classified
#> 395   -77.032  38.8895 High income Not classified
#> 396   -77.032  38.8895 High income Not classified
#> 397   -77.032  38.8895 High income Not classified
#> 398   -77.032  38.8895 High income Not classified
#> 399   -77.032  38.8895 High income Not classified
#> 400   -77.032  38.8895 High income Not classified
#> 401   -77.032  38.8895 High income Not classified
#> 402   -77.032  38.8895 High income Not classified
#> 403   -77.032  38.8895 High income Not classified
#> 404   -77.032  38.8895 High income Not classified
#> 405   -77.032  38.8895 High income Not classified
#> 406   -77.032  38.8895 High income Not classified
#> 407   -77.032  38.8895 High income Not classified
#> 408   -77.032  38.8895 High income Not classified
#> 409   -77.032  38.8895 High income Not classified
#> 410   -77.032  38.8895 High income Not classified
#> 411   -77.032  38.8895 High income Not classified
#> 412   -77.032  38.8895 High income Not classified
#> 413   -77.032  38.8895 High income Not classified
#> 414   -77.032  38.8895 High income Not classified
#> 415   -77.032  38.8895 High income Not classified
#> 416   -77.032  38.8895 High income Not classified
#> 417   -77.032  38.8895 High income Not classified
#> 418   -77.032  38.8895 High income Not classified
#> 419   -77.032  38.8895 High income Not classified
#> 420   -77.032  38.8895 High income Not classified
#> 421   -77.032  38.8895 High income Not classified
#> 422   -77.032  38.8895 High income Not classified
#> 423   -77.032  38.8895 High income Not classified
#> 424   -77.032  38.8895 High income Not classified
#> 425   -77.032  38.8895 High income Not classified
#> 426   -77.032  38.8895 High income Not classified
#> 427   -77.032  38.8895 High income Not classified
#> 428   -77.032  38.8895 High income Not classified
#> 429   -77.032  38.8895 High income Not classified
#> 430   -77.032  38.8895 High income Not classified
#> 431   -77.032  38.8895 High income Not classified
#> 432   -77.032  38.8895 High income Not classified
#> 433   -77.032  38.8895 High income Not classified
#> 434   -77.032  38.8895 High income Not classified
```


## World Inequility Report - WIR2022

* World Inequality Report: https://wir2022.wid.world/
* Executive Summary: https://wir2022.wid.world/executive-summary/
* Methodology: https://wir2022.wid.world/methodology/
* URL of Executive Summary Data: https://wir2022.wid.world/www-site/uploads/2022/03/WIR2022TablesFigures-Summary.xlsx

Please add `mode="wb"` (web binary). This should work better. 


```r
url_summary <- "https://wir2022.wid.world/www-site/uploads/2022/03/WIR2022TablesFigures-Summary.xlsx"
download.file(url = url_summary, 
              destfile = "./data/WIR2022s.xlsx", 
              mode = "wb") 
```

If you get an error, download the file directory from the methodology site into your computer, then open it with Excel and save it in the data folder of your R Studio project. Then R studio can recognize it easily as an Excel data.

Generally, a text file such as a CSV file is easy to import, but a binary file is difficult to handle. It is because unless R can recognize its file type, for example, Excel or so, R cannot import the data.


```r
excel_sheets("./data/WIR2022s.xlsx")
#>  [1] "Index"     "F1"        "F2"        "F3"       
#>  [5] "F4"        "F5."       "F6"        "F7"       
#>  [9] "F8"        "F9"        "F10"       "F11"      
#> [13] "F12"       "F13"       "F14"       "F15"      
#> [17] "T1"        "data-F1"   "data-F2"   "data-F3"  
#> [21] "data-F4"   "data-F5"   "data-F6"   "data-F7"  
#> [25] "data-F8"   "data-F9"   "data-F10"  "data-F11" 
#> [29] "data-F12"  "data-F13." "data-F14." "data-F15"
```


# General Comments

## Create a PDF or Word file.

A Notebook file is created by pressing the Preview button, and the outputs appear as is. However, making a file with another format, R runs all code chunks from the top. So if the object is not defined above the code used, the knit program stops with an error message. I recommend the following steps.

0. Create a PDF right after you create a new (R Notebook) file (using Template). By this step, you can check your 'Knit to PDF' process by `tinytex` is working well. Please let me know if you fail to create a PDF and cannot solve the problem. I will look at the setting of your PC in class. 

1. Run all codes before you preview Notebook. You can use 'Run All', and 'Run All Code Chunks Below' under the 'Run' button if there is an incomplete code chunk. 

2. Before you create a PDF or word, you need to correct all errors. But if you could not, add `eval = FALSE` as an option. 



````markdown
```{r eval=FALSE}
# code chunk with errors
```
````


You can add a similar option from the gear mark at the top right in the code chunk. Select show nothing (don't run code); it adds `{r eval = FALSE, include = FALSE}`, and the code chunk itself is skipped.

3. Rerun all. If you can reach the end of the file without having an error, 'Knit to PDF' or 'Knit to Word'.

Creating a Word file is similar, and should be more accessible.

If you fail to create a PDF using `Knit to PDF` or `Knit to Word,` the alternative is to open the notebook wile with nb.html at the end in your web browser, such as Google Chrome, Edge, or Safari, and use the functionality of printing to PDF of your browser. 

### Other Code Chunk Options

Please review EDA5, and try options under the gear mark at the top right of each code chunk. I will add two useful options, I use often

1. `cash = TRUE` option. Downloading data and accessing to the internet takes time, and may cause trouble for the hosting site. With this option, you can avoid it, and shorten the compilation time to render. I always add this option to `WDI()`. As for `WDIsearch()`, if you use `cache = wdi_cache`, you do not need to add this option. It is another benefit to use `cache = wdi_cache`.

````markdown
```{r cash = TRUE}
# download from the internet
```
````



2. `echo = FALSE` option. When you create a PDF with a limit of pages, you do not want to include some code chunks. Then use this option. The output is included, but the code chunk is not. You can select this option by choosing 'Show output only` option.

### Reference

* https://yihui.org/knitr/options/
* Cheat Sheet. We distributed in class. You can download the same from Help: Cheatsheet at the top menu of R Stduio.

### Long Table

If you do not want to include a long table in your PDF or Word, use the following.


```r
wdi_cache$series %>% slice(1:10)
#>               indicator
#> 1    1.0.HCount.1.90usd
#> 2     1.0.HCount.2.5usd
#> 3  1.0.HCount.Mid10to50
#> 4       1.0.HCount.Ofcl
#> 5   1.0.HCount.Poor4uds
#> 6   1.0.HCount.Vul4to10
#> 7      1.0.PGap.1.90usd
#> 8       1.0.PGap.2.5usd
#> 9     1.0.PGap.Poor4uds
#> 10     1.0.PSev.1.90usd
#>                                       name
#> 1          Poverty Headcount ($1.90 a day)
#> 2          Poverty Headcount ($2.50 a day)
#> 3    Middle Class ($10-50 a day) Headcount
#> 4  Official Moderate Poverty Rate-National
#> 5             Poverty Headcount ($4 a day)
#> 6       Vulnerable ($4-10 a day) Headcount
#> 7                Poverty Gap ($1.90 a day)
#> 8                Poverty Gap ($2.50 a day)
#> 9                   Poverty Gap ($4 a day)
#> 10          Poverty Severity ($1.90 a day)
#>                                                                                                                                                                                                                                                                    description
#> 1                                                                                                                                     The poverty headcount index measures the proportion of the population with daily per capita income (in 2011 PPP) below the poverty line.
#> 2                                                                                                                                     The poverty headcount index measures the proportion of the population with daily per capita income (in 2005 PPP) below the poverty line.
#> 3                                                                                                                                     The poverty headcount index measures the proportion of the population with daily per capita income (in 2005 PPP) below the poverty line.
#> 4                                                                                                                The poverty headcount index measures the proportion of the population with daily per capita income below the official poverty line developed by each country.
#> 5                                                                                                                                     The poverty headcount index measures the proportion of the population with daily per capita income (in 2005 PPP) below the poverty line.
#> 6                                                                                                                                     The poverty headcount index measures the proportion of the population with daily per capita income (in 2005 PPP) below the poverty line.
#> 7  The poverty gap captures the mean aggregate income or consumption shortfall relative to the poverty line across the entire population. It measures the total resources needed to bring all the poor to the level of the poverty line (averaged over the total population). 
#> 8  The poverty gap captures the mean aggregate income or consumption shortfall relative to the poverty line across the entire population. It measures the total resources needed to bring all the poor to the level of the poverty line (averaged over the total population). 
#> 9  The poverty gap captures the mean aggregate income or consumption shortfall relative to the poverty line across the entire population. It measures the total resources needed to bring all the poor to the level of the poverty line (averaged over the total population). 
#> 10                                                                                                        The poverty severity index combines information on both poverty and inequality among the poor by averaging the squares of the poverty gaps relative the poverty line
#>    sourceDatabase
#> 1  LAC Equity Lab
#> 2  LAC Equity Lab
#> 3  LAC Equity Lab
#> 4  LAC Equity Lab
#> 5  LAC Equity Lab
#> 6  LAC Equity Lab
#> 7  LAC Equity Lab
#> 8  LAC Equity Lab
#> 9  LAC Equity Lab
#> 10 LAC Equity Lab
#>                                                       sourceOrganization
#> 1      LAC Equity Lab tabulations of SEDLAC (CEDLAS and the World Bank).
#> 2      LAC Equity Lab tabulations of SEDLAC (CEDLAS and the World Bank).
#> 3      LAC Equity Lab tabulations of SEDLAC (CEDLAS and the World Bank).
#> 4  LAC Equity Lab tabulations of data from National Statistical Offices.
#> 5      LAC Equity Lab tabulations of SEDLAC (CEDLAS and the World Bank).
#> 6      LAC Equity Lab tabulations of SEDLAC (CEDLAS and the World Bank).
#> 7      LAC Equity Lab tabulations of SEDLAC (CEDLAS and the World Bank).
#> 8      LAC Equity Lab tabulations of SEDLAC (CEDLAS and the World Bank).
#> 9      LAC Equity Lab tabulations of SEDLAC (CEDLAS and the World Bank).
#> 10     LAC Equity Lab tabulations of SEDLAC (CEDLAS and the World Bank).
```

This will print only the first ten rows. The following R Basic code does almost the same.


```r
head(wdi_cache$country, 10)
#>    iso3c iso2c                     country
#> 1    ABW    AW                       Aruba
#> 2    AFE    ZH Africa Eastern and Southern
#> 3    AFG    AF                 Afghanistan
#> 4    AFR    A9                      Africa
#> 5    AFW    ZI  Africa Western and Central
#> 6    AGO    AO                      Angola
#> 7    ALB    AL                     Albania
#> 8    AND    AD                     Andorra
#> 9    ARB    1A                  Arab World
#> 10   ARE    AE        United Arab Emirates
#>                        region          capital longitude
#> 1   Latin America & Caribbean       Oranjestad  -70.0167
#> 2                  Aggregates                           
#> 3                  South Asia            Kabul   69.1761
#> 4                  Aggregates                           
#> 5                  Aggregates                           
#> 6          Sub-Saharan Africa           Luanda    13.242
#> 7       Europe & Central Asia           Tirane   19.8172
#> 8       Europe & Central Asia Andorra la Vella    1.5218
#> 9                  Aggregates                           
#> 10 Middle East & North Africa        Abu Dhabi   54.3705
#>    latitude              income        lending
#> 1   12.5167         High income Not classified
#> 2                    Aggregates     Aggregates
#> 3   34.5228          Low income            IDA
#> 4                    Aggregates     Aggregates
#> 5                    Aggregates     Aggregates
#> 6  -8.81155 Lower middle income           IBRD
#> 7   41.3317 Upper middle income           IBRD
#> 8   42.5075         High income Not classified
#> 9                    Aggregates     Aggregates
#> 10  24.4764         High income Not classified
```

# Your Work

Here is a list of data your classmates used for Assignment Five.

## World Development Indicators - WDI

- SP.POP.TOTL: Population, total
- NY.GDP.MKTP.KD.ZG: GDP annual growth
- NY.GDP.MKTP.CD: GDP (current US$)
- NY.GDP.MKTP.KD.ZG: GDP growth (annual %)
- NY.GDP.PCAP.KD: GDP per capita (constant 2015 US$)
- NY.GNS.ICTR.ZS: Gross savings (% of GDP)
- BX.TRF.PWKR.CD.DT: Personal remittances, received (current US$)
- SI.POV.GINI: Gini index
- SL.TLF.TOTL.FE.ZS: Labor force, female (% of total labor force)
- SI.DST.10TH.10: Income share held by highest 10%
- SL.UEM.TOTL.ZS: Unemployment, total (% of total labor force) (modeled ILO estimate)
- BX.KLT.DINV.CD.WD: Foreign Direct Investment (FDI)
- AG.LND.FRST.K2: Forest area (sq. km) 
- EN.ATM.CO2E.KT: CO2 emissions (kt)
- EG.USE.ELEC.KH.PC:Electric power consumption (kWh per capita) 
- FB.ATM.TOTL.P5: Automated teller machines (ATMs) (per 100,000 adults)
- SM.POP.REFG.OR: Refugee population by country or territory of origin
- SG.GEN.PARL.ZS: Proportion of seats held by women in national parliaments (%)
- SE.XPD.TOTL.GD.ZS: Government expenditure on education, total (% of GDP)
- GB.XPD.RSDV.GD.ZS: Research and development expenditure (% of GDP)
- SE.SEC.ENRR: School enrollment rate, secondary (% gross) 
- IP.PAT.RESD: Patent applications, residents
- IP.IDS.RSCT: Industrial design applications, resident, by count
- IP.JRN.ARTC.SC: Scientific and technical journal articles
- BM.GSR.ROYL.CD: Intellectual Property Payments (BOP, Current US$)

## Worldbank

- Climate Change Knowledge Portal: https://climateknowledgeportal.worldbank.org
  + country summary

## OECD Data

* Public spending on education: https://data.oecd.org/eduresource/public-spending-on-education.htm
* Private spending on education: https://data.oecd.org/eduresource/private-spending-on-education.htm

## WIR DAta

* Executive Summary: https://wir2022.wid.world/executive-summary/

## Toy Data

* `datasets::mtcars`: Motor Trend Car Road Tests

# Responses to Questions

## Q. How can we include values in the graphs

A. Use `geom_text()`. Sometimes `geom_label()` works better.

The first example is for `geom_column()`.


* `geom_text` or `geom_label`
  * `aes(country, gini, label = gini)`: x and y value together with the data you want to include. In this case, I chose the same x and y value as `geom_com()`, and `gini` for the value.
  * `vjust`: Change the value to find the best location. You can also use `hjust`, for the horizontal justification.


```r
df1 %>% filter(country %in% g7, year == 2013) %>%
  ggplot(aes(country, gini, fill = country)) + geom_col() +
  geom_text(aes(label = gini), vjust = -0.1) + labs(x = "") +
  theme(legend.position = "none")
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-14-1.png)<!-- -->

I do not think the following charts are fancy, but you can apply these techniques when appropriate.

* Scatter plot, and line plot
  - `aes(label = paste("(",gini,",", round(parl,1),")")`: x and y value together with the data you want to include. In this case, I included the `gini` value and the `parl` value, surrounded by parentheses. However, the `parl` value has long decimal places; I used `round` to cut it shorter to keep only one decimal place.
  - `vjust = -0.1`: Just above the point.
  - `size = 3`: Used a bit smaller font size.
  - `geom_line(arrow = arrow(length = unit(0.03, "npc"), ends="last", type = "closed"))`: Added line segments with arrow heads.
  - `ylim(5,40), xlim(29,42)`: The range of the coordinate plane to include labels.
* Compare with `geom_label`.
  - I also changed the shape of the arrow heads.


```r
df1 %>% filter(country %in% g7, year %in% c(2010, 2013)) %>%
  ggplot(aes(gini, parl, color = country)) + geom_point(aes(shape = factor(year))) +
  geom_text(aes(label = paste("(",gini,",", round(parl,1),")")), vjust = -0.1, size = 3) + 
  geom_line(arrow = arrow(length = unit(0.03, "npc"), ends="last", type = "closed")) + ylim(5,40) + xlim(29,42)
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-15-1.png)<!-- -->


```r
df1 %>% filter(country %in% g7, year %in% c(2010, 2013)) %>%
  ggplot(aes(gini, parl, color = country)) + geom_point(aes(shape = factor(year))) +
  geom_label(aes(gini, parl, label = iso2c), vjust = -0.1, size = 3) + 
  geom_line(aes(gini, parl, color = country), arrow = arrow(length = unit(0.03, "npc"), ends="last", type = "open"))
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-16-1.png)<!-- -->


See Responses to Assignment Four, See 4.4. 

https://icu-hsuzuki.github.io/da4r2022_note/a4_resp.nb.html

See also:

https://ds-sl.github.io/data-analysis/wir2022.nb.html

Explanation of F1.

# My Comments after Review

## NA values

It is challenging to handle NA values properly. Let me introduce basics. In the following we use two data sets. One is `df1` used above, containing the indicator related the women's sheet in parliament and the GINI index, and the following data on the forest area.


```r
wdi_cache$series %>% filter(indicator == "AG.LND.FRST.K2")
#>        indicator                 name
#> 1 AG.LND.FRST.K2 Forest area (sq. km)
#>                                                                                                                                                                                                                                                                            description
#> 1 Forest area is land under natural or planted stands of trees of at least 5 meters in situ, whether productive or not, and excludes tree stands in agricultural production systems (for example, in fruit plantations and agroforestry systems) and trees in urban parks and gardens.
#>                 sourceDatabase
#> 1 World Development Indicators
#>                                                  sourceOrganization
#> 1 Food and Agriculture Organization, electronic files and web site.
```


```r
df2 <- WDI(country = "all", indicator = c(area = "AG.LND.FRST.K2"), extra = TRUE, cache = wdi_cache) 
df2 %>% slice(1:10)
#>        country iso2c iso3c year    area status lastupdated
#> 1  Afghanistan    AF   AFG 2021      NA         2022-12-22
#> 2  Afghanistan    AF   AFG 2020 12084.4         2022-12-22
#> 3  Afghanistan    AF   AFG 2019 12084.4         2022-12-22
#> 4  Afghanistan    AF   AFG 2018 12084.4         2022-12-22
#> 5  Afghanistan    AF   AFG 2017 12084.4         2022-12-22
#> 6  Afghanistan    AF   AFG 2016 12084.4         2022-12-22
#> 7  Afghanistan    AF   AFG 2015 12084.4         2022-12-22
#> 8  Afghanistan    AF   AFG 2014 12084.4         2022-12-22
#> 9  Afghanistan    AF   AFG 2013 12084.4         2022-12-22
#> 10 Afghanistan    AF   AFG 2012 12084.4         2022-12-22
#>        region capital longitude latitude     income lending
#> 1  South Asia   Kabul   69.1761  34.5228 Low income     IDA
#> 2  South Asia   Kabul   69.1761  34.5228 Low income     IDA
#> 3  South Asia   Kabul   69.1761  34.5228 Low income     IDA
#> 4  South Asia   Kabul   69.1761  34.5228 Low income     IDA
#> 5  South Asia   Kabul   69.1761  34.5228 Low income     IDA
#> 6  South Asia   Kabul   69.1761  34.5228 Low income     IDA
#> 7  South Asia   Kabul   69.1761  34.5228 Low income     IDA
#> 8  South Asia   Kabul   69.1761  34.5228 Low income     IDA
#> 9  South Asia   Kabul   69.1761  34.5228 Low income     IDA
#> 10 South Asia   Kabul   69.1761  34.5228 Low income     IDA
```

1. Get rid of all NA values. nrow(data) gives the number of rows, the length of data.


```r
df1_wona <- df1 %>% drop_na(); nrow(df1_wona)
#> [1] 114
```


```r
df2_wona <- df2 %>% drop_na(); nrow(df2_wona)
#> [1] 7723
```

2. Drop data only a specified indicator.


```r
df1_wona_parl <- df1 %>% drop_na(parl); nrow(df1_wona_parl)
#> [1] 173
```


```r
df1_wona_gini <- df1 %>% drop_na(gini); nrow(df1_wona_gini)
#> [1] 155
```


```r
df1_wona_parl_gini <- df1 %>% drop_na(parl, gini); nrow(df1_wona_parl_gini)
#> [1] 114
```


```r
df2_wona_area <- df2 %>% drop_na(area); nrow(df2_wona_area)
#> [1] 7909
```

Can you see why the number above is larger than the row number of `df2_wona`? Since there are many column imported using `extra=TRUE`, there may be NA values in the othre columns. 

So, generally speaking, it is better to drop NA only for the indicators.

3. Selecting year or other conditions with many data.

I chose the year 2013 and 2010, because those are the only years we had data for all G7 countries with two indicator values, i.e., `parl` and `gini`.


```r
df1 %>% drop_na(parl, gini) %>% 
  group_by(year) %>% summarize(n = n()) %>% 
  arrange(desc(n), desc(year)) %>% top_n(1)
#> Selecting by n
#> # A tibble: 2 × 2
#>    year     n
#>   <int> <int>
#> 1  2013     7
#> 2  2010     7
```

4. Compare the following charts. In the second chart, we can get rid of the warnings. You can simply remove the warning by adding a chunk optiion `warning=FALSE` by {r warning=FALSE} or choosing an option using the gear mark.


```r
df1 %>% ggplot(aes(x = year, y = gini, col = country)) + geom_line()
#> Warning: Removed 184 rows containing missing values
#> (`geom_line()`).
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-26-1.png)<!-- -->


```r
df1 %>% drop_na(gini) %>% 
  ggplot(aes(x = year, y = gini, col = country)) + geom_line()
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-27-1.png)<!-- -->

### Reference

* Posit Primers: [Tidy Your Data](https://posit.cloud/learn/primers/4)


## Comparing Two

There are several ways if you want to compare two charts. You can incllude them in one chart, as I gave examples above using `geom_line` with arrows. 

We want to combine the following charts into one. A few comments on the charts.

* Since `year` is fixed, we set `x = ""` in `labs` and add a title specifying the year. 
* `scale_x_discrete(labels = function(x) stringr::str_wrap(x, width = 7))`: Since the labels of the x-axis are long, we used a unique technique to wrap the tags to fit into a fixed length.
* `theme(legend. position = "none")`: Since the legend is the same as the labels, we removed it.



```r
df2_wona_area %>% filter(region %in% c("East Asia & Pacific", "Europe & Central Asia", "Latin America & Caribbean", "Middle East & North Africa", "North America", "South Asia", "Sub-Saharan Africa"), year == 1990) %>%
  ggplot(aes(x = region, y = area, fill = region)) + 
  geom_col() + labs(title = "Graph 1. Forest areas in 1990", x = "", y = "Forest area (sq. km)") + scale_x_discrete(labels = function(x) stringr::str_wrap(x, width = 7)) + 
  theme(legend.position = "none")
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-28-1.png)<!-- -->


```r
df2_wona_area %>% filter(region %in% c("East Asia & Pacific", "Europe & Central Asia", "Latin America & Caribbean", "Middle East & North Africa", "North America", "South Asia", "Sub-Saharan Africa"), year == 2020) %>%
  ggplot(aes(x = region, y = area, fill = region)) + 
  geom_col() + labs(title = "Graph 2. Forest areas in 2020", subtitle = "Regions of the world", x = "", y = "Forest area (sq. km)") + scale_x_discrete(labels = function(x) stringr::str_wrap(x, width = 7)) + 
  theme(legend.position = "none")
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-29-1.png)<!-- -->

1. position("dodge")


```r
df2_wona_area %>% filter(region %in% c("East Asia & Pacific", "Europe & Central Asia", "Latin America & Caribbean", "Middle East & North Africa", "North America", "South Asia", "Sub-Saharan Africa"), year %in% c(1990,2020)) %>%
  ggplot(aes(x = region, y = area, fill = factor(year))) + 
  geom_col(position="dodge", width = 0.8) + labs(title = "Forest areas", x = "", y = "Forest area (sq. km)", fill = "year: ") + scale_x_discrete(labels = function(x) stringr::str_wrap(x, width = 12)) + 
  theme(legend.position = "top")
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-30-1.png)<!-- -->

2. `facet_wrap`


```r
df2_wona_area %>% filter(region %in% c("East Asia & Pacific", "Europe & Central Asia", "Latin America & Caribbean", "Middle East & North Africa", "North America", "South Asia", "Sub-Saharan Africa"), year %in% c(1990,2020)) %>%
  ggplot(aes(x = region, y = area, fill = region)) + 
  geom_col(position="dodge", width = 0.8) + labs(title = "Forest areas", x = "", y = "Forest area (sq. km)") + scale_x_discrete(labels = function(x) stringr::str_wrap(x, width = 3)) + 
  theme(legend.position = "none") + facet_wrap(.~year)
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-31-1.png)<!-- -->


```r
df2_wona_area %>% filter(region %in% c("East Asia & Pacific", "Europe & Central Asia", "Latin America & Caribbean", "Middle East & North Africa", "North America", "South Asia", "Sub-Saharan Africa"), year %in% c(1990,2020)) %>%
  ggplot(aes(x = region, y = area, fill = region)) + 
  geom_col(position="dodge", width = 0.8) + labs(title = "Forest areas", x = "", y = "Forest area (sq. km)") + scale_x_discrete(labels = function(x) stringr::str_wrap(x, width = 3)) + 
  theme(legend.position = "none") + facet_wrap(.~year, nrow = 2)
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-32-1.png)<!-- -->

See Posit Primers: [Visualize Data](https://posit.cloud/learn/primers/3)

## The package `broom` for models

There is another package for models in `tidyverse`, i.e., `modelr`. Some of the functions of `modelr` can be useful, but it is mainly for sampling and machine learning. So we explain the package `broom` only.

Although, `broom` is installed when you install `tidyverse`, it is not loaded. So you need to add `library(broom)`.


```r
library(broom)
```



```r
df3 <- datasets::iris
```

There are two ways to set a model.

* `lm(y~x, data)` and `data %>% lm(y~x, .)`.

You can see model summary by the following.

* `summary(lm(y~x, data))` and `data %>% lm(y~x, .) %>% summary()`.


```r
mod <- df3 %>% lm(Sepal.Length ~ Sepal.Width, .)
mod
#> 
#> Call:
#> lm(formula = Sepal.Length ~ Sepal.Width, data = .)
#> 
#> Coefficients:
#> (Intercept)  Sepal.Width  
#>      6.5262      -0.2234
```

```r
df3 %>% lm(Sepal.Length ~ Sepal.Width, .) %>% summary()
#> 
#> Call:
#> lm(formula = Sepal.Length ~ Sepal.Width, data = .)
#> 
#> Residuals:
#>     Min      1Q  Median      3Q     Max 
#> -1.5561 -0.6333 -0.1120  0.5579  2.2226 
#> 
#> Coefficients:
#>             Estimate Std. Error t value Pr(>|t|)    
#> (Intercept)   6.5262     0.4789   13.63   <2e-16 ***
#> Sepal.Width  -0.2234     0.1551   -1.44    0.152    
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> Residual standard error: 0.8251 on 148 degrees of freedom
#> Multiple R-squared:  0.01382,	Adjusted R-squared:  0.007159 
#> F-statistic: 2.074 on 1 and 148 DF,  p-value: 0.1519
```

Since the coefficients are under Estimate of the summary, you think you do not need `mod = lm(y~x, data)`. But it is not the case.

`mod <- lm(y~x, data)` defines a model. 

We intoroduce only `tidy()`, `glance()` and `augment()`

### `tidy`

It produces the coefficients part of the model summary. So, the two values under estimate is the y-intercept and the slope.


```r
mod %>% tidy()
#> # A tibble: 2 × 5
#>   term        estimate std.error statistic  p.value
#>   <chr>          <dbl>     <dbl>     <dbl>    <dbl>
#> 1 (Intercept)    6.53      0.479     13.6  6.47e-28
#> 2 Sepal.Width   -0.223     0.155     -1.44 1.52e- 1
```


### `glance`

It produces the latter half of the model summary. The fist is the R squared followed by adjusted R squared, and other values.


```r
mod %>% glance()
#> # A tibble: 1 × 12
#>   r.squared adj.r…¹ sigma stati…² p.value    df logLik   AIC
#>       <dbl>   <dbl> <dbl>   <dbl>   <dbl> <dbl>  <dbl> <dbl>
#> 1    0.0138 0.00716 0.825    2.07   0.152     1  -183.  372.
#> # … with 4 more variables: BIC <dbl>, deviance <dbl>,
#> #   df.residual <int>, nobs <int>, and abbreviated variable
#> #   names ¹​adj.r.squared, ²​statistic
```
### `augment`

* The first column is the vector corresponding to y.
* The second column is the vector corresponding to x.
* `.fitted` is the y value of the fitted line. So
$$.fitted = y-intercept + slope \cdot x.$$
* `.resid` is the residue, i.e., y-value minus .fitted (or predicted) value.


```r
mod %>% augment() %>% arrange(Sepal.Width)
#> # A tibble: 150 × 8
#>    Sepal.Len…¹ Sepal…² .fitted  .resid   .hat .sigma .cooksd
#>          <dbl>   <dbl>   <dbl>   <dbl>  <dbl>  <dbl>   <dbl>
#>  1         5       2      6.08 -1.08   0.0462  0.823 4.34e-2
#>  2         6       2.2    6.03 -0.0348 0.0326  0.828 3.11e-5
#>  3         6.2     2.2    6.03  0.165  0.0326  0.828 6.99e-4
#>  4         6       2.2    6.03 -0.0348 0.0326  0.828 3.11e-5
#>  5         4.5     2.3    6.01 -1.51   0.0269  0.818 4.78e-2
#>  6         5.5     2.3    6.01 -0.512  0.0269  0.827 5.49e-3
#>  7         6.3     2.3    6.01  0.288  0.0269  0.828 1.73e-3
#>  8         5       2.3    6.01 -1.01   0.0269  0.824 2.14e-2
#>  9         4.9     2.4    5.99 -1.09   0.0219  0.823 2.00e-2
#> 10         5.5     2.4    5.99 -0.490  0.0219  0.827 4.05e-3
#> # … with 140 more rows, 1 more variable: .std.resid <dbl>,
#> #   and abbreviated variable names ¹​Sepal.Length,
#> #   ²​Sepal.Width
```

Hence, the following two charts are the same.


```r
mod %>% augment() %>% ggplot(aes(x = Sepal.Width)) + geom_point(aes(y = Sepal.Length)) + geom_line(aes(y = .fitted), col = "blue")
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-40-1.png)<!-- -->



```r
df3 %>% ggplot(aes(x = Sepal.Width, y = Sepal.Length)) + geom_point() + geom_smooth(formula = y~x, method = "lm", se = FALSE)
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-41-1.png)<!-- -->


```r
df3 %>% ggplot(aes(x = Sepal.Width, y = Sepal.Length)) + geom_point() + geom_smooth(formula = y~x, method = "lm")
```

![](15-a5resp_files/figure-epub3/unnamed-chunk-42-1.png)<!-- -->

The shaded band is supposed to tell you the range of the prediction line should be under some assumption. I did not explain it because you need to be careful when interpreting its meaning.


### References of `broom`

* Official site of R project:  https://CRAN.R-project.org/package=broom
* Manual: https://cran.r-project.org/web/packages/broom/broom.pdf
* vignette: [Introduction to broom](https://cran.r-project.org/web/packages/broom/vignettes/broom.html)
* vignette: [broom and dplyr](https://cran.r-project.org/web/packages/broom/vignettes/broom_and_dplyr.html)
* [Augment data with information from a(n) lm object](https://broom.tidymodels.org/reference/augment.lm.html)
