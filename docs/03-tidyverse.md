# `tidyverse` Packages {#tidyverse}

### Brief Introduction to R on RStudio

#### Four Panes and Tabs

1. Top Left: Source Editor
2. Top Right: Environment, History, etc.
3. Bottom Left: Console, Terminal, Render, Background Jobs
4. Bottom Right: Files, Plots, Packages, Help, Viewer, Presentation

---

#### Set up

* Highly recommend to set the language to be "English".
* Create "data" directory.


```r
Sys.setenv(LANG = "en")
dir.create("data")
```

---

#### Three Ways to Run Codes

1. Console - Bottom Left Pane
2. R Script - pull down menu under File
3. R Notebook, R Markdown - pull down menu under File

---

### Second Way: R Script

#### Examples: R Scripts in Moodle

* `basics.R`
* `coronavirus.R`

1. Copy a script in Moodle: _{file name}.R_
2. In RStudio (create Project in RStudio) choose File > New File > R Script and paste it.
3. Choose File > Save with a name; e.g. _{file names}_ (.R will be added automatically)

To run a code: at the cursor press *Ctrl+Shift+Enter* (Win) or *Cmd+Shift+Enter* (Mac). 

---

### Packages

R packages are extensions to the R statistical programming language. R packages contain code, data, and documentation in a standardised collection format that can be installed by users of R, typically via a centralised software repository such as CRAN (the Comprehensive R Archive Network).

#### Installation and attachement

You can install packages by "Install Packages..." under "Tool" in the top menu.

* `install.packages("tidyverse")`
* `install.packages("rmarkdown")`

---

### Third Way: R Notebook

Choose R Notebook from the pull down File menu in the top bar.

### Edit YAML

**Default* is as follows**

```
---
title: "R Notebook"
output: html_notebook
---
```

---

**Template**

```
---
title: "Title of R Notebook"
author: "ID and Your Name"
date: "2023-02-07" 
output: 
  html_notebook:
#    number_sections: yes
#    toc: true
#    toc_float: true
---
```

* Don't change the format. Indention matters.
* The statement after \# is ignored.
* Date is automatically inserted when you compile the file.
* You can replace "2023-02-07" by "2022-12-14" or in any date format surrounded by double quotation marks.
* Section numbers: - default is `number_sections: no`.
* Table of contents, `toc: true` - default is `toc: false`.
* Floating table of contents in HTML output, `toc_float: true` - default is `toc_float: false`

---

### Create a Code Chunk to Attach Packages

Insert Chunk in Code pull down menu in the top bar, or use the <kbd>C</kbd> button on top. You can use shortcut keys listed under Tools in the top bar.


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

## First Example

### Importing data

Let us assign the `iris` data in the pre-installed package `datasets` to `df_iris`. You can give any name starting from an alphabet, though there are some rules. 


```r
df_iris <- datasets::iris
class(df_iris)
#> [1] "data.frame"
```

The class of data `iris` is `data.frame`, the basic data class of R. You can assign the same data as a `tibble`, the data class of `tidyverse` as follows.


```r
tbl_iris <- as_tibble(datasets::iris)
class(tbl_iris)
#> [1] "tbl_df"     "tbl"        "data.frame"
```

* `df_iris <- iris` can replace  `df_iris <- datasets::iris` because the package `datasets` is installed and attached as default. Since you may have other data called `iris` included in a different package or you may have changed `iris` before, it is safer to specify the name of the package with the name of the data.
* Within R Notebook or in Console, you may get different output, and `tf_iris` and `tbl_iris` behave differently. It is because of the default settings of R Markdown. 

---

### Look at the data

#### Several ways to view the data.

The `View` command open up a window to show the contents of the data and you can use the filter as well.


```r
View(df_iris)
```

---

The following simple command also shows the data. 

```r
df_iris
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

The output within R Notebook is a tibble style. Try the same command in Console.

---


```r
slice(df_iris, 1:10)
#>    Sepal.Length Sepal.Width Petal.Length Petal.Width
#> 1           5.1         3.5          1.4         0.2
#> 2           4.9         3.0          1.4         0.2
#> 3           4.7         3.2          1.3         0.2
#> 4           4.6         3.1          1.5         0.2
#> 5           5.0         3.6          1.4         0.2
#> 6           5.4         3.9          1.7         0.4
#> 7           4.6         3.4          1.4         0.3
#> 8           5.0         3.4          1.5         0.2
#> 9           4.4         2.9          1.4         0.2
#> 10          4.9         3.1          1.5         0.1
#>    Species
#> 1   setosa
#> 2   setosa
#> 3   setosa
#> 4   setosa
#> 5   setosa
#> 6   setosa
#> 7   setosa
#> 8   setosa
#> 9   setosa
#> 10  setosa
```



```r
1:10
#>  [1]  1  2  3  4  5  6  7  8  9 10
```
`
---

#### Data Structure

Let us look at the structure of the data. You can try `str(df_iris)` on Console or by adding a code chunk in R Notebook introducing later.


```r
glimpse(df_iris)
#> Rows: 150
#> Columns: 5
#> $ Sepal.Length <dbl> 5.1, 4.9, 4.7, 4.6, 5.0, 5.4, 4.6, 5.…
#> $ Sepal.Width  <dbl> 3.5, 3.0, 3.2, 3.1, 3.6, 3.9, 3.4, 3.…
#> $ Petal.Length <dbl> 1.4, 1.4, 1.3, 1.5, 1.4, 1.7, 1.4, 1.…
#> $ Petal.Width  <dbl> 0.2, 0.2, 0.2, 0.2, 0.2, 0.4, 0.3, 0.…
#> $ Species      <fct> setosa, setosa, setosa, setosa, setos…
```

There are six types of data in R; Double, Integer, Character, Logical, Raw, Complex.

The names after $ are column names. If you call `df_iris$Species`, you have the Species column. Species is in the 5th collumn, `typeof(df_iris[[5]])` does the same as the next. 

`df_iris[2,4] = `0.2 is the fourth entry of Sepal.Width.

---


```r
typeof(df_iris$Species)
#> [1] "integer"
```


```r
class(df_iris$Species)
#> [1] "factor"
```

For `factors = fct` see [the R Document](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/factor) or an explanation in [Factor in R: Categorical Variable & Continuous Variables](https://www.guru99.com/r-factor-categorical-continuous.html).

---


```r
typeof(df_iris$Sepal.Length)
#> [1] "double"
class(df_iris$Sepal.Length)
#> [1] "numeric"
```


**Q1.** What are the differences of`df_iris`, `slice(df_iris, 1:10)` and `glimpse(df_iris)` above?

**Q2.** What are the differences of`df_iris`, `slice(df_iris, 1:10)` and `glimpse(df_iris)` in the console?

---

#### Summary of the Data

The following is very convenient to get the summary information of a data.


```r
summary(df_iris)
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

Minimum, 1st Quadrant (25%),  Median, Mean, 3rd Quadrant (75%), Maximum, and the count of each factor.

---

### Visualizing Data

#### Scatter Plot

We use `ggplot` to draw graphs. The scatter plot is a projection of data with two variables $x$ and $y$. 
```
ggplot(data = <data>, aes(x = <column name for x>, y = <column name for y>)) +
  geom_point()
```
```
ggplot(data = df_iris, aes(x = Sepal.Length, y = Sepal.Width)) +
  geom_point()
```

---



```r
ggplot(data = df_iris, aes(x = Sepal.Length, y = Sepal.Width)) +
  geom_point()
```

![](03-tidyverse_files/figure-epub3/unnamed-chunk-9-1.png)<!-- -->

---

#### Scatter Plot with [Labels](https://ggplot2.tidyverse.org/reference/labs.html)

Add title and labels adding `labs()`. 

```
ggplot(data = <data>, aes(x = <column name for x>, y = <column name for y>)) +
  geom_point() +
  labs(title = "Title", x = "Label for x", y = "Label for y")
```
---


```r
ggplot(data = df_iris, aes(x = Sepal.Length, y = Sepal.Width)) +
  geom_point() + 
  labs(title = "Scatter Plot of Sepal Data of Iris", x = "Sepal Length", y = "Sepal Width")
```

![](03-tidyverse_files/figure-epub3/unnamed-chunk-10-1.png)<!-- -->

---

#### Scatter Plot with [Colors](https://ggplot2.tidyverse.org/reference/aes_colour_fill_alpha.html)

Add different colors automatically to each species. Can you see each group?


```r
ggplot(data = df_iris, aes(x = Sepal.Length, y = Sepal.Width, color = Species)) +
  geom_point()
```

![](03-tidyverse_files/figure-epub3/unnamed-chunk-11-1.png)<!-- -->

---

#### Scatter Plot with Shapes


```r
ggplot(data = df_iris, aes(x = Sepal.Length, y = Sepal.Width, shape = Species)) +
  geom_point()
```

![](03-tidyverse_files/figure-epub3/unnamed-chunk-12-1.png)<!-- -->

---

#### [Boxplot](https://ggplot2.tidyverse.org/reference/geom_boxplot.html)

The boxplot compactly displays the distribution of a continuous variable. 


```r
ggplot(data = df_iris, aes(x = Species, y = Sepal.Length)) +
  geom_boxplot()
```

![](03-tidyverse_files/figure-epub3/unnamed-chunk-13-1.png)<!-- -->

---

#### [Histogram](https://ggplot2.tidyverse.org/reference/geom_histogram.html)

Visualize the distribution of a single continuous variable by dividing the x axis into bins and counting the number of observations in each bin. Histograms (geom_histogram()) display the counts with bars. 


```r
ggplot(data = df_iris, aes(x = Sepal.Length)) +
  geom_histogram()
```

![](03-tidyverse_files/figure-epub3/unnamed-chunk-14-1.png)<!-- -->

---

Change the number of bins by `bins =` `<number>`.


```r
ggplot(data = df_iris, aes(x = Sepal.Length)) +
  geom_histogram(bins = 10)
```

![](03-tidyverse_files/figure-epub3/unnamed-chunk-15-1.png)<!-- -->

---

### Data Modeling 

Professor Kaizoji will cover the mathematical models and hypothesis testings. 


```r
ggplot(data = df_iris, aes(x = Sepal.Length, y = Sepal.Width)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE)
```

![](03-tidyverse_files/figure-epub3/unnamed-chunk-16-1.png)<!-- -->

## Comments on Week 2

#### Helpful Resources

* Cheat Sheet in RStudio: https://www.rstudio.com/resources/cheatsheets/  
  - [RStudio IED](https://raw.githubusercontent.com/rstudio/cheatsheets/main/rstudio-ide.pdf)
  - [Base R Cheat Sheet](https://github.com/rstudio/cheatsheets/raw/main/base-r.pdf)
* 'Quick R' by DataCamp: https://www.statmethods.net/management

* [An Introduction to R](https://cran.rstudio.com)


:::{.alertblock}
#### Practicum

* Posit Primers: The Basics: https://posit.cloud/learn/primers/1
  - Complete Visualization Basics and Programming Basics

---

#### Assignments - See Moodle

1. Assignment Week 2-1: Introduction Plus Forum  
  - Due: Tuesday, 20 December 2022, 11:59 PM

2. Assignment Week 2-2: Quiz 1 on R Basics 
  - Due: Tuesday, 20 December 2022, 11:59 PM

:::


## Swirl: An interactive learning environment for R and statistics

* {`swirl`} website: https://swirlstats.com
* JHU Data Science in coursera uses `swirl` for exercises.

### Swirl Courses

1. R Programming: The basics of programming in R
2. Regression Models: The basics of regression modeling in R
3. Statistical Inference: The basics of statistical inference in R
4. Exploratory Data Analysis: The basics of exploring data in R

You can install other `swirl` courses as well

* [Swirl Courses Organized by Title](http://swirlstats.com/scn/title.html)
* [Swirl Courses Organized by Author’s Name](http://swirlstats.com/scn/surname.html)
* [Github: swirl courses](https://github.com/swirldev/swirl_courses#swirl-courses) 
  - `install_course("Course Name Here")`

---

### Install and Start Swirl Courses

#### Three Steps to Start Swirl

```
install.packages("swirl") # Only the first time.
library(swirl) # Everytime you start swirl
swirl() # Everytime you start or resume swirl
```

#### R Programming: The basics of programming in R

\scriptsize
```
 1: Basic Building Blocks      2: Workspace and Files     3: Sequences of Numbers    
 4: Vectors                    5: Missing Values          6: Subsetting Vectors      
 7: Matrices and Data Frames   8: Logic                   9: Functions               
10: lapply and sapply         11: vapply and tapply      12: Looking at Data         
13: Simulation                14: Dates and Times        15: Base Graphics          
```


#### Recommended Sections in Order

1, 3, 4, 5, 6, 7, 12, 15, 14, 8, 9, 10, 11, 13, 2

* Section 2 discusses the directories and file systems of a computer
* Sections 9, 10, 11 are for programming

---

#### Controling a `swirl` Session

* ...  <-- That's your cue to press Enter to continue

  
* You can exit swirl and return to the R prompt (>) at any time by pressing the Esc key.

* If you are already at the prompt, type bye() to exit and save your progress. When you exit properly, you'll see a short message letting you know you've done so.

When you are at the R prompt (>):

1. Typing skip() allows you to skip the current question.
2. Typing play() lets you experiment with R on your own; swirl will ignore what you do...
3. UNTIL you type nxt() which will regain swirl's attention.
4. Typing bye() causes swirl to exit. Your progress will be saved.
5. Typing main() returns you to swirl's main menu.
6. Typing info() displays these options again.

---

#### Final Remark

You will encounter the message like ‘Would you like to receive credit for completing this course on Coursera.org?’ at the end of each course. This is for `coursera` courses. Select 'NO'. 



## More on R Script: Examples

### R Scripts in Moodle

* basics.R
* coronavirus.R

1. Copy a script in Moodle: _{file name}.R_
2. In RStudio (Workspace in RStudio.cloud, Project in RStudio) choose File > New File > R Script and paste it.
3. Choose File > Save with a name; e.g. _{file names}_ (.R will be added automatically)

---

### `basics.R`

The script with the outputs.


```r
#################
#
# basics.R
#
################
# 'Quick R' by DataCamp may be a handy reference: 
#     https://www.statmethods.net/management/index.html
# Cheat Sheet at RStudio: https://www.rstudio.com/resources/cheatsheets/
# Base R Cheat Sheet: https://github.com/rstudio/cheatsheets/raw/main/base-r.pdf
# To execute the line: Control + Enter (Window and Linux), Command + Enter (Mac)
## try your experiments on the console

## calculator

3 + 7

### +, -, *, /, ^ (or **), %%, %/%

3 + 10 / 2

3^2

2^3

2*2*2

### assignment: <-, (=, ->, assign()) 

x <- 5

x 

#### object_name <- value, '<-' shortcut: Alt (option) + '-' (hyphen or minus) 
#### Object names must start with a letter and can only contain letter, numbers, _ and .

this_is_a_long_name <- 5^3

this_is_a_long_name

char_name <- "What is your name?"

char_name

#### Use 'tab completion' and 'up arrow'

### ls(): list of all assignments

ls()
ls.str()

#### check Environment in the upper right pane

### (atomic) vectors

5:10

a <- seq(5,10)

a

b <- 5:10

identical(a,b)

seq(5,10,2) # same as seq(from = 5, to = 10, by = 2)

c1 <- seq(0,100, by = 10)

c2 <- seq(0,100, length.out = 10)

c1

c2

length(c1)

#### ? seq   ? length   ? identical

(die <- 1:6)

zero_one <- c(0,1) # same as 0:1

die + zero_one # c(1,2,3,4,5,6) + c(0,1). re-use

d1 <- rep(1:3,2) # repeat


d1

die == d1

d2 <- as.character(die == d1)

d2

d3 <- as.numeric(die == d1)

d3

### class() for class and typeof() for mode
### class of vectors: numeric, charcters, logical
### types of vectors: doubles, integers, characters, logicals (complex and raw)

typeof(d1); class(d1)

typeof(d2); class(d2)

typeof(d3); class(d3)

sqrt(2)

sqrt(2)^2

sqrt(2)^2 - 2

typeof(sqrt(2))

typeof(2)

typeof(2L)

5 == c(5)

length(5)

### Subsetting

(A_Z <- LETTERS)

A_F <- A_Z[1:6]

A_F

A_F[3]

A_F[c(3,5)]

large <- die > 3

large

even <- die %in% c(2,4,6)

even

A_F[large]

A_F[even]

A_F[die < 4]

### Compare df with df1 <- data.frame(number = die, alphabet = A_F)
df <- data.frame(number = die, alphabet = A_F, stringsAsFactors = FALSE)

df

df$number

df$alphabet

df[3,2]

df[4,1]

df[1]

class(df[1])

class(df[[1]])

identical(df[[1]], die)

identical(df[1],die)

####################
# The First Example
####################

plot(cars)

# Help

? cars

# cars is in the 'datasets' package

data()

# help(cars) does the same as ? cars
# You can use Help tab in the right bottom pane

help(plot)
? par

head(cars)

str(cars)

summary(cars)

x <- cars$speed
y <- cars$dist

min(x)
mean(x)
quantile(x)

plot(cars)

abline(lm(cars$dist ~ cars$speed))

summary(lm(cars$dist ~ cars$speed))

boxplot(cars)

hist(cars$speed)
hist(cars$dist)
hist(cars$dist, breaks = seq(0,120, 10))
```

---

### coronavirus.R

The script and its outputs.
__coronavirus.csv__ is very large


```r
# https://coronavirus.jhu.edu/map.html
# JHU Covid-19 global time series data
# See R pakage coronavirus at: https://github.com/RamiKrispin/coronavirus
# Data taken from: https://github.com/RamiKrispin/coronavirus/tree/master/csv
# Last Updated
Sys.Date()

## Download and read csv (comma separated value) file
coronavirus <- read.csv("https://github.com/RamiKrispin/coronavirus/raw/master/csv/coronavirus.csv")
# write.csv(coronavirus, "data/coronavirus.csv")

## Summaries and structures of the data
head(coronavirus)
str(coronavirus)
coronavirus$date <- as.Date(coronavirus$date)
str(coronavirus)

range(coronavirus$date)
unique(coronavirus$country)
unique(coronavirus$type)

## Set Country
COUNTRY <- "Japan"
df0 <- coronavirus[coronavirus$country == COUNTRY,]
head(df0)
tail(df0)
(pop <- df0$population[1])
df <- df0[c(1,6,7,13)]
str(df)
head(df)
### alternatively,
head(df0[c("date", "type", "cases", "population")])
###

## Set types
df_confirmed <- df[df$type == "confirmed",]
df_death <- df[df$type == "death",]
df_recovery <- df[df$data_type == "recovery",]
head(df_confirmed)
head(df_death)
head(df_recovery)

## Histogram
plot(df_confirmed$date, df_confirmed$cases, type = "h")
plot(df_death$date, df_death$cases, type = "h")
# plot(df_recovered$date, df_recovered$cases, type = "h") # no data for recovery

## Scatter plot and correlation
plot(df_confirmed$cases, df_death$cases, type = "p")
cor(df_confirmed$cases, df_death$cases)


## In addition set a period
start_date <- as.Date("2021-07-01")
end_date <- Sys.Date() 
df_date <- df[df$date >=start_date & df$date <= end_date,]
##

## Set types
df_date_confirmed <- df_date[df_date$type == "confirmed",]
df_date_death <- df_date[df_date$type == "death",]
df_date_recovery <- df_date[df_date$data_type == "recovery",]
head(df_date_confirmed)
head(df_date_death)
head(df_date_recovery)

## Histogram
plot(df_date_confirmed$date, df_date_confirmed$cases, type = "h")
plot(df_date_death$date, df_date_death$cases, type = "h")
# plot(df_date_recovered$date, df_date_recovered$cases, type = "h") # no data for recovery

plot(df_date_confirmed$cases, df_date_death$cases, type = "p")
cor(df_date_confirmed$cases, df_date_death$cases)

### Q0. Change the values of the location and the period and see the outcomes.
### Q1. What is the correlation between df_confirmed$cases and df_death$cases?
### Q2. Do we have a larger correlation value if we shift the dates to implement the time-lag?
### Q3. Do you have any other questions to explore?

#### Extra
plot(df_confirmed$date, df_confirmed$cases, type = "h", 
     main = paste("Comfirmed Cases in",COUNTRY), 
     xlab = "Date", ylab = "Number of Cases")
```

:::

## `gapminder` Package

### Hans Rosling (1948 – 2017)

> Hans Rosling was a Swedish physician, academic, and public speaker. He was a professor of international health at Karolinska Institute[4] and was the co-founder and chairman of the Gapminder Foundation, which developed the Trendalyzer software system. ([wikipedia](https://en.wikipedia.org/wiki/Hans_Rosling))

* Books: 
  - Factfulness: Ten Reasons We're Wrong About The World - And Why Things Are Better Than You Think, 2018
  - How I Learned to Understand the World: A Memoir, 2020
* Gapminder: https://www.gapminder.org
  - [You are probably wrong about: Upgrade Your World View](https://upgrader.gapminder.org)
  - [Bubble Chart](https://www.gapminder.org/tools/#$state$time$value=2020;;&chart-type=bubbles): Income vs Life Expectancy over time, 1800 - 2020
    + How many variables?
* Videos: [The best stats you’ve ever seen, Hans Rosling](http://www.edtech.events/the-best-stats-youve-ever-seen-hans-rosling/)

---

### Factfulness is ... \hfill _From the book_

recognizing when a decision feels urgent and remembering that it rarely is.

To control the urgency instinct, take small steps.

* Take a breath. When your urgency instinct is triggered, your other instincts kick in and your analysis shuts down. Ask for more time and more information. It’s rarely now or never and it’s rarely either/or.
* Insist on the data. If something is urgent and important, it should be measured. Beware of data that is relevant but inaccurate, or accurate but irrelevant. Only relevant and accurate data is useful.
* Beware of fortune-tellers. Any prediction about the future is uncertain. Be wary of predictions that fail to acknowledge that. Insist on a full range of scenarios, never just the best or worst case. Ask how often such predictions have been right before.
* Be wary of drastic action. Ask what the side effects will be. Ask how the idea has been tested. Step-by-step practical improvements, and evaluation of their impact, are less dramatic but usually more effective.

---


```r
# install.packages("gapminder")
library(gapminder)
```

```r
df <- gapminder
df
#> # A tibble: 1,704 × 6
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
#> # … with 1,694 more rows
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

### Questions

* List questions based on this data.
* What do you want to see? 
* What kind of chart do you want to construct?

## Review {-}

* R on R Studio/Posit Cloud (RStudio Cloud)
* Three ways to run codes
  1. Console
  2. R Script
  3. Code Chunk in R Notebook
* Packages
  1. `tidyverse`
  2. `rmarkdown`
  3. `gapminder`
