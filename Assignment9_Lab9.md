Assignment Nine - Lab9
================
Taehoon Ha
11/14/2018

-   [Question 1](#question-1)
-   [Question 2](#question-2)
-   [Question 3](#question-3)
    -   [(1) N=1000](#n1000)
    -   [(2) N=10000](#n10000)
    -   [(3) N=1000000](#n1000000)
    -   [Summary Tables](#summary-tables)

### Question 1

Submit your solution for the Problem 1 from Activity 9.

-   Consider a function that counts number of odd numbers in a given vector.

``` r
odd_count <- function(x) {
    odd_num <- 0
    for (i in 1:length(x)) {
        if (x[i]%%2 == 1) 
            odd_num <- odd_num + 1
    }
    return(odd_num)
}
```

``` r
odd_count2 <- function(x) {
    odd_num <- sum(x %% 2 != 0)
    return(odd_num)
}
```

-   Write a vectorized form of this function and compare their performance using `microbenchmark()` function from the package `microbenchmark`.

``` r
microbenchmark(odd_count(1:100),
               odd_count2(1:100),
               times = 100)
```

    ## Unit: microseconds
    ##               expr    min      lq      mean  median     uq       max neval
    ##   odd_count(1:100) 28.047 31.7160  72.25646 32.6505 34.823  3875.112   100
    ##  odd_count2(1:100)  2.669  3.3375 179.88415  3.8345  4.411 17512.621   100

<br><br>

### Question 2

Modify the sorting function (`sort_vec`) from "**Assignment 8**" **(problem 3)** so that it should take an additional argument **ascending** which causes sorting in increasing order when '`ascending = TRUE`' In other words,

-   `sort_vect(c(3, 1, 2), ascending = TRUE) = (1, 2, 3)`
-   `sort_vect(c(3, 1, 2), ascending = FALSE) = (3, 2, 1)`

``` r
sort_vec <- function(x, ascending = TRUE) {
    
    if (length(x) < 2) {
        return(x)
    
    }
    
    for (last in length(x):2) {
        for (first in 1:(last - 1)) {
            
            if (ascending == TRUE) {
                if (x[first] > x[first + 1]) {
                  temp <- x[first]
                  x[first] <- x[first + 1]
                  x[first + 1] <- temp
                  
                }
            }
            
            if (ascending == FALSE) {
                if (x[first] < x[first + 1]) {
                  temp <- x[first]
                  x[first] <- x[first + 1]
                  x[first + 1] <- temp
                  
                }
            }
        }
    }
    return(x)
}


#Test sets                
sort_vec(c(3, 1, 2), ascending = T)
```

    ## [1] 1 2 3

``` r
sort_vec(c(3, 1, 2), ascending = F)
```

    ## [1] 3 2 1

<br><br>

### Question 3

Consider a simple random walk with starting point 0 and a step -1 or 1. Below is the code with dynamically allocated memory. Write your code with preallocated memory and compare time for both versions using `system.time()` function (use **N = 1000, 10000 and 1000000**).

``` r
N = 1000
data_series = 0
system.time({for (i in 2:N) {
    data_series[i] = data_series[i-1] + sample(c(-1, 1), 1)
}
})
```

    ##    user  system elapsed 
    ##   0.008   0.001   0.009

<br>

#### (1) N=1000

-   Dynamically allocated memory

``` r
N <- 1000
data_series <- 0
D1000 <- system.time({
    for (i in 2:N) {
        data_series[i] <- data_series[i - 1] + sample(c(-1, 1), 1)
    }
})
D1000
```

    ##    user  system elapsed 
    ##   0.008   0.001   0.009

-   Preallocated memory

``` r
N <- 1000
P1000 <- system.time({
    vector_series <- vector(length = N)
    for (i in 2:N) {
        vector_series[i] <- vector_series[i - 1] + sample(c(-1, 1), 1)
    }
})
P1000
```

    ##    user  system elapsed 
    ##   0.006   0.000   0.007

<br>

#### (2) N=10000

-   Dynamically allocated memory

``` r
N <- 10000
data_series <- 0
D10000 <- system.time({
    for (i in 2:N) {
        data_series[i] <- data_series[i - 1] + sample(c(-1, 1), 1)
    }
})
D10000
```

    ##    user  system elapsed 
    ##   0.049   0.010   0.059

-   Preallocated memory

``` r
N <- 10000
P10000 <- system.time({
    vector_series <- vector(length = N)
    for (i in 2:N) {
        vector_series[i] <- vector_series[i - 1] + sample(c(-1, 1), 1)
    }
})
P10000
```

    ##    user  system elapsed 
    ##   0.038   0.002   0.041

<br>

#### (3) N=1000000

-   Dynamically allocated memory

``` r
N <- 1000000
data_series <- 0
D1000000 <- system.time({
    for (i in 2:N) {
        data_series[i] <- data_series[i - 1] + sample(c(-1, 1), 1)
    }
})
D1000000
```

    ##    user  system elapsed 
    ##   3.822   0.260   4.124

-   Preallocated memory

``` r
N <- 1000000
P1000000 <- system.time({
    vector_series <- vector(length = N)
    for (i in 2:N) {
        vector_series[i] <- vector_series[i - 1] + sample(c(-1, 1), 1)
    }
})
P1000000
```

    ##    user  system elapsed 
    ##   3.552   0.229   3.817

#### Summary Tables

``` r
sum.tab <- rbind(D1000, P1000, 
                 D10000, P10000,
                 D1000000, P1000000)[,1:3] %>%
    t %>%
    data.frame

dynamic <- sum.tab %>% select(D1000, D10000, D1000000)
pre <- sum.tab %>% select(P1000, P10000, P1000000)

knitr::kable(sum.tab, caption = "Comprehensive Summary Table")
```

|           |  D1000|  P1000|  D10000|  P10000|  D1000000|  P1000000|
|-----------|------:|------:|-------:|-------:|---------:|---------:|
| user.self |  0.008|  0.006|   0.049|   0.038|     3.822|     3.552|
| sys.self  |  0.001|  0.000|   0.010|   0.002|     0.260|     0.229|
| elapsed   |  0.009|  0.007|   0.059|   0.041|     4.124|     3.817|

``` r
knitr::kable(dynamic, caption = "Dynaically Allocated Memory by N")
```

|           |  D1000|  D10000|  D1000000|
|-----------|------:|-------:|---------:|
| user.self |  0.008|   0.049|     3.822|
| sys.self  |  0.001|   0.010|     0.260|
| elapsed   |  0.009|   0.059|     4.124|

``` r
knitr::kable(pre, caption = "Preallocated Momory by N")
```

|           |  P1000|  P10000|  P1000000|
|-----------|------:|-------:|---------:|
| user.self |  0.006|   0.038|     3.552|
| sys.self  |  0.000|   0.002|     0.229|
| elapsed   |  0.007|   0.041|     3.817|
