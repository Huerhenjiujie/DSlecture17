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
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -2.9216 -0.4919  0.1222  0.2217  0.8901  3.3275

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
    ##  [1] 3.8566041 4.7392566 2.0325671 3.0129169 1.7859782 3.0113870 3.3528772
    ##  [8] 3.5680226 3.9070516 4.8372474 2.3761485 3.8564259 2.4917546 3.6431905
    ## [15] 3.4303925 3.9256450 3.7778761 4.1155645 0.1941031 1.5216950
    ## 
    ## $b
    ##  [1]  0.5845054  6.1092363 -4.3501998  4.4506928 -6.8707856 -4.0711163
    ##  [7]  5.4249101  3.5779948 -1.1934327  2.7804233 -0.2607614 -1.3204447
    ## [13] -0.5464125  1.1166137 -2.5843793 -5.6329171  4.4304841  4.7001414
    ## [19] -5.1518399 -2.5760149 -0.6705017  1.3761810 10.7950708  3.4964144
    ## [25]  4.1285594 -3.9373136  1.6377135  2.0100311 -4.9233752  7.5466221
    ## 
    ## $c
    ##  [1]  9.947165 10.011845 10.211636 10.025819 10.307643 10.002317 10.180909
    ##  [8]  9.988687  9.714500  9.984368 10.081786 10.008222 10.207883  9.928959
    ## [15] 10.071220 10.057551 10.192527 10.072766  9.978487 10.141873 10.090154
    ## [22]  9.922475  9.801226  9.568841  9.769120 10.408707 10.427769  9.847429
    ## [29]  9.777226  9.590410 10.070630  9.902767  9.824248 10.084329  9.650685
    ## [36]  9.693048 10.137799  9.906215 10.501666  9.951406
    ## 
    ## $d
    ##  [1] -3.262678 -2.881477 -2.412568 -3.703268 -5.239356 -2.379153 -3.852490
    ##  [8] -1.842405 -1.625988 -1.232932 -3.943847 -1.978676 -3.097103 -2.294072
    ## [15] -1.702277 -2.145899 -1.837419 -3.853545 -3.548610 -4.331629

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
    ## 1  3.17  1.15

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.669  4.34

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.216

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.86  1.08

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
    ##  [1] 3.8566041 4.7392566 2.0325671 3.0129169 1.7859782 3.0113870 3.3528772
    ##  [8] 3.5680226 3.9070516 4.8372474 2.3761485 3.8564259 2.4917546 3.6431905
    ## [15] 3.4303925 3.9256450 3.7778761 4.1155645 0.1941031 1.5216950
    ## 
    ## $b
    ##  [1]  0.5845054  6.1092363 -4.3501998  4.4506928 -6.8707856 -4.0711163
    ##  [7]  5.4249101  3.5779948 -1.1934327  2.7804233 -0.2607614 -1.3204447
    ## [13] -0.5464125  1.1166137 -2.5843793 -5.6329171  4.4304841  4.7001414
    ## [19] -5.1518399 -2.5760149 -0.6705017  1.3761810 10.7950708  3.4964144
    ## [25]  4.1285594 -3.9373136  1.6377135  2.0100311 -4.9233752  7.5466221
    ## 
    ## $c
    ##  [1]  9.947165 10.011845 10.211636 10.025819 10.307643 10.002317 10.180909
    ##  [8]  9.988687  9.714500  9.984368 10.081786 10.008222 10.207883  9.928959
    ## [15] 10.071220 10.057551 10.192527 10.072766  9.978487 10.141873 10.090154
    ## [22]  9.922475  9.801226  9.568841  9.769120 10.408707 10.427769  9.847429
    ## [29]  9.777226  9.590410 10.070630  9.902767  9.824248 10.084329  9.650685
    ## [36]  9.693048 10.137799  9.906215 10.501666  9.951406
    ## 
    ## $d
    ##  [1] -3.262678 -2.881477 -2.412568 -3.703268 -5.239356 -2.379153 -3.852490
    ##  [8] -1.842405 -1.625988 -1.232932 -3.943847 -1.978676 -3.097103 -2.294072
    ## [15] -1.702277 -2.145899 -1.837419 -3.853545 -3.548610 -4.331629

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
    ## 1  3.17  1.15

can i just map

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.17  1.15
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.669  4.34
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.216
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.86  1.08

can i list a column

``` r
listcol_df %>% 
  mutate(summary = map_df(samp, mean_and_sd),
         medians = map_dbl(samp, median))
```

    ## # A tibble: 4 × 4
    ##   name  samp         summary$mean   $sd medians
    ##   <chr> <named list>        <dbl> <dbl>   <dbl>
    ## 1 a     <dbl [20]>          3.17  1.15    3.50 
    ## 2 b     <dbl [30]>          0.669 4.34    0.851
    ## 3 c     <dbl [40]>         10.0   0.216  10.0  
    ## 4 d     <dbl [20]>         -2.86  1.08   -2.65

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
