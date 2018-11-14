# Lab9
Biostatistics1 - Lab9

### Question 1
Submit your solution for the Problem 1 from Activity 9.

+ Consider a function that counts number of odd numbers in a given vector.

```{r}
odd_count <- function(x) {
    odd_num <- 0
    for (i in 1:length(x)) {
        if (x[i]%%2 == 1) 
            odd_num <- odd_num + 1
    }
    return(odd_num)
}
```

+ Write a vectorized form of this function and compare their performance using `microbenchmark()` function from the package `microbenchmark`.

<br><br>

### Question 2
Modify the sorting function (`sort_vec`) from "**Assignment 8**" **(problem 3)** so that it should take an additional argument **ascending** which causes sorting in increasing order when '`ascending = TRUE`' In other words,

+ `sort_vect(c(3, 1, 2), ascending = TRUE) = (1, 2, 3)`
+ `sort_vect(c(3, 1, 2), ascending = FALSE) = (3, 2, 1)`

<br><br>

### Question 3
Consider a simple random walk with starting point 0 and a step -1 or 1. Below is the code with dynamically allocated memory. Write your code with preallocated memory and compare time for both versions using `system.time()` function (use **N = 1000, 10000 and 1000000**).

```{r}
N = 1000
data_series = 0
system.time({for (i in 2:N) {
    data_series[i] = data_series[i-1] + sample(c(-1, 1), 1)
}
})
```

<br>
