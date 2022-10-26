PM566-lab09-Tiansheng-Jin
================
Tiansheng Jin
2022-10-26

``` r
library(parallel)
```

## Problem 2

``` r
set.seed(156)

fun1 <- function(n = 100, k = 4, lambda = 4) {
  x <- NULL
  
  for (i in 1:n)
    x <- rbind(x, rpois(k, lambda))
  
  return(x)
}
f1 <- fun1(100, 4)
mean(f1)
```

    ## [1] 4.02

``` r
f1 <- fun1(1000, 4)
f1 <- fun1(10000, 4)
f1 <- fun1(50000, 4)

fun1alt <- function(n = 100, k = 4, lambda = 4) {
  x <- matrix(rpois(n*k, lambda) , ncol = 4)
}

# Benchmarking
microbenchmark::microbenchmark(
  fun1(),
  fun1alt()
)
```

    ## Unit: microseconds
    ##       expr     min       lq      mean   median       uq     max neval
    ##     fun1() 156.497 169.1455 179.68824 179.5390 188.7845 232.019   100
    ##  fun1alt()  13.079  13.6530  23.19944  14.0425  14.5755 902.246   100
