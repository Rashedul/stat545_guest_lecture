cm008 Exercises
================

Install `nycflights13` package
------------------------------

``` r
install.packages("nycflights13")
```

``` r
suppressPackageStartupMessages(library(tidyverse))
suppressPackageStartupMessages(library(nycflights13))
```

Types of mutating join
----------------------

### Let's join data.frames using four mutating functions:

-   create two data.frames named `a` and `b`, similar to Data Wrangling Cheatsheet
-   use `left_join`, `right_join`, `inner_join` and `full_join` functions
-   example for `left_join`: Join matching rows from b to a
-   example for `right_join`: Join matching rows from a to b
-   example for `inner_join`: Join data. Retain only rows in both sets
-   example for `full_join`: Join data. Retain all values, all rows
-   example of using two different variables from two datasets
-   example of two variables have identical names

``` r
#create two data.frames named a and b
(a <- data.frame(x1 = LETTERS[1:3], x2 = 1:3))
```

    ##   x1 x2
    ## 1  A  1
    ## 2  B  2
    ## 3  C  3

``` r
(b <- data.frame(x1 = LETTERS[c(1,2,4)], x3 = c("T", "F", "T")))
```

    ##   x1 x3
    ## 1  A  T
    ## 2  B  F
    ## 3  D  T

``` r
#left_join: Join matching rows from b to a
left_join(a, b, by = "x1")
```

    ## Warning: Column `x1` joining factors with different levels, coercing to
    ## character vector

    ##   x1 x2   x3
    ## 1  A  1    T
    ## 2  B  2    F
    ## 3  C  3 <NA>

``` r
#right_join: Join matching rows from a to b.
right_join(a, b, by = "x1")
```

    ## Warning: Column `x1` joining factors with different levels, coercing to
    ## character vector

    ##   x1 x2 x3
    ## 1  A  1  T
    ## 2  B  2  F
    ## 3  D NA  T

``` r
#inner_join: Join data. Retain only rows in both sets
inner_join(a, b, by = "x1")
```

    ## Warning: Column `x1` joining factors with different levels, coercing to
    ## character vector

    ##   x1 x2 x3
    ## 1  A  1  T
    ## 2  B  2  F

``` r
#full_join: Join data. Retain all values, all rows.
full_join(a, b, by = "x1")
```

    ## Warning: Column `x1` joining factors with different levels, coercing to
    ## character vector

    ##   x1 x2   x3
    ## 1  A  1    T
    ## 2  B  2    F
    ## 3  C  3 <NA>
    ## 4  D NA    T

``` r
#what happen if we do not specify by option?
left_join(a, b)
```

    ## Joining, by = "x1"

    ## Warning: Column `x1` joining factors with different levels, coercing to
    ## character vector

    ##   x1 x2   x3
    ## 1  A  1    T
    ## 2  B  2    F
    ## 3  C  3 <NA>

``` r
#what happen if we specify two different variables from two data.frames?
left_join(a, b, by = c("x1" = "x3"))
```

    ## Warning: Column `x1`/`x3` joining factors with different levels, coercing
    ## to character vector

    ##   x1 x2 x1.y
    ## 1  A  1 <NA>
    ## 2  B  2 <NA>
    ## 3  C  3 <NA>

``` r
#what happen if two columns have the identical names
(c <- data.frame(x1 = c(LETTERS[1:2],"x"), x2 = c(1,4,5)))
```

    ##   x1 x2
    ## 1  A  1
    ## 2  B  4
    ## 3  x  5

``` r
inner_join(a, c)
```

    ## Joining, by = c("x1", "x2")

    ## Warning: Column `x1` joining factors with different levels, coercing to
    ## character vector

    ##   x1 x2
    ## 1  A  1

`nycflights13` dataset has four tibbles e.g., `flights`, `airports`, `planes` and `weather`.
--------------------------------------------------------------------------------------------

### Explore and subset data:

-   Explore `nycflights13` dataset
-   to reduce the running time, subset `flights` data.frame taking first 1000 rows and year, tailnum, carrier, time\_hour columns.

### Practice Exercises:

-   check which variables are common in `weather` and `flights2` datasets
-   add `weather` information to the `flights2` dataset by matching "year" and "time\_hour" variables. -add `weather` information to the `flights2` dataset by matching only "time\_hour" variable.

``` r
#check the tibbles included in `nycflights13` package
class(flights)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

``` r
colnames(flights)
```

    ##  [1] "year"           "month"          "day"            "dep_time"      
    ##  [5] "sched_dep_time" "dep_delay"      "arr_time"       "sched_arr_time"
    ##  [9] "arr_delay"      "carrier"        "flight"         "tailnum"       
    ## [13] "origin"         "dest"           "air_time"       "distance"      
    ## [17] "hour"           "minute"         "time_hour"

``` r
colnames(airports)
```

    ## [1] "faa"   "name"  "lat"   "lon"   "alt"   "tz"    "dst"   "tzone"

``` r
colnames(planes)
```

    ## [1] "tailnum"      "year"         "type"         "manufacturer"
    ## [5] "model"        "engines"      "seats"        "speed"       
    ## [9] "engine"

``` r
colnames(weather)
```

    ##  [1] "origin"     "year"       "month"      "day"        "hour"      
    ##  [6] "temp"       "dewp"       "humid"      "wind_dir"   "wind_speed"
    ## [11] "wind_gust"  "precip"     "pressure"   "visib"      "time_hour"

``` r
# Drop unimportant variables so it's easier to understand the join results. Also take first 1000 rows to run it faster.
flights2 <- flights[1:1000,] %>% 
  select(year, tailnum, carrier, time_hour)

dim(flights2)
```

    ## [1] 1000    4

``` r
colnames(flights2)
```

    ## [1] "year"      "tailnum"   "carrier"   "time_hour"

``` r
#add airline names from `airlines` dataset 
colnames(airlines)
```

    ## [1] "carrier" "name"

``` r
flights2 %>% 
  left_join(airlines) 
```

    ## Joining, by = "carrier"

    ## # A tibble: 1,000 x 5
    ##     year tailnum carrier time_hour           name                    
    ##    <int> <chr>   <chr>   <dttm>              <chr>                   
    ##  1  2013 N14228  UA      2013-01-01 05:00:00 United Air Lines Inc.   
    ##  2  2013 N24211  UA      2013-01-01 05:00:00 United Air Lines Inc.   
    ##  3  2013 N619AA  AA      2013-01-01 05:00:00 American Airlines Inc.  
    ##  4  2013 N804JB  B6      2013-01-01 05:00:00 JetBlue Airways         
    ##  5  2013 N668DN  DL      2013-01-01 06:00:00 Delta Air Lines Inc.    
    ##  6  2013 N39463  UA      2013-01-01 05:00:00 United Air Lines Inc.   
    ##  7  2013 N516JB  B6      2013-01-01 06:00:00 JetBlue Airways         
    ##  8  2013 N829AS  EV      2013-01-01 06:00:00 ExpressJet Airlines Inc.
    ##  9  2013 N593JB  B6      2013-01-01 06:00:00 JetBlue Airways         
    ## 10  2013 N3ALAA  AA      2013-01-01 06:00:00 American Airlines Inc.  
    ## # ... with 990 more rows

``` r
#add weather information to the flights2 dataset by matching "year" and "time_hour" variables.
colnames(weather)
```

    ##  [1] "origin"     "year"       "month"      "day"        "hour"      
    ##  [6] "temp"       "dewp"       "humid"      "wind_dir"   "wind_speed"
    ## [11] "wind_gust"  "precip"     "pressure"   "visib"      "time_hour"

``` r
flights2 %>% left_join(weather)
```

    ## Joining, by = c("year", "time_hour")

    ## # A tibble: 2,888 x 17
    ##     year tailnum carrier time_hour           origin month   day  hour  temp
    ##    <dbl> <chr>   <chr>   <dttm>              <chr>  <dbl> <int> <int> <dbl>
    ##  1  2013 N14228  UA      2013-01-01 05:00:00 EWR        1     1     5  39.0
    ##  2  2013 N14228  UA      2013-01-01 05:00:00 JFK        1     1     5  39.0
    ##  3  2013 N14228  UA      2013-01-01 05:00:00 LGA        1     1     5  39.9
    ##  4  2013 N24211  UA      2013-01-01 05:00:00 EWR        1     1     5  39.0
    ##  5  2013 N24211  UA      2013-01-01 05:00:00 JFK        1     1     5  39.0
    ##  6  2013 N24211  UA      2013-01-01 05:00:00 LGA        1     1     5  39.9
    ##  7  2013 N619AA  AA      2013-01-01 05:00:00 EWR        1     1     5  39.0
    ##  8  2013 N619AA  AA      2013-01-01 05:00:00 JFK        1     1     5  39.0
    ##  9  2013 N619AA  AA      2013-01-01 05:00:00 LGA        1     1     5  39.9
    ## 10  2013 N804JB  B6      2013-01-01 05:00:00 EWR        1     1     5  39.0
    ## # ... with 2,878 more rows, and 8 more variables: dewp <dbl>, humid <dbl>,
    ## #   wind_dir <dbl>, wind_speed <dbl>, wind_gust <dbl>, precip <dbl>,
    ## #   pressure <dbl>, visib <dbl>

``` r
#add weather information to the flights2 dataset by matching only "time_hour" variable.
flights2 %>% left_join(weather, by = "time_hour")
```

    ## # A tibble: 2,888 x 18
    ##    year.x tailnum carrier time_hour           origin year.y month   day
    ##     <int> <chr>   <chr>   <dttm>              <chr>   <dbl> <dbl> <int>
    ##  1   2013 N14228  UA      2013-01-01 05:00:00 EWR      2013     1     1
    ##  2   2013 N14228  UA      2013-01-01 05:00:00 JFK      2013     1     1
    ##  3   2013 N14228  UA      2013-01-01 05:00:00 LGA      2013     1     1
    ##  4   2013 N24211  UA      2013-01-01 05:00:00 EWR      2013     1     1
    ##  5   2013 N24211  UA      2013-01-01 05:00:00 JFK      2013     1     1
    ##  6   2013 N24211  UA      2013-01-01 05:00:00 LGA      2013     1     1
    ##  7   2013 N619AA  AA      2013-01-01 05:00:00 EWR      2013     1     1
    ##  8   2013 N619AA  AA      2013-01-01 05:00:00 JFK      2013     1     1
    ##  9   2013 N619AA  AA      2013-01-01 05:00:00 LGA      2013     1     1
    ## 10   2013 N804JB  B6      2013-01-01 05:00:00 EWR      2013     1     1
    ## # ... with 2,878 more rows, and 10 more variables: hour <int>, temp <dbl>,
    ## #   dewp <dbl>, humid <dbl>, wind_dir <dbl>, wind_speed <dbl>,
    ## #   wind_gust <dbl>, precip <dbl>, pressure <dbl>, visib <dbl>

Types of filtering join
-----------------------

### Let's filter data.frames using two filtering functions:

-   create two data.frames named `a` and `b`, similar to Data Wrangling Cheatsheet
-   use `semi_join`, `anti_join` functions
-   example for `semi_join`: All rows in a that have a match in b
-   example for `anti_join`: All rows in a that do not have a match in b
-   example of using two different variables from two datasets

``` r
#create two data.frames named a and b
(a <- data.frame(x1 = LETTERS[1:3], x2 = 1:3))
```

    ##   x1 x2
    ## 1  A  1
    ## 2  B  2
    ## 3  C  3

``` r
(b <- data.frame(x1 = LETTERS[c(1,2,4)], x3 = c("T", "F", "T")))
```

    ##   x1 x3
    ## 1  A  T
    ## 2  B  F
    ## 3  D  T

``` r
# example for `semi_join`: All rows in a that have a match in b
semi_join(a,b)
```

    ## Joining, by = "x1"

    ## Warning: Column `x1` joining factors with different levels, coercing to
    ## character vector

    ##   x1 x2
    ## 1  A  1
    ## 2  B  2

``` r
# example for `anti_join`: All rows in a that do not have a match in b
anti_join(a,b)
```

    ## Joining, by = "x1"

    ## Warning: Column `x1` joining factors with different levels, coercing to
    ## character vector

    ##   x1 x2
    ## 1  C  3

``` r
# example of using two different variables from two datasets

(c <- data.frame(x1 = c(LETTERS[1:2],"x"), x2 = c(1,4,5)))
```

    ##   x1 x2
    ## 1  A  1
    ## 2  B  4
    ## 3  x  5

``` r
semi_join(a, c)
```

    ## Joining, by = c("x1", "x2")

    ## Warning: Column `x1` joining factors with different levels, coercing to
    ## character vector

    ##   x1 x2
    ## 1  A  1

Types of Set Operations for two datasets
----------------------------------------

### Let's use three `set` functions:

-   create two data.frames named `y` and `z`, similar to Data Wrangling Cheatsheet
-   use `intersect`, `union` and `setdiff` functions
-   example for `intersect`: Rows that appear in both `y` and `z`
-   example for `union`: Rows that appear in either or both `y` and `z`
-   example for `setdiff`: Rows that appear in `y` but not `z`. **Caution:** `setdiff` for `y` to `z` and `z` to `y` are different.

Types of binding datasets
-------------------------

### Let's bind datasets by rows or column using two binding functions:

-   create two data.frames named `y` and `z`, similar to Data Wrangling Cheatsheet
-   use `bind_rows`, `bind_cols` functions
-   example for `bind_rows`: Append z to y as new rows
-   example for `bind_cols`: Append z to y as new columns. **Caution**: matches rows by position

Practice Exercises
------------------

Practice these concepts in the following exercises. It might help you to first identify the type of function you are applying.

### Let's create a data.frame `a` with x1 and x2 coulmns and have duplicated element in x1 column. Create another data.frame `b` with x1 and x3 columns. Then apply `left_join` function a to b and b to a.
