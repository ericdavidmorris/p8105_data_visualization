Visualization with ggplot2
================
Eric Morris
9/27/2018

Important NOAA Data
-------------------

``` r
library(rnoaa)

weather_df = 
  rnoaa::meteo_pull_monitors(c("USW00094728", "USC00519397", "USS0023B17S"),
                      var = c("PRCP", "TMIN", "TMAX"), 
                      date_min = "2017-01-01",
                      date_max = "2017-12-31") %>%
  mutate(
    name = recode(id, USW00094728 = "CentralPark_NY", 
                      USC00519397 = "Waikiki_HA",
                      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

Making a plot
-------------

Blank plot..

``` r
ggplot(weather_df, aes(x = tmin, y = tmax))
```

![](viz_1_files/figure-markdown_github/unnamed-chunk-1-1.png)

Scatterplot

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_1_files/figure-markdown_github/unnamed-chunk-2-1.png)

Filtering Central Park without having to also create a new data frame

``` r
weather_df %>% 
  filter(name == "CentralPark_NY") %>% 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point()
```

![](viz_1_files/figure-markdown_github/unnamed-chunk-3-1.png)

More plot options
-----------------

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name))
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_1_files/figure-markdown_github/unnamed-chunk-4-1.png)

Add a geom (se = FALSE will turn off confidence bands)

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name)) + 
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_1_files/figure-markdown_github/unnamed-chunk-5-1.png)

Making points more transparent

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name), alpha = 0.4) + 
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_1_files/figure-markdown_github/unnamed-chunk-6-1.png)

Adding some more options and switching everything around

``` r
ggplot(weather_df, aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = 0.4) + 
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_1_files/figure-markdown_github/unnamed-chunk-7-1.png)

Facetting...

``` r
ggplot(weather_df, aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = 0.4) + 
  geom_smooth(se = FALSE) +
  facet_grid( ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](viz_1_files/figure-markdown_github/unnamed-chunk-8-1.png)

A more interesting plot

``` r
ggplot(weather_df, aes(x = date, y = tmax, color = name)) + 
  geom_point()
```

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](viz_1_files/figure-markdown_github/unnamed-chunk-9-1.png)

Lines instead of points

``` r
ggplot(weather_df, aes(x = date, y = tmax, color = name)) + 
  geom_line()
```

![](viz_1_files/figure-markdown_github/unnamed-chunk-10-1.png)

Back to points and adding more stuff

``` r
ggplot(weather_df, aes(x = date, y = tmax, color = name)) + 
  geom_point(aes(size = prcp), alpha = .5) +
  geom_smooth(se = FALSE) + 
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](viz_1_files/figure-markdown_github/unnamed-chunk-11-1.png)

``` r
ggplot(weather_df, aes(x = date, y = tmax, color = name, size = prcp)) + 
  geom_point(alpha = .5) +
  geom_smooth(se = FALSE) + 
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](viz_1_files/figure-markdown_github/unnamed-chunk-12-1.png)
