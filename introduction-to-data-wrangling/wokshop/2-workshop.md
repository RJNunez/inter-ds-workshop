Workshop: Introduction to Data Wrangling II
================

1.  Install and load the **Lahman** library. This database includes data
    related to Baseball teams. It includes summary statistics about how
    the players performed on offense and defense for several years. It
    also includes personal information about the players.

Here is a data frame with the offensive statistics for all players in
2016.

``` r
library(Lahman)
Batting %>% 
  filter(yearID == 2016) %>% 
  as_tibble()
```

You can see the top 10 hitters by running this code:

``` r
top <- Batting %>% 
  filter(yearID == 2016) %>%
  arrange(desc(HR)) %>%
  slice(1:10)
top
```

But who are these players? We see an ID, but not the names. The player
names are in this table

``` r
Master %>% 
  as_tibble()
```

We can see column names `nameFirst` and `nameLast`. Use the `left_join`
function to create a table of the top home run hitters. The table should
have `playerID`, first name, last name, and number of home runs (HR).
Rewrite the object `top` with this new table.

**Answer**

2.  Now use the `Salaries` data frame to add each player’s salary to the
    table you created in exercise 1. Note that salaries are different
    every year so make sure to filter for the year 2016, then use
    `right_join`. This time show first name, last name, team, HR and
    salary.

**Answer:**

3.  In a previous exercise, we created a tidy version of the `co2`
    dataset:

<!-- end list -->

``` r
co2_wide <- data.frame(matrix(co2, ncol = 12, byrow = TRUE)) %>% 
  setNames(1:12) %>%
  mutate(year = 1959:1997) %>%
  gather(month, co2, -year, convert = TRUE)
```

We want to see if the monthly trend is changing so we are going to
remove the year effects and the plot the data. We will first compute the
year averages. Use the `group_by` and `summarize` to compute the average
co2 for each year. Save in an object called `yearly_avg`.

**Answer**

4.  Now use the `left_join` function to add the yearly average to the
    `co2_wide` dataset. Then compute the residuals: observed co2 measure
    - yearly average.

**Answer**

5.  Make a plot of the seasonal trends by year but only after removing
    the year effect.

**Answer**
