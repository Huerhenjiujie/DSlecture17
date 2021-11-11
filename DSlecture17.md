DSlecture17
================
Hening CUi
11/10/2021

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.5     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
    ## ✓ readr   2.0.2     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

## lists

youcan put anything

``` r
l =list(
vec_numeric = 5:8,
vec_logical = c(TRUE, TRUE, FALSE, FALSE, FALSE, TRUE),
mat = matrix(1:8, nrow = 2),
summary = summary(rnorm(100)))
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE FALSE FALSE FALSE  TRUE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -2.81150 -0.70960  0.01382  0.01832  0.77477  2.95666

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## for loop

``` r
list_norm =
  list(
    a = rnorm(20, mean = 3, sd = 1),
    b = rnorm(30, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = .2),
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm
```

    ## $a
    ##  [1] 3.738209 3.205771 1.451265 2.931114 2.836179 3.246893 2.926306 3.884934
    ##  [9] 3.622098 3.765161 2.541241 1.903820 3.347292 3.252684 2.211492 1.989654
    ## [17] 1.665243 2.831331 2.806850 3.256556
    ## 
    ## $b
    ##  [1]  0.5620898  7.8519851  0.2980118 -4.7682151  0.5142227 -4.1465977
    ##  [7] -0.3521295 -4.7265752  1.0583515 -8.6996906  4.9883972  5.9918094
    ## [13]  5.3076742 -4.4462850 -9.1807791  6.9810887 -5.1065698 -0.7737956
    ## [19]  9.8158189  3.8566302 -4.7014349  6.7929442  1.2519421 -7.6060452
    ## [25] -3.4814328 -1.0460253  3.2358162  0.5618282 -3.7823479  0.8119774
    ## 
    ## $c
    ##  [1] 10.135167 10.244123 10.034745 10.036194 10.129991 10.012332  9.914593
    ##  [8]  9.943112 10.162606  9.681835  9.808040  9.778369  9.970380  9.864225
    ## [15] 10.009107  9.982933  9.768999 10.220325  9.704711 10.146813 10.131662
    ## [22] 10.295825  9.901113 10.223930  9.618775  9.961879  9.944546 10.236621
    ## [29]  9.841297  9.981309 10.121499  9.948533  9.722200  9.824248  9.940961
    ## [36]  9.987561 10.003031 10.219835 10.058762  9.970125
    ## 
    ## $d
    ##  [1] -2.746398 -2.521697 -3.460921 -4.257124 -3.493243 -3.875478 -3.362064
    ##  [8] -2.084445 -3.371795 -2.897132 -2.756778 -2.642492 -2.303611 -1.766960
    ## [15] -4.640457 -3.792857 -3.542069 -2.056515 -3.249056 -2.323155

pause and get my function

``` r
mean_and_sd = function(x){
  if (!is.numeric(x)){
    stop("input must be numeric")
  }
  
  if (length(x) < 3){
    stop("input must have at least 3 number")
  }
  
 mean_x = mean(x)
 sd_x = sd(x)
 tibble::tibble(
   mean = mean_x,
   sd = sd_x
 )
}
```

i can apply to each element

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.87 0.713

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0979  5.08

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.99 0.170

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.06 0.773

let use for loop

``` r
output = vector("list", length = 4)

for(i in 1:4){
  output[[i]] = mean_and_sd(list_norm[[i]])}
```

## let try map

``` r
output = map(list_norm, mean_and_sd)
```

what if different function

``` r
output = map(list_norm, IQR)
```

``` r
output = map_dbl(list_norm, IQR)
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input")
```

## list column

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"), 
    samp = list_norm
  )
```

``` r
listcol_df %>% 
  pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 3.738209 3.205771 1.451265 2.931114 2.836179 3.246893 2.926306 3.884934
    ##  [9] 3.622098 3.765161 2.541241 1.903820 3.347292 3.252684 2.211492 1.989654
    ## [17] 1.665243 2.831331 2.806850 3.256556
    ## 
    ## $b
    ##  [1]  0.5620898  7.8519851  0.2980118 -4.7682151  0.5142227 -4.1465977
    ##  [7] -0.3521295 -4.7265752  1.0583515 -8.6996906  4.9883972  5.9918094
    ## [13]  5.3076742 -4.4462850 -9.1807791  6.9810887 -5.1065698 -0.7737956
    ## [19]  9.8158189  3.8566302 -4.7014349  6.7929442  1.2519421 -7.6060452
    ## [25] -3.4814328 -1.0460253  3.2358162  0.5618282 -3.7823479  0.8119774
    ## 
    ## $c
    ##  [1] 10.135167 10.244123 10.034745 10.036194 10.129991 10.012332  9.914593
    ##  [8]  9.943112 10.162606  9.681835  9.808040  9.778369  9.970380  9.864225
    ## [15] 10.009107  9.982933  9.768999 10.220325  9.704711 10.146813 10.131662
    ## [22] 10.295825  9.901113 10.223930  9.618775  9.961879  9.944546 10.236621
    ## [29]  9.841297  9.981309 10.121499  9.948533  9.722200  9.824248  9.940961
    ## [36]  9.987561 10.003031 10.219835 10.058762  9.970125
    ## 
    ## $d
    ##  [1] -2.746398 -2.521697 -3.460921 -4.257124 -3.493243 -3.875478 -3.362064
    ##  [8] -2.084445 -3.371795 -2.897132 -2.756778 -2.642492 -2.303611 -1.766960
    ## [15] -4.640457 -3.792857 -3.542069 -2.056515 -3.249056 -2.323155

``` r
listcol_df %>% filter(name == "a")
```

    ## # A tibble: 1 × 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

lets try some operation

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.87 0.713

can i just map

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.87 0.713
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0979  5.08
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.99 0.170
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.06 0.773

can i list a column

``` r
listcol_df %>% 
  mutate(summary = map_df(samp, mean_and_sd),
         medians = map_dbl(samp, median))
```

    ## # A tibble: 4 × 4
    ##   name  samp         summary$mean   $sd medians
    ##   <chr> <named list>        <dbl> <dbl>   <dbl>
    ## 1 a     <dbl [20]>         2.87   0.713   2.93 
    ## 2 b     <dbl [30]>        -0.0979 5.08    0.406
    ## 3 c     <dbl [40]>         9.99   0.170   9.98 
    ## 4 d     <dbl [20]>        -3.06   0.773  -3.07
