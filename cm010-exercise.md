cm008 Exercises
================

``` r
suppressPackageStartupMessages(library(tidyverse))
#install.packages("nycflights13")
suppressPackageStartupMessages(library(nycflights13))
```

Types of join
-------------

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
(c <- data.frame(x1 = c(LETTERS[1:2],"x"), x2 = letters[1:3]))
```

    ##   x1 x2
    ## 1  A  a
    ## 2  B  b
    ## 3  x  c

``` r
#inner_join(a, c)
```

`nycflights13` dataset has four tibbles e.g., `flights`, `airports`, `planes`
-----------------------------------------------------------------------------

``` r
#check the tibbles included in `nycflights13` package
class(flights)
colnames(flights)
colnames(airports)
colnames(planes)

# Drop unimportant variables so it's easier to understand the join results. Also take first 20000 rows to run it faster.
#flights2 <- flights %>% select(year:day, hour, origin, dest, tailnum, carrier)
flights2 <- flights[1:20000,] %>% select(year, tailnum, carrier)
dim(flights2)

#add airline names from `airlines` by carrier 
flights2 %>% 
  left_join(airlines) 

#flights2 %>% left_join(weather)
```

misc
====

The coloured column represents the “key” variable: these are used to match the rows between the tables.

A join is a way of connecting each row in x to zero, one, or more rows in y.

Duplicates in one of the tables

Other implementations
