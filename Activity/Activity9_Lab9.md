Activity Nine - Lab9
================
Taehoon Ha
11/7/2018

-   [Question 1:](#question-1)
-   [Question 2:](#question-2)
-   [Question 3:](#question-3)

### Question 1:

Consider a function that counts number of odd numbers in a given vector.

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

<br>

Write a vectorized form of this function and compare their performance using **`microbenchmark` function** from the package `microbenchmark`.

``` r
microbenchmark(odd_count(1:100), odd_count2(1:100), times = 100)
```

    ## Unit: microseconds
    ##               expr    min      lq      mean  median      uq      max neval
    ##   odd_count(1:100) 15.270 15.6605 179.09089 15.9825 16.8160 16284.81   100
    ##  odd_count2(1:100)  1.517  1.5740  17.53363  1.6730  1.7485  1573.89   100

<br><br>

### Question 2:

Create a matrix and calculate sum of each column: + using loop + using `apply()` function + using `colSums()` function

``` r
(mat <- matrix(1:12, ncol = 3))
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    5    9
    ## [2,]    2    6   10
    ## [3,]    3    7   11
    ## [4,]    4    8   12

``` r
# using loop
matrix_sum <- function(mat) {
    matsum <- c(0,0,0)
    for (j in 1:ncol(mat)) {
        for (i in 1:nrow(mat)) {
            matsum[j] <- matsum[j] + mat[i,j]
        }
    }
    matsum
}
matrix_sum(mat)
```

    ## [1] 10 26 42

``` r
# using `apply()` function
apply(mat, 2, sum)
```

    ## [1] 10 26 42

``` r
# using `colSums()` function
colSums(mat)
```

    ## [1] 10 26 42

<br>

Compare performance of the functions using `microbenchmark()` function.

``` r
n <- 1:100
microbenchmark(matrix_sum(mat),
               apply(mat, 2, sum),
               colSums(mat), times = 100)
```

    ## Unit: microseconds
    ##                expr    min      lq     mean  median      uq     max neval
    ##     matrix_sum(mat)  2.986  3.2425  3.54405  3.4315  3.5875   9.447   100
    ##  apply(mat, 2, sum) 11.083 11.5580 13.79196 11.8930 12.2560 124.851   100
    ##        colSums(mat)  2.547  2.8715  3.49387  3.2235  3.4010  29.887   100

<br><br>

### Question 3:

Create a random vector of positive integers of length 1,000,000 elements. Use `profvis()` function to profile the `odd_count()` function defined in the problem 1 with the vector as an input.

``` r
profvis({
    dat <- sample(1:100, 1000000, replace = T)
    odd_count(dat)
    })
```

<p align="center">
<img src = 'https://ws2.sinaimg.cn/large/006tNbRwly1fx7nb72zvzj31hi0wm0zx.jpg'>
</p>
<br>
