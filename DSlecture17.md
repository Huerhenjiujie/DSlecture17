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
    ## -2.89100 -0.53659  0.03254  0.09774  0.83101  2.28540

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
    ##  [1] 2.600294 3.685167 2.368144 2.854438 2.233432 2.777066 2.476758 3.156529
    ##  [9] 2.292710 1.858162 3.215732 1.771987 4.618819 3.446407 4.330447 4.234304
    ## [17] 2.509792 2.689100 2.808607 2.743088
    ## 
    ## $b
    ##  [1]  0.9027487  1.8872541  8.2252443 -1.6850420 -1.9118357 -0.6906120
    ##  [7] -6.0726948 -1.8677810  6.3528176  0.8634710 -3.9544000  3.2803041
    ## [13]  8.1601866  9.0102996 -0.9263423 -4.8671941  2.5836384  7.3240914
    ## [19] -2.2357745  8.8802470  1.3034170 -0.2825600 -7.2339273  1.0327641
    ## [25] -2.6481482 -0.2016755  7.2229416  5.1734401 -6.1808770 -3.9200836
    ## 
    ## $c
    ##  [1]  9.985052 10.245275 10.091458 10.258664  9.971481  9.900616 10.125972
    ##  [8] 10.384423  9.999500 10.160697 10.153709  9.773705 10.126915 10.031712
    ## [15]  9.685585 10.013331 10.525846 10.027160  9.826059  9.890942 10.217464
    ## [22] 10.168938  9.864896  9.998130  9.934333 10.228965 10.041954 10.224362
    ## [29] 10.267322  9.899851  9.852625  9.958896 10.043991  9.840653  9.819101
    ## [36]  9.998959 10.244252  9.726344 10.020926  9.744885
    ## 
    ## $d
    ##  [1] -2.351437 -4.350834 -3.914427 -3.955274 -3.844233 -2.645277 -2.677572
    ##  [8] -3.009590 -4.505481 -3.031861 -3.991696 -3.149922 -3.786308 -3.511972
    ## [15] -4.329667 -3.183991 -2.722486 -1.379119 -1.867163 -4.878115

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
    ## 1  2.93 0.788

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.917  4.82

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.188

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.35 0.915

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
    ##  [1] 2.600294 3.685167 2.368144 2.854438 2.233432 2.777066 2.476758 3.156529
    ##  [9] 2.292710 1.858162 3.215732 1.771987 4.618819 3.446407 4.330447 4.234304
    ## [17] 2.509792 2.689100 2.808607 2.743088
    ## 
    ## $b
    ##  [1]  0.9027487  1.8872541  8.2252443 -1.6850420 -1.9118357 -0.6906120
    ##  [7] -6.0726948 -1.8677810  6.3528176  0.8634710 -3.9544000  3.2803041
    ## [13]  8.1601866  9.0102996 -0.9263423 -4.8671941  2.5836384  7.3240914
    ## [19] -2.2357745  8.8802470  1.3034170 -0.2825600 -7.2339273  1.0327641
    ## [25] -2.6481482 -0.2016755  7.2229416  5.1734401 -6.1808770 -3.9200836
    ## 
    ## $c
    ##  [1]  9.985052 10.245275 10.091458 10.258664  9.971481  9.900616 10.125972
    ##  [8] 10.384423  9.999500 10.160697 10.153709  9.773705 10.126915 10.031712
    ## [15]  9.685585 10.013331 10.525846 10.027160  9.826059  9.890942 10.217464
    ## [22] 10.168938  9.864896  9.998130  9.934333 10.228965 10.041954 10.224362
    ## [29] 10.267322  9.899851  9.852625  9.958896 10.043991  9.840653  9.819101
    ## [36]  9.998959 10.244252  9.726344 10.020926  9.744885
    ## 
    ## $d
    ##  [1] -2.351437 -4.350834 -3.914427 -3.955274 -3.844233 -2.645277 -2.677572
    ##  [8] -3.009590 -4.505481 -3.031861 -3.991696 -3.149922 -3.786308 -3.511972
    ## [15] -4.329667 -3.183991 -2.722486 -1.379119 -1.867163 -4.878115

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
    ## 1  2.93 0.788

can i just map

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.93 0.788
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.917  4.82
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.188
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.35 0.915

can i list a column

``` r
listcol_df %>% 
  mutate(summary = map_df(samp, mean_and_sd),
         medians = map_dbl(samp, median))
```

    ## # A tibble: 4 × 4
    ##   name  samp         summary$mean   $sd medians
    ##   <chr> <named list>        <dbl> <dbl>   <dbl>
    ## 1 a     <dbl [20]>          2.93  0.788   2.76 
    ## 2 b     <dbl [30]>          0.917 4.82    0.331
    ## 3 c     <dbl [40]>         10.0   0.188  10.0  
    ## 4 d     <dbl [20]>         -3.35  0.915  -3.35

## weather data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2021-10-21 14:33:12 (7.606)

    ## file min/max dates: 1869-01-01 / 2021-10-31

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2021-10-21 14:33:16 (1.697)

    ## file min/max dates: 1965-01-01 / 2020-02-29

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2021-10-21 14:33:19 (0.912)

    ## file min/max dates: 1999-09-01 / 2021-10-31

get our list column

``` r
weather_nest = 
  weather_df %>% 
  nest(data = date:tmin)
```

``` r
weather_nest %>% pull(name)
```

    ## [1] "CentralPark_NY" "Waikiki_HA"     "Waterhole_WA"

``` r
weather_nest %>% pull(data)
```

    ## [[1]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # … with 355 more rows

``` r
weather_nest$data[[1]]
```

    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows

suppose i want to regress tmax on tmin for each station

``` r
lm(tmax~tmin, data = weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

lets write a funton

``` r
weather_lm = function(df){
  lm(tmax~tmin, data = df)
}

output = vector("list", 3)

for (i in 1:3){
  output[[i]] = weather_lm(weather_nest$data[[i]])
}
```

what about map

``` r
map(weather_nest$data, weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

what a map in a list colum

``` r
weather_nest =
  weather_nest %>% 
  mutate(models = map(data, weather_lm))

weather_nest$models
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

#output = map2(.x = input_1, .y = input_2, \~func(arg_1 = .x, arg_2 =
.y))

dynamite_reviews = tibble( page = 1:5, urls = str_c(url_base, page)) %>%
mutate(reviews = map(urls, read_page_reviews)) %>% unnest()
