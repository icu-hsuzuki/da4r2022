# R with R Studio

---

### Course Contents  {-}

  1. 2022-12-07: Introduction: About the course　[lead by TK]
    - An introduction to open and public data, and data science
  2. **2022-12-14: Exploratory Data Analysis (EDA) 1 [lead by hs]  
    - R Basics with RStudio and/or RStudio.cloud; Toy Data**
  3. 2022-12-21: Exploratory Data Analysis (EDA) 2 [lead by hs]   
    - R Markdown; Introduction to `tidyverse` I; Public Data, WDI
  4. 2023-01-11: Exploratory Data Analysis (EDA) 3 [lead by hs]  
    - Introduction to `tidyverse`II; WDI, WIR, etc
  5. 2023-01-18: Exploratory Data Analysis (EDA) 4 [lead by hs]  
    - Introduction to `tidyverse` III; WDI, WIR, etc
  6. 2023-01-25: Exploratory Data Analysis (EDA) 5 [lead by hs]  
    - Introduction to `tidyverse` III; WDI, WIR, etc
  7. 2023-02-01: Introduction to PPDAC (Problem-Plan-Data-Analysis-Conclusion) Cycle: [lead by TK]
  8. 2023-02-08: Model building I [lead by TK]
    -Collecting and visualizing data and Introduction to WDI  
         (World Development Indicators by World Bank)
  9. 2023-02-15: Model building II [lead by TK]
    -Analyzing data and communications
  10. 2023-02-22: Project Presentation

---

## Learning Resources

### Textbooks and References

* "R for Data Science" by Hadley Wickham and Garrett Grolemund: 
  - Free Online Book: https://r4ds.had.co.nz

* Visit `bookdown` site: https://bookdown.org 
  - Many more on the [archive page](https://bookdown.org/home/archive/).


## Interactive Exercises

* Posit Primers:https://posit.cloud/learn/primers:  
  - The Basics, Work with Data, Visualize Data, Tidy Your Data, Report Reproducibly

* {swirl} Learn R, in R: https://swirlstats.com
  - Designed and developed by a team at Johns Hopkins University for `coursera` courses

---

## Posit Primers created by `learnr`

* [`learnr` Interactive Tutorials for R](https://rstudio.github.io/learnr/index.html)

::: {.block}
### Posit Primers https://posit.cloud/learn/primers

1. The Basics -- [r4ds: Explore, I](https://r4ds.had.co.nz/explore-intro.html#explore-intro)
  - [Visualization Basics](https://rstudio.cloud/learn/primers/1.1)
  - [Programming Basics](https://rstudio.cloud/learn/primers/1.2)
2. Work with Data -- [r4ds: Wrangle, I](https://r4ds.had.co.nz/wrangle-intro.html#wrangle-intro)
  - Working with Tibbles
  - Isolating Data with dplyr
  - Deriving Information with dplyr
3. Visualize Data -- [r4ds: Explore, II](https://r4ds.had.co.nz/explore-intro.html#explore-intro)
4. Tidy Your Data -- [r4ds: Wrangle, II](https://r4ds.had.co.nz/wrangle-intro.html#wrangle-intro)
5. Iterate -- [r4ds: Program](https://r4ds.had.co.nz/program-intro.html#program-intro)
6. Write Functions -- [r4ds: Program](https://r4ds.had.co.nz/program-intro.html#program-intro)
:::
---

## Data Science and EDA

### Wikipedia https://en.wikipedia.org/wiki/Data_science

> An inter-disciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from many structural and unstructured data.

* Create Insights
* Impact Decision Making
* Maintain & Improve Overtime

---

## What is R?

### R (programming language), [Wikipedia](https://en.wikipedia.org/wiki/R_(programming_language))

* **R is a programming language** and **free software** environment for **statistical computing and graphics** supported by the R Foundation for Statistical Computing. 

* The R language is widely used among statisticians and data miners for developing statistical software and data analysis.

* A **GNU package**, the official R software environment is written primarily in C, Fortran, and R itself (thus, it is partially self-hosting) and is freely available under the GNU General Public License. 

---

### History of R and more

"R Programming for Data Science" by Roger Peng

* [Chapter 2. History and Overview of R](https://bookdown.org/rdpeng/rprogdatascience/history-and-overview-of-r.html)
* [Overview and History of R: Youtube video](https://www.youtube.com/watch?v=STihTnVSZnI&feature=youtu.be)

---

## Why R? -- Responses by Hadley Wickham

### [r4ds](https://r4ds.had.co.nz/introduction.html#python-julia-and-friends): R is a great place to start your data science journey because

* R is an environment designed from the ground up to support data science. 
* R is not just a programming language, but it is also an interactive environment for doing data science. 
* To support interaction, R is a much more flexible language than many of its peers. 

---

### Why R today?

When you talk about choosing programming languages, I always say you shouldn’t pick them based on technical merits, but rather pick them based on the community. And I think the R community is like really, really strong, vibrant, free, welcoming, and embraces a wide range of domains. So, if there are like people like you using R, then your life is going to be much easier. That’s the first reason. 

**Interview**: ["Advice to Young (and Old) Programmers, H. Wickham"](https://www.r-bloggers.com/2018/08/advice-to-young-and-old-programmers-a-conversation-with-hadley-wickham/)

---

## What is RStudio? https://posit.com

> RStudio is an integrated development environment, or IDE, for R programming. 

### R Studio (Wikipedia)

RStudio is an integrated development environment (IDE) for R, a programming language for statistical computing and graphics. It is available in two formats: RStudio Desktop is a regular desktop application while RStudio Server runs on a remote server and allows accessing RStudio using a web browser.

---

## Installation of R and R Studio

### R Installation

To download R, go to CRAN, the comprehensive R archive network. CRAN is composed of a set of mirror servers distributed around the world and is used to distribute R and R packages. Don’t try and pick a mirror that’s close to you: instead use the cloud mirror, https://cloud.r-project.org, which automatically figures it out for you.

A new major version of R comes out once a year, and there are 2-3 minor releases each year. It’s a good idea to update regularly.

### R Studio Installation

Download and install it from http://www.rstudio.com/download. 

RStudio is updated a couple of times a year. When a new version is available, RStudio will let you know.

---

## R Studio

### The First Step
1. Start R Studio Application
2. Top Menu: File > New Project > New Directory > New Project > _Directory name or Browse the directory and choose the parent directory you want to create the directory_

### When You Start the Project
1. Go to the directory you created
2. Double click _'Directory Name'.Rproj
  
Or,

1. Start R Studio
2. File > Open Project (or choose from Recent Project)

_In this way the working directory of the session is set to the project directory and R can search releted files without difficulty_ (`getwd()`, `setwd()`)

---

## Posit Cloud

RStudio Cloud is a lightweight, cloud-based solution that allows anyone to do, share, teach and learn data science online.

### Cloud Free

* Up to 15 projects total
* 1 shared space (5 members and 10 projects max)
* 15 project hours per month
* Up to 1 GB RAM per project
* Up to 1 CPU per project
* Up to 1 hour background execution time

---

### How to Start Posit Cloud

1. Go to https://posit.cloud/
2. Sign Up: _top right_
  - Email address or Google account
3. New Project: _Project Name_
4. R Console


## Let's Get Started 

Start RStudio and create a project, or login to Posit Cloud and create a project.

---

### The First Examples

Input the following codes into Console in the left bottom pane.

* The first two:


```r
head(cars)
#>   speed dist
#> 1     4    2
#> 2     4   10
#> 3     7    4
#> 4     7   22
#> 5     8   16
#> 6     9   10
```

---


```r
str(cars)
#> 'data.frame':	50 obs. of  2 variables:
#>  $ speed: num  4 4 7 7 8 9 10 10 10 11 ...
#>  $ dist : num  2 10 4 22 16 10 18 26 34 17 ...
```

---

* Two more:


```r
summary(cars)
#>      speed           dist       
#>  Min.   : 4.0   Min.   :  2.00  
#>  1st Qu.:12.0   1st Qu.: 26.00  
#>  Median :15.0   Median : 36.00  
#>  Mean   :15.4   Mean   : 42.98  
#>  3rd Qu.:19.0   3rd Qu.: 56.00  
#>  Max.   :25.0   Max.   :120.00
```

---


```r
plot(cars)
```

![](02-R_and_RStudio_files/figure-epub3/cars_plot-1.png)<!-- -->

---

* And three more:


```r
plot(cars) # cars: Speed and Stopping Distances of Cars
abline(lm(cars$dist~cars$speed))
```
![](02-R_and_RStudio_files/figure-epub3/unnamed-chunk-6-1.png)<!-- -->

---


```r
lm(cars$dist~cars$speed)
#> 
#> Call:
#> lm(formula = cars$dist ~ cars$speed)
#> 
#> Coefficients:
#> (Intercept)   cars$speed  
#>     -17.579        3.932
```

---


```r
summary(lm(cars$dist~cars$speed))
#> 
#> Call:
#> lm(formula = cars$dist ~ cars$speed)
#> 
#> Residuals:
#>     Min      1Q  Median      3Q     Max 
#> -29.069  -9.525  -2.272   9.215  43.201 
#> 
#> Coefficients:
#>             Estimate Std. Error t value Pr(>|t|)    
#> (Intercept) -17.5791     6.7584  -2.601   0.0123 *  
#> cars$speed    3.9324     0.4155   9.464 1.49e-12 ***
#> ---
#> Signif. codes:  
#> 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> Residual standard error: 15.38 on 48 degrees of freedom
#> Multiple R-squared:  0.6511,	Adjusted R-squared:  0.6438 
#> F-statistic: 89.57 on 1 and 48 DF,  p-value: 1.49e-12
```

---

#### Brief Explanation

* `head(cars)`: The first 6 rows of the pre-installed data `cars`.
* `str(cars)`: The data structure of the pre-installed data `cars`.
* `summary(cars)`: The summary of the pre-installed data `cars`.
* `plot(cars)`: A scatter plot of the pre-installed data `cars`.
  - `plot(cars$dist~cars$speed)`
  - `cars$dist`, `cars$[[2]]`, `cars[,2]` are same
* `abline(lm(cars$dist~cars$speed))`: Add a regression line of a linear model
* `lm(cars$dist~cars$speed)`: The equation of the regression line
* `summary(lm(cars$dist~cars$speed)`: The summary of the linear regression model

---


```r
hist(cars$dist)
```
![](02-R_and_RStudio_files/figure-epub3/unnamed-chunk-10-1.png)<!-- -->

---


```r
hist(cars$speed)
```
![](02-R_and_RStudio_files/figure-epub3/unnamed-chunk-12-1.png)<!-- -->

---

#### View and help

* `View(cars)`
* `?cars`: same as `help(cars)`
* `??cars`: same as `help.search("cars")

#### `datasets`

* `?datasets`
* `library(help = "datasets")`

* `data()` shows all data already attached and available.

---

### Practicum

Pick a data in the datasets package and try

* `head()`
* `str()`
* `summary()`

and some more.

---

### `iris`


```r
head(iris)
#>   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
#> 1          5.1         3.5          1.4         0.2  setosa
#> 2          4.9         3.0          1.4         0.2  setosa
#> 3          4.7         3.2          1.3         0.2  setosa
#> 4          4.6         3.1          1.5         0.2  setosa
#> 5          5.0         3.6          1.4         0.2  setosa
#> 6          5.4         3.9          1.7         0.4  setosa
```

---


```r
str(iris)
#> 'data.frame':	150 obs. of  5 variables:
#>  $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
#>  $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
#>  $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
#>  $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
#>  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
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

Can you plot?


```r
plot(iris$Sepal.Length, iris$Sepal.Width)
```
![](02-R_and_RStudio_files/figure-epub3/unnamed-chunk-17-1.png)<!-- -->

