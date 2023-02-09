# `tidyverse`  {#tidyverse}

## Brief Introduction to R on RStudio - Review

### Create a New Project

After starting RStudio, create a new project using `New Project` under the top `File` menu. From next time, you can find your project from `Open Project` or `Recent Project` under the top `File` menu.

You can also start the project by clicking or opening the `project_name`.Rpoj file.

### Four Panes and Tabs

1. Top Left: Source Editor
2. Top Right: Environment, History, etc.
3. Bottom Left: Console, Terminal, Render, Background Jobs
4. Bottom Right: Files, Plots, Packages, Help, Viewer, Presentation



## Set up

* Highly recommend to set the language to be "English".
* Create "data" directory.


```r
Sys.setenv(LANG = "en")
dir.create("./data")
```



## Three Ways to Run Codes

1. Console - Bottom Left Pane  
  - We have run codes on the console already!
2. R Script - pull-down menu under File
3. R Notebook, R Markdown - pull down menu under File



## R Script - Second Way to Run Codes

### Examples: R Scripts in Moodle

* `basics.R`
* `coronavirus.R`

1. Copy a script in Moodle: _{file name}.R_
2. In RStudio (create Project in RStudio) choose File > New File > R Script and paste it.
3. Choose File > Save As, save with a name; e.g. _{file names}_ (.R will be added automatically)

To run a code: at the cursor press *Ctrl+Shift+Enter* (Win) or *Cmd+Shift+Enter* (Mac). 


* Top Manu: Help > Keyboard Short Cut Help contains many shortcuts.
* Bottom Right Pane: Check the files by selecting the `Files` tab.



## Packages

R packages are extensions to the R statistical programming language. R packages contain code, data, and documentation in a standardised collection format that can be installed by users of R, typically via a centralised software repository such as CRAN (the Comprehensive R Archive Network).

### Installation and attachement

You can install packages by "Install Packages..." under "Tool" in the top menu.
It is also possible to install packages by running the following codes in the console.

* `install.packages("tidyverse")`
* `install.packages("rmarkdown")`



## R Notebook - Third Way to Run Codes

Choose R Notebook from the pull down File menu in the top bar.

### Edit YAML

**Default* is as follows**

```

title: "R Notebook"
output: html_notebook

```



**Template**

```

title: "Title of R Notebook"
author: "ID and Your Name"
date: "2023-02-10" 
output: 
  html_notebook:
#    number_sections: yes
#    toc: true
#    toc_float: true

```

* Don't change the format. Indention matters.
* The statement after \# is ignored.
* Date is automatically inserted when you compile the file.
* You can replace "2023-02-10" by "2022-12-14" or in any date format surrounded by double quotation marks.
* Section numbers: - default is `number_sections: no`.
* Table of contents, `toc: true` - default is `toc: false`.
* Floating table of contents in HTML output, `toc_float: true` - default is `toc_float: false`



### Create a Code Chunk to Attach Packages

Insert Chunk in Code pull down menu in the top bar, or use the <kbd>C</kbd> button on top. You can use shortcut keys listed under Tools in the top bar.

We have installed the package `tidyverse`. However, in order to use it, we need to attach, or load, by calling `library(tidyverse)`.

You can run the code in a code chunk by clicking the triangle mark at the top right corner. 


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

* `df_iris <- iris` can replace  `df_iris <- datasets::iris` because the package `datasets` is installed and attached as default. Since you may have other data called `iris` included in a different package or you may have changed `iris` before, it is safer to specify the package's name with the data's name.
* Within R Notebook or in Console, you may get different output, and `tf_iris` and `tbl_iris` behave differently. It is because of the default settings of R Markdown. 



### Look at the data

#### Several ways to view the data.

The `View` command opens up a window to show the contents of the data, and you can also use the filter.


```r
View(df_iris)
```


The following simple command also shows the data. 


```r
df_iris
```

```
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

`%>%` is called a pipe command, and we use it often. 

The output within R Notebook is a tibble style. Try the same command in Console.


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

There are six types of data in R; Double, Integer, Character, Logical, Raw, and Complex. In this course, we use only the first four.

The names after $ are column names. If you call `df_iris$Species`, you have the Species column. Species is in the 5th column, `typeof(df_iris[[5]])` does the same as the next. 

`df_iris[2,4] = `0.2 is the fourth entry of Sepal.Width.




```r
typeof(df_iris$Species)
#> [1] "integer"
```


```r
class(df_iris$Species)
#> [1] "factor"
```

For `factors = fct` see [the R Document](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/factor) or an explanation in [Factor in R: Categorical Variable & Continuous Variables](https://www.guru99.com/r-factor-categorical-continuous.html).




```r
typeof(df_iris$Sepal.Length)
#> [1] "double"
class(df_iris$Sepal.Length)
#> [1] "numeric"
```


**Q1.** What are the differences of `df_iris`, `slice(df_iris, 1:10)` and `glimpse(df_iris)` above?

**Q2.** What are the differences of`df_iris`, `slice(df_iris, 1:10)` and `glimpse(df_iris)` in the console?



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





```r
ggplot(data = df_iris, aes(x = Sepal.Length, y = Sepal.Width)) +
  geom_point()
```

<img src="03-tidyverse_files/figure-html/unnamed-chunk-10-1.png" width="672" />



#### Scatter Plot with [Labels](https://ggplot2.tidyverse.org/reference/labs.html)

Add title and labels adding `labs()`. 

```
ggplot(data = <data>, aes(x = <column name for x>, y = <column name for y>)) +
  geom_point() +
  labs(title = "Title", x = "Label for x", y = "Label for y")
```



```r
ggplot(data = df_iris, aes(x = Sepal.Length, y = Sepal.Width)) +
  geom_point() + 
  labs(title = "Scatter Plot of Sepal Data of Iris", x = "Sepal Length", y = "Sepal Width")
```

<img src="03-tidyverse_files/figure-html/unnamed-chunk-11-1.png" width="672" />



#### Scatter Plot with [Colors](https://ggplot2.tidyverse.org/reference/aes_colour_fill_alpha.html)

Add different colors automatically to each species. Can you see each group?


```r
ggplot(data = df_iris, aes(x = Sepal.Length, y = Sepal.Width, color = Species)) +
  geom_point()
```

<img src="03-tidyverse_files/figure-html/unnamed-chunk-12-1.png" width="672" />



#### Scatter Plot with Shapes


```r
ggplot(data = df_iris, aes(x = Sepal.Length, y = Sepal.Width, shape = Species)) +
  geom_point()
```

<img src="03-tidyverse_files/figure-html/unnamed-chunk-13-1.png" width="672" />



#### [Boxplot](https://ggplot2.tidyverse.org/reference/geom_boxplot.html)

The boxplot compactly displays the distribution of a continuous variable. 


```r
ggplot(data = df_iris, aes(x = Species, y = Sepal.Length)) +
  geom_boxplot()
```

<img src="03-tidyverse_files/figure-html/unnamed-chunk-14-1.png" width="672" />



#### [Histogram](https://ggplot2.tidyverse.org/reference/geom_histogram.html)

Visualize the distribution of a single continuous variable by dividing the x axis into bins and counting the number of observations in each bin. Histograms (geom_histogram()) display the counts with bars. 


```r
ggplot(data = df_iris, aes(x = Sepal.Length)) +
  geom_histogram()
```

<img src="03-tidyverse_files/figure-html/unnamed-chunk-15-1.png" width="672" />



Change the number of bins by `bins =` `<number>`.


```r
ggplot(data = df_iris, aes(x = Sepal.Length)) +
  geom_histogram(bins = 10)
```

<img src="03-tidyverse_files/figure-html/unnamed-chunk-16-1.png" width="672" />



### Data Modeling 

Professor Kaizoji will cover the mathematical models and hypothesis testings. 


```r
ggplot(data = df_iris, aes(x = Sepal.Length, y = Sepal.Width)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE)
```

<img src="03-tidyverse_files/figure-html/unnamed-chunk-17-1.png" width="672" />

## Comments

### Helpful Resources

* Cheat Sheet in RStudio: https://www.rstudio.com/resources/cheatsheets/  
  - [RStudio IED](https://raw.githubusercontent.com/rstudio/cheatsheets/main/rstudio-ide.pdf)
  - [Base R Cheat Sheet](https://github.com/rstudio/cheatsheets/raw/main/base-r.pdf)
* 'Quick R' by DataCamp: https://www.statmethods.net/management

* [An Introduction to R](https://cran.rstudio.com)



### Practicum

* Posit Primers: The Basics: https://posit.cloud/learn/primers/1
  - Complete Visualization Basics and Programming Basics






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
  
  
## First Assignment

1. Assignment Week 2-1: Introduction Plus Forum  

Please write the following.

* A brief self-introduction. ... please call me ......
* What do you expect from this course?
* A list of five to ten questions on Covid-19 or other topics you want to study by data.

My response (an example):

* Hiroshi Suzuki, an instructor of this course, retired from ICU in 2019. Please call me Suzuki-san or Suzuki-sensei.
* I hope to develop a learning community on data science.
* Questions (on Covid-19).

1. How can we prepare for the next pandemic?
2. What are the key ethical issues of vaccine mandate and vaccine passport?
3. How can we assess the efficacy of various vaccines?
4. What are the determining factors of the mortality rate of each country?
5. What can we do in this pandemic for the most vulnerable people?

  - Due: Tuesday, 20 December 2022, 11:59 PM

### Responses - Except the self-introduction

**A.**

* As I have no prior knowledge of using applications such as R studio for data analysis, I would like to understand and obtain the knowledge of the techniques and put it into use in my studies and career in the future.

* Questions about covid

  1. What are the impacts of COVID-19 on mental health?
  2. What group of people is the most affected by COVID-19 on their menntal health issue? (age, gender, work class, etc)
  3. What can we do to alleviate the impact of COVID-19 on mental health?
  4. What are the impacts of COVID-19 on envrionment? 
  5. How do the governments in different countries deal with solid waste expose during the pandemic?

**B.**

* From this course I expect to learn how to use R, strengthen my skills in doing quantitative research in coalition with qualitative approaches (mixed-methods) and visualizing data, and process some data related to my thesis research topic.  

* Questions 

  1. Can a gender approach give new understandings of the armed conflict in Colombia?
  2. How can we assess the relationship between gender and the occurrence of violence within the Colombian armed conflict?
  3. How are masculine values and patriarchy related to violence within the Colombian armed conflict?
  4. How can be the relationship between gender and violence be quantified?
  5. What kind of initiatives could be effective to promote cultural and gender values for peace in Colombia?

**C.**

* I have no prior knowledge or skill when it comes to data analysis and collection so I hope to gain some knowledge and understand how to analyse data in my academic career.

* COVID-19 questions:

  1. How can countries be better prepared for future pandemic outbreaks?
  2. What various methods were adopted by different communities when dealing with the mental health effects of lockdown?
  3. How has the pandemic impacted the socialisation of toddlers and young children?
  4. Will the use of face masks become the new normal or will people eventually defer back to a mask-less existence?
  5. What are the chances of recovering from long covid in young people?

**D.**

* I hope to develop my skills in data science and data analysis through this course. 

* Questions on Global Health issues:

  1. Is the COVID-19 pandemic over? 
  
  2. Were underrepresented communities less or more affected by the COVID-19 pandemic?
  
  3. How can access to healthcare barriers in remote areas be reduced? 
  
  4. Is there a correlation between income levels and the prevalence of non-communicable diseases? 
  
  5. Can basic health education help decrease the prevalence of non-communicable diseases in communities? 

**E.**

* My expectation from this course

I hope to learn the fundamentals of data science by using the free software, R and its IDE, R Studio. This course will be helped me to learn, how to collect data, transform data into appropriate forms, visualize data, how to analyze data and present the findings to others. 

* Questions (on Covid-19)

1) How does Covid-19 compare to other public health threats?

2) How effective are masks and do they also need to cover my nose?

3) Is Covid-19 worse than flu?

4) Why do governments benefit from helping to ensure other countries access vaccines?

5) What are the determining factors of the mortality rate of each country?

6) What are the risks of re-infection?

7) Should I be concerned that the sample sizes in vaccine clinical trails were not bigger?

8) Do I still need to worry about infection even though I am fit and healthy?

9) How can we prepare for the next pandemic?

10) How can we trust vaccines that have been developed so fast?



**F.**

* In this course I would like to review basic statistical skills with the application of R programming. 

* Questions on Covid-19:

  1. What are the key characteristics of Covid-19 related data?
  2. What are the major data issues of Covid-19?
  3. How can data accuracy during a pandemic such as Covid-19 be improved?
  4. What is to be considered when comparing Covid-19 data across countries?
  5. How can data support measures of disaster risk reduction to reduce further outbreaks?

**G.**

* I hope to learn how to effectively use R for data analysis and visualization for my thesis and future work. 

* I would like to look at poverty data: 

  1. What is the best way to measure poverty? 
  
  2. Is poverty to be regarded as absolute or relative?
  
  3. is it possible to compare poverty data across societies? 
  
  4. What policies are most effective to alleviate poverty? 
  
  5. How do we evaluate policies and programs with regard to multi-dimensional poverty?

**H.**

* From this course, I wish to gain skill which will enable me analyze data using Rstudio applications  related to my field of research and other aspect of  life and the world in general. 

* questions (Food Inflection)

  1. What are the targeted areas of the world is the world bank more concern in term of food crises?
  
  2. what are the interest rate attributed to the support given to these countries by the world bank?
  
  3. it is possible for countries with the most fertile and unused land ranked as countries with high food inflection rate? why?
  
  4.  On what facts are the data between the world bank, developed nations and developing nations analyzed?
  
  5. How will the raise of modified genetic food affect the world in generations to come?
  

**I.**

* Data analysis is an essential aspect of the research process. R is a free, convenient, and popular program among scholars. I hope to learn the R program and use R in data analysis for my final thesis.

* Questions

  1. What adverse effects are associated with vaccination?
  
  2. How does the risk-benefit ratio for COVID-19 vaccination differ in children?
  
  3. How does the COVID-19 pandemic affect economic growth?
  
  4. How efficacious is vaccination at preventing symptomatic COVID-19?
  
  5. How effective is vaccination against Omicron and its subvariants?
  
  6. What are the indications and contraindications of vaccination?
  
  7. What factors affect the high mortality in elderly people?


**J.**

* I hope I can learn more about analyzing data via R. It will help me a lot in my assignments and research.

* Questions (about climate change):

  1. What is the greenhouse effect?
  
  2. How can we assess the effect of climate change to economic?
  
  3. What are the most vunerable countries effected by climate change? Developed countries or Developing countries
  
  4. Which countries have the largest CO2 emissions? 
  
  5. What can we do to stop global warming?

**K.**

* From this course I hope to gain skills that I can use in my current research, or future research/career. 

* Questions (on Covid-19).

  1. What is the prevalence of long COVID? 

  2. What groups of people does long COVID most impact? 

  3. Are there certain music genres that may help people with alzheimer's?

  4.  will we actually run out of fossil fuel, if ever?

  5. What present day animal species are on the brink of extinction?


**L.**

* My expectation on the course is to learn the way of mulivarible analyses including PCA (principal component analysis) and other mochine learning methods with R.

* Questions (Japanese views on Religion, Nature, and Science)

  1. What percentage of Japanese are religious?

  2. What kind of religions are Japanese believe in?

  3. What do Japanese expect on religions?

  4. What percentage of Japanese believe existence of Heaven?

  5. What percentage of Japanese believe Creation of Earth and human by God?

  6. What percentage of Japanese disbelieve Evolution theory?

  7. What percentage of Japanese believe existence of extra-terrestrials?

  8.Correlations among above items.

**M.**

* I hope to review R basics and learn about using R for data cleaning and visualisation. I had the opportunity before to use R to run various linear regressions, so I. hope to learn R programming knowledge that would complete that. 

* Questions on COVID-19:

  1) What was the effect of COVID-19 on worldwide inequalities, particularly concerning health care access?
  
  2) Did the Japanese government actions, such as distributing 100,000 Yen to households during the COVID, stimulated consumption or savings?
  
  3) What percentage of government spendings were allocated to the COVID-19 when the pandemic started compared to now?
  
  4) How did the COVID-19 affected economic development of underdeveloped countries?
  
  5) How was the job market affected by the COVID-19?

**N.**

* Questions.

  1. Impact of workers remmitance of savings rate in Pakistan.

  2. How the exchange rate affects the worker's remittances in Pakistan.

  3. What is the impact of the interest rate on the savings rate?

  4. How fiscal policy affects the savings rate.

5. What is the relationship between savings and consumption? 

**O.**

* I am interested in learning analysis by using R software and intend to use R in data analysis for my thesis. 


* Questions:

  1. What is impact of FDI on Economic growth of a country?
  
  2. What is impact of FDI on stock exchange performance of a country?
  
  3. How inward FDI contributes to GDP growth of a country?
  
  4. How can we analyze timeseries data?
  
  5. How do we analyze the impact of an incident on time series data? How to perform Pre and post incident analysis?

**P.**

* Learning R-Studio will help me in my research methodology. I am hopeful to learn how to assess the different databases using R-studio and make graphs.
 

* Questions (Climate Change).

  1. How can we save the world from the effects of climate change?

  2. What are the key ethical issues of Climate change on temperature and precipitation?

  3. How can we assess climate change's impact on countries' economic growth?

  4. What factors determine the climate change impact on the economic growth of each country?

  5. How can we reduce the effect of climate change on countries?

**Q.**

* I hope I can learn more on how I can analyze data via R. There are a lot of different models in R, if I can learn more about these models will be perfect.

* Questions:

  1. P value and T check and etc, I only have a very sensitive feeling about them. Can we learn more about this?
  
  2. How can I do time seris analysis  in R?
  
  3. If I have my own hypothesis about a specific topic, how can I check the data via R?
  
  4.Will we learn more about economic analysis?
  
  5.From the data analysis, we know a lot of  corelation check, what's more we can explore ? 

 
**R.**

*  I want to explore and compare the different data related to global higher Education from this course.

* Questions:

  1. After Japan released international traveling, how is Covid-19 impacted Japan’s health system?
  
  2. What necessary methods have been done in Japan during the post-pandemic period?
  
  3. Compared to the previous vaccination policy, how does data show the percentage of people who got the fourth vaccination?
  
  4. What are the differences between Japan and other developed countries to Covid-19 prevention measures in universities?
  
  5. How do students and faculties react to these pandemic precautionary approaches? Whether international and local students have similar perspectives?

**S.**

* I hope to experience using R, hopefully become more comfortable with playing with numerical data. My quantitative research method skill right now is very limited. 

* Questions (Covid-19):

  1. How pandemic affected the movement of foreign technical trainees?

  2. How can we assist social rights of foreigner's living in Japan during the pandemic?
  
  3. What are the key factors that affected college student's mental health during pandemic?

  4. What were the key reinforcement during the pandemic for high criminal rates of foreign technical trainees?

  5. How pandemic affected the domestic violence in Japan?

  6. How public education were impacted from pandemic on students' study habit?















