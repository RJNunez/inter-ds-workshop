Workshop: Visualizing data distributions
================

## Manipulating data tables

We will be using the tidyverse in this lab. So let’s start by loading
it.

``` r
library(tidyverse)
```

Your lab conducted an experiment in which four different fragments of
chromosome 21 were integrated into mice. The four parts are denoted with
*141G6*, *152F7*, *230E8* and *285E6*. The mice were bred resulting in
dozens of transgenic mice. The DNA fragment is not always inherted so
some mice have the extra copy and others don’t. We are interested in
determining if any of these fragments result in an increase in weight, a
characteristic associated with trisomic mice.

The data can be loaded like this.

``` r
load("../rdas/mouse.rda")
```

Which loads the `dat` object into your environment:

``` r
dat
```

    ## # A tibble: 537 x 8
    ##      DNA line         tg   sex   age weight    bp  cage
    ##    <dbl> <chr>     <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl>
    ##  1     3 #50-69-1      1     1   113   31.6  123.     1
    ##  2     3 #50-69-2      1     1   113   31.2  125.     1
    ##  3     3 #50-69-3      1     1   113   28.6  122      1
    ##  4     3 #50-69-4      0     1   113   30.1  126      1
    ##  5     3 #50-69-11     0     1   121   31.3  129.     2
    ##  6     3 #50-69-12     0     1   121   36.4  126.     2
    ##  7     3 #50-69-13     1     1   121   36.5  126.     2
    ##  8     3 #50-69-15     1     1   121   29.8  124.     2
    ##  9     3 #50-69-16     1     1   121   35.6  127      2
    ## 10     3 #50-69-17     1     1   121   33.5  123.     2
    ## # … with 527 more rows

The columns included in this table are the following:

  - *DNA*: Fragment of chromosome 21 integrated in parent mouse
    (1=141G6; 2=152F7; 3=230E8; 4=285E6).
  - *line*: Family line.
  - *tg* - Whether the mouse contains the extra DNA (1) or not (0).
  - *sex*: Sex of mouse (1=male; 0=female).
  - *age*: Age of mouse (in days) at time of weighing.
  - *weight*: Weight of mouse in grams, to the nearest tenth of a gram.
  - *bp*: Blood pressure of the mosue.
  - *cage*: Number of the cage in which the mouse lived

Let’s start by comparing the weights of the no trisomic mice to the
weights of mice with the other four fragmets. Determine which columns
tells us the fragment of the mouse and determine the number of mouse in
each group? Hint: use the *count* function.

$$

$$

Note that the names are 1, 2, 3, 4. Let’s change these to the actual
names 1=141G6; 2=152F7; 3=230E8; 4=285E6. Create a new column called
`fragment` with the actual fragment names. Hint: Use the `recode`
function.

``` r
dat <- mutate(dat, fragment = recode(DNA, 
                                "1"="141G6", 
                                "2"="152F7", 
                                "3"="230E8", 
                                "4"="285E6"))
```

Note that all the mice in our table have one of these names. However, we
know that not all mouse have the fragments. Remember that not all
inhereted the extra copy. Use `filter` and `count` to see how many mice
in the `141G6` group have the extra copy.

``` r
filter(dat, fragment == "141G6") %>% count(tg)
```

    ## # A tibble: 2 x 2
    ##      tg     n
    ##   <dbl> <int>
    ## 1     0    78
    ## 2     1   104

Now change the `fragment` column so that the mice that do not have the
extra copy, have are called `No trisomy`. Hint: use the `ifelse`
function.

``` r
dat <- dat %>% mutate(fragment = ifelse(tg == 0, "No trisomy", fragment)) 
```

Before we continue let’s learn about the `n()` function. Note that we
can perform the same as the `count()` function using `group_by()` and
`n()`

``` r
dat %>% group_by(DNA) %>% summarize(freq = n())
```

    ## # A tibble: 4 x 2
    ##     DNA  freq
    ##   <dbl> <int>
    ## 1     1   182
    ## 2     2   158
    ## 3     3    37
    ## 4     4   160

Now compute the average and standard error in each of the four groups
and the control. Hint: Use `group_by` and `summarize`.

$$

$$

Bonus: Is the above difference statistically signficant at the 0.05
level?

$$

$$

This is what the dynamite plot would look like:

![](3-workshop_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

Does this make sense? In the next section we demonstrate how data
exploration allows us to detect a problem. We also learn how to make
publication ready plots.

## Data visualization

### Data exploration to identify outliers

A problem with the summary statistics and the barplot above is that it
only shows the average and we learn little about the distribution of the
data. Use the *geom\_boxplot* function to show the five number summary.
Do you see a problem? What is it?

$$

$$

We know that a 1000 gram mice does not exist. In fact 100 grams is
already a huge mouse. Use filter to show the data from the mice weighing
more than 100 grams.

$$

$$

What are the weights?

$$

$$

An unfortunate common practice is to use the number 999 to denote
missing data. The recommended practice is to acutely type NA. Use the
fitler function to remove all the rows with these missing values, then
remake the figure and recompute the averages and standard errors.

$$

$$

This makes much more sense.

## Data exploration to help answer scientific question

Let us start by creating a boxplot of weight vs fragments:

``` r
dat %>% ggplot(aes(fragment, weight)) + geom_boxplot()
```

![](3-workshop_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

We prefer showing the control data first. To achieve this we need to let
R know that `No trisomy` is the reference. We can do this by converting
the variable into a factor and using the `relevel` function. Like this:

``` r
dat <- mutate(dat, fragment = factor(fragment)) %>% 
        mutate(fragment = relevel(fragment, ref = "No trisomy"))

dat %>% ggplot(aes(fragment, weight)) + geom_boxplot()
```

![](3-workshop_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

Notice that the boxplot do show evidence of asymetry. Use the
`geom_point()` function to add the points to the boxplot.

``` r
dat %>% ggplot(aes(fragment, weight)) + geom_boxplot() + 
  geom_point()
```

![](3-workshop_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

The points are all cluster together and its hard to see them. We can use
`geom_jitter` instead of `geom_point` to fix this:

``` r
dat %>% ggplot(aes(fragment, weight)) + geom_boxplot() + 
  geom_jitter()
```

![](3-workshop_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

What did `geom_jitter` do? Let’s make the spread smaller and alpha-blend
the data points

``` r
dat %>% ggplot(aes(fragment, weight)) +
  geom_boxplot() + 
  geom_jitter(width = 0.1, alpha = 0.5)
```

![](3-workshop_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

Do you see anything interesting? Note that an unexpected result is
revealed, for some groups it seems like we have a bimodal distribution.
We can use histograms to assess this. For example, here is the histogram
for the control group:

``` r
filter(dat, fragment == "No trisomy") %>%
  ggplot(aes(weight)) +
  geom_histogram()
```

![](3-workshop_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

To avoid the warning we can add the argument `bindwith = 1` and to make
it nicer looking we can add `color = "black"`.

``` r
filter(dat, fragment == "No trisomy") %>%
  ggplot(aes(weight)) +
  geom_histogram(binwidth = 1, color = "black")
```

![](3-workshop_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

There appears to be evidence for two modes. We can also see this using
the smooth density estimator using `geom_density`:

``` r
filter(dat, fragment == "No trisomy") %>%
  ggplot(aes(weight)) +
  geom_density()
```

![](3-workshop_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

Note that this is similar to the histogram above. Now let’s make it
nice:

``` r
filter(dat, fragment == "No trisomy") %>%
  ggplot(aes(weight)) +
  geom_density(fill = 1, alpha = 0.5) + 
  xlim(c(19,45))
```

![](3-workshop_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

We only looked at the distribution of `weight` in the control group. We
can use ridge plots to compare the distribution across all groups. To
use ridge plots we need to install and load the `ggridges` functions.

``` r
library(ggridges)
dat %>% 
  ggplot(aes(weight, fragment)) +
  geom_density_ridges() 
```

![](3-workshop_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

To avoid the overlap you can use:

``` r
library(ggridges)
dat %>% 
  ggplot(aes(weight, fragment)) +
  geom_density_ridges(scale = 0.9) 
```

![](3-workshop_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

We see that for most groups we see two modes. What could explain this?

One variable to consider is sex. We can remake the boxplot with points
above but this time use color to distinguish the data from males and
females.

``` r
dat %>% 
  ggplot(aes(fragment, weight, color = sex)) +
  geom_boxplot(width = 0.5, color = "grey") + 
  geom_jitter(width = 0.1, alpha = 0.5)
```

![](3-workshop_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

A problem here is that sex is a number\! It should be a character or a
factor. Change it to a character vector using `mutate` then remake the
plot

``` r
dat <- mutate(dat, sex = ifelse(sex == 1, "Male", "Female"))

dat %>% ggplot(aes(fragment, weight, color = sex)) +
  geom_boxplot(width = 0.5, color = "grey") + 
  geom_jitter(width = 0.1, alpha = 0.5)
```

![](3-workshop_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

Now that we see that males and females have different distributions, the
question arises if the fragment effects are different for males and
females. We can explore this by remaking the plot for each sex
separately. This can be achieved using the the faceting.

``` r
dat %>% ggplot(aes(fragment, weight, color = sex)) +
  geom_boxplot(width = 0.5, color = "grey") + 
  geom_jitter(width = 0.1, alpha = 0.5) + 
  facet_grid(.~sex)
```

![](3-workshop_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

A quick pause to describe themes. `ggplot` provides the options of
changing the general look of plots. There is an entire package,
**ggthemes**, dedicated to providing different themes. Here we use the
black and white theme:

``` r
dat %>% ggplot(aes(fragment, weight, color = sex)) +
  geom_boxplot(width = 0.5, color = "grey") + 
  geom_jitter(width = 0.1, alpha = 0.5) + 
  facet_grid(.~sex) + 
  theme_bw()
```

![](3-workshop_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

This plot is quite informative. First it shows strong evidence that the
152F7 has an effect on weight, especially on females.

### Cage effect?

Are males and females caged together?

$$

$$

Is cage counfounded with fragment? Make boxplots for the females.

$$

$$

When order by meaningful values.

``` r
dat %>% 
  filter(sex == "Female") %>%
  mutate(cage = factor(cage)) %>%
  mutate(cage = reorder(cage, weight, median)) %>%
  ggplot(aes(cage, weight, color = fragment)) +
  geom_boxplot(width = 0.5, color = "grey") + 
  geom_jitter(width = 0.1, alpha = 0.5) + 
  theme_bw()
```

![](3-workshop_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

Take a closer look. Compare just controls and 152F7. Are trisomic mice
higher within each cage? Is what we are seeing really a cage effect?

$$

$$

### Confounding

For the female mice compute the correlation between blood pressure and
weight.

$$

$$

The correlation is negative. Does this make sense? Confirm with a plot
that there are not outliers driving this result.

$$

$$

The plot does confirm that higher weight mice have, on average slightly
lower blood pressure. But we do see clusters. What could these be? Use
color to decipher what cuases the clustering.

$$

$$

Now use faceting to plot the scatter plot for each fragment separately.

$$

$$

We note that the correlation appears positive in each group. Use
`group_by` and `summarize` to corroborate this.

$$

$$

Note: Cases in which the overall correlation is the opposite sign as
when looking strata is referred to as Simpsons Paradox.
