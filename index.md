---
title       : An introduction to tidy data and the tidyverse
subtitle    : 
author      : Daniel Anderson
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : zenburn      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
--- 
<style>
em {
  font-style: italic
}
</style>

<style>
strong {
  font-weight: bold;
}
</style>




## Agenda
* Introduce the tidyverse
* Introduce the concept of tidy data
* Tidy a simple dataset together
* Discuss why tidy data are useful (particularly when used within the tidyverse)
* Overview packages in the tidyverse

---- &twocol
## tidyverse (briefly)

*** =right
![had](assets/img/had.png)

*** =left

* Collection of packages written primarily by Hadley Wickham. 
* All built around a common philosophy 
* *tidy* is actually a technical term for a specific data format
* Today will be focused around the tidyverse as an efficient way for you to "break in" to R

----
## tidyverse packages 

The primary packages included in the tidyverse include:

* *ggplot2* for data visualization
* *dplyr* for data manipulation
* *tidyr* for getting data in a tidy format (in the technical sense)
* *readr* for importing data
* *purrr* for functional programming
* *tibble* for working with a different kind of data frame, called a tibble

---- &twocol
## Other tidyverse packages

*** =left

* *hms* for time data
* *stringr* for string data
* *lubridate* for date/time data
* *forcats* for factors
* *DBI* for databases
* *haven* for SPSS, SAS and Stata files

*** =right

* *httr* for web apis
* *jsonlite* for JSON
* *readxl* for Excel files
* *rvest* for web scraping
* *xml2* for XML
* *modelr* for simple modeling
* *broom* for transforming model results into tidy data

----
## `tidyr` and `dplyr`
Two packages we will use most today
* `tidyr`: Helps you get your data into a tidy format
* `dplyr`: Helps you manipulate your data (create, remove, summarize, etc. )

----
## Getting started

* Install once with `install.packages()`
* Load each time with `library()`


```r
# Install once
install.packages("tidyverse")

# Load each time
library(tidyverse)
```


----
## Important qualifications
Everything that can be done through the tidyverse can be done through base R. 
  (and some things can even be done multiple ways through the tidyverse and base R)

We'll focus on the tidyverse because it's a good way to "break in" to R.

Later, you can go deeper on your own (or perhaps through another course) to 
  learn more about base R.

*I know base R better than the tidyverse: I'm learning the tidyverse with you*

----
## Why tidyverse?
* Verbs
* Efficiency
* More intuitive when getting first learning R

<br>

We will learn some base R - it is unavoidable. A *huge* part of learning R is learning how to not only search for answers when you're having problems, but also to understand the answers provided! In many cases, this requires understanding at least some base R.

---- &twocol
## Philosophy of the tidyverse

*** =left
![stack-1](assets/img/stack-1.jpeg)
![stack-2](assets/img/stack-2.jpg)

*** =right
![pit](assets/img/pit.jpeg)


--- bg:Lavender &twocol

## Tidy data

*** =left

<div align = "right">
<img src = assets/img/tidy_up.png width = 600 height = 200>
</div>

<div align = "left">
<img src = assets/img/tidy_data.png width = 600 height = 200>
</div>

*** =right

<div align = "center">
<img src = assets/img/tidy_up2.jpeg width = 300 height = 300>
</div>

----- .quote
<q> It is often said that 80% of data analysis is spent on the process of cleaning and preparing the data.

---- &twocol
*** =left

* Persistent and varied challenge
* Little research on how to do it well
	+ Enter Hadley Wickham

*** =right

<div align = "center">
<img src = assets/img/hadley_JSS.png width = 500 height = 750>
</div>

---- 
## Tidy data

# Definition
1. Each variable is a column
2. Each observation is a row
3. Each type of observational unit forms a table

<div align = "left">
<img src = assets/img/tidy_data.png width = 1000 height = 350>
</div>


----
## Common ways rectangular datasets are "messy"
(We won't get into multiple data files and how they interact, i.e., relational databases)
* Column headers are values, not variable names
* Multiple variables stored in one column
* Variables are stored in both rows and columns


----
## An example with SEDA data

Any ideas why this is messy? (keep in mind, this dataset includes "long" in the file name)


|  leaid|leaname                | fips|stateabb | year| grade| mean_link_ela| se_link_ela| mean_link_math| se_link_math|
|------:|:----------------------|----:|:--------|----:|-----:|-------------:|-----------:|--------------:|------------:|
| 100002|ALABAMA YOUTH SERVICES |    1|AL       | 2009|     8|        210.55|        6.72|         220.67|         7.06|
| 100002|ALABAMA YOUTH SERVICES |    1|AL       | 2011|     8|        231.66|        6.74|             NA|           NA|
| 100002|ALABAMA YOUTH SERVICES |    1|AL       | 2012|     8|        226.18|        6.66|         233.13|         6.98|
| 100005|ALBERTVILLE CITY       |    1|AL       | 2009|     3|        204.47|        2.75|         216.36|         2.01|
| 100005|ALBERTVILLE CITY       |    1|AL       | 2009|     4|        207.40|        2.94|         223.03|         2.34|
| 100005|ALBERTVILLE CITY       |    1|AL       | 2009|     5|        216.86|        2.60|         230.42|         2.29|

----
The offending columns

<br>


| mean_link_ela| se_link_ela| mean_link_math| se_link_math|
|-------------:|-----------:|--------------:|------------:|
|        210.55|        6.72|         220.67|         7.06|
|        231.66|        6.74|             NA|           NA|
|        226.18|        6.66|         233.13|         6.98|
|        204.47|        2.75|         216.36|         2.01|
|        207.40|        2.94|         223.03|         2.34|
|        216.86|        2.60|         230.42|         2.29|
Problem? The column headers are values, not variables. 

----
## Tidied version


|  leaid|leaname                | fips|stateabb | year| grade|Subject |     mean|       se|
|------:|:----------------------|----:|:--------|----:|-----:|:-------|--------:|--------:|
| 100002|ALABAMA YOUTH SERVICES |    1|AL       | 2009|     8|ela     | 210.5474| 6.723581|
| 100002|ALABAMA YOUTH SERVICES |    1|AL       | 2011|     8|ela     | 231.6601| 6.741922|
| 100002|ALABAMA YOUTH SERVICES |    1|AL       | 2012|     8|ela     | 226.1813| 6.657756|
| 100005|ALBERTVILLE CITY       |    1|AL       | 2009|     3|ela     | 204.4659| 2.747565|
| 100005|ALBERTVILLE CITY       |    1|AL       | 2009|     4|ela     | 207.4045| 2.939638|
| 100005|ALBERTVILLE CITY       |    1|AL       | 2009|     5|ela     | 216.8594| 2.599224|

----
## Other examples
(from the JSS paper)


|religion                | <$10k| $10-20k| $20-30k| $30-40k| $40-50k| $50-75k| $75-100k| $100-150k| >150k| Don't know/refused|
|:-----------------------|-----:|-------:|-------:|-------:|-------:|-------:|--------:|---------:|-----:|------------------:|
|Agnostic                |    27|      34|      60|      81|      76|     137|      122|       109|    84|                 96|
|Atheist                 |    12|      27|      37|      52|      35|      70|       73|        59|    74|                 76|
|Buddhist                |    27|      21|      30|      34|      33|      58|       62|        39|    53|                 54|
|Catholic                |   418|     617|     732|     670|     638|    1116|      949|       792|   633|               1489|
|Don’t know/refused      |    15|      14|      15|      11|      10|      35|       21|        17|    18|                116|
|Evangelical Prot        |   575|     869|    1064|     982|     881|    1486|      949|       723|   414|               1529|
|Hindu                   |     1|       9|       7|       9|      11|      34|       47|        48|    54|                 37|
|Historically Black Prot |   228|     244|     236|     238|     197|     223|      131|        81|    78|                339|
|Jehovah's Witness       |    20|      27|      24|      24|      21|      30|       15|        11|     6|                 37|
|Jewish                  |    19|      19|      25|      25|      30|      95|       69|        87|   151|                162|
|Mainline Prot           |   289|     495|     619|     655|     651|    1107|      939|       753|   634|               1328|
|Mormon                  |    29|      40|      48|      51|      56|     112|       85|        49|    42|                 69|
|Muslim                  |     6|       7|       9|      10|       9|      23|       16|         8|     6|                 22|
|Orthodox                |    13|      17|      23|      32|      32|      47|       38|        42|    46|                 73|
|Other Christian         |     9|       7|      11|      13|      13|      14|       18|        14|    12|                 18|
|Other Faiths            |    20|      33|      40|      46|      49|      63|       46|        40|    41|                 71|
|Other World Religions   |     5|       2|       3|       4|       2|       7|        3|         4|     4|                  8|
|Unaffiliated            |   217|     299|     374|     365|     341|     528|      407|       321|   258|                597|

----
## The tidied version


|religion |income             | freq|
|:--------|:------------------|----:|
|Agnostic |<$10k              |   27|
|Agnostic |$10-20k            |   34|
|Agnostic |$20-30k            |   60|
|Agnostic |$30-40k            |   81|
|Agnostic |$40-50k            |   76|
|Agnostic |$50-75k            |  137|
|Agnostic |$75-100k           |  122|
|Agnostic |$100-150k          |  109|
|Agnostic |>150k              |   84|
|Agnostic |Don't know/refused |   96|
|Atheist  |<$10k              |   12|
|Atheist  |$10-20k            |   27|

----
## Yet another example

|country | year| m014| m1524| m2534| m3544| m4554| mu| f014| f1524| f2534| f3544| f4554|
|:-------|----:|----:|-----:|-----:|-----:|-----:|--:|----:|-----:|-----:|-----:|-----:|
|AD      | 2000|    0|     0|     1|     0|     0|   |     |      |      |      |      |
|AE      | 2000|    2|     4|     4|     6|     5|   |    3|    16|     1|     3|     0|
|AF      | 2000|   52|   228|   183|   149|   129|   |   93|   414|   565|   339|   205|
|AG      | 2000|    0|     0|     0|     0|     0|   |    1|     1|     1|     0|     0|
|AL      | 2000|    2|    19|    21|    14|    24|   |    3|    11|    10|     8|     8|
|AM      | 2000|    2|   152|   130|   131|    63|   |    1|    24|    27|    24|     8|
|AN      | 2000|    0|     0|     1|     2|     0|   |    0|     0|     1|     0|     0|
|AO      | 2000|  186|   999|  1003|   912|   482|   |  247|  1142|  1091|   844|   417|
|AR      | 2000|   97|   278|   594|   402|   419|   |  121|   544|   479|   262|   230|
|AS      | 2000|     |      |      |      |     1|   |     |      |      |      |     1|

In this example, *M* indicates if the data came from a male, while *F* indicates female. The subsequent numbers represent the age range. Tidying these data will be a two step process.

----
## Step one


|country | year|variable | cases|
|:-------|----:|:--------|-----:|
|AD      | 2000|m014     |     0|
|AE      | 2000|m014     |     2|
|AF      | 2000|m014     |    52|
|AG      | 2000|m014     |     0|
|AL      | 2000|m014     |     2|
|AM      | 2000|m014     |     2|
|AN      | 2000|m014     |     0|
|AO      | 2000|m014     |   186|
|AR      | 2000|m014     |    97|
|AS      | 2000|m014     |    NA|
Notice this is much closer to what we want, but we have a problem now in that we have **two variables stored in one column**.

----
## Step two: Tidied data


|country | year|sex |age_range | cases|
|:-------|----:|:---|:---------|-----:|
|AD      | 2000|m   |0-14      |     0|
|AD      | 2000|m   |15-24     |     0|
|AD      | 2000|m   |25-34     |     1|
|AD      | 2000|m   |35-44     |     0|
|AD      | 2000|m   |45-54     |     0|
|AD      | 2000|m   |55-64     |     0|
|AD      | 2000|m   |65+       |     0|
|AE      | 2000|m   |0-14      |     2|
|AE      | 2000|m   |15-24     |     4|
|AE      | 2000|m   |25-34     |     4|

----
## Variables as rows and columns

|id      | year| month|element | d1|   d2|   d3| d4|   d5| d6| d7| d8|
|:-------|----:|-----:|:-------|--:|----:|----:|--:|----:|--:|--:|--:|
|MX17004 | 2010|     1|tmax    |   |     |     |   |     |   |   |   |
|MX17004 | 2010|     1|tmin    |   |     |     |   |     |   |   |   |
|MX17004 | 2010|     2|tmax    |   | 27.3| 24.1|   |     |   |   |   |
|MX17004 | 2010|     2|tmin    |   | 14.4| 14.4|   |     |   |   |   |
|MX17004 | 2010|     3|tmax    |   |     |     |   | 32.1|   |   |   |
|MX17004 | 2010|     3|tmin    |   |     |     |   | 14.2|   |   |   |
|MX17004 | 2010|     4|tmax    |   |     |     |   |     |   |   |   |
|MX17004 | 2010|     4|tmin    |   |     |     |   |     |   |   |   |
|MX17004 | 2010|     5|tmax    |   |     |     |   |     |   |   |   |
|MX17004 | 2010|     5|tmin    |   |     |     |   |     |   |   |   |

---- &twocol
## Two Steps



*** =left
# Step 1

|id      | year| month|element |day_key | value|
|:-------|----:|-----:|:-------|:-------|-----:|
|MX17004 | 2010|    12|tmax    |d1      |  29.9|
|MX17004 | 2010|    12|tmin    |d1      |  13.8|
|MX17004 | 2010|     2|tmax    |d2      |  27.3|
|MX17004 | 2010|     2|tmin    |d2      |  14.4|
|MX17004 | 2010|    11|tmax    |d2      |  31.3|
|MX17004 | 2010|    11|tmin    |d2      |  16.3|
|MX17004 | 2010|     2|tmax    |d3      |  24.1|
|MX17004 | 2010|     2|tmin    |d3      |  14.4|
|MX17004 | 2010|     7|tmax    |d3      |  28.6|
|MX17004 | 2010|     7|tmin    |d3      |  17.5|

*** =right
# Step 2

|id      |date       | tmax| tmin|
|:-------|:----------|----:|----:|
|MX17004 |2010-01-01 | 27.8| 14.5|
|MX17004 |2010-02-02 | 29.7| 13.4|
|MX17004 |2010-02-02 | 27.3| 14.4|
|MX17004 |2010-02-02 | 29.9| 10.7|
|MX17004 |2010-02-02 | 24.1| 14.4|
|MX17004 |2010-03-03 | 34.5| 16.8|
|MX17004 |2010-03-03 | 31.1| 17.6|
|MX17004 |2010-03-03 | 32.1| 14.2|
|MX17004 |2010-04-04 | 36.3| 16.7|
|MX17004 |2010-05-05 | 33.2| 18.2|

----
## A caveat
* There are many reasons why you might want to have messy data. However, tidy data is an extremely useful format generally, and particularly useful when applying tools within the *tidyverse*. 

* All packages within the tidyverse are designed to either help you get your data in a tidy format, or assume your data are already in a tidy format.

* Assuming a common data format leads to large jumps in efficiency, as the output from certain functions can be directly input into others.

----
## The tidyverse data analysis philosophy

<div align = "left">
<img src = assets/img/data-science.png width = 1000 height = 400>
</div>


----
## Why tidy data?

* When you put your data in a tidy format they are "ready to go" for essentially all the packages in the tidyverse. Let's look at some example item-level data.

* First, let's tidy some data together, as a group. Then, I'll demonstrate why this format can be so helpful within the tidyverse

----
## Load the data

Because we're working through the *tidyverse*, we'll use the *readr* package and the `read_csv` function, rather than `base::read.csv`. These functions differ in the following ways:

<div align = "left">
<img src = assets/img/read_csv.png width = 1000 height = 300>
</div>


---- .segue
# Load data with RStudio demo

----
## Other ways to load data




```r
setwd("/Users/Daniel/Dropbox/Teaching/tidyverse_course/2_tidy_data/data/")
library(readr)
d <- read_csv("exam1.csv")
knitr::kable(head(d))
```



|stu_name |gender | item_1| item_2| item_3| item_4| item_5| item_6| item_7| item_8| item_9| item_10| item_11| item_12| item_13| item_14| item_15| item_16| item_17| item_18|
|:--------|:------|------:|------:|------:|------:|------:|------:|------:|------:|------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|-------:|
|Adam     |M      |      1|      1|      1|      1|      1|      1|      1|      0|      0|       0|       0|       0|       0|       0|       0|       0|       0|       0|
|Anne     |F      |      1|      1|      1|      1|      1|      1|      1|      1|      1|       1|       0|       0|       0|       0|       0|       0|       0|       0|
|Audrey   |F      |      1|      1|      1|      1|      1|      1|      1|      1|      1|       1|       0|       0|       1|       0|       0|       0|       0|       0|
|Barbara  |F      |      1|      1|      1|      1|      0|      0|      1|      0|      0|       1|       0|       0|       0|       0|       0|       0|       0|       0|
|Bert     |M      |      1|      1|      1|      1|      1|      0|      1|      0|      1|       1|       0|       0|       0|       0|       0|       0|       0|       0|
|Betty    |F      |      1|      1|      1|      1|      1|      1|      1|      1|      1|       0|       0|       0|       0|       0|       0|       0|       0|       0|



----
## Pop Quiz Time
* Are these data tidy?
* If not, what needs to happen to make them tidy?
* What are the variables? What are the values?

----
## Verbs: `tidyr`
* `gather()`
* `spread()`
* `separate()` and `extract()`
* `unite()`
* `nest()`

What do you think each do?

----
## Quick aside: `%>%`
The "pipe" operator takes the output from one function and feeds it into the 
next. For example:


```r
d$gender %>% table()
```

```
## .
##  F  M 
## 18 17
```

```r
d %>% str()
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	35 obs. of  20 variables:
##  $ stu_name: chr  "Adam" "Anne" "Audrey" "Barbara" ...
##  $ gender  : chr  "M" "F" "F" "F" ...
##  $ item_1  : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ item_2  : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ item_3  : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ item_4  : int  1 1 1 1 1 1 1 1 0 1 ...
##  $ item_5  : int  1 1 1 0 1 1 1 1 1 0 ...
##  $ item_6  : int  1 1 1 0 0 1 1 1 1 0 ...
##  $ item_7  : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ item_8  : int  0 1 1 0 0 1 1 1 1 0 ...
##  $ item_9  : int  0 1 1 0 1 1 1 1 1 1 ...
##  $ item_10 : int  0 1 1 1 1 0 1 1 0 0 ...
##  $ item_11 : int  0 0 0 0 0 0 1 0 0 0 ...
##  $ item_12 : int  0 0 0 0 0 0 1 0 0 0 ...
##  $ item_13 : int  0 0 1 0 0 0 1 0 0 0 ...
##  $ item_14 : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ item_15 : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ item_16 : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ item_17 : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ item_18 : int  0 0 0 0 0 0 0 0 0 0 ...
```

----
## Step 1: `gather` the item variables
* Change all item variables into two variables: `item` and `score`

<br>

![gather](assets/img/gather.png)

----
Try running the following code


```r
d %>% 
  gather(key = item, value = score, -1:-2) 
```
* Note that in the above code, we omit the `data` argument, because it's being "piped" in.
* Third argument to `...` says we want to omit the first and second columns in when gathering.

<br>

What do you get? Are these data tidy now?

---
* The code on the previous slide basically puts our data in a tidy format. 

**BUT**

* We didn't store the result in an object (try printing `d` now)
* Need to store the tidy data in a new object (or reassign `d`)
* To "clean up" some, could transform the `item` variable to numeric

----
## Finish tidying the data


```r
td <- d %>% 
  gather(item, score, -1:-2) %>% 
  mutate(item = parse_number(item))
```
* `parse_number()` comes from the *readr* package.
* To see the data you'll need to print `td` (or whatever you reassigned it as) to the console.


|stu_name |gender | item| score|
|:--------|:------|----:|-----:|
|Adam     |M      |    1|     1|
|Anne     |F      |    1|     1|
|Audrey   |F      |    1|     1|
|Barbara  |F      |    1|     1|
|Bert     |M      |    1|     1|
|Betty    |F      |    1|     1|

----
## An alternative
(please run this code, following the explanation)


```r
td <- d %>% 
  gather(item, score, -1:-2) %>% 
  separate(item, c("discard", "item"), sep = "_") %>% 
  select(-discard)
```

----
## Why are tidy data useful?
* When used in conjunction with `dplyr`, tidy data can result in large gains in efficiency.

For example, suppose we want to calculate the proportion of students responding correctly to each item.


```r
td %>% 
  group_by(item) %>% 
  summarize(prop = mean(score))
```

----

```
## # A tibble: 18 × 2
##     item       prop
##    <chr>      <dbl>
## 1      1 1.00000000
## 2     10 0.68571429
## 3     11 0.34285714
## 4     12 0.17142857
## 5     13 0.20000000
## 6     14 0.08571429
## 7     15 0.02857143
## 8     16 0.02857143
## 9     17 0.02857143
## 10    18 0.00000000
## 11     2 1.00000000
## 12     3 1.00000000
## 13     4 0.91428571
## 14     5 0.88571429
## 15     6 0.85714286
## 16     7 0.88571429
## 17     8 0.77142857
## 18     9 0.85714286
```

----
What if we also wanted to know the standard deviation?


```r
td %>% 
  group_by(item) %>% 
  summarize(prop = mean(score),
            sd = sd(score))
```

```
## # A tibble: 18 × 3
##     item       prop        sd
##    <chr>      <dbl>     <dbl>
## 1      1 1.00000000 0.0000000
## 2     10 0.68571429 0.4710082
## 3     11 0.34285714 0.4815940
## 4     12 0.17142857 0.3823853
## 5     13 0.20000000 0.4058397
## 6     14 0.08571429 0.2840286
## 7     15 0.02857143 0.1690309
## 8     16 0.02857143 0.1690309
## 9     17 0.02857143 0.1690309
## 10    18 0.00000000 0.0000000
## 11     2 1.00000000 0.0000000
## 12     3 1.00000000 0.0000000
## 13     4 0.91428571 0.2840286
## 14     5 0.88571429 0.3228029
## 15     6 0.85714286 0.3550358
## 16     7 0.88571429 0.3228029
## 17     8 0.77142857 0.4260430
## 18     9 0.85714286 0.3550358
```
----
What if we wanted to know the proportion correct for each item by gender?


```r
td %>% 
  group_by(item, gender) %>% 
  summarize(prop = mean(score))
```

```
## Source: local data frame [36 x 3]
## Groups: item [?]
## 
##     item gender       prop
##    <chr>  <chr>      <dbl>
## 1      1      F 1.00000000
## 2      1      M 1.00000000
## 3     10      F 0.72222222
## 4     10      M 0.64705882
## 5     11      F 0.05555556
## 6     11      M 0.64705882
## 7     12      F 0.00000000
## 8     12      M 0.35294118
## 9     13      F 0.22222222
## 10    13      M 0.17647059
## # ... with 26 more rows
```

----
## Verbs: *dplyr* 

* `group_by()`
* `filter()` and `slice()`
* `arrange()`
* `select()` and `rename()`
* `distinct()`
* `mutate()` and `transmutate()`
* `summarize()` (or `summarise()`)
* `sample_n()` and `sample_frac()`

What do you think each of the above do?

* Good overview of `dplyr` [here](https://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html).

----
We can take the previous example further, by piping the output into a plot


```r
td %>% 
  group_by(item, gender) %>% 
  summarize(prop = mean(score)) %>% 
  mutate(gender = as.factor(gender)) %>% 
  ggplot(aes(x = item, y = prop, color = gender)) +
  geom_point() +
  geom_line(aes(group = item))
```

![plot of chunk prop_correct_by_gender_plot](assets/fig/prop_correct_by_gender_plot-1.png)

----
But, probably better (clearer) to do it in two steps. 
<br>

First produce the data


```r
pd <- td %>% 
  group_by(item, gender) %>% 
  summarize(prop = mean(score)) %>% 
  mutate(gender = as.factor(gender))
```
Then produce the plot


```r
ggplot(pd, aes(x = item, y = prop, color = gender)) +
  geom_point() +
  geom_line(aes(group = item))
```

----
## Note on plotting
The entire next lecture will be on plotting. We're discussing tidy data now because it's a great format for plotting with *ggplot2*. 

----
## Challenge (work by yourself or with a neighbor)
Remember, the following code calculates the mean score for each item. 


```r
td %>% 
  group_by(item) %>% 
  summarize(prop = mean(score))
```

Try to modify the above code to produce raw scores for every student. If you're successful, try thinking about how you could calculate the average raw score by gender.

----
## Calculate Raw Scores
Modify the prior to:
* `group_by` student name
* `sum` score (rather than average it with `mean`)


```r
td %>% 
  group_by(stu_name) %>% 
  summarize(raw_score = sum(score))
```

```
## # A tibble: 35 × 2
##    stu_name raw_score
##       <chr>     <int>
## 1      Adam         7
## 2      Anne        10
## 3    Audrey        11
## 4   Barbara         6
## 5      Bert         8
## 6     Betty         9
## 7    Blaise        13
## 8    Brenda        10
## 9   Britton         8
## 10    Carol         6
## # ... with 25 more rows
```

----
## Raw Scores by Gender
* `group_by` name and gender (so gender is in the summary)
* calculate raw scores
* redefine `group_by` to gender alone
* calculate mean


```r
td %>% 
  group_by(stu_name, gender) %>% 
  summarize(raw_score = sum(score)) %>% 
  group_by(gender) %>% 
  summarize(means = mean(raw_score))
```

```
## # A tibble: 2 × 2
##   gender     means
##    <chr>     <dbl>
## 1      F  9.444444
## 2      M 10.058824
```

----
## Point-biserial correlation
The point-biserial correlation represents the correlation between an item response (0/1) and the total score. It represents an index of item discrimination, because generally students' responding correctly should have higher raw scores than students responding incorrectly.

<br>
To calculate point biserial correlations, we need to 
* merge the raw scores into the raw data
* calculate the correlation between the item *score* and *raw_score* for each item

----
## Calculate raw scores and merge

Raw score calculation


```r
raw <- td %>% 
  group_by(stu_name) %>% 
  summarize(raw_score = sum(score))
```

Merge with `td`


```r
td <- left_join(td, raw)
```

```
## Joining, by = "stu_name"
```

----

```r
filter(td, stu_name == "Barbara")
```

```
## # A tibble: 18 × 5
##    stu_name gender  item score raw_score
##       <chr>  <chr> <chr> <int>     <int>
## 1   Barbara      F     1     1         6
## 2   Barbara      F     2     1         6
## 3   Barbara      F     3     1         6
## 4   Barbara      F     4     1         6
## 5   Barbara      F     5     0         6
## 6   Barbara      F     6     0         6
## 7   Barbara      F     7     1         6
## 8   Barbara      F     8     0         6
## 9   Barbara      F     9     0         6
## 10  Barbara      F    10     1         6
## 11  Barbara      F    11     0         6
## 12  Barbara      F    12     0         6
## 13  Barbara      F    13     0         6
## 14  Barbara      F    14     0         6
## 15  Barbara      F    15     0         6
## 16  Barbara      F    16     0         6
## 17  Barbara      F    17     0         6
## 18  Barbara      F    18     0         6
```


---
## Calculate Point-Biserials
(note, you get some warnings here about no variance)

```r
td %>% 
  group_by(item) %>% 
  summarize(pt_biserial = cor(score, raw_score))
```

```
## # A tibble: 18 × 2
##     item pt_biserial
##    <chr>       <dbl>
## 1      1          NA
## 2     10   0.6046692
## 3     11   0.5381468
## 4     12   0.4031706
## 5     13   0.5693729
## 6     14   0.2064809
## 7     15   0.3095606
## 8     16   0.3095606
## 9     17   0.3095606
## 10    18          NA
## 11     2          NA
## 12     3          NA
## 13     4   0.5724589
## 14     5   0.5700584
## 15     6   0.5440214
## 16     7   0.4177527
## 17     8   0.7484416
## 18     9   0.7171191
```

---
## Spreading the data back out

Tidy data is great when conducting preliminary descriptives and for plotting the data. But if you're using other packages for analysis, it may need to be in a different format. 

![spread](assets/img/spread.png)

----
## Spread *td*


```r
s_d <- td %>% 
  spread(item, score)
```
(print object to see data)


|stu_name |gender | raw_score|  1| 10| 11| 12| 13| 14| 15| 16| 17| 18|  2|  3|  4|  5|  6|  7|  8|  9|
|:--------|:------|---------:|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|
|Adam     |M      |         7|  1|  0|  0|  0|  0|  0|  0|  0|  0|  0|  1|  1|  1|  1|  1|  1|  0|  0|
|Anne     |F      |        10|  1|  1|  0|  0|  0|  0|  0|  0|  0|  0|  1|  1|  1|  1|  1|  1|  1|  1|
|Audrey   |F      |        11|  1|  1|  0|  0|  1|  0|  0|  0|  0|  0|  1|  1|  1|  1|  1|  1|  1|  1|
|Barbara  |F      |         6|  1|  1|  0|  0|  0|  0|  0|  0|  0|  0|  1|  1|  1|  0|  0|  1|  0|  0|
|Bert     |M      |         8|  1|  1|  0|  0|  0|  0|  0|  0|  0|  0|  1|  1|  1|  1|  0|  1|  0|  1|
|Betty    |F      |         9|  1|  0|  0|  0|  0|  0|  0|  0|  0|  0|  1|  1|  1|  1|  1|  1|  1|  1|

----
## Fit model
We'll fit a 1PL IRT model.
* `ltm` package
* `rasch` function requires only item response data, with each column representing a unique item.


```r
md <- s_d %>% 
  select(-1:-3)

# install.packages("ltm")
library(ltm)
model <- rasch(md)
```

----

```r
summary(model)
```

```
## 
## Call:
## rasch(data = md)
## 
## Model Summary:
##    log.Lik      AIC      BIC
##  -156.3639 350.7277 380.2793
## 
## Coefficients:
##              value    std.err  z.vals
## Dffclt.1  -14.0580 12394.5102 -0.0011
## Dffclt.10  -0.6312     0.3161 -1.9971
## Dffclt.11   0.5733     0.3035  1.8886
## Dffclt.12   1.3009     0.3802  3.4217
## Dffclt.13   1.1568     0.3601  3.2128
## Dffclt.14   1.8738     0.4872  3.8457
## Dffclt.15   2.6305     0.7249  3.6288
## Dffclt.16   2.6305     0.7249  3.6288
## Dffclt.17   2.6305     0.7249  3.6288
## Dffclt.18  14.0580 16371.2499  0.0009
## Dffclt.2  -14.0580 12394.5102 -0.0011
## Dffclt.3  -14.0580 12394.5102 -0.0011
## Dffclt.4   -1.9363     0.5052 -3.8327
## Dffclt.5   -1.6854     0.4542 -3.7108
## Dffclt.6   -1.4794     0.4181 -3.5383
## Dffclt.7   -1.6854     0.4542 -3.7108
## Dffclt.8   -1.0019     0.3521 -2.8458
## Dffclt.9   -1.4798     0.4182 -3.5387
## Dscrmn      1.8186     0.3370  5.3960
## 
## Integration:
## method: Gauss-Hermite
## quadrature points: 21 
## 
## Optimization:
## Convergence: 0 
## max(|grad|): 0.002 
## quasi-Newton: BFGS
```

----
## One last note
For many models, you can get tidy output using the *broom* package (part of the *tidyverse*)


```r
lmd <- td %>% 
  distinct(stu_name, .keep_all = TRUE) %>% 
  mutate(gender = as.factor(gender))

mod <- lm(raw_score ~ gender, data = lmd)
```

----

```r
library(broom)
tidy(mod, conf.int = TRUE)
```

```
##          term  estimate std.error statistic      p.value  conf.low
## 1 (Intercept) 9.4444444 0.5676249 16.638531 1.300394e-17  8.289603
## 2     genderM 0.6143791 0.8144623  0.754337 4.559964e-01 -1.042657
##   conf.high
## 1 10.599286
## 2  2.271415
```

```r
glance(mod)
```

```
##    r.squared adj.r.squared    sigma statistic   p.value df    logLik
## 1 0.01695088   -0.01283849 2.408228 0.5690244 0.4559964  2 -79.39434
##        AIC      BIC deviance df.residual
## 1 164.7887 169.4547 191.3856          33
```

----
Broom is particularly useful for things like plotting. The below code will work for any linear model (with any number of predictors)


```r
tidy_mod <- tidy(mod, conf.int = TRUE)
ggplot(tidy_mod, aes(estimate, term, color = term)) +
  geom_errorbarh(aes(xmin = conf.low, xmax = conf.high)) +
    geom_vline(xintercept = 0)
```

![plot of chunk broom_plot](assets/fig/broom_plot-1.png)

---- .segue
# Lab

----
## *iris* data
Already available to you as soon as you launch R. 


```r
data(iris)
head(iris)
str(iris)
summary(iris)
View(iris)
```


| Sepal.Length| Sepal.Width| Petal.Length| Petal.Width|Species |
|------------:|-----------:|------------:|-----------:|:-------|
|          5.1|         3.5|          1.4|         0.2|setosa  |
|          4.9|         3.0|          1.4|         0.2|setosa  |
|          4.7|         3.2|          1.3|         0.2|setosa  |
|          4.6|         3.1|          1.5|         0.2|setosa  |
|          5.0|         3.6|          1.4|         0.2|setosa  |
|          5.4|         3.9|          1.7|         0.4|setosa  |

----
## Work with a neighbor to
* Identify the variables (not the column names, the variables)
* Sketch how these data would look  in a tidy form (at least mentally)
* What would be the first step in tidying these data? (use `View(iris)` to see the full dataset easier). 

Work with a partner to try to conduct the first step in tidying the data

----
## Step 1: Gather the sepal and petal columns


```r
iris %>% 
  tbl_df() %>% 
  gather(flower_part, measurement, -Species)
```

```
## # A tibble: 600 × 3
##    Species  flower_part measurement
##     <fctr>        <chr>       <dbl>
## 1   setosa Sepal.Length         5.1
## 2   setosa Sepal.Length         4.9
## 3   setosa Sepal.Length         4.7
## 4   setosa Sepal.Length         4.6
## 5   setosa Sepal.Length         5.0
## 6   setosa Sepal.Length         5.4
## 7   setosa Sepal.Length         4.6
## 8   setosa Sepal.Length         5.0
## 9   setosa Sepal.Length         4.4
## 10  setosa Sepal.Length         4.9
## # ... with 590 more rows
```

----
## iris data
What needs to happen next? Are the data tidy?

>* Work with a partner to figure out the next step, and try to do it.
    + *Hint:* Use `"\\."` as your separator rather than just `"."`. I'll explain why momentarily.

----
## Step 2: Separate the flower_part column


```r
iris %>% 
  tbl_df() %>% 
  gather(flower_part, measurement, -Species) %>% 
  separate(flower_part, c("flower_part", "measure_of"), sep = "\\.")
```

```
## # A tibble: 600 × 4
##    Species flower_part measure_of measurement
## *   <fctr>       <chr>      <chr>       <dbl>
## 1   setosa       Sepal     Length         5.1
## 2   setosa       Sepal     Length         4.9
## 3   setosa       Sepal     Length         4.7
## 4   setosa       Sepal     Length         4.6
## 5   setosa       Sepal     Length         5.0
## 6   setosa       Sepal     Length         5.4
## 7   setosa       Sepal     Length         4.6
## 8   setosa       Sepal     Length         5.0
## 9   setosa       Sepal     Length         4.4
## 10  setosa       Sepal     Length         4.9
## # ... with 590 more rows
```

----
## iris data
What needs to happen next? Are the data tidy?

>* Nothing! They are tidy! Just need to store them in an object.
>* Now... Calculate the average Sepal Width by Species.
>* Explore the data in a few other ways of your choosing.

----
## Some descriptives

```r
iris_tidy <- iris %>% 
  tbl_df() %>% 
  gather(flower_part, measurement, -Species) %>% 
  separate(flower_part, c("flower_part", "measure_of"), sep = "\\.")

iris_tidy %>% 
  group_by(Species, measure_of) %>% 
  summarize(mean = mean(measurement)) %>% 
  filter(measure_of == "Width")
```

```
## Source: local data frame [3 x 3]
## Groups: Species [3]
## 
##      Species measure_of  mean
##       <fctr>      <chr> <dbl>
## 1     setosa      Width 1.837
## 2 versicolor      Width 2.048
## 3  virginica      Width 2.500
```

----
## Some more descriptives


```r
iris_tidy %>% 
  group_by(Species, flower_part, measure_of) %>% 
  summarize(mean = mean(measurement), 
            standard_dev = sd(measurement),
            iqr = IQR(measurement))
```

```
## Source: local data frame [12 x 6]
## Groups: Species, flower_part [?]
## 
##       Species flower_part measure_of  mean standard_dev   iqr
##        <fctr>       <chr>      <chr> <dbl>        <dbl> <dbl>
## 1      setosa       Petal     Length 1.462    0.1736640 0.175
## 2      setosa       Petal      Width 0.246    0.1053856 0.100
## 3      setosa       Sepal     Length 5.006    0.3524897 0.400
## 4      setosa       Sepal      Width 3.428    0.3790644 0.475
## 5  versicolor       Petal     Length 4.260    0.4699110 0.600
## 6  versicolor       Petal      Width 1.326    0.1977527 0.300
## 7  versicolor       Sepal     Length 5.936    0.5161711 0.700
## 8  versicolor       Sepal      Width 2.770    0.3137983 0.475
## 9   virginica       Petal     Length 5.552    0.5518947 0.775
## 10  virginica       Petal      Width 2.026    0.2746501 0.500
## 11  virginica       Sepal     Length 6.588    0.6358796 0.675
## 12  virginica       Sepal      Width 2.974    0.3224966 0.375
```
