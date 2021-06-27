Workshop: Introduction to Data Wrangling I
================

1.  Run the following command to define the `co2_wide` object:

<!-- end list -->

``` r
co2_wide <- data.frame(matrix(co2, ncol = 12, byrow = TRUE)) %>% 
  setNames(1:12) %>%
  mutate(year = as.character(1959:1997))
```

Use the `pivot_longer` function to wrangle this into a tidy dataset.
Call the column with the CO2 measurements `co2` and call the month
column `month`. Call the resulting object `co2_tidy`.

**Answer**

2.  Plot CO2 versus month with a different curve for each year using
    this code:

<!-- end list -->

``` r
co2_tidy %>% ggplot(aes(month, co2, color = year)) + geom_line()
```

If the expected plot is not made, it is probably because
`co2_tidy$month` is not numeric:

``` r
class(co2_tidy$month)
```

Rewrite the call to `pivot_longer` using an argument that assures the
month column will be numeric. Then make the plot.

**Answer**

3.  What do we learn from this plot?
    
    A. CO2 measures increase monotonically from 1959 to 1997. B. CO2
    measures are higher in the summer and the yearly average increased
    from 1959 to 1997. C. CO2 measures appear constant and random
    variability explains the differences. D. CO2 measures do not have a
    seasonal trend.

4.  Now load the `admissions` data set which contains admission
    information for men and women across six majors and keep only the
    admitted percentage column:

<!-- end list -->

``` r
load(admissions)
dat <- admissions %>% select(-applicants)
```

If we think of an observation as a major, and that each observation has
two variables, men admitted percentage and women admitted percentage,
then this is not tidy. Use the `pivot_wider` function to wrangle into
tidy shape: one row for each major.

**Answer**

5.  Now we will try a more advanced wrangling challenge. We want to
    wrangle the admissions data so that for each major we have 4
    observations: `admitted_men`, `admitted_women`, `applicants_men` and
    `applicants_women`. The *trick* we perform here is actually quite
    common: first pivot longer to generate an intermediate data frame
    and then pivot wider to obtain the tidy data we want. We will go
    step by step in this and the next two exercises.

Use the `pivot_longer` function to create a `tmp` data frame with a
column containing the type of observation `admitted` or `applicants`.
Call the new columns `key` and `value`.

**Answer**

6.  Now you have an object `tmp` with columns `major`, `gender`, `key`
    and `value`. Note that if you combine the key and gender, we get the
    column names we want: `admitted_men`, `admitted_women`,
    `applicants_men` and `applicants_women`. Use the function `unite` to
    create a new column called `column_name`.

**Answer**

7.  Now use the `pivot_wider` function to generate the tidy data with
    four variables for each major.

**Answer**

8.  Now use the pipe to write a line of code that turns admission to the
    table produced in the previous exercise.

**Answer**
