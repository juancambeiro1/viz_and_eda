Untitled
================
Juan Cambeiro
2022-10-24

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.5 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.3      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(ggridges)
```

# Visualization with ggplot2 part 1

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

    ## date created (size, mb): 2022-10-24 08:19:39 (8.411)

    ## file min/max dates: 1869-01-01 / 2022-10-31

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2022-10-24 08:19:42 (1.697)

    ## file min/max dates: 1965-01-01 / 2020-02-29

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2022-10-24 08:19:43 (0.954)

    ## file min/max dates: 1999-09-01 / 2022-10-31

Make a scatterplot: min vs. max temp. Need to define dataset, x and y
axes. Also add geometries (e.g. geom_point() to get all points).

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) +
  geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and-eda_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

Let’s fancy things up. Add color to have different colors for each
location.

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point() +
  geom_smooth()
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_and-eda_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

## Univariate plots..

Density plots.

For comparing distribution of max temps across each of the 3 weather
stations.

``` r
weather_df %>% 
  ggplot(aes(x = tmax, fill = name)) +
  geom_density(alpha = .3)
```

    ## Warning: Removed 3 rows containing non-finite values (stat_density).

![](viz_and-eda_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Boxplots.

``` r
weather_df %>% 
  ggplot(aes(x = name, y = tmax, fill =name )) +
  geom_boxplot()
```

    ## Warning: Removed 3 rows containing non-finite values (stat_boxplot).

![](viz_and-eda_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Violin plot: tells same kind of stuff as boxplot tells you but also info
about complete distribution. Comes in handy if you’re comparing dozens
of different things. If is only 3, do density plot or boxplot. Density
plot is like smoothed-over histogram.

# Visualization with ggplot2 part 2

## Patchwork

getting error for below, oh well.

# EDA

Exploratory data analysis. `group_by` makes grouping explicit and adds a
layer to a data based on existing variables. `summarize()` makes one
number summaries
