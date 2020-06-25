Workshop: Data Visualization Principles
================

Today’s workshop is an adaptation of [Chapter 9 of Introduction to Data
Science: Data Analysis and Prediction Algorithms with
R](https://rafalab.github.io/dsbook/gapminder.html). We will demonstrate
the usefulness of **ggplot2** with a case study on insights on poverty
popularized by [Hans
Rosling](https://en.wikipedia.org/wiki/Hans_Rosling), the co-founder of
the [Gapmider Foundation](http://www.gapminder.org/). Specifically,
today we will use data to attempt to answer the following two questions:

1.  Is it a fair characterization of today’s world to say it is divided
    into western rich nations and the developing world in Africa, Asia,
    and Latin America?
2.  Has income inequality across countries worsened during the last 40
    years?

To answer these questions, we will be using the `gapminder` dataset
provided in **dslabs**. This dataset was created using a number of
spreadsheets available from the Gapminder Foundation. You can access the
table like this:

``` r
library(tidyverse)
library(dslabs)
data(gapminder)
ds_theme_set()
gapminder %>% as_tibble()
```

    ## # A tibble: 10,545 x 9
    ##    country  year infant_mortality life_expectancy fertility population      gdp
    ##    <fct>   <int>            <dbl>           <dbl>     <dbl>      <dbl>    <dbl>
    ##  1 Albania  1960            115.             62.9      6.19    1636054 NA      
    ##  2 Algeria  1960            148.             47.5      7.65   11124892  1.38e10
    ##  3 Angola   1960            208              36.0      7.32    5270844 NA      
    ##  4 Antigu…  1960             NA              63.0      4.43      54681 NA      
    ##  5 Argent…  1960             59.9            65.4      3.11   20619075  1.08e11
    ##  6 Armenia  1960             NA              66.9      4.55    1867396 NA      
    ##  7 Aruba    1960             NA              65.7      4.82      54208 NA      
    ##  8 Austra…  1960             20.3            70.9      3.45   10292328  9.67e10
    ##  9 Austria  1960             37.3            68.8      2.7     7065525  5.24e10
    ## 10 Azerba…  1960             NA              61.3      5.57    3897889 NA      
    ## # … with 10,535 more rows, and 2 more variables: continent <fct>, region <fct>

### Hans Rosling’s quiz

Let’s start by testing our knowledge regarding differences in child
mortality across different countries. For each of the pairs below, which
country do you think had the highest child mortality rates in 2015?
Which pairs do you think are most similar?

1.  Sri Lanka or Turkey
2.  Poland or South Korea
3.  Malaysia or Russia
4.  Pakistan or Vietnam
5.  Thailand or South Africa

When answering these questions without data, the non-European countries
are typically picked as having higher child mortality rates: Sri Lanka
over Turkey, South Korea over Poland, and Malaysia over Russia. It is
also common to assume that countries considered to be part of the
developing world: Pakistan, Vietnam, Thailand, and South Africa, have
similarly high mortality rates.

To answer these questions **with data**, we can use **dplyr**. Use the
`gapminder` dataset to answer the questions above:

It turns out that when Hans Rosling gave this quiz to educated groups of
people, the average score was less than 2.5 out of 5, worse than what
they would have obtained had they guessed randomly. This implies that
more than ignorant, we are misinformed. Let’s see how data visualization
helps to inform us.

## Scatterplots

The reason for this stems from the preconceived notion that the world is
divided into two groups: the western world (Western Europe and North
America), characterized by long life spans and small families, versus
the developing world (Africa, Asia, and Latin America) characterized by
short life spans and large families. But do the data support this
dichotomous view?

The necessary data to answer this question is also available in our
`gapminder` table. Using our newly learned data visualization skills, we
will be able to tackle this challenge.

Start by creating a scatterplot of life expectancy versus fertility
rates (average number of children per woman) on 1952:

Most points fall into two distinct categories:

1.  Life expectancy around 70 years and 3 or fewer children per family.
2.  Life expectancy lower than 65 years and more than 5 children per
    family.

As we learned today, let’s use color to codify continent:

There is some truth on “the West versus developing world” view back in
1962. Is this still true?

## Faceting

Let’s plot the 2012 data in the same way we did for 1962. Recall one of
the principles discussed today: **use common axes**. In **ggplot2**, we
can achieve this by **faceting** variables with `facet_grid`. Here is an
example:

``` r
filter(gapminder, year%in%c(1962, 2012)) %>%
  ggplot(aes(fertility, life_expectancy, col = continent)) +
  geom_point() +
  facet_grid(continent~year)
```

<img src="4-workshop_files/figure-gfm/unnamed-chunk-2-1.png" width="100%" />

This is just an example and more than what we want, which is simply to
compare 1962 and 2012. In this case, we only want a window for 1962 and
another for 2012. Fix the code from above to achieve this:

``` r
filter(gapminder, year%in%c(1962, 2012)) %>%
  ggplot(aes(fertility, life_expectancy, col = continent)) +
  geom_point() +
  facet_grid(. ~ year)
```

<img src="4-workshop_files/figure-gfm/unnamed-chunk-3-1.png" width="100%" />

This plot clearly shows that the majority of countries have moved from
the *developing world* cluster to the *western world* one. In 2012, the
western versus developing world view no longer makes sense. This is
particularly clear when comparing Europe to Asia, the latter of which
includes several countries that have made great improvements.

### `facet_wrap`

To explore how this transformation happened through the years, we can
make the plot for several years. For example, we can add 1970, 1980,
1990, and 2000. Use `facet_wrap` to achieve this:

``` r
years <- c(1962, 1980, 1990, 2000, 2012)
continents <- c("Europe", "Asia")
gapminder %>%
  filter(year %in% years & continent %in% continents) %>%
  ggplot( aes(fertility, life_expectancy, col = continent)) +
  geom_point() +
  facet_wrap(~year) 
```

<img src="4-workshop_files/figure-gfm/unnamed-chunk-4-1.png" width="100%" />

This plot clearly shows how most Asian countries have improved at a much
faster rate than European ones.

## Time series plots

The visualizations above illustrate that the data no longer supports the
western versus developing world view. Now a few new questions emerge:

  - Which countries are improving more and which ones less?
  - Was the improvement constant during the last 50 years or was it more
    accelerated during certain periods?

For a closer look that may help answer these questions, we introduce
*time series plots*. Time series plots have time in the x-axis and an
outcome or measurement of interest on the y-axis. For example, here is a
trend plot of United States fertility rates:

``` r
gapminder %>%
  filter(country == "United States") %>%
  ggplot(aes(year, fertility)) +
  geom_point()
```

![](4-workshop_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Note that the trend is not linear. We see a sharp decrease during the
1960s and 1970s to below 2 and then a slow increase and subsequent
plateau around 2.

When the points are regularly and densely spaced, as they are here, we
create curves by joining the points with lines, to convey that these
data are from a single series, here a country. To do this, we use the
`geom_line` function instead of `geom_point`. Try it for yourself:

``` r
gapminder %>%
  filter(country == "United States") %>%
  ggplot(aes(year, fertility)) +
  geom_line()
```

![](4-workshop_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

This is particularly helpful when we look at two countries. Subset the
data to include two countries, say, South Korea (Asia) and Germany
(Europr). Use the code above:

Any comments? Is this the plot we want? How can we fix it? Try it for
yourself:

This figure clearly shows how South Korea’s fertility rate dropped
drasticallly during the 1960 and 70s, and by 2000 had a similar rate to
that of Germany.

### Labels instead of legends

For trend plots we recommend labeling the lines rather than using
legends since the viewer can quickly see which line is which country.
This suggestion actually applies to most plots: labeling is usually
preferred over legends.

Use the code from above to create a time-series plot of life expectancy
for the same countries:

We demonstrate how we can do this using the life expectancy data. We
define a data table with the label locations and then use a second
mapping just for these labels:

``` r
countries <- c("South Korea", "Germany")
labels <- data.frame(country = countries, 
                     x = c(1975,1965), 
                     y = c(60,72))

gapminder %>%
  filter(country %in% countries) %>%
  ggplot(aes(year, life_expectancy, col = country)) +
  geom_line() +
  geom_text(data = labels, aes(x, y, label = country), size = 5) +
  theme(legend.position = "none")
```

![](4-workshop_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

The plot clearly shows how an improvement in life expectancy followed
the drops in fertility rates. In 1960, Germans lived 15 years longer
than South Koreans, although by 2010 the gap is completely closed. It
exemplifies the improvement that many non-western countries have
achieved in the last 40 years.

## Visualizing multimodal distributions

### Ridge plots

Showing each individual point does not always reveal important
characteristics of the distribution. Although not the case here, when
the number of data points is so large that there is over-plotting,
showing the data can be counterproductive. Boxplots help with this by
providing a five-number summary, but this has limitations too. For
example, boxplots will not permit us to discover bimodal distributions.
To see this, note that the two plots below are summarizing the same
dataset:

<img src="4-workshop_files/figure-gfm/unnamed-chunk-8-1.png" width="100%" />

In cases in which we are concerned that the boxplot summary is too
simplistic, we can show stacked smooth densities or histograms. We refer
to these as *ridge plots*. The package **ggridges** provides a
convenient function for doing this. Here is the GDP per continent in
1970 with a *ridge plot*:

``` r
library(ggridges)
p <- gapminder %>%
  filter(year == 1970 & !is.na(gdp)) %>%
  ggplot(aes(gdp, continent)) +
  scale_x_continuous(trans = "log10")
p + geom_density_ridges()
```

![](4-workshop_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

Note that we have to invert the `x` and `y` used for the boxplot. A
useful `geom_density_ridges` parameter is `scale`, which lets you
determine the amount of overlap, with `scale = 1` meaning no overlap and
larger values resulting in more overlap. Here is an example:

``` r
p + geom_density_ridges(scale = 2)
```

![](4-workshop_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

If the number of data points is small enough, we can add them to the
ridge plot using the following code:

``` r
p + geom_density_ridges(jittered_points = TRUE)
```

![](4-workshop_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

By default, the height of the points is jittered and should not be
interpreted in any way. To show data points, but without using jitter we
can use the following code to add what is referred to as a *rug
representation* of the data.

``` r
p + geom_density_ridges(jittered_points = TRUE,
                        position = position_points_jitter(height = 0),
                        point_shape = '|', point_size = 3,
                        point_alpha = 1, alpha = 0.7, scale = 1)
```

![](4-workshop_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->
