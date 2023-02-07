# Data Transforamtion with `dplyr` {#dplyr}


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
```


## `dplyr` [Overview](https://dplyr.tidyverse.org)

dplyr is a grammar of data manipulation, providing a consistent set of verbs that help you solve the most common data manipulation challenges:

* `select()` picks variables based on their names.
* `filter()` picks cases based on their values.
* `mutate()` adds new variables that are functions of existing variables
* `summarise()` reduces multiple values down to a single summary.
* `arrange()` changes the ordering of the rows.
* `group_by()` takes an existing tbl and converts it into a grouped tbl.

You can learn more about them in vignette("dplyr"). As well as these single-table verbs, dplyr also provides a variety of two-table verbs, which you can learn about in vignette("two-table").

If you are new to dplyr, the best place to start is [the data transformation chapter in R for data science](http://r4ds.had.co.nz/transform.html).

---

## [`select`](https://dplyr.tidyverse.org/reference/select.html): Subset columns using their names and types

Helper Function	| Use	| Example
---|-------|--------
-	| Columns except	| select(babynames, -prop)
:	| Columns between (inclusive)	| select(babynames, year:n)
contains() |	Columns that contains a string |	select(babynames, contains("n"))
ends_with()	| Columns that ends with a string	| select(babynames, ends_with("n"))
matches()	| Columns that matches a regex |	select(babynames, matches("n"))
num_range()	| Columns with a numerical suffix in the range | Not applicable with babynames
one_of() |	Columns whose name appear in the given set |	select(babynames, one_of(c("sex", "gender")))
starts_with()	| Columns that starts with a string	| select(babynames, starts_with("n"))

---

## [`filter`](https://dplyr.tidyverse.org/reference/filter.html): Subset rows using column values

Logical operator	| tests	| Example
--|-----|---
>	| Is x greater than y? |	x > y
>=	| Is x greater than or equal to y? |	x >= y
<	| Is x less than y?	| x < y
<=	| Is x less than or equal to y? | 	x <= y
==	| Is x equal to y? |	x == y
!=	| Is x not equal to y? |	x != y
is.na()	| Is x an NA?	| is.na(x)
!is.na() |	Is x not an NA? |	!is.na(x)

---

## [`arrange`](https://dplyr.tidyverse.org/reference/arrange.html) and `Pipe %>%`

* `arrange()` orders the rows of a data frame by the values of selected columns.

Unlike other `dplyr` verbs, `arrange()` largely ignores grouping; you need to explicitly mention grouping variables (`or use .by_group = TRUE) in order to group by them, and functions of variables are evaluated once per data frame, not once per group.

* [`pipes`](https://r4ds.had.co.nz/pipes.html) in R for Data Science.

---

## [`mutate`](https://dplyr.tidyverse.org/reference/mutate.html) 

* Create, modify, and delete columns

* Useful mutate functions

  - +, -, log(), etc., for their usual mathematical meanings

  - lead(), lag()

  - dense_rank(), min_rank(), percent_rank(), row_number(), cume_dist(), ntile()

  - cumsum(), cummean(), cummin(), cummax(), cumany(), cumall()

  - na_if(), coalesce()### `group_by()` and `summarise()`

---

## [`group_by`](https://dplyr.tidyverse.org/reference/group_by.html)

---

## [`summarise` or `summarize`](https://dplyr.tidyverse.org/reference/summarise.html)

#### Summary functions

So far our summarise() examples have relied on sum(), max(), and mean(). But you can use any function in summarise() so long as it meets one criteria: the function must take a vector of values as input and return a single value as output. Functions that do this are known as summary functions and they are common in the field of descriptive statistics. Some of the most useful summary functions include:

1. Measures of location - mean(x), median(x), quantile(x, 0.25), min(x), and max(x)
2. Measures of spread - sd(x), var(x), IQR(x), and mad(x)
3. Measures of position - first(x), nth(x, 2), and last(x)
4. Counts - n_distinct(x) and n(), which takes no arguments, and returns the size of the current group or data frame.
5. Counts and proportions of logical values - sum(!is.na(x)), which counts the number of TRUEs returned by a logical test; mean(y == 0), which returns the proportion of TRUEs returned by a logical test.


  - if_else(), recode(), case_when()

---

## Learn `dplyr` by Examples

### Data `iris`


```r
iris
#>     Sepal.Length Sepal.Width Petal.Length Petal.Width
#> 1            5.1         3.5          1.4         0.2
#> 2            4.9         3.0          1.4         0.2
#> 3            4.7         3.2          1.3         0.2
#> 4            4.6         3.1          1.5         0.2
#> 5            5.0         3.6          1.4         0.2
#> 6            5.4         3.9          1.7         0.4
#> 7            4.6         3.4          1.4         0.3
#> 8            5.0         3.4          1.5         0.2
#> 9            4.4         2.9          1.4         0.2
#> 10           4.9         3.1          1.5         0.1
#> 11           5.4         3.7          1.5         0.2
#> 12           4.8         3.4          1.6         0.2
#> 13           4.8         3.0          1.4         0.1
#> 14           4.3         3.0          1.1         0.1
#> 15           5.8         4.0          1.2         0.2
#> 16           5.7         4.4          1.5         0.4
#> 17           5.4         3.9          1.3         0.4
#> 18           5.1         3.5          1.4         0.3
#> 19           5.7         3.8          1.7         0.3
#> 20           5.1         3.8          1.5         0.3
#> 21           5.4         3.4          1.7         0.2
#> 22           5.1         3.7          1.5         0.4
#> 23           4.6         3.6          1.0         0.2
#> 24           5.1         3.3          1.7         0.5
#> 25           4.8         3.4          1.9         0.2
#> 26           5.0         3.0          1.6         0.2
#> 27           5.0         3.4          1.6         0.4
#> 28           5.2         3.5          1.5         0.2
#> 29           5.2         3.4          1.4         0.2
#> 30           4.7         3.2          1.6         0.2
#> 31           4.8         3.1          1.6         0.2
#> 32           5.4         3.4          1.5         0.4
#> 33           5.2         4.1          1.5         0.1
#> 34           5.5         4.2          1.4         0.2
#> 35           4.9         3.1          1.5         0.2
#> 36           5.0         3.2          1.2         0.2
#> 37           5.5         3.5          1.3         0.2
#> 38           4.9         3.6          1.4         0.1
#> 39           4.4         3.0          1.3         0.2
#> 40           5.1         3.4          1.5         0.2
#> 41           5.0         3.5          1.3         0.3
#> 42           4.5         2.3          1.3         0.3
#> 43           4.4         3.2          1.3         0.2
#> 44           5.0         3.5          1.6         0.6
#> 45           5.1         3.8          1.9         0.4
#> 46           4.8         3.0          1.4         0.3
#> 47           5.1         3.8          1.6         0.2
#> 48           4.6         3.2          1.4         0.2
#> 49           5.3         3.7          1.5         0.2
#> 50           5.0         3.3          1.4         0.2
#> 51           7.0         3.2          4.7         1.4
#> 52           6.4         3.2          4.5         1.5
#> 53           6.9         3.1          4.9         1.5
#> 54           5.5         2.3          4.0         1.3
#> 55           6.5         2.8          4.6         1.5
#> 56           5.7         2.8          4.5         1.3
#> 57           6.3         3.3          4.7         1.6
#> 58           4.9         2.4          3.3         1.0
#> 59           6.6         2.9          4.6         1.3
#> 60           5.2         2.7          3.9         1.4
#> 61           5.0         2.0          3.5         1.0
#> 62           5.9         3.0          4.2         1.5
#> 63           6.0         2.2          4.0         1.0
#> 64           6.1         2.9          4.7         1.4
#> 65           5.6         2.9          3.6         1.3
#> 66           6.7         3.1          4.4         1.4
#> 67           5.6         3.0          4.5         1.5
#> 68           5.8         2.7          4.1         1.0
#> 69           6.2         2.2          4.5         1.5
#> 70           5.6         2.5          3.9         1.1
#> 71           5.9         3.2          4.8         1.8
#> 72           6.1         2.8          4.0         1.3
#> 73           6.3         2.5          4.9         1.5
#> 74           6.1         2.8          4.7         1.2
#> 75           6.4         2.9          4.3         1.3
#> 76           6.6         3.0          4.4         1.4
#> 77           6.8         2.8          4.8         1.4
#> 78           6.7         3.0          5.0         1.7
#> 79           6.0         2.9          4.5         1.5
#> 80           5.7         2.6          3.5         1.0
#> 81           5.5         2.4          3.8         1.1
#> 82           5.5         2.4          3.7         1.0
#> 83           5.8         2.7          3.9         1.2
#> 84           6.0         2.7          5.1         1.6
#> 85           5.4         3.0          4.5         1.5
#> 86           6.0         3.4          4.5         1.6
#> 87           6.7         3.1          4.7         1.5
#> 88           6.3         2.3          4.4         1.3
#> 89           5.6         3.0          4.1         1.3
#> 90           5.5         2.5          4.0         1.3
#> 91           5.5         2.6          4.4         1.2
#> 92           6.1         3.0          4.6         1.4
#> 93           5.8         2.6          4.0         1.2
#> 94           5.0         2.3          3.3         1.0
#> 95           5.6         2.7          4.2         1.3
#> 96           5.7         3.0          4.2         1.2
#> 97           5.7         2.9          4.2         1.3
#> 98           6.2         2.9          4.3         1.3
#> 99           5.1         2.5          3.0         1.1
#> 100          5.7         2.8          4.1         1.3
#> 101          6.3         3.3          6.0         2.5
#> 102          5.8         2.7          5.1         1.9
#> 103          7.1         3.0          5.9         2.1
#> 104          6.3         2.9          5.6         1.8
#> 105          6.5         3.0          5.8         2.2
#> 106          7.6         3.0          6.6         2.1
#> 107          4.9         2.5          4.5         1.7
#> 108          7.3         2.9          6.3         1.8
#> 109          6.7         2.5          5.8         1.8
#> 110          7.2         3.6          6.1         2.5
#> 111          6.5         3.2          5.1         2.0
#> 112          6.4         2.7          5.3         1.9
#> 113          6.8         3.0          5.5         2.1
#> 114          5.7         2.5          5.0         2.0
#> 115          5.8         2.8          5.1         2.4
#> 116          6.4         3.2          5.3         2.3
#> 117          6.5         3.0          5.5         1.8
#> 118          7.7         3.8          6.7         2.2
#> 119          7.7         2.6          6.9         2.3
#> 120          6.0         2.2          5.0         1.5
#> 121          6.9         3.2          5.7         2.3
#> 122          5.6         2.8          4.9         2.0
#> 123          7.7         2.8          6.7         2.0
#> 124          6.3         2.7          4.9         1.8
#> 125          6.7         3.3          5.7         2.1
#> 126          7.2         3.2          6.0         1.8
#> 127          6.2         2.8          4.8         1.8
#> 128          6.1         3.0          4.9         1.8
#> 129          6.4         2.8          5.6         2.1
#> 130          7.2         3.0          5.8         1.6
#> 131          7.4         2.8          6.1         1.9
#> 132          7.9         3.8          6.4         2.0
#> 133          6.4         2.8          5.6         2.2
#> 134          6.3         2.8          5.1         1.5
#> 135          6.1         2.6          5.6         1.4
#> 136          7.7         3.0          6.1         2.3
#> 137          6.3         3.4          5.6         2.4
#> 138          6.4         3.1          5.5         1.8
#> 139          6.0         3.0          4.8         1.8
#> 140          6.9         3.1          5.4         2.1
#> 141          6.7         3.1          5.6         2.4
#> 142          6.9         3.1          5.1         2.3
#> 143          5.8         2.7          5.1         1.9
#> 144          6.8         3.2          5.9         2.3
#> 145          6.7         3.3          5.7         2.5
#> 146          6.7         3.0          5.2         2.3
#> 147          6.3         2.5          5.0         1.9
#> 148          6.5         3.0          5.2         2.0
#> 149          6.2         3.4          5.4         2.3
#> 150          5.9         3.0          5.1         1.8
#>        Species
#> 1       setosa
#> 2       setosa
#> 3       setosa
#> 4       setosa
#> 5       setosa
#> 6       setosa
#> 7       setosa
#> 8       setosa
#> 9       setosa
#> 10      setosa
#> 11      setosa
#> 12      setosa
#> 13      setosa
#> 14      setosa
#> 15      setosa
#> 16      setosa
#> 17      setosa
#> 18      setosa
#> 19      setosa
#> 20      setosa
#> 21      setosa
#> 22      setosa
#> 23      setosa
#> 24      setosa
#> 25      setosa
#> 26      setosa
#> 27      setosa
#> 28      setosa
#> 29      setosa
#> 30      setosa
#> 31      setosa
#> 32      setosa
#> 33      setosa
#> 34      setosa
#> 35      setosa
#> 36      setosa
#> 37      setosa
#> 38      setosa
#> 39      setosa
#> 40      setosa
#> 41      setosa
#> 42      setosa
#> 43      setosa
#> 44      setosa
#> 45      setosa
#> 46      setosa
#> 47      setosa
#> 48      setosa
#> 49      setosa
#> 50      setosa
#> 51  versicolor
#> 52  versicolor
#> 53  versicolor
#> 54  versicolor
#> 55  versicolor
#> 56  versicolor
#> 57  versicolor
#> 58  versicolor
#> 59  versicolor
#> 60  versicolor
#> 61  versicolor
#> 62  versicolor
#> 63  versicolor
#> 64  versicolor
#> 65  versicolor
#> 66  versicolor
#> 67  versicolor
#> 68  versicolor
#> 69  versicolor
#> 70  versicolor
#> 71  versicolor
#> 72  versicolor
#> 73  versicolor
#> 74  versicolor
#> 75  versicolor
#> 76  versicolor
#> 77  versicolor
#> 78  versicolor
#> 79  versicolor
#> 80  versicolor
#> 81  versicolor
#> 82  versicolor
#> 83  versicolor
#> 84  versicolor
#> 85  versicolor
#> 86  versicolor
#> 87  versicolor
#> 88  versicolor
#> 89  versicolor
#> 90  versicolor
#> 91  versicolor
#> 92  versicolor
#> 93  versicolor
#> 94  versicolor
#> 95  versicolor
#> 96  versicolor
#> 97  versicolor
#> 98  versicolor
#> 99  versicolor
#> 100 versicolor
#> 101  virginica
#> 102  virginica
#> 103  virginica
#> 104  virginica
#> 105  virginica
#> 106  virginica
#> 107  virginica
#> 108  virginica
#> 109  virginica
#> 110  virginica
#> 111  virginica
#> 112  virginica
#> 113  virginica
#> 114  virginica
#> 115  virginica
#> 116  virginica
#> 117  virginica
#> 118  virginica
#> 119  virginica
#> 120  virginica
#> 121  virginica
#> 122  virginica
#> 123  virginica
#> 124  virginica
#> 125  virginica
#> 126  virginica
#> 127  virginica
#> 128  virginica
#> 129  virginica
#> 130  virginica
#> 131  virginica
#> 132  virginica
#> 133  virginica
#> 134  virginica
#> 135  virginica
#> 136  virginica
#> 137  virginica
#> 138  virginica
#> 139  virginica
#> 140  virginica
#> 141  virginica
#> 142  virginica
#> 143  virginica
#> 144  virginica
#> 145  virginica
#> 146  virginica
#> 147  virginica
#> 148  virginica
#> 149  virginica
#> 150  virginica
```


---


```r
summary(iris)
#>   Sepal.Length    Sepal.Width     Petal.Length  
#>  Min.   :4.300   Min.   :2.000   Min.   :1.000  
#>  1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600  
#>  Median :5.800   Median :3.000   Median :4.350  
#>  Mean   :5.843   Mean   :3.057   Mean   :3.758  
#>  3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100  
#>  Max.   :7.900   Max.   :4.400   Max.   :6.900  
#>   Petal.Width          Species  
#>  Min.   :0.100   setosa    :50  
#>  1st Qu.:0.300   versicolor:50  
#>  Median :1.300   virginica :50  
#>  Mean   :1.199                  
#>  3rd Qu.:1.800                  
#>  Max.   :2.500
```

---

### `select` 1 - columns 1, 2, 5


```r
select(iris, c(1,2,5))
#>     Sepal.Length Sepal.Width    Species
#> 1            5.1         3.5     setosa
#> 2            4.9         3.0     setosa
#> 3            4.7         3.2     setosa
#> 4            4.6         3.1     setosa
#> 5            5.0         3.6     setosa
#> 6            5.4         3.9     setosa
#> 7            4.6         3.4     setosa
#> 8            5.0         3.4     setosa
#> 9            4.4         2.9     setosa
#> 10           4.9         3.1     setosa
#> 11           5.4         3.7     setosa
#> 12           4.8         3.4     setosa
#> 13           4.8         3.0     setosa
#> 14           4.3         3.0     setosa
#> 15           5.8         4.0     setosa
#> 16           5.7         4.4     setosa
#> 17           5.4         3.9     setosa
#> 18           5.1         3.5     setosa
#> 19           5.7         3.8     setosa
#> 20           5.1         3.8     setosa
#> 21           5.4         3.4     setosa
#> 22           5.1         3.7     setosa
#> 23           4.6         3.6     setosa
#> 24           5.1         3.3     setosa
#> 25           4.8         3.4     setosa
#> 26           5.0         3.0     setosa
#> 27           5.0         3.4     setosa
#> 28           5.2         3.5     setosa
#> 29           5.2         3.4     setosa
#> 30           4.7         3.2     setosa
#> 31           4.8         3.1     setosa
#> 32           5.4         3.4     setosa
#> 33           5.2         4.1     setosa
#> 34           5.5         4.2     setosa
#> 35           4.9         3.1     setosa
#> 36           5.0         3.2     setosa
#> 37           5.5         3.5     setosa
#> 38           4.9         3.6     setosa
#> 39           4.4         3.0     setosa
#> 40           5.1         3.4     setosa
#> 41           5.0         3.5     setosa
#> 42           4.5         2.3     setosa
#> 43           4.4         3.2     setosa
#> 44           5.0         3.5     setosa
#> 45           5.1         3.8     setosa
#> 46           4.8         3.0     setosa
#> 47           5.1         3.8     setosa
#> 48           4.6         3.2     setosa
#> 49           5.3         3.7     setosa
#> 50           5.0         3.3     setosa
#> 51           7.0         3.2 versicolor
#> 52           6.4         3.2 versicolor
#> 53           6.9         3.1 versicolor
#> 54           5.5         2.3 versicolor
#> 55           6.5         2.8 versicolor
#> 56           5.7         2.8 versicolor
#> 57           6.3         3.3 versicolor
#> 58           4.9         2.4 versicolor
#> 59           6.6         2.9 versicolor
#> 60           5.2         2.7 versicolor
#> 61           5.0         2.0 versicolor
#> 62           5.9         3.0 versicolor
#> 63           6.0         2.2 versicolor
#> 64           6.1         2.9 versicolor
#> 65           5.6         2.9 versicolor
#> 66           6.7         3.1 versicolor
#> 67           5.6         3.0 versicolor
#> 68           5.8         2.7 versicolor
#> 69           6.2         2.2 versicolor
#> 70           5.6         2.5 versicolor
#> 71           5.9         3.2 versicolor
#> 72           6.1         2.8 versicolor
#> 73           6.3         2.5 versicolor
#> 74           6.1         2.8 versicolor
#> 75           6.4         2.9 versicolor
#> 76           6.6         3.0 versicolor
#> 77           6.8         2.8 versicolor
#> 78           6.7         3.0 versicolor
#> 79           6.0         2.9 versicolor
#> 80           5.7         2.6 versicolor
#> 81           5.5         2.4 versicolor
#> 82           5.5         2.4 versicolor
#> 83           5.8         2.7 versicolor
#> 84           6.0         2.7 versicolor
#> 85           5.4         3.0 versicolor
#> 86           6.0         3.4 versicolor
#> 87           6.7         3.1 versicolor
#> 88           6.3         2.3 versicolor
#> 89           5.6         3.0 versicolor
#> 90           5.5         2.5 versicolor
#> 91           5.5         2.6 versicolor
#> 92           6.1         3.0 versicolor
#> 93           5.8         2.6 versicolor
#> 94           5.0         2.3 versicolor
#> 95           5.6         2.7 versicolor
#> 96           5.7         3.0 versicolor
#> 97           5.7         2.9 versicolor
#> 98           6.2         2.9 versicolor
#> 99           5.1         2.5 versicolor
#> 100          5.7         2.8 versicolor
#> 101          6.3         3.3  virginica
#> 102          5.8         2.7  virginica
#> 103          7.1         3.0  virginica
#> 104          6.3         2.9  virginica
#> 105          6.5         3.0  virginica
#> 106          7.6         3.0  virginica
#> 107          4.9         2.5  virginica
#> 108          7.3         2.9  virginica
#> 109          6.7         2.5  virginica
#> 110          7.2         3.6  virginica
#> 111          6.5         3.2  virginica
#> 112          6.4         2.7  virginica
#> 113          6.8         3.0  virginica
#> 114          5.7         2.5  virginica
#> 115          5.8         2.8  virginica
#> 116          6.4         3.2  virginica
#> 117          6.5         3.0  virginica
#> 118          7.7         3.8  virginica
#> 119          7.7         2.6  virginica
#> 120          6.0         2.2  virginica
#> 121          6.9         3.2  virginica
#> 122          5.6         2.8  virginica
#> 123          7.7         2.8  virginica
#> 124          6.3         2.7  virginica
#> 125          6.7         3.3  virginica
#> 126          7.2         3.2  virginica
#> 127          6.2         2.8  virginica
#> 128          6.1         3.0  virginica
#> 129          6.4         2.8  virginica
#> 130          7.2         3.0  virginica
#> 131          7.4         2.8  virginica
#> 132          7.9         3.8  virginica
#> 133          6.4         2.8  virginica
#> 134          6.3         2.8  virginica
#> 135          6.1         2.6  virginica
#> 136          7.7         3.0  virginica
#> 137          6.3         3.4  virginica
#> 138          6.4         3.1  virginica
#> 139          6.0         3.0  virginica
#> 140          6.9         3.1  virginica
#> 141          6.7         3.1  virginica
#> 142          6.9         3.1  virginica
#> 143          5.8         2.7  virginica
#> 144          6.8         3.2  virginica
#> 145          6.7         3.3  virginica
#> 146          6.7         3.0  virginica
#> 147          6.3         2.5  virginica
#> 148          6.5         3.0  virginica
#> 149          6.2         3.4  virginica
#> 150          5.9         3.0  virginica
```

---

### `select` 2 - except Species


```r
select(iris, -Species)
#>     Sepal.Length Sepal.Width Petal.Length Petal.Width
#> 1            5.1         3.5          1.4         0.2
#> 2            4.9         3.0          1.4         0.2
#> 3            4.7         3.2          1.3         0.2
#> 4            4.6         3.1          1.5         0.2
#> 5            5.0         3.6          1.4         0.2
#> 6            5.4         3.9          1.7         0.4
#> 7            4.6         3.4          1.4         0.3
#> 8            5.0         3.4          1.5         0.2
#> 9            4.4         2.9          1.4         0.2
#> 10           4.9         3.1          1.5         0.1
#> 11           5.4         3.7          1.5         0.2
#> 12           4.8         3.4          1.6         0.2
#> 13           4.8         3.0          1.4         0.1
#> 14           4.3         3.0          1.1         0.1
#> 15           5.8         4.0          1.2         0.2
#> 16           5.7         4.4          1.5         0.4
#> 17           5.4         3.9          1.3         0.4
#> 18           5.1         3.5          1.4         0.3
#> 19           5.7         3.8          1.7         0.3
#> 20           5.1         3.8          1.5         0.3
#> 21           5.4         3.4          1.7         0.2
#> 22           5.1         3.7          1.5         0.4
#> 23           4.6         3.6          1.0         0.2
#> 24           5.1         3.3          1.7         0.5
#> 25           4.8         3.4          1.9         0.2
#> 26           5.0         3.0          1.6         0.2
#> 27           5.0         3.4          1.6         0.4
#> 28           5.2         3.5          1.5         0.2
#> 29           5.2         3.4          1.4         0.2
#> 30           4.7         3.2          1.6         0.2
#> 31           4.8         3.1          1.6         0.2
#> 32           5.4         3.4          1.5         0.4
#> 33           5.2         4.1          1.5         0.1
#> 34           5.5         4.2          1.4         0.2
#> 35           4.9         3.1          1.5         0.2
#> 36           5.0         3.2          1.2         0.2
#> 37           5.5         3.5          1.3         0.2
#> 38           4.9         3.6          1.4         0.1
#> 39           4.4         3.0          1.3         0.2
#> 40           5.1         3.4          1.5         0.2
#> 41           5.0         3.5          1.3         0.3
#> 42           4.5         2.3          1.3         0.3
#> 43           4.4         3.2          1.3         0.2
#> 44           5.0         3.5          1.6         0.6
#> 45           5.1         3.8          1.9         0.4
#> 46           4.8         3.0          1.4         0.3
#> 47           5.1         3.8          1.6         0.2
#> 48           4.6         3.2          1.4         0.2
#> 49           5.3         3.7          1.5         0.2
#> 50           5.0         3.3          1.4         0.2
#> 51           7.0         3.2          4.7         1.4
#> 52           6.4         3.2          4.5         1.5
#> 53           6.9         3.1          4.9         1.5
#> 54           5.5         2.3          4.0         1.3
#> 55           6.5         2.8          4.6         1.5
#> 56           5.7         2.8          4.5         1.3
#> 57           6.3         3.3          4.7         1.6
#> 58           4.9         2.4          3.3         1.0
#> 59           6.6         2.9          4.6         1.3
#> 60           5.2         2.7          3.9         1.4
#> 61           5.0         2.0          3.5         1.0
#> 62           5.9         3.0          4.2         1.5
#> 63           6.0         2.2          4.0         1.0
#> 64           6.1         2.9          4.7         1.4
#> 65           5.6         2.9          3.6         1.3
#> 66           6.7         3.1          4.4         1.4
#> 67           5.6         3.0          4.5         1.5
#> 68           5.8         2.7          4.1         1.0
#> 69           6.2         2.2          4.5         1.5
#> 70           5.6         2.5          3.9         1.1
#> 71           5.9         3.2          4.8         1.8
#> 72           6.1         2.8          4.0         1.3
#> 73           6.3         2.5          4.9         1.5
#> 74           6.1         2.8          4.7         1.2
#> 75           6.4         2.9          4.3         1.3
#> 76           6.6         3.0          4.4         1.4
#> 77           6.8         2.8          4.8         1.4
#> 78           6.7         3.0          5.0         1.7
#> 79           6.0         2.9          4.5         1.5
#> 80           5.7         2.6          3.5         1.0
#> 81           5.5         2.4          3.8         1.1
#> 82           5.5         2.4          3.7         1.0
#> 83           5.8         2.7          3.9         1.2
#> 84           6.0         2.7          5.1         1.6
#> 85           5.4         3.0          4.5         1.5
#> 86           6.0         3.4          4.5         1.6
#> 87           6.7         3.1          4.7         1.5
#> 88           6.3         2.3          4.4         1.3
#> 89           5.6         3.0          4.1         1.3
#> 90           5.5         2.5          4.0         1.3
#> 91           5.5         2.6          4.4         1.2
#> 92           6.1         3.0          4.6         1.4
#> 93           5.8         2.6          4.0         1.2
#> 94           5.0         2.3          3.3         1.0
#> 95           5.6         2.7          4.2         1.3
#> 96           5.7         3.0          4.2         1.2
#> 97           5.7         2.9          4.2         1.3
#> 98           6.2         2.9          4.3         1.3
#> 99           5.1         2.5          3.0         1.1
#> 100          5.7         2.8          4.1         1.3
#> 101          6.3         3.3          6.0         2.5
#> 102          5.8         2.7          5.1         1.9
#> 103          7.1         3.0          5.9         2.1
#> 104          6.3         2.9          5.6         1.8
#> 105          6.5         3.0          5.8         2.2
#> 106          7.6         3.0          6.6         2.1
#> 107          4.9         2.5          4.5         1.7
#> 108          7.3         2.9          6.3         1.8
#> 109          6.7         2.5          5.8         1.8
#> 110          7.2         3.6          6.1         2.5
#> 111          6.5         3.2          5.1         2.0
#> 112          6.4         2.7          5.3         1.9
#> 113          6.8         3.0          5.5         2.1
#> 114          5.7         2.5          5.0         2.0
#> 115          5.8         2.8          5.1         2.4
#> 116          6.4         3.2          5.3         2.3
#> 117          6.5         3.0          5.5         1.8
#> 118          7.7         3.8          6.7         2.2
#> 119          7.7         2.6          6.9         2.3
#> 120          6.0         2.2          5.0         1.5
#> 121          6.9         3.2          5.7         2.3
#> 122          5.6         2.8          4.9         2.0
#> 123          7.7         2.8          6.7         2.0
#> 124          6.3         2.7          4.9         1.8
#> 125          6.7         3.3          5.7         2.1
#> 126          7.2         3.2          6.0         1.8
#> 127          6.2         2.8          4.8         1.8
#> 128          6.1         3.0          4.9         1.8
#> 129          6.4         2.8          5.6         2.1
#> 130          7.2         3.0          5.8         1.6
#> 131          7.4         2.8          6.1         1.9
#> 132          7.9         3.8          6.4         2.0
#> 133          6.4         2.8          5.6         2.2
#> 134          6.3         2.8          5.1         1.5
#> 135          6.1         2.6          5.6         1.4
#> 136          7.7         3.0          6.1         2.3
#> 137          6.3         3.4          5.6         2.4
#> 138          6.4         3.1          5.5         1.8
#> 139          6.0         3.0          4.8         1.8
#> 140          6.9         3.1          5.4         2.1
#> 141          6.7         3.1          5.6         2.4
#> 142          6.9         3.1          5.1         2.3
#> 143          5.8         2.7          5.1         1.9
#> 144          6.8         3.2          5.9         2.3
#> 145          6.7         3.3          5.7         2.5
#> 146          6.7         3.0          5.2         2.3
#> 147          6.3         2.5          5.0         1.9
#> 148          6.5         3.0          5.2         2.0
#> 149          6.2         3.4          5.4         2.3
#> 150          5.9         3.0          5.1         1.8
```

---

### `select` 3 - change column names


```r
select(iris, sl = Sepal.Length, sw = Sepal.Width, sp = Species)
#>      sl  sw         sp
#> 1   5.1 3.5     setosa
#> 2   4.9 3.0     setosa
#> 3   4.7 3.2     setosa
#> 4   4.6 3.1     setosa
#> 5   5.0 3.6     setosa
#> 6   5.4 3.9     setosa
#> 7   4.6 3.4     setosa
#> 8   5.0 3.4     setosa
#> 9   4.4 2.9     setosa
#> 10  4.9 3.1     setosa
#> 11  5.4 3.7     setosa
#> 12  4.8 3.4     setosa
#> 13  4.8 3.0     setosa
#> 14  4.3 3.0     setosa
#> 15  5.8 4.0     setosa
#> 16  5.7 4.4     setosa
#> 17  5.4 3.9     setosa
#> 18  5.1 3.5     setosa
#> 19  5.7 3.8     setosa
#> 20  5.1 3.8     setosa
#> 21  5.4 3.4     setosa
#> 22  5.1 3.7     setosa
#> 23  4.6 3.6     setosa
#> 24  5.1 3.3     setosa
#> 25  4.8 3.4     setosa
#> 26  5.0 3.0     setosa
#> 27  5.0 3.4     setosa
#> 28  5.2 3.5     setosa
#> 29  5.2 3.4     setosa
#> 30  4.7 3.2     setosa
#> 31  4.8 3.1     setosa
#> 32  5.4 3.4     setosa
#> 33  5.2 4.1     setosa
#> 34  5.5 4.2     setosa
#> 35  4.9 3.1     setosa
#> 36  5.0 3.2     setosa
#> 37  5.5 3.5     setosa
#> 38  4.9 3.6     setosa
#> 39  4.4 3.0     setosa
#> 40  5.1 3.4     setosa
#> 41  5.0 3.5     setosa
#> 42  4.5 2.3     setosa
#> 43  4.4 3.2     setosa
#> 44  5.0 3.5     setosa
#> 45  5.1 3.8     setosa
#> 46  4.8 3.0     setosa
#> 47  5.1 3.8     setosa
#> 48  4.6 3.2     setosa
#> 49  5.3 3.7     setosa
#> 50  5.0 3.3     setosa
#> 51  7.0 3.2 versicolor
#> 52  6.4 3.2 versicolor
#> 53  6.9 3.1 versicolor
#> 54  5.5 2.3 versicolor
#> 55  6.5 2.8 versicolor
#> 56  5.7 2.8 versicolor
#> 57  6.3 3.3 versicolor
#> 58  4.9 2.4 versicolor
#> 59  6.6 2.9 versicolor
#> 60  5.2 2.7 versicolor
#> 61  5.0 2.0 versicolor
#> 62  5.9 3.0 versicolor
#> 63  6.0 2.2 versicolor
#> 64  6.1 2.9 versicolor
#> 65  5.6 2.9 versicolor
#> 66  6.7 3.1 versicolor
#> 67  5.6 3.0 versicolor
#> 68  5.8 2.7 versicolor
#> 69  6.2 2.2 versicolor
#> 70  5.6 2.5 versicolor
#> 71  5.9 3.2 versicolor
#> 72  6.1 2.8 versicolor
#> 73  6.3 2.5 versicolor
#> 74  6.1 2.8 versicolor
#> 75  6.4 2.9 versicolor
#> 76  6.6 3.0 versicolor
#> 77  6.8 2.8 versicolor
#> 78  6.7 3.0 versicolor
#> 79  6.0 2.9 versicolor
#> 80  5.7 2.6 versicolor
#> 81  5.5 2.4 versicolor
#> 82  5.5 2.4 versicolor
#> 83  5.8 2.7 versicolor
#> 84  6.0 2.7 versicolor
#> 85  5.4 3.0 versicolor
#> 86  6.0 3.4 versicolor
#> 87  6.7 3.1 versicolor
#> 88  6.3 2.3 versicolor
#> 89  5.6 3.0 versicolor
#> 90  5.5 2.5 versicolor
#> 91  5.5 2.6 versicolor
#> 92  6.1 3.0 versicolor
#> 93  5.8 2.6 versicolor
#> 94  5.0 2.3 versicolor
#> 95  5.6 2.7 versicolor
#> 96  5.7 3.0 versicolor
#> 97  5.7 2.9 versicolor
#> 98  6.2 2.9 versicolor
#> 99  5.1 2.5 versicolor
#> 100 5.7 2.8 versicolor
#> 101 6.3 3.3  virginica
#> 102 5.8 2.7  virginica
#> 103 7.1 3.0  virginica
#> 104 6.3 2.9  virginica
#> 105 6.5 3.0  virginica
#> 106 7.6 3.0  virginica
#> 107 4.9 2.5  virginica
#> 108 7.3 2.9  virginica
#> 109 6.7 2.5  virginica
#> 110 7.2 3.6  virginica
#> 111 6.5 3.2  virginica
#> 112 6.4 2.7  virginica
#> 113 6.8 3.0  virginica
#> 114 5.7 2.5  virginica
#> 115 5.8 2.8  virginica
#> 116 6.4 3.2  virginica
#> 117 6.5 3.0  virginica
#> 118 7.7 3.8  virginica
#> 119 7.7 2.6  virginica
#> 120 6.0 2.2  virginica
#> 121 6.9 3.2  virginica
#> 122 5.6 2.8  virginica
#> 123 7.7 2.8  virginica
#> 124 6.3 2.7  virginica
#> 125 6.7 3.3  virginica
#> 126 7.2 3.2  virginica
#> 127 6.2 2.8  virginica
#> 128 6.1 3.0  virginica
#> 129 6.4 2.8  virginica
#> 130 7.2 3.0  virginica
#> 131 7.4 2.8  virginica
#> 132 7.9 3.8  virginica
#> 133 6.4 2.8  virginica
#> 134 6.3 2.8  virginica
#> 135 6.1 2.6  virginica
#> 136 7.7 3.0  virginica
#> 137 6.3 3.4  virginica
#> 138 6.4 3.1  virginica
#> 139 6.0 3.0  virginica
#> 140 6.9 3.1  virginica
#> 141 6.7 3.1  virginica
#> 142 6.9 3.1  virginica
#> 143 5.8 2.7  virginica
#> 144 6.8 3.2  virginica
#> 145 6.7 3.3  virginica
#> 146 6.7 3.0  virginica
#> 147 6.3 2.5  virginica
#> 148 6.5 3.0  virginica
#> 149 6.2 3.4  virginica
#> 150 5.9 3.0  virginica
```

---

### `filter` - by names


```r
filter(iris, Species == "virginica")
#>    Sepal.Length Sepal.Width Petal.Length Petal.Width
#> 1           6.3         3.3          6.0         2.5
#> 2           5.8         2.7          5.1         1.9
#> 3           7.1         3.0          5.9         2.1
#> 4           6.3         2.9          5.6         1.8
#> 5           6.5         3.0          5.8         2.2
#> 6           7.6         3.0          6.6         2.1
#> 7           4.9         2.5          4.5         1.7
#> 8           7.3         2.9          6.3         1.8
#> 9           6.7         2.5          5.8         1.8
#> 10          7.2         3.6          6.1         2.5
#> 11          6.5         3.2          5.1         2.0
#> 12          6.4         2.7          5.3         1.9
#> 13          6.8         3.0          5.5         2.1
#> 14          5.7         2.5          5.0         2.0
#> 15          5.8         2.8          5.1         2.4
#> 16          6.4         3.2          5.3         2.3
#> 17          6.5         3.0          5.5         1.8
#> 18          7.7         3.8          6.7         2.2
#> 19          7.7         2.6          6.9         2.3
#> 20          6.0         2.2          5.0         1.5
#> 21          6.9         3.2          5.7         2.3
#> 22          5.6         2.8          4.9         2.0
#> 23          7.7         2.8          6.7         2.0
#> 24          6.3         2.7          4.9         1.8
#> 25          6.7         3.3          5.7         2.1
#> 26          7.2         3.2          6.0         1.8
#> 27          6.2         2.8          4.8         1.8
#> 28          6.1         3.0          4.9         1.8
#> 29          6.4         2.8          5.6         2.1
#> 30          7.2         3.0          5.8         1.6
#> 31          7.4         2.8          6.1         1.9
#> 32          7.9         3.8          6.4         2.0
#> 33          6.4         2.8          5.6         2.2
#> 34          6.3         2.8          5.1         1.5
#> 35          6.1         2.6          5.6         1.4
#> 36          7.7         3.0          6.1         2.3
#> 37          6.3         3.4          5.6         2.4
#> 38          6.4         3.1          5.5         1.8
#> 39          6.0         3.0          4.8         1.8
#> 40          6.9         3.1          5.4         2.1
#> 41          6.7         3.1          5.6         2.4
#> 42          6.9         3.1          5.1         2.3
#> 43          5.8         2.7          5.1         1.9
#> 44          6.8         3.2          5.9         2.3
#> 45          6.7         3.3          5.7         2.5
#> 46          6.7         3.0          5.2         2.3
#> 47          6.3         2.5          5.0         1.9
#> 48          6.5         3.0          5.2         2.0
#> 49          6.2         3.4          5.4         2.3
#> 50          5.9         3.0          5.1         1.8
#>      Species
#> 1  virginica
#> 2  virginica
#> 3  virginica
#> 4  virginica
#> 5  virginica
#> 6  virginica
#> 7  virginica
#> 8  virginica
#> 9  virginica
#> 10 virginica
#> 11 virginica
#> 12 virginica
#> 13 virginica
#> 14 virginica
#> 15 virginica
#> 16 virginica
#> 17 virginica
#> 18 virginica
#> 19 virginica
#> 20 virginica
#> 21 virginica
#> 22 virginica
#> 23 virginica
#> 24 virginica
#> 25 virginica
#> 26 virginica
#> 27 virginica
#> 28 virginica
#> 29 virginica
#> 30 virginica
#> 31 virginica
#> 32 virginica
#> 33 virginica
#> 34 virginica
#> 35 virginica
#> 36 virginica
#> 37 virginica
#> 38 virginica
#> 39 virginica
#> 40 virginica
#> 41 virginica
#> 42 virginica
#> 43 virginica
#> 44 virginica
#> 45 virginica
#> 46 virginica
#> 47 virginica
#> 48 virginica
#> 49 virginica
#> 50 virginica
```


---

### `arrange`  - ascending and descending order


```r
arrange(iris, Sepal.Length, desc(Sepal.Width))
#>     Sepal.Length Sepal.Width Petal.Length Petal.Width
#> 1            4.3         3.0          1.1         0.1
#> 2            4.4         3.2          1.3         0.2
#> 3            4.4         3.0          1.3         0.2
#> 4            4.4         2.9          1.4         0.2
#> 5            4.5         2.3          1.3         0.3
#> 6            4.6         3.6          1.0         0.2
#> 7            4.6         3.4          1.4         0.3
#> 8            4.6         3.2          1.4         0.2
#> 9            4.6         3.1          1.5         0.2
#> 10           4.7         3.2          1.3         0.2
#> 11           4.7         3.2          1.6         0.2
#> 12           4.8         3.4          1.6         0.2
#> 13           4.8         3.4          1.9         0.2
#> 14           4.8         3.1          1.6         0.2
#> 15           4.8         3.0          1.4         0.1
#> 16           4.8         3.0          1.4         0.3
#> 17           4.9         3.6          1.4         0.1
#> 18           4.9         3.1          1.5         0.1
#> 19           4.9         3.1          1.5         0.2
#> 20           4.9         3.0          1.4         0.2
#> 21           4.9         2.5          4.5         1.7
#> 22           4.9         2.4          3.3         1.0
#> 23           5.0         3.6          1.4         0.2
#> 24           5.0         3.5          1.3         0.3
#> 25           5.0         3.5          1.6         0.6
#> 26           5.0         3.4          1.5         0.2
#> 27           5.0         3.4          1.6         0.4
#> 28           5.0         3.3          1.4         0.2
#> 29           5.0         3.2          1.2         0.2
#> 30           5.0         3.0          1.6         0.2
#> 31           5.0         2.3          3.3         1.0
#> 32           5.0         2.0          3.5         1.0
#> 33           5.1         3.8          1.5         0.3
#> 34           5.1         3.8          1.9         0.4
#> 35           5.1         3.8          1.6         0.2
#> 36           5.1         3.7          1.5         0.4
#> 37           5.1         3.5          1.4         0.2
#> 38           5.1         3.5          1.4         0.3
#> 39           5.1         3.4          1.5         0.2
#> 40           5.1         3.3          1.7         0.5
#> 41           5.1         2.5          3.0         1.1
#> 42           5.2         4.1          1.5         0.1
#> 43           5.2         3.5          1.5         0.2
#> 44           5.2         3.4          1.4         0.2
#> 45           5.2         2.7          3.9         1.4
#> 46           5.3         3.7          1.5         0.2
#> 47           5.4         3.9          1.7         0.4
#> 48           5.4         3.9          1.3         0.4
#> 49           5.4         3.7          1.5         0.2
#> 50           5.4         3.4          1.7         0.2
#> 51           5.4         3.4          1.5         0.4
#> 52           5.4         3.0          4.5         1.5
#> 53           5.5         4.2          1.4         0.2
#> 54           5.5         3.5          1.3         0.2
#> 55           5.5         2.6          4.4         1.2
#> 56           5.5         2.5          4.0         1.3
#> 57           5.5         2.4          3.8         1.1
#> 58           5.5         2.4          3.7         1.0
#> 59           5.5         2.3          4.0         1.3
#> 60           5.6         3.0          4.5         1.5
#> 61           5.6         3.0          4.1         1.3
#> 62           5.6         2.9          3.6         1.3
#> 63           5.6         2.8          4.9         2.0
#> 64           5.6         2.7          4.2         1.3
#> 65           5.6         2.5          3.9         1.1
#> 66           5.7         4.4          1.5         0.4
#> 67           5.7         3.8          1.7         0.3
#> 68           5.7         3.0          4.2         1.2
#> 69           5.7         2.9          4.2         1.3
#> 70           5.7         2.8          4.5         1.3
#> 71           5.7         2.8          4.1         1.3
#> 72           5.7         2.6          3.5         1.0
#> 73           5.7         2.5          5.0         2.0
#> 74           5.8         4.0          1.2         0.2
#> 75           5.8         2.8          5.1         2.4
#> 76           5.8         2.7          4.1         1.0
#> 77           5.8         2.7          3.9         1.2
#> 78           5.8         2.7          5.1         1.9
#> 79           5.8         2.7          5.1         1.9
#> 80           5.8         2.6          4.0         1.2
#> 81           5.9         3.2          4.8         1.8
#> 82           5.9         3.0          4.2         1.5
#> 83           5.9         3.0          5.1         1.8
#> 84           6.0         3.4          4.5         1.6
#> 85           6.0         3.0          4.8         1.8
#> 86           6.0         2.9          4.5         1.5
#> 87           6.0         2.7          5.1         1.6
#> 88           6.0         2.2          4.0         1.0
#> 89           6.0         2.2          5.0         1.5
#> 90           6.1         3.0          4.6         1.4
#> 91           6.1         3.0          4.9         1.8
#> 92           6.1         2.9          4.7         1.4
#> 93           6.1         2.8          4.0         1.3
#> 94           6.1         2.8          4.7         1.2
#> 95           6.1         2.6          5.6         1.4
#> 96           6.2         3.4          5.4         2.3
#> 97           6.2         2.9          4.3         1.3
#> 98           6.2         2.8          4.8         1.8
#> 99           6.2         2.2          4.5         1.5
#> 100          6.3         3.4          5.6         2.4
#> 101          6.3         3.3          4.7         1.6
#> 102          6.3         3.3          6.0         2.5
#> 103          6.3         2.9          5.6         1.8
#> 104          6.3         2.8          5.1         1.5
#> 105          6.3         2.7          4.9         1.8
#> 106          6.3         2.5          4.9         1.5
#> 107          6.3         2.5          5.0         1.9
#> 108          6.3         2.3          4.4         1.3
#> 109          6.4         3.2          4.5         1.5
#> 110          6.4         3.2          5.3         2.3
#> 111          6.4         3.1          5.5         1.8
#> 112          6.4         2.9          4.3         1.3
#> 113          6.4         2.8          5.6         2.1
#> 114          6.4         2.8          5.6         2.2
#> 115          6.4         2.7          5.3         1.9
#> 116          6.5         3.2          5.1         2.0
#> 117          6.5         3.0          5.8         2.2
#> 118          6.5         3.0          5.5         1.8
#> 119          6.5         3.0          5.2         2.0
#> 120          6.5         2.8          4.6         1.5
#> 121          6.6         3.0          4.4         1.4
#> 122          6.6         2.9          4.6         1.3
#> 123          6.7         3.3          5.7         2.1
#> 124          6.7         3.3          5.7         2.5
#> 125          6.7         3.1          4.4         1.4
#> 126          6.7         3.1          4.7         1.5
#> 127          6.7         3.1          5.6         2.4
#> 128          6.7         3.0          5.0         1.7
#> 129          6.7         3.0          5.2         2.3
#> 130          6.7         2.5          5.8         1.8
#> 131          6.8         3.2          5.9         2.3
#> 132          6.8         3.0          5.5         2.1
#> 133          6.8         2.8          4.8         1.4
#> 134          6.9         3.2          5.7         2.3
#> 135          6.9         3.1          4.9         1.5
#> 136          6.9         3.1          5.4         2.1
#> 137          6.9         3.1          5.1         2.3
#> 138          7.0         3.2          4.7         1.4
#> 139          7.1         3.0          5.9         2.1
#> 140          7.2         3.6          6.1         2.5
#> 141          7.2         3.2          6.0         1.8
#> 142          7.2         3.0          5.8         1.6
#> 143          7.3         2.9          6.3         1.8
#> 144          7.4         2.8          6.1         1.9
#> 145          7.6         3.0          6.6         2.1
#> 146          7.7         3.8          6.7         2.2
#> 147          7.7         3.0          6.1         2.3
#> 148          7.7         2.8          6.7         2.0
#> 149          7.7         2.6          6.9         2.3
#> 150          7.9         3.8          6.4         2.0
#>        Species
#> 1       setosa
#> 2       setosa
#> 3       setosa
#> 4       setosa
#> 5       setosa
#> 6       setosa
#> 7       setosa
#> 8       setosa
#> 9       setosa
#> 10      setosa
#> 11      setosa
#> 12      setosa
#> 13      setosa
#> 14      setosa
#> 15      setosa
#> 16      setosa
#> 17      setosa
#> 18      setosa
#> 19      setosa
#> 20      setosa
#> 21   virginica
#> 22  versicolor
#> 23      setosa
#> 24      setosa
#> 25      setosa
#> 26      setosa
#> 27      setosa
#> 28      setosa
#> 29      setosa
#> 30      setosa
#> 31  versicolor
#> 32  versicolor
#> 33      setosa
#> 34      setosa
#> 35      setosa
#> 36      setosa
#> 37      setosa
#> 38      setosa
#> 39      setosa
#> 40      setosa
#> 41  versicolor
#> 42      setosa
#> 43      setosa
#> 44      setosa
#> 45  versicolor
#> 46      setosa
#> 47      setosa
#> 48      setosa
#> 49      setosa
#> 50      setosa
#> 51      setosa
#> 52  versicolor
#> 53      setosa
#> 54      setosa
#> 55  versicolor
#> 56  versicolor
#> 57  versicolor
#> 58  versicolor
#> 59  versicolor
#> 60  versicolor
#> 61  versicolor
#> 62  versicolor
#> 63   virginica
#> 64  versicolor
#> 65  versicolor
#> 66      setosa
#> 67      setosa
#> 68  versicolor
#> 69  versicolor
#> 70  versicolor
#> 71  versicolor
#> 72  versicolor
#> 73   virginica
#> 74      setosa
#> 75   virginica
#> 76  versicolor
#> 77  versicolor
#> 78   virginica
#> 79   virginica
#> 80  versicolor
#> 81  versicolor
#> 82  versicolor
#> 83   virginica
#> 84  versicolor
#> 85   virginica
#> 86  versicolor
#> 87  versicolor
#> 88  versicolor
#> 89   virginica
#> 90  versicolor
#> 91   virginica
#> 92  versicolor
#> 93  versicolor
#> 94  versicolor
#> 95   virginica
#> 96   virginica
#> 97  versicolor
#> 98   virginica
#> 99  versicolor
#> 100  virginica
#> 101 versicolor
#> 102  virginica
#> 103  virginica
#> 104  virginica
#> 105  virginica
#> 106 versicolor
#> 107  virginica
#> 108 versicolor
#> 109 versicolor
#> 110  virginica
#> 111  virginica
#> 112 versicolor
#> 113  virginica
#> 114  virginica
#> 115  virginica
#> 116  virginica
#> 117  virginica
#> 118  virginica
#> 119  virginica
#> 120 versicolor
#> 121 versicolor
#> 122 versicolor
#> 123  virginica
#> 124  virginica
#> 125 versicolor
#> 126 versicolor
#> 127  virginica
#> 128 versicolor
#> 129  virginica
#> 130  virginica
#> 131  virginica
#> 132  virginica
#> 133 versicolor
#> 134  virginica
#> 135 versicolor
#> 136  virginica
#> 137  virginica
#> 138 versicolor
#> 139  virginica
#> 140  virginica
#> 141  virginica
#> 142  virginica
#> 143  virginica
#> 144  virginica
#> 145  virginica
#> 146  virginica
#> 147  virginica
#> 148  virginica
#> 149  virginica
#> 150  virginica
```

---

### `mutate` - rank


```r
iris %>% mutate(sl_rank = min_rank(Sepal.Length)) %>% arrange(sl_rank)
#>     Sepal.Length Sepal.Width Petal.Length Petal.Width
#> 1            4.3         3.0          1.1         0.1
#> 2            4.4         2.9          1.4         0.2
#> 3            4.4         3.0          1.3         0.2
#> 4            4.4         3.2          1.3         0.2
#> 5            4.5         2.3          1.3         0.3
#> 6            4.6         3.1          1.5         0.2
#> 7            4.6         3.4          1.4         0.3
#> 8            4.6         3.6          1.0         0.2
#> 9            4.6         3.2          1.4         0.2
#> 10           4.7         3.2          1.3         0.2
#> 11           4.7         3.2          1.6         0.2
#> 12           4.8         3.4          1.6         0.2
#> 13           4.8         3.0          1.4         0.1
#> 14           4.8         3.4          1.9         0.2
#> 15           4.8         3.1          1.6         0.2
#> 16           4.8         3.0          1.4         0.3
#> 17           4.9         3.0          1.4         0.2
#> 18           4.9         3.1          1.5         0.1
#> 19           4.9         3.1          1.5         0.2
#> 20           4.9         3.6          1.4         0.1
#> 21           4.9         2.4          3.3         1.0
#> 22           4.9         2.5          4.5         1.7
#> 23           5.0         3.6          1.4         0.2
#> 24           5.0         3.4          1.5         0.2
#> 25           5.0         3.0          1.6         0.2
#> 26           5.0         3.4          1.6         0.4
#> 27           5.0         3.2          1.2         0.2
#> 28           5.0         3.5          1.3         0.3
#> 29           5.0         3.5          1.6         0.6
#> 30           5.0         3.3          1.4         0.2
#> 31           5.0         2.0          3.5         1.0
#> 32           5.0         2.3          3.3         1.0
#> 33           5.1         3.5          1.4         0.2
#> 34           5.1         3.5          1.4         0.3
#> 35           5.1         3.8          1.5         0.3
#> 36           5.1         3.7          1.5         0.4
#> 37           5.1         3.3          1.7         0.5
#> 38           5.1         3.4          1.5         0.2
#> 39           5.1         3.8          1.9         0.4
#> 40           5.1         3.8          1.6         0.2
#> 41           5.1         2.5          3.0         1.1
#> 42           5.2         3.5          1.5         0.2
#> 43           5.2         3.4          1.4         0.2
#> 44           5.2         4.1          1.5         0.1
#> 45           5.2         2.7          3.9         1.4
#> 46           5.3         3.7          1.5         0.2
#> 47           5.4         3.9          1.7         0.4
#> 48           5.4         3.7          1.5         0.2
#> 49           5.4         3.9          1.3         0.4
#> 50           5.4         3.4          1.7         0.2
#> 51           5.4         3.4          1.5         0.4
#> 52           5.4         3.0          4.5         1.5
#> 53           5.5         4.2          1.4         0.2
#> 54           5.5         3.5          1.3         0.2
#> 55           5.5         2.3          4.0         1.3
#> 56           5.5         2.4          3.8         1.1
#> 57           5.5         2.4          3.7         1.0
#> 58           5.5         2.5          4.0         1.3
#> 59           5.5         2.6          4.4         1.2
#> 60           5.6         2.9          3.6         1.3
#> 61           5.6         3.0          4.5         1.5
#> 62           5.6         2.5          3.9         1.1
#> 63           5.6         3.0          4.1         1.3
#> 64           5.6         2.7          4.2         1.3
#> 65           5.6         2.8          4.9         2.0
#> 66           5.7         4.4          1.5         0.4
#> 67           5.7         3.8          1.7         0.3
#> 68           5.7         2.8          4.5         1.3
#> 69           5.7         2.6          3.5         1.0
#> 70           5.7         3.0          4.2         1.2
#> 71           5.7         2.9          4.2         1.3
#> 72           5.7         2.8          4.1         1.3
#> 73           5.7         2.5          5.0         2.0
#> 74           5.8         4.0          1.2         0.2
#> 75           5.8         2.7          4.1         1.0
#> 76           5.8         2.7          3.9         1.2
#> 77           5.8         2.6          4.0         1.2
#> 78           5.8         2.7          5.1         1.9
#> 79           5.8         2.8          5.1         2.4
#> 80           5.8         2.7          5.1         1.9
#> 81           5.9         3.0          4.2         1.5
#> 82           5.9         3.2          4.8         1.8
#> 83           5.9         3.0          5.1         1.8
#> 84           6.0         2.2          4.0         1.0
#> 85           6.0         2.9          4.5         1.5
#> 86           6.0         2.7          5.1         1.6
#> 87           6.0         3.4          4.5         1.6
#> 88           6.0         2.2          5.0         1.5
#> 89           6.0         3.0          4.8         1.8
#> 90           6.1         2.9          4.7         1.4
#> 91           6.1         2.8          4.0         1.3
#> 92           6.1         2.8          4.7         1.2
#> 93           6.1         3.0          4.6         1.4
#> 94           6.1         3.0          4.9         1.8
#> 95           6.1         2.6          5.6         1.4
#> 96           6.2         2.2          4.5         1.5
#> 97           6.2         2.9          4.3         1.3
#> 98           6.2         2.8          4.8         1.8
#> 99           6.2         3.4          5.4         2.3
#> 100          6.3         3.3          4.7         1.6
#> 101          6.3         2.5          4.9         1.5
#> 102          6.3         2.3          4.4         1.3
#> 103          6.3         3.3          6.0         2.5
#> 104          6.3         2.9          5.6         1.8
#> 105          6.3         2.7          4.9         1.8
#> 106          6.3         2.8          5.1         1.5
#> 107          6.3         3.4          5.6         2.4
#> 108          6.3         2.5          5.0         1.9
#> 109          6.4         3.2          4.5         1.5
#> 110          6.4         2.9          4.3         1.3
#> 111          6.4         2.7          5.3         1.9
#> 112          6.4         3.2          5.3         2.3
#> 113          6.4         2.8          5.6         2.1
#> 114          6.4         2.8          5.6         2.2
#> 115          6.4         3.1          5.5         1.8
#> 116          6.5         2.8          4.6         1.5
#> 117          6.5         3.0          5.8         2.2
#> 118          6.5         3.2          5.1         2.0
#> 119          6.5         3.0          5.5         1.8
#> 120          6.5         3.0          5.2         2.0
#> 121          6.6         2.9          4.6         1.3
#> 122          6.6         3.0          4.4         1.4
#> 123          6.7         3.1          4.4         1.4
#> 124          6.7         3.0          5.0         1.7
#> 125          6.7         3.1          4.7         1.5
#> 126          6.7         2.5          5.8         1.8
#> 127          6.7         3.3          5.7         2.1
#> 128          6.7         3.1          5.6         2.4
#> 129          6.7         3.3          5.7         2.5
#> 130          6.7         3.0          5.2         2.3
#> 131          6.8         2.8          4.8         1.4
#> 132          6.8         3.0          5.5         2.1
#> 133          6.8         3.2          5.9         2.3
#> 134          6.9         3.1          4.9         1.5
#> 135          6.9         3.2          5.7         2.3
#> 136          6.9         3.1          5.4         2.1
#> 137          6.9         3.1          5.1         2.3
#> 138          7.0         3.2          4.7         1.4
#> 139          7.1         3.0          5.9         2.1
#> 140          7.2         3.6          6.1         2.5
#> 141          7.2         3.2          6.0         1.8
#> 142          7.2         3.0          5.8         1.6
#> 143          7.3         2.9          6.3         1.8
#> 144          7.4         2.8          6.1         1.9
#> 145          7.6         3.0          6.6         2.1
#> 146          7.7         3.8          6.7         2.2
#> 147          7.7         2.6          6.9         2.3
#> 148          7.7         2.8          6.7         2.0
#> 149          7.7         3.0          6.1         2.3
#> 150          7.9         3.8          6.4         2.0
#>        Species sl_rank
#> 1       setosa       1
#> 2       setosa       2
#> 3       setosa       2
#> 4       setosa       2
#> 5       setosa       5
#> 6       setosa       6
#> 7       setosa       6
#> 8       setosa       6
#> 9       setosa       6
#> 10      setosa      10
#> 11      setosa      10
#> 12      setosa      12
#> 13      setosa      12
#> 14      setosa      12
#> 15      setosa      12
#> 16      setosa      12
#> 17      setosa      17
#> 18      setosa      17
#> 19      setosa      17
#> 20      setosa      17
#> 21  versicolor      17
#> 22   virginica      17
#> 23      setosa      23
#> 24      setosa      23
#> 25      setosa      23
#> 26      setosa      23
#> 27      setosa      23
#> 28      setosa      23
#> 29      setosa      23
#> 30      setosa      23
#> 31  versicolor      23
#> 32  versicolor      23
#> 33      setosa      33
#> 34      setosa      33
#> 35      setosa      33
#> 36      setosa      33
#> 37      setosa      33
#> 38      setosa      33
#> 39      setosa      33
#> 40      setosa      33
#> 41  versicolor      33
#> 42      setosa      42
#> 43      setosa      42
#> 44      setosa      42
#> 45  versicolor      42
#> 46      setosa      46
#> 47      setosa      47
#> 48      setosa      47
#> 49      setosa      47
#> 50      setosa      47
#> 51      setosa      47
#> 52  versicolor      47
#> 53      setosa      53
#> 54      setosa      53
#> 55  versicolor      53
#> 56  versicolor      53
#> 57  versicolor      53
#> 58  versicolor      53
#> 59  versicolor      53
#> 60  versicolor      60
#> 61  versicolor      60
#> 62  versicolor      60
#> 63  versicolor      60
#> 64  versicolor      60
#> 65   virginica      60
#> 66      setosa      66
#> 67      setosa      66
#> 68  versicolor      66
#> 69  versicolor      66
#> 70  versicolor      66
#> 71  versicolor      66
#> 72  versicolor      66
#> 73   virginica      66
#> 74      setosa      74
#> 75  versicolor      74
#> 76  versicolor      74
#> 77  versicolor      74
#> 78   virginica      74
#> 79   virginica      74
#> 80   virginica      74
#> 81  versicolor      81
#> 82  versicolor      81
#> 83   virginica      81
#> 84  versicolor      84
#> 85  versicolor      84
#> 86  versicolor      84
#> 87  versicolor      84
#> 88   virginica      84
#> 89   virginica      84
#> 90  versicolor      90
#> 91  versicolor      90
#> 92  versicolor      90
#> 93  versicolor      90
#> 94   virginica      90
#> 95   virginica      90
#> 96  versicolor      96
#> 97  versicolor      96
#> 98   virginica      96
#> 99   virginica      96
#> 100 versicolor     100
#> 101 versicolor     100
#> 102 versicolor     100
#> 103  virginica     100
#> 104  virginica     100
#> 105  virginica     100
#> 106  virginica     100
#> 107  virginica     100
#> 108  virginica     100
#> 109 versicolor     109
#> 110 versicolor     109
#> 111  virginica     109
#> 112  virginica     109
#> 113  virginica     109
#> 114  virginica     109
#> 115  virginica     109
#> 116 versicolor     116
#> 117  virginica     116
#> 118  virginica     116
#> 119  virginica     116
#> 120  virginica     116
#> 121 versicolor     121
#> 122 versicolor     121
#> 123 versicolor     123
#> 124 versicolor     123
#> 125 versicolor     123
#> 126  virginica     123
#> 127  virginica     123
#> 128  virginica     123
#> 129  virginica     123
#> 130  virginica     123
#> 131 versicolor     131
#> 132  virginica     131
#> 133  virginica     131
#> 134 versicolor     134
#> 135  virginica     134
#> 136  virginica     134
#> 137  virginica     134
#> 138 versicolor     138
#> 139  virginica     139
#> 140  virginica     140
#> 141  virginica     140
#> 142  virginica     140
#> 143  virginica     143
#> 144  virginica     144
#> 145  virginica     145
#> 146  virginica     146
#> 147  virginica     146
#> 148  virginica     146
#> 149  virginica     146
#> 150  virginica     150
```

---

### `group_by` and `summarize`


```r
iris %>% 
  group_by(Species) %>% 
  summarize(sl = mean(Sepal.Length), sw = mean(Sepal.Width), 
  pl = mean(Petal.Length), pw = mean(Petal.Width))
#> # A tibble: 3 × 5
#>   Species       sl    sw    pl    pw
#>   <fct>      <dbl> <dbl> <dbl> <dbl>
#> 1 setosa      5.01  3.43  1.46 0.246
#> 2 versicolor  5.94  2.77  4.26 1.33 
#> 3 virginica   6.59  2.97  5.55 2.03
```

* mean: `mean()` or `mean(x, na.rm = TRUE)` - arithmetic mean (average)
* median: `median()` or `median(x, na.rm = TRUE)` - mid value

---

For more examples see 

[dplr_iris](https://icu-hsuzuki.github.io/da4r2022_note/dplyr-iris.nb.html)


## References of `dplyr`

* Textbook: [R for Data Science, Part II Explore](https://r4ds.had.co.nz/wrangle-intro.html#wrangle-intro)

::: {.block}
### RStudio Primers: See References in Moodle at the bottom

1. The Basics -- [r4ds: Explore, I](https://r4ds.had.co.nz/explore-intro.html#explore-intro)
  - [Visualization Basics](https://rstudio.cloud/learn/primers/1.1)
  - [Programming Basics](https://rstudio.cloud/learn/primers/1.2)
2. **Work with Data** -- [r4ds: Wrangle, I](https://r4ds.had.co.nz/wrangle-intro.html#wrangle-intro)
  - **Working with Tibbles**
  - **Isolating Data with dplyr**
  - **Deriving Information with dplyr**
3. Visualize Data -- [r4ds: Explore, II](https://r4ds.had.co.nz/explore-intro.html#explore-intro)
4. Tidy Your Data -- [r4ds: Wrangle, II](https://r4ds.had.co.nz/wrangle-intro.html#wrangle-intro)
5. Iterate -- [r4ds: Program](https://r4ds.had.co.nz/program-intro.html#program-intro)
6. Write Functions -- [r4ds: Program](https://r4ds.had.co.nz/program-intro.html#program-intro)
::: 

---

## Learn `dplyr` by Examples II - `gapminder`



### `ggplot2` [Overview](https://ggplot2.tidyverse.org)

`ggplot2` is a system for declaratively creating graphics, based on [The Grammar of Graphics](https://amzn.to/2ef1eWp). You provide the data, tell ggplot2 how to map variables to aesthetics, what graphical primitives to use, and it takes care of the details.

**Examples**
```
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy))
```
```
ggplot(data = mpg) + 
  geom_boxplot(mapping = aes(x = class, y = hwy))
```

**Template**
```
ggplot(data = <DATA>) + 
  <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>))
```

---

#### Gapminder and R Package `gapminder`

> Gapminder was founded by Ola Rosling, Anna Rosling Rönnlund, and Hans Rosling

-   Gapminder: <https://www.gapminder.org>

    -   Test on Top: You are probably wrong about - upgrade your worldview
    -   Bubble Chart: <https://www.gapminder.org/tools/#$chart-type=bubbles&url=v1>
    -   Dallar Street: <https://www.gapminder.org/tools/#$chart-type=bubbles&url=v1>
    -   Data: <https://www.gapminder.org/data/>

-   R Package gapminder by Jennifer Bryan

    -   Package site: <https://CRAN.R-project.org/package=gapminder>
    -   Site: <https://github.com/jennybc/gapminder>
    -   Documents: <https://www.rdocumentation.org/packages/gapminder/versions/0.3.0>

-   Package Help `?gapminder` or `gapminder` in the search window of Help

    -   The main data frame gapminder has 1704 rows and 6 variables:
        -   country: factor with 142 levels
        -   continent: factor with 5 levels
        -   year: ranges from 1952 to 2007 in increments of 5 years
        -   lifeExp: life expectancy at birth, in years
        -   pop: population
        -   gdpPercap: GDP per capita (US\$, inflation-adjusted)

---


```r
library(tidyverse)
library(gapminder)
library(WDI)
```

---

#### R Package `gapminder` data


```r
df <- gapminder
df %>% slice(1:10)
#> # A tibble: 10 × 6
#>    country     continent  year lifeExp      pop gdpPercap
#>    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
#>  1 Afghanistan Asia       1952    28.8  8425333      779.
#>  2 Afghanistan Asia       1957    30.3  9240934      821.
#>  3 Afghanistan Asia       1962    32.0 10267083      853.
#>  4 Afghanistan Asia       1967    34.0 11537966      836.
#>  5 Afghanistan Asia       1972    36.1 13079460      740.
#>  6 Afghanistan Asia       1977    38.4 14880372      786.
#>  7 Afghanistan Asia       1982    39.9 12881816      978.
#>  8 Afghanistan Asia       1987    40.8 13867957      852.
#>  9 Afghanistan Asia       1992    41.7 16317921      649.
#> 10 Afghanistan Asia       1997    41.8 22227415      635.
```

---


```r
glimpse(df)
#> Rows: 1,704
#> Columns: 6
#> $ country   <fct> "Afghanistan", "Afghanistan", "Afghanist…
#> $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia…
#> $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982…
#> $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, …
#> $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13…
#> $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, …
```

---


```r
summary(df)
#>         country        continent        year     
#>  Afghanistan:  12   Africa  :624   Min.   :1952  
#>  Albania    :  12   Americas:300   1st Qu.:1966  
#>  Algeria    :  12   Asia    :396   Median :1980  
#>  Angola     :  12   Europe  :360   Mean   :1980  
#>  Argentina  :  12   Oceania : 24   3rd Qu.:1993  
#>  Australia  :  12                  Max.   :2007  
#>  (Other)    :1632                                
#>     lifeExp           pop              gdpPercap       
#>  Min.   :23.60   Min.   :6.001e+04   Min.   :   241.2  
#>  1st Qu.:48.20   1st Qu.:2.794e+06   1st Qu.:  1202.1  
#>  Median :60.71   Median :7.024e+06   Median :  3531.8  
#>  Mean   :59.47   Mean   :2.960e+07   Mean   :  7215.3  
#>  3rd Qu.:70.85   3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
#>  Max.   :82.60   Max.   :1.319e+09   Max.   :113523.1  
#> 
```

---

#### Tidyverse::ggplot

##### First Try - with failures


```r
ggplot(df, aes(x = year, y = lifeExp)) + geom_point()
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-14-1.png)<!-- -->

---


```r
ggplot(df, aes(x = year, y = lifeExp)) + geom_line()
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-15-1.png)<!-- -->

---


```r
ggplot(df, aes(x = year, y = lifeExp)) + geom_boxplot()
#> Warning: Continuous x aesthetic
#> ℹ did you forget `aes(group = ...)`?
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-16-1.png)<!-- -->

---


```r
typeof(pull(df, year)) # same as typeof(df$year)
#> [1] "integer"
```

---


```r
ggplot(df, aes(y = lifeExp, group = year)) + geom_boxplot()
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-18-1.png)<!-- -->

---

##### Box Plot


```r
ggplot(df, aes(x = as_factor(year), y = lifeExp)) + geom_boxplot()
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-19-1.png)<!-- -->

---

#### Applications of `dplyr`

##### `filter`


```r
df %>% filter(country == "Afghanistan") %>%
  ggplot(aes(x = year, y = lifeExp)) + geom_line()
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-20-1.png)<!-- -->

---


```r
df %>% filter(country %in% c("Afghanistan", "Japan")) %>%
  ggplot(aes(x = year, y = lifeExp, color = country)) + geom_line()
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-21-1.png)<!-- -->

---


```r
df %>% distinct(country) %>% pull()
#>   [1] Afghanistan              Albania                 
#>   [3] Algeria                  Angola                  
#>   [5] Argentina                Australia               
#>   [7] Austria                  Bahrain                 
#>   [9] Bangladesh               Belgium                 
#>  [11] Benin                    Bolivia                 
#>  [13] Bosnia and Herzegovina   Botswana                
#>  [15] Brazil                   Bulgaria                
#>  [17] Burkina Faso             Burundi                 
#>  [19] Cambodia                 Cameroon                
#>  [21] Canada                   Central African Republic
#>  [23] Chad                     Chile                   
#>  [25] China                    Colombia                
#>  [27] Comoros                  Congo, Dem. Rep.        
#>  [29] Congo, Rep.              Costa Rica              
#>  [31] Cote d'Ivoire            Croatia                 
#>  [33] Cuba                     Czech Republic          
#>  [35] Denmark                  Djibouti                
#>  [37] Dominican Republic       Ecuador                 
#>  [39] Egypt                    El Salvador             
#>  [41] Equatorial Guinea        Eritrea                 
#>  [43] Ethiopia                 Finland                 
#>  [45] France                   Gabon                   
#>  [47] Gambia                   Germany                 
#>  [49] Ghana                    Greece                  
#>  [51] Guatemala                Guinea                  
#>  [53] Guinea-Bissau            Haiti                   
#>  [55] Honduras                 Hong Kong, China        
#>  [57] Hungary                  Iceland                 
#>  [59] India                    Indonesia               
#>  [61] Iran                     Iraq                    
#>  [63] Ireland                  Israel                  
#>  [65] Italy                    Jamaica                 
#>  [67] Japan                    Jordan                  
#>  [69] Kenya                    Korea, Dem. Rep.        
#>  [71] Korea, Rep.              Kuwait                  
#>  [73] Lebanon                  Lesotho                 
#>  [75] Liberia                  Libya                   
#>  [77] Madagascar               Malawi                  
#>  [79] Malaysia                 Mali                    
#>  [81] Mauritania               Mauritius               
#>  [83] Mexico                   Mongolia                
#>  [85] Montenegro               Morocco                 
#>  [87] Mozambique               Myanmar                 
#>  [89] Namibia                  Nepal                   
#>  [91] Netherlands              New Zealand             
#>  [93] Nicaragua                Niger                   
#>  [95] Nigeria                  Norway                  
#>  [97] Oman                     Pakistan                
#>  [99] Panama                   Paraguay                
#> [101] Peru                     Philippines             
#> [103] Poland                   Portugal                
#> [105] Puerto Rico              Reunion                 
#> [107] Romania                  Rwanda                  
#> [109] Sao Tome and Principe    Saudi Arabia            
#> [111] Senegal                  Serbia                  
#> [113] Sierra Leone             Singapore               
#> [115] Slovak Republic          Slovenia                
#> [117] Somalia                  South Africa            
#> [119] Spain                    Sri Lanka               
#> [121] Sudan                    Swaziland               
#> [123] Sweden                   Switzerland             
#> [125] Syria                    Taiwan                  
#> [127] Tanzania                 Thailand                
#> [129] Togo                     Trinidad and Tobago     
#> [131] Tunisia                  Turkey                  
#> [133] Uganda                   United Kingdom          
#> [135] United States            Uruguay                 
#> [137] Venezuela                Vietnam                 
#> [139] West Bank and Gaza       Yemen, Rep.             
#> [141] Zambia                   Zimbabwe                
#> 142 Levels: Afghanistan Albania Algeria Angola ... Zimbabwe
```

---


```r
df %>% filter(country %in% c("Brazil", "Russia", "India", "China")) %>%
  ggplot(aes(x = year, y = lifeExp, color = country)) + geom_line()
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-23-1.png)<!-- -->

Russian data is missing.

---

### Exercises

1.  Change `lifeExp` to `pop` and `gdpPercap` and do the same.
2.  Choose ASEAN countries and do the similar investigations.

-   Brunei, Cambodia, Indonesia, Laos, Malaysia, Myanmar, Philippines, Singapore.

3.  Choose several countries by yourself and do the similar investigations.

---

### `group_by` and `summarize`

Let us use the variable `continent` and summarize the data.


```r
df_lifeExp <- df %>% group_by(continent, year) %>% 
  summarize(mean_lifeExp = mean(lifeExp), median_lifeExp = median(lifeExp), max_lifeExp = max(lifeExp), min_lifeExp = min(lifeExp), .groups = "keep")
```

---


```r
df_lifeExp %>% slice(1:10)
#> # A tibble: 60 × 6
#> # Groups:   continent, year [60]
#>    continent  year mean_lifeExp median_lif…¹ max_l…² min_l…³
#>    <fct>     <int>        <dbl>        <dbl>   <dbl>   <dbl>
#>  1 Africa     1952         39.1         38.8    52.7    30  
#>  2 Africa     1957         41.3         40.6    58.1    31.6
#>  3 Africa     1962         43.3         42.6    60.2    32.8
#>  4 Africa     1967         45.3         44.7    61.6    34.1
#>  5 Africa     1972         47.5         47.0    64.3    35.4
#>  6 Africa     1977         49.6         49.3    67.1    36.8
#>  7 Africa     1982         51.6         50.8    69.9    38.4
#>  8 Africa     1987         53.3         51.6    71.9    39.9
#>  9 Africa     1992         53.6         52.4    73.6    23.6
#> 10 Africa     1997         53.6         52.8    74.8    36.1
#> # … with 50 more rows, and abbreviated variable names
#> #   ¹​median_lifeExp, ²​max_lifeExp, ³​min_lifeExp
```

---


```r
df %>% filter(year %in% c(1952, 1987, 2007)) %>%
  ggplot(aes(x=as_factor(year), y = lifeExp, fill = continent)) +
  geom_boxplot()
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-26-1.png)<!-- -->

---


```r
df_lifeExp %>% ggplot(aes(x = year, y = mean_lifeExp, color = continent)) +
  geom_line()
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-27-1.png)<!-- -->

---


```r
df_lifeExp %>% ggplot(aes(x = year, y = mean_lifeExp, color = continent, linetype = continent)) +
  geom_line()
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-28-1.png)<!-- -->

---


```r
df_lifeExp %>% ggplot() +
  geom_line(aes(x = year, y = mean_lifeExp, color = continent)) + 
  geom_line(aes(x = year, y = median_lifeExp, linetype = continent))
```

![](05-dplyr_files/figure-epub3/unnamed-chunk-29-1.png)<!-- -->


## The Week Two Assignment (in Moodle)

**R Markdown and `dplyr`**

* Create an R Notebook of a Data Analysis containing the following and submit the rendered HTML file (eg. `a2_123456.nb.html`)
  1. create an R Notebook using the R Notebook Template in Moodle,  save as `a2_123456.Rmd`, 
  2. write your name and ID and the contents, 
  3. run each code block, 
  4. preview to create `a2_123456.nb.html`,
  5. submit  `a2_123456.nb.html` to Moodle.

1. Pick data from the built-in datasets besides `cars`. (`library(help = "datasets")` or go to the site [The R Datasets Package](https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/00Index.html))

    - Information of the data: Name, Description, Usage, Format, Source, References (Hint: ?cars)
    - Use `head()`, `str()`, ..., and create at least one chart using `ggplot2` - Code Chunk.
      + Don't forget to add `library(tidyverse)` in the first code chunk.
    - An observation of the chart - in your own words.

---

2. Load `gapminder` by `library(gapminder)`.

    - Choose `pop` or `gdpPercap`, or both, one country in the data, a group of countries in the data.
    - Create charts using ggplot2 with geom_line and the variables and countries chosen in 1. (See examples of the charts for `lifeExp`.)
    - Study the data as you like.
    - Observations and difficulties encountered.

**Due:** 2023-01-09 23:59:00. Submit your R Notebook file in Moodle (The Second Assignment). Due on Monday!

---

### Original Data? WDI?


```r
gapminder %>% slice(1:10)
#> # A tibble: 10 × 6
#>    country     continent  year lifeExp      pop gdpPercap
#>    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
#>  1 Afghanistan Asia       1952    28.8  8425333      779.
#>  2 Afghanistan Asia       1957    30.3  9240934      821.
#>  3 Afghanistan Asia       1962    32.0 10267083      853.
#>  4 Afghanistan Asia       1967    34.0 11537966      836.
#>  5 Afghanistan Asia       1972    36.1 13079460      740.
#>  6 Afghanistan Asia       1977    38.4 14880372      786.
#>  7 Afghanistan Asia       1982    39.9 12881816      978.
#>  8 Afghanistan Asia       1987    40.8 13867957      852.
#>  9 Afghanistan Asia       1992    41.7 16317921      649.
#> 10 Afghanistan Asia       1997    41.8 22227415      635.
```

---

#### WDI

* SP.DYN.LE00.IN: Life expectancy at birth, total (years)
* NY.GDP.PCAP.KD: GDP per capita (constant 2015 US$)
* SP.POP.TOTL: Population, total


```r
df_wdi <- WDI(
  country = "all", 
  indicator = c(lifeExp = "SP.DYN.LE00.IN", pop = "SP.POP.TOTL", gdpPercap = "NY.GDP.PCAP.KD")
)
```


```
#> Rows: 16492 Columns: 7
#> ── Column specification ────────────────────────────────────
#> Delimiter: ","
#> chr (3): country, iso2c, iso3c
#> dbl (4): year, lifeExp, pop, gdpPercap
#> 
#> ℹ Use `spec()` to retrieve the full column specification for this data.
#> ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

---


```r
df_wdi %>% slice(1:10)
#> # A tibble: 10 × 7
#>    country     iso2c iso3c  year lifeExp      pop gdpPercap
#>    <chr>       <chr> <chr> <dbl>   <dbl>    <dbl>     <dbl>
#>  1 Afghanistan AF    AFG    1960    32.5  8622466        NA
#>  2 Afghanistan AF    AFG    1961    33.1  8790140        NA
#>  3 Afghanistan AF    AFG    1962    33.5  8969047        NA
#>  4 Afghanistan AF    AFG    1963    34.0  9157465        NA
#>  5 Afghanistan AF    AFG    1964    34.5  9355514        NA
#>  6 Afghanistan AF    AFG    1965    35.0  9565147        NA
#>  7 Afghanistan AF    AFG    1966    35.5  9783147        NA
#>  8 Afghanistan AF    AFG    1967    35.9 10010030        NA
#>  9 Afghanistan AF    AFG    1968    36.4 10247780        NA
#> 10 Afghanistan AF    AFG    1969    36.9 10494489        NA
```

---


```r
df_wdi_extra <- WDI(
  country = "all", 
  indicator = c(lifeExp = "SP.DYN.LE00.IN", pop = "SP.POP.TOTL", gdpPercap = "NY.GDP.PCAP.KD"), 
  extra = TRUE
)
```


```
#> Rows: 16492 Columns: 15
#> ── Column specification ────────────────────────────────────
#> Delimiter: ","
#> chr  (7): country, iso2c, iso3c, region, capital, income...
#> dbl  (6): year, lifeExp, pop, gdpPercap, longitude, lati...
#> lgl  (1): status
#> date (1): lastupdated
#> 
#> ℹ Use `spec()` to retrieve the full column specification for this data.
#> ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

---


```r
df_wdi_extra
#> # A tibble: 16,492 × 15
#>    country     iso2c iso3c  year status lastupdated lifeExp
#>    <chr>       <chr> <chr> <dbl> <lgl>  <date>        <dbl>
#>  1 Afghanistan AF    AFG    1993 NA     2022-12-22     51.5
#>  2 Afghanistan AF    AFG    1997 NA     2022-12-22     53.6
#>  3 Afghanistan AF    AFG    1994 NA     2022-12-22     51.5
#>  4 Afghanistan AF    AFG    1995 NA     2022-12-22     52.5
#>  5 Afghanistan AF    AFG    2001 NA     2022-12-22     55.8
#>  6 Afghanistan AF    AFG    1998 NA     2022-12-22     52.9
#>  7 Afghanistan AF    AFG    1999 NA     2022-12-22     54.8
#>  8 Afghanistan AF    AFG    2007 NA     2022-12-22     59.1
#>  9 Afghanistan AF    AFG    2008 NA     2022-12-22     59.9
#> 10 Afghanistan AF    AFG    1980 NA     2022-12-22     39.6
#> # … with 16,482 more rows, and 8 more variables: pop <dbl>,
#> #   gdpPercap <dbl>, region <chr>, capital <chr>,
#> #   longitude <dbl>, latitude <dbl>, income <chr>,
#> #   lending <chr>
```

