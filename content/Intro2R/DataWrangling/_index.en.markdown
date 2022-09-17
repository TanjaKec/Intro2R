---
date: "2016-04-09T16:50:16+02:00"
title: Data Wrangling
output: 
  learnr::tutorial
weight: 1
---

### How do we do it? 🤔

![Red variant](/day2/DataWrangling/images/Program_HW.png?width=40pc)

This diagram is taken from the [R for Data Science](https://r4ds.had.co.nz/) book by [Garrett Grolemund](https://twitter.com/statgarrett
) and [Hadley Wickham](https://twitter.com/hadleywickham), which it is a great resource for learning R. There is a whole community built around it and you could join it and start learning together: [R4DS online learning community](https://www.rfordatasci.com/).

### Dataset

To learn and practise how to organise data we will use a `gapminder` data set available from the `gapminder` package in R. This dataset is put into R by [Jennifer Bryan](https://jennybryan.org/) from a tank of data sets available from [Gapminder](https://www.gapminder.org).

[Gapminder](https://www.gapminder.org) is an independent Swedish foundation that helps to promote sustainable global development by collecting and analysing relevant data and by developing and designing  teaching/learning tools. [Gapminder](https://en.wikipedia.org/wiki/Gapminder_Foundation) was founded in Sweden by [Hans Rosling](https://en.wikipedia.org/wiki/Hans_Rosling) who was a mastermind for distinctive and insightful storytelling about global development using visual animation.

You can see Hans in action in this [BBC documentary](https://www.bbc.co.uk/programmes/p02q33dg) [The joy of Stats](https://www.youtube.com/watch?v=cdf0k545yDA) available on [YouTube](https://www.youtube.com).

##### Gapminder Data

For each of 142 countries, the package provides values for life expectancy, GDP per capita, and population, every five years, from 1952 to 2007.

Before you can take a look at this data set first run the folowing code

```
# install necessary packages:
install.packages("dplyr", repos = "http://cran.us.r-project.org")
install.packages("ggplot2", repos = "http://cran.us.r-project.org")
install.packages("gapminder", repos = "http://cran.us.r-project.org")
```


```r
# have a look at the data 
head(gapminder::gapminder)
```

```
## # A tibble: 6 x 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
## 4 Afghanistan Asia       1967    34.0 11537966      836.
## 5 Afghanistan Asia       1972    36.1 13079460      740.
## 6 Afghanistan Asia       1977    38.4 14880372      786.
```

{{% notice note %}}
💡Note that there are 6 columns, each of which we call a variable.
{{% /notice %}}

**Description**: Excerpt of the Gapminder data on life expectancy, GDP per capita, and population by country.

The main data frame gapminder has **1704 rows** and **6 variables**:
- **country**: factor with 142 levels
- **continent**: factor with 5 levels
- **year**: ranges from 1952 to 2007 in increments of 5 years
- **lifeExp**: life expectancy at birth, in years
- **pop**: population
- **gdpPercap**: GDP per capita


```r
gapminder::gapminder[1:3,]
```

```
## # A tibble: 3 x 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
```

1st look at the data using the following functions: <span style="color:red">`dim()`</span> & <span style="color:dim">`head()`</span>


```r
library(gapminder)
dim(gapminder)
```

```
## [1] 1704    6
```

```r
head(gapminder, n=10)
```

```
## # A tibble: 10 x 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.
##  2 Afghanistan Asia       1957    30.3  9240934      821.
##  3 Afghanistan Asia       1962    32.0 10267083      853.
##  4 Afghanistan Asia       1967    34.0 11537966      836.
##  5 Afghanistan Asia       1972    36.1 13079460      740.
##  6 Afghanistan Asia       1977    38.4 14880372      786.
##  7 Afghanistan Asia       1982    39.9 12881816      978.
##  8 Afghanistan Asia       1987    40.8 13867957      852.
##  9 Afghanistan Asia       1992    41.7 16317921      649.
## 10 Afghanistan Asia       1997    41.8 22227415      635.
```

Can you tell what each of the two functions does?! 

Do we have the information about the structure of the data? 🤔 We can examine the structure using <span style="color:red">`str()`</span> function, but the **output could look messy** and hard to follow if the data set is big. 🤪


```r
str(gapminder) 
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	1704 obs. of  6 variables:
##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
##  $ gdpPercap: num  779 821 853 836 740 ...
```

#### The `dplyr` Package

The <span style="color:red">**`dplyr`**</span> provides a “grammar” (the verbs) for data manipulation and for operating on data frames in a tidy way. The key operator and the essential verbs are:

<span style="color:red">%>%</span>: the “pipe” operator used to connect multiple verb actions together into a pipeline.

<span style="color:red">select()</span>: return a subset of the columns of a data frame.

<span style="color:red">mutate()</span>: add new variables/columns or transform existing variables.

<span style="color:red">filter()</span>: extract a subset of rows from a data frame based on logical conditions.

<span style="color:red">arrange()</span>: reorder rows of a data frame according to single or multiple variables.

<span style="color:red">summarise() / summarize()</span>: reduce each group to a single row by calculating aggregate measures.

We can have a look at the data and its structure by using the <span style="color:red">`glimpse()`</span> function from the `dplyr` package.


```r
suppressPackageStartupMessages(library(dplyr))
glimpse(gapminder) 
```

```
## Observations: 1,704
## Variables: 6
## $ country   <fct> Afghanistan, Afghanistan, Afghanistan, Afghanistan, Af…
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, …
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, …
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854…
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 148803…
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.…
```

{{% notice note %}}
🤓💡: Notice how we can prevent display of the messages that appear when uploading the packages by using, in this case, `suppressPackageStartupMessages()`!
{{% /notice %}}

Now we have the `dplyr` package uploaded, let us learn its verbs. 😇

#### The pipeline operater: <span style="color:red">`%>%`</span>

**Left Hand Side (LHS)**   <span style="color:red">`%>%`</span>    **Right Hand Side (RHS)**

<span style="color:red">x %>% f(..., y)</span> 

<span style="color:red">    f(x,y)</span>

The "pipe" **passes the result of the *LHS as the 1st operator argument of the function on the RHS**.

<pre>
<span style="color:red">3 %>% sum(4)</span>      <==>      <span style="color:red">  sum(3, 4)</span>
</pre>

<span style="color:red">`%>%`</span> is very practical for chaining together multiple <span style="color:red">`dplyr`</span> functions in a sequence of operations.

#### Pick variables by their names: <span style="color:red">`select()`</span>,

<img src="images/select().png" width="450px" />

- <span style="color:red">`starts_with("X")`</span> every name that starts with "X".

- <span style="color:red">`ends_with("X")`</span> every name that ends with "X".

- <span style="color:red">`contains("X")`</span> every name that contains "X".

- <span style="color:red">`matches("X")`</span> every name that matches "X", where "X" can be a regular expression.

- <span style="color:red">`num_range("x", 1:5)`</span>  the variables named x01, x02, x03, x04, x05.

- <span style="color:red">`one_of(x)`</span> => every name that appears in x, which should be a character vector.

##### 👉 Practice ⏰💻: Select your variables 

1) that ends with letter `p`

2) starts with letter `co`. Try to do this selection using base R.  

##### 😃🙌 Solutions:


```r
gm_pop_gdp <- select(gapminder, ends_with("p"))
head(gm_pop_gdp, n = 1)
```

```
## # A tibble: 1 x 3
##   lifeExp     pop gdpPercap
##     <dbl>   <int>     <dbl>
## 1    28.8 8425333      779.
```

```r
gm_cc <- select(gapminder, starts_with("co"))
head(gm_cc, n = 1)
```

```
## # A tibble: 1 x 2
##   country     continent
##   <fct>       <fct>    
## 1 Afghanistan Asia
```

of course all of this could be done using **base R** for example:


```r
gm_cc <- gapminder[c("country", "continent")]
```

but it's less intuitive and often requires more typing. 

#### Create new variables of existing variables: <span style="color:red">`mutate()`</span>

<img src="images/mutate().png" width="400px" />

It will allow you to add to the data frame `df` a new column, `z`, which is the multiplication of the columns `x` and `y`: `mutate(df, z = x * y)`.
If we would like to observe `lifeExp` measured in months we could create a new column `lifeExp_month`: 

```r
gapminder2 <- mutate(gapminder, LifeExp_month = lifeExp * 12) 
head(gapminder2, n = 2)
```

```
## # A tibble: 2 x 7
##   country     continent  year lifeExp     pop gdpPercap LifeExp_month
##   <fct>       <fct>     <int>   <dbl>   <int>     <dbl>         <dbl>
## 1 Afghanistan Asia       1952    28.8 8425333      779.          346.
## 2 Afghanistan Asia       1957    30.3 9240934      821.          364.
```

#### Pick observations by their values: <span style="color:red">`filter()`</span>
<img src="images/filter().png" width="450px" />

There is a set of logical operators in **R** that you can use inside `filter()`:

- `x < y`: `TRUE` if `x` is less than `y`
- `x <= y`: `TRUE` if `x` is less than or equal to `y`
- `x == y`: `TRUE` if `x` equals `y`
- `x != y`: `TRUE` if `x` does not equal `y`
- `x >= y`: `TRUE` if `x` is greater than or equal to `y`
- `x > y`: `TRUE` if `x` is greater than `y`
- `x %in% c(a, b, c)`: `TRUE` if `x` is in the vector `c(a, b, c)`
- `is.na(x)`:  Is `NA`
- `!is.na(x)`: Is not `NA`

##### 👉 Practice ⏰💻: Filter your data

Use `gapminder2` `df` to filter:

1) only European countries and save it as `gapmEU`

2) only European countries from 2000 onward and save it as `gapmEU21c`

3) rows where the life expectancy is greater than 80

Don't forget to **use `==` instead of `=`**! and
Don't forget the quotes **`""`**


##### 😃🙌 Solutions:

```r
gapmEU <- filter(gapminder2, continent == "Europe") 
head(gapmEU, 2)
```

```
## # A tibble: 2 x 7
##   country continent  year lifeExp     pop gdpPercap LifeExp_month
##   <fct>   <fct>     <int>   <dbl>   <int>     <dbl>         <dbl>
## 1 Albania Europe     1952    55.2 1282697     1601.          663.
## 2 Albania Europe     1957    59.3 1476505     1942.          711.
```


```r
gapmEU21c <- filter(gapminder2, continent == "Europe" & year >= 2000)
head(gapmEU21c, 2)
```

```
## # A tibble: 2 x 7
##   country continent  year lifeExp     pop gdpPercap LifeExp_month
##   <fct>   <fct>     <int>   <dbl>   <int>     <dbl>         <dbl>
## 1 Albania Europe     2002    75.7 3508512     4604.          908.
## 2 Albania Europe     2007    76.4 3600523     5937.          917.
```


```r
filter(gapminder2, lifeExp > 80)
```

#### Reorder the rows: <span style="color:red">`arrange()`</span>
is used to reorder rows of a **d**ata **f**rame (df) according to one of the variables/columns.

<img src="images/arrange().png" width="300px" />

- If you pass `arrange()` a character variable, **R** will rearrange the rows in alphabetical order according to values of the variable. 

- If you pass a factor variable, **R** will rearrange the rows according to the order of the levels in your factor (running `levels()` on the variable reveals this order).

##### 👉 Practice ⏰💻: Arranging your data
1) Arrange countries in `gapmEU21c` `df` by life expectancy in ascending and descending order.

2) Using `gapminder df`
  - Find the records with the smallest population
  
  - Find the records with the largest life expectancy.

##### 😃🙌 Solution 1):

```r
gapmEU21c_h2l <- arrange(gapmEU21c, lifeExp)
head(gapmEU21c_h2l, 2)
```

```
## # A tibble: 2 x 7
##   country continent  year lifeExp      pop gdpPercap LifeExp_month
##   <fct>   <fct>     <int>   <dbl>    <int>     <dbl>         <dbl>
## 1 Turkey  Europe     2002    70.8 67308928     6508.          850.
## 2 Romania Europe     2002    71.3 22404337     7885.          856.
```

```r
gapmEU21c_l2h <- arrange(gapmEU21c, desc(lifeExp)) # note the use of desc()
head(gapmEU21c_l2h, 2)
```

```
## # A tibble: 2 x 7
##   country     continent  year lifeExp     pop gdpPercap LifeExp_month
##   <fct>       <fct>     <int>   <dbl>   <int>     <dbl>         <dbl>
## 1 Iceland     Europe     2007    81.8  301931    36181.          981.
## 2 Switzerland Europe     2007    81.7 7554661    37506.          980.
```

##### 😃🙌 Solution 2):

```r
arrange(gapminder, pop)
```

```
## # A tibble: 1,704 x 6
##    country               continent  year lifeExp   pop gdpPercap
##    <fct>                 <fct>     <int>   <dbl> <int>     <dbl>
##  1 Sao Tome and Principe Africa     1952    46.5 60011      880.
##  2 Sao Tome and Principe Africa     1957    48.9 61325      861.
##  3 Djibouti              Africa     1952    34.8 63149     2670.
##  4 Sao Tome and Principe Africa     1962    51.9 65345     1072.
##  5 Sao Tome and Principe Africa     1967    54.4 70787     1385.
##  6 Djibouti              Africa     1957    37.3 71851     2865.
##  7 Sao Tome and Principe Africa     1972    56.5 76595     1533.
##  8 Sao Tome and Principe Africa     1977    58.6 86796     1738.
##  9 Djibouti              Africa     1962    39.7 89898     3021.
## 10 Sao Tome and Principe Africa     1982    60.4 98593     1890.
## # … with 1,694 more rows
```


```r
arrange(gapminder, desc(lifeExp))
```

```
## # A tibble: 1,704 x 6
##    country          continent  year lifeExp       pop gdpPercap
##    <fct>            <fct>     <int>   <dbl>     <int>     <dbl>
##  1 Japan            Asia       2007    82.6 127467972    31656.
##  2 Hong Kong, China Asia       2007    82.2   6980412    39725.
##  3 Japan            Asia       2002    82   127065841    28605.
##  4 Iceland          Europe     2007    81.8    301931    36181.
##  5 Switzerland      Europe     2007    81.7   7554661    37506.
##  6 Hong Kong, China Asia       2002    81.5   6762476    30209.
##  7 Australia        Oceania    2007    81.2  20434176    34435.
##  8 Spain            Europe     2007    80.9  40448191    28821.
##  9 Sweden           Europe     2007    80.9   9031088    33860.
## 10 Israel           Asia       2007    80.7   6426679    25523.
## # … with 1,694 more rows
```

#### Collapse many values down to a single summary: <span style="color:red">`summarise()`</span>

<img src="images/summarise().png" width="450px" />

The syntax of summarise() is simple and consistent with the other verbs included in the `dplyr` package.

- uses the same syntax as `mutate()`, but the resulting dataset consists of a single row instead of an entire new column in the case of `mutate()`. 

- builds a new dataset that contains only the summarising statistics.

| Objective | Function                | Description                    |
| --------- | ----------------------- | ------------------------------ |
| basic     | `sum(x)`                | Sum of vector x                |
| centre    | `mean(x)`               | Mean (average) of vector x     |
|           | `median(x)`             | Median of vector x             | 
| spread    | `sd(x)`                 | Standard deviation of vector x |
|           | `quantile(x, probs)`    | Quantile of vector x           |
|           | `range(x)`              | Range of vector x              |
|           | `diff(range(x)))`       | Width of the range of vector x |
|           | `min(x)`                | Min of vector x                |
|           | `max(x)`                | Max of vector x                |
|           | `abs(x)`                | Absolute value of a number x   | 

##### 👉 Practice ⏰💻: Use `summarise()`:

1) to print out a summary of gapminder containing two variables: max_lifeExp and max_gdpPercap.

2) to print out a summary of gapminder containing two variables: mean_lifeExp and mean_gdpPercap.

##### 😃🙌 Solution: Summarise your data


```r
summarise(gapminder, max_lifeExp = max(lifeExp), max_gdpPercap = max(gdpPercap))
```

```
## # A tibble: 1 x 2
##   max_lifeExp max_gdpPercap
##         <dbl>         <dbl>
## 1        82.6       113523.
```


```r
summarise(gapminder, mean_lifeExp = mean(lifeExp), mean_gdpPercap = mean(gdpPercap))
```

```
## # A tibble: 1 x 2
##   mean_lifeExp mean_gdpPercap
##          <dbl>          <dbl>
## 1         59.5          7215.
```

#### Subsetting: <span style="color:red">`group_by()`</span>

dplyr's `group_by()` function enables you to group your data. It allows you to create a separate df that splits the original df by a variable.

The function `summarise()` can be combined with `group_by()`.

| Objective | Function                | Description                               |
| --------- | ----------------------- | ----------------------------------------- |
| Position	| first()	                | First observation of the group            |
|           | last()	                | Last observation of the group             |
|           | nth()	                  | n-th observation of the group             |
| Count	    | n()	                    | Count the number of rows                  |
|           | n_distinct()	          | Count the number of distinct observations |


##### 👉 Practice ⏰💻: Subset your data

1) Identify how many countries are given in gapminder data for each continent.

##### 😃🙌 Solution: 


```r
gapminder %>%
     group_by(continent) %>%
     summarise(n_distinct(country))
```

```
## # A tibble: 5 x 2
##   continent `n_distinct(country)`
##   <fct>                     <int>
## 1 Africa                       52
## 2 Americas                     25
## 3 Asia                         33
## 4 Europe                       30
## 5 Oceania                       2
```

#### Let's `%>%` all up!

You can try to get into the habit of using a shortcut for the pipe operator 
<img src="images/pipe_short_cut.png" width="450px" style="display: block; margin: auto;" />

##### 🗣👥 Confer with your neighbours: 
What relationship do you expect to see between population size (`pop`) and life expectancy (`lifeExp`)?

**Look what this code produces**


```r
gapminder_pipe <- gapminder %>%
  filter(continent == "Europe" & year ==  2007) %>%
  mutate(pop_e6 = pop / 1000000)
plot(gapminder_pipe$pop_e6, gapminder_pipe$lifeExp, cex = 0.5, col = "red")
```

<img src="/module2/DataWrangling/_index.en_files/figure-html/unnamed-chunk-24-1.png" width="576" style="display: block; margin: auto;" />

#### `tidyr`

The `tidyr` can help you to create **tidy data**. Tidy data is data where:

- Every **column** is a **variable**
- Every **row** is an **observation**
- Every **cell** is a **single value**

<img src="images/tidyr.png" width="450px" />

The `tidyr` package embraces the **principles of tidy data** and provides the standard key functionalities to organise data values within a dataset.

[Hadley Wickham](http://hadley.nz/) the author of the `tidyr` package talks in his paper [Tidy Data](https://vita.had.co.nz/papers/tidy-data.pdf) about the importance of the data cleaning process and structuring datasets to facilitate data analysis.

Real datasets, that you are most likely to download from <https://data.gov.rs/> or any other open source data platform, would often violate the three precepts of tidy data in all kinds of different ways:

- Variables would not have their names and column headers are values.
-	A number of variables are stored in one column
-	A single variable is stored in several columns 
-	Same information stored multiple times as different variables
 
to name a few.

To illustrate this, let us go back onto <https://data.gov.rs/> and access [Kvalitet Vazduha 2017](https://data.gov.rs/sr/datasets/kvalitet-vazduha-2017/). In particular, we want to access [2017-NO2.csv](http://data.sepa.gov.rs/dataset/ca463c44-fbfa-4de9-9a75-790995bf2830/resource/74516688-5fb5-47b2-becc-6b6e31a24d80/download/2017-no2.csv) data.


```r
## If you don't have tidyr installed yet, uncomment and run the line below
#install.packeges("tidyr")
library(tidyr)
# access 2017-NO2.csv data
no2 <- read.csv("http://data.sepa.gov.rs/dataset/ca463c44-fbfa-4de9-9a75-790995bf2830/resource/74516688-5fb5-47b2-becc-6b6e31a24d80/download/2017-no2.csv",
                stringsAsFactors = FALSE, 
                fileEncoding = "latin1")
# have a look at the data
glimpse(no2)
```

```
## Observations: 365
## Variables: 8
## $ Datum                   <chr> "01.01.2017", "02.01.2017", "03.01.2017"…
## $ Novi.Sad.SPENS.NO2      <dbl> 22.89, 32.94, 14.86, 22.73, 20.89, 10.47…
## $ Beograd.Mostar.NO2      <dbl> 28.83, 39.12, 25.20, 28.48, 27.82, 41.91…
## $ Beograd.Vraèar.NO2      <dbl> 182.84, 244.56, 147.49, 159.12, 124.16, …
## $ Beograd.Zeleno.brdo.NO2 <dbl> 36.32, 36.68, 27.00, 35.15, 24.92, 8.27,…
## $ Kragujevac..NO2         <dbl> 38.92, 43.50, 40.17, 45.44, 43.04, 34.55…
## $ U.ice..NO2              <dbl> 49.84, 50.34, 36.35, 49.07, 33.33, 39.88…
## $ Ni..IZJZ.Ni...NO2       <dbl> 18.61, 22.87, 15.68, 21.97, 21.68, 12.11…
```

It shows that this data set has `365` observations and `8` variables. Nonetheless, we need to consider what type of information we have here:

- `date` NO2 measurement taken: given in a single column -> ✅ tidy🙂
- `places` where NO2 measurement was taken: given in several columns -> ❎ tidy🙁
- `NO2` measurements: given in several columns -> ❎ tidy🙁

Hmmm… 🤔 This doesn’t look tidy at all 😳

This data is about measurement level of NO2(µg/m3) in several different towns/places, which means that NO2 is our main response variable. The way in which this variable is given in this data is certainly not tidy. It defeats the key principles of tidy data: **Every column is a variable** and furthermore, **Every row is <span style="color:orangered">NOT</span> an observation**.

It appears that this data has `8` variables, but we have realised that there are only `3`: `date`, `place` and `no2`. To tidy it, we need to **stack it** by turning columns into rows. We are happy with the variable `date` and it should remain as a single column, the other `7` columns we want to convert into two variables: `place` and `no2`.

To make *wide format* data into *tall format* we have to turn columns into rows using `gather()` function.

<img src="images/gather.png" width="450px" />

We will create variable `place` in which we will hold the headers as given in the columns 2:8. The values inside those columns will be saved in the new variable `no2`.


```r
new_no2 <- no2 %>%
  gather("place", "no2", -Datum, factor_key = TRUE) # stack all columns apart from `Datum`
glimpse(new_no2)
```

```
## Observations: 2,555
## Variables: 3
## $ Datum <chr> "01.01.2017", "02.01.2017", "03.01.2017", "04.01.2017", "0…
## $ place <fct> Novi.Sad.SPENS.NO2, Novi.Sad.SPENS.NO2, Novi.Sad.SPENS.NO2…
## $ no2   <dbl> 22.89, 32.94, 14.86, 22.73, 20.89, 10.47, 9.58, 15.99, 14.…
```

Let us see the names of the places


```r
new_no2 %>%
     group_by(place) %>%
     summarise(n())
```

```
## # A tibble: 7 x 2
##   place                   `n()`
##   <fct>                   <int>
## 1 Novi.Sad.SPENS.NO2        365
## 2 Beograd.Mostar.NO2        365
## 3 Beograd.Vraèar.NO2        365
## 4 Beograd.Zeleno.brdo.NO2   365
## 5 Kragujevac..NO2           365
## 6 U.ice..NO2                365
## 7 Ni..IZJZ.Ni...NO2         365
```

Those names look very messy. We could use function from [`stringr`](https://stringr.tidyverse.org) package `str_sub()`. To begin with let's remove <span style="color:orangered">.NO2</span> at the end of each name.


```r
## If you don't have stringr installed yet, uncomment and run the line below
#install.packages("stringr")
library(stringr)
new_no2$place <- new_no2$place %>% 
    str_sub(end = -5)
glimpse(new_no2)
```

```
## Observations: 2,555
## Variables: 3
## $ Datum <chr> "01.01.2017", "02.01.2017", "03.01.2017", "04.01.2017", "0…
## $ place <chr> "Novi.Sad.SPENS", "Novi.Sad.SPENS", "Novi.Sad.SPENS", "Nov…
## $ no2   <dbl> 22.89, 32.94, 14.86, 22.73, 20.89, 10.47, 9.58, 15.99, 14.…
```

```r
new_no2 %>%
    group_by(place) %>%
    summarise(n())
```

```
## # A tibble: 7 x 2
##   place               `n()`
##   <chr>               <int>
## 1 Beograd.Mostar        365
## 2 Beograd.Vraèar        365
## 3 Beograd.Zeleno.brdo   365
## 4 Kragujevac.           365
## 5 Ni..IZJZ.Ni..         365
## 6 Novi.Sad.SPENS        365
## 7 U.ice.                365
```

It still doesn't look right. 😟 This could be a tedious job. 😥 It is no wonder that many data analysts grumble about time spent on the process of cleaning and preparing the data. It could be a very long and time consuming process, but the more you do it and the more experience you gain the easier and less painful it gets.


Perhaps, you can try to explore other available packages in R that could help you with organising your data into your ideal format. To give you an idea we will show you how it can easily be done when using `forcats::fct_recode()` function.


```r
## If you don't have forcats installed yet, uncomment and run the line below
#install.packages("forcats")
library(forcats)
new_no2 <- no2 %>%
  gather("place", "no2", -Datum, factor_key = TRUE) %>% # stack all columns apart from `Datum`
  mutate(place = fct_recode(place, 
                            "NS_Spens" = "Novi.Sad.SPENS.NO2",
                            "BG_Most" = "Beograd.Mostar.NO2",
                            "BG_Vracar" = "Beograd.Vraèar.NO2", 
                            "BG_ZelenoBrdo" = "Beograd.Zeleno.brdo.NO2", 
                            "KG" = "Kragujevac..NO2", 
                            "NI" = "Ni..IZJZ.Ni...NO2",
                            "UZ" = "U.ice..NO2"))
glimpse(new_no2)
```

```
## Observations: 2,555
## Variables: 3
## $ Datum <chr> "01.01.2017", "02.01.2017", "03.01.2017", "04.01.2017", "0…
## $ place <fct> NS_Spens, NS_Spens, NS_Spens, NS_Spens, NS_Spens, NS_Spens…
## $ no2   <dbl> 22.89, 32.94, 14.86, 22.73, 20.89, 10.47, 9.58, 15.99, 14.…
```

{{% notice note %}}
By now, you should have gained enough knowledge in using R to give you the necessary confidence to start exploring other functions of the [`tidyr`](https://tidyr.tidyverse.org/) package. You should not stop there, but go beyond and explore the whole of the [`tidyverse`](https://www.tidyverse.org/) opinionated collection of R packages for data science. 😇🎶
{{% /notice %}}

To learn more about **tidy data in r** check [Data Tidying](https://garrettgman.ithuthe b.io/tidying/) section from the famous [Data Science with R](https://garrettgman.github.io) by [Garrett Grolemund](https://resources.rstudio.com/authors/garrett-grolemund)

{{% notice tip %}}
Have you tried learning data science by posting your questions and discussing it with other people within the R community? 👥💻📊📈🗣 [RStudio Community](https://community.rstudio.com)
{{% /notice %}}

## YOUR TURN 👇

Practise by doing the following set of exercises:

1) Install and upload the `rattle` package and see what it does.

2) Create a new R script file to explore `weatherAUS` dataset. 

  i) `select()` variable: `MinTemp`, `MaxTemp`, `Rainfall` and `Sunshine` by *pipping* the dataset into `dplyr::select() function.
  
  ii) produce a *summary*  using `base::summary()` function of these numeric values.
  
  iii) within this selection filter only those observations where `Rainfall >= 1` and save the results into the computer's memory (ie. save the results as an object).
  
  iv) Try to think of how else you can use other `dplyr` verbs on this `weatherAUS` dataset. Write your question first, before embarking on typing the code.
  
  v) Write a short report on what visualisation you think would be interesting to produce for this `weatherAUS` dataset and why?

##### Useful Links

[Data Wrangling cheat sheet](https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf)

[Data Transformation with dplyr cheat sheet](https://4.files.edl.io/b9e2/07/12/19/142839-a23788fb-1d3a-4665-9dc4-33bfd442c296.pdf)

-----------------------------
© 2020 Tatjana Kecojevic
