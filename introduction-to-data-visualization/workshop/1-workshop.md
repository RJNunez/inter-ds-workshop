Workshop: Introduction to the Tidyverse
================

In this workshop we will go through the material shown earlier today.
Each set of excercises will focus on one of the main topics:

  - Tidy data
  - Transforming data
      - `mutate`
      - `filter`
      - `select`
  - The pipe operator `%>%`
  - Summaring data
  - Sorting data frames
  - The `do` function

## Exercise set 1: Tidy data

1.  Examine the dataset `co2`. Which of the following is true:
    
    A. `co2` is tidy data: it has one year for each row.
    
    B. `co2` is not tidy: we need at least one column with a character
    vector.
    
    C. `co2` is not tidy: it is a matrix not a data frame.
    
    D. `co2` is not tidy: to be tidy we would have to wrangle it to have
    three columns: year, month and value; then each co2 observation has
    a row.

2.  Examine the dataset `ChickWeight`. Which of the following is true:
    
    1.  `ChickWeight` is not tidy: each chick has more than one row.
    
    2.  `ChickWeight` is tidy: each observation, here a weight, is
        represented by one row. The chick from which this measurement
        came from is one the variables.
    
    3.  `ChickWeight` is not a tidy: we are missing the year column.
    
    4.  `ChickWeight` is tidy: it is stored in a data frame.

3.  Examine the dataset `BOD`. Which of the following is true:
    
    A. `BOD` is not tidy: it only has six rows.
    
    B. `BOD` is not tidy: the first column is just an index.
    
    C. `BOD` is tidy: each row is an observation with two values, time
    and demand.
    
    D. `BOD` is tidy: all small datasets are tidy by definition.

4.  Which of the following datasets is tidy (you can pick more than
    one):
    
    A. `BJsales`
    
    B. `EuStockMarkets`
    
    C. `DNase`
    
    D. `Formaldehyde`
    
    E. `Orange`
    
    F. `UCBAdmissions`

## Exercise set 2: Transforming data

1.  Load the **dplyr** package and the murders dataset.

<!-- end list -->

``` r
library(dplyr)
library(dslabs)
data(murders)
```

You can add columns using the **dplyr** function `mutate`. This function
is aware of the column names and inside the function you can call them
unquoted:

``` r
murders <- mutate(murders, population_in_millions = population / 10^6)
```

We can write `population` rather than `murders$population`. The function
`mutate` knows we are grabbing columns from `murders`.

Use the function `mutate` to add a murders column named `rate` with the
per 100,000 murder rate as in the example code above. Make sure you
redefine `murders` as done in the example code above ( murders \<-
\[your code\]) so we can keep using this variable.

2.  If `rank(x)` gives you the ranks of `x` from lowest to highest,
    `rank(-x)` gives you the ranks from highest to lowest. Use the
    function `mutate` to add a column `rank` containing the rank, from
    highest to lowest murder rate. Make sure you redefine `murders` so
    we can keep using this variable.

3.  With **dplyr**, we can use `select` to show only certain columns.
    For example, with this code we would only show the states and
    population sizes:

<!-- end list -->

``` r
select(murders, state, population) %>% head()
```

Use `select` to show the state names and abbreviations in `murders`. Do
not redefine `murders`, just show the results.

4.  The **dplyr** function `filter` is used to choose specific rows of
    the data frame to keep. Unlike `select` which is for columns,
    `filter` is for rows. For example, you can show just the New York
    row like this:

<!-- end list -->

``` r
filter(murders, state == "New York")
```

You can use other logical vectors to filter rows.

Use `filter` to show the top 5 states with the highest murder rates.
After we add murder rate and rank, do not change the murders dataset,
just show the result. Remember that you can filter based on the `rank`
column.

5.  We can remove rows using the `!=` operator. For example, to remove
    Florida, we would do this:

<!-- end list -->

``` r
no_florida <- filter(murders, state != "Florida")
```

Create a new data frame called `no_south` that removes states from the
South region. How many states are in this category? You can use the
function `nrow` for this.

6.  We can also use `%in%` to filter with **dplyr**. You can therefore
    see the data from New York and Texas like this:

<!-- end list -->

``` r
filter(murders, state %in% c("New York", "Texas"))
```

Create a new data frame called `murders_nw` with only the states from
the Northeast and the West. How many states are in this category?

7.  Suppose you want to live in the Northeast or West **and** want the
    murder rate to be less than 1. We want to see the data for the
    states satisfying these options. Note that you can use logical
    operators with `filter`. Here is an example in which we filter to
    keep only small states in the Northeast region.

<!-- end list -->

``` r
filter(murders, population < 5000000 & region == "Northeast")
```

Make sure `murders` has been defined with `rate` and `rank` and still
has all states. Create a table, call it `my_states`, that contains rows
for states satisfying both the conditions: it is in the Northeast or
West and the murder rate is less than 1. Use `select` to show only the
state name, the rate and the rank.

## Exercise set 3: The pipe operator `%>%`

1.  The pipe `%>%` can be used to perform operations sequentially
    without having to define intermediate objects. Start by redefining
    murder to include rate and
rank.

<!-- end list -->

``` r
murders <- mutate(murders, rate =  total / population * 100000, rank = rank(-rate))
```

In the solution to the previous exercise, we did the
following:

``` r
my_states <- filter(murders, region %in% c("Northeast", "West") & rate < 1)

select(my_states, state, rate, rank)
```

The pipe `%>%` permits us to perform both operations sequentially
without having to define an intermediate variable `my_states`. We
therefore could have mutated and selected in the same line like
this:

``` r
mutate(murders, rate =  total / population * 100000, rank = rank(-rate)) %>%
  select(state, rate, rank)
```

Notice that `select` no longer has a data frame as the first argument.
The first argument is assumed to be the result of the operation
conducted right before the `%>%`.

Repeat the previous exercise, but now instead of creating a new object,
show the result and only include the state, rate, and rank columns. Use
a pipe `%>%` to do this in just one line.

2.  Reset `murders` to the original table by using `data(murders)`. Use
    a pipe to create a new data frame, called `my_states`, that has a
    murder rate and a rank column, considers only states in the
    Northeast or West which have a murder rate lower than 1, and
    contains only the state, rate and rank columns. The pipe should also
    have four components separated by three `%>%`.

<!-- end list -->

  - The original dataset `murders`.
  - A call to `mutate` to add the murder rate and the rank.
  - A call to `filter` to keep only the states from the Northeast or
    West and that have a murder rate below 1.
  - A call to `select` that keeps only the columns with the state name,
    the murder rate and the rank.

The code should look something like this:

``` r
my_states <- murders %>%
  mutate SOMETHING %>% 
  filter SOMETHING %>% 
  select SOMETHING
```

## Exercise set 4: Summarize and sorting

For these exercises, we will be using the data from the survey collected
by the United States National Center for Health Statistics (NCHS). This
center has conducted a series of health and nutrition surveys since the
1960’s. Starting in 1999, about 5,000 individuals of all ages have been
interviewed every year and they complete the health examination
component of the survey. Part of the data is made available via the
**NHANES** package which can install using:

``` r
install.packages("NHANES")
```

Once you install it, you can load the data this way:

``` r
library(NHANES)
data(NHANES)
```

The NHANES data has many missing values. Remember that the main
summarization function in R will return `NA` if any of the entries of
the input vector is an `NA`. Here is an example:

``` r
library(dslabs)
data(na_example)
mean(na_example)
```

    ## [1] NA

``` r
sd(na_example)
```

    ## [1] NA

To ignore the `NA`s we can use the `na.rm` argument:

``` r
mean(na_example, na.rm = TRUE)
```

    ## [1] 2.301754

``` r
sd(na_example, na.rm = TRUE)
```

    ## [1] 1.22338

Let’s now explore the NHANES data.

1.  We will provide some basic facts about blood pressure. First let’s
    select a group to set the standard. We will use 20-29 year old
    females. `AgeDecade` is a categorical variable with these ages. Note
    that the category is coded like " 20-29", with a space in front\!
    What is the average and standard deviation of systolic blood
    pressure as saved in the `BPSysAve` variable? Save it to a variable
    called `ref`.

Hint: Use `filter` and `summarize` and use the `na.rm = TRUE` argument
when computing the average and standard deviation. You can also filter
the NA values using `filter`.

2.  Using a pipe, assign the average to a numeric variable `ref_avg`.
    Hint: Use the code similar to above and then `pull`.

3.  Now report the min and max values for the same group.

4.  Compute the average and standard deviation for females, but for each
    age group separately. Note that the age groups are defined by
    `AgeDecade`. Hint: rather than filtering by age, filter by `Gender`
    and then use `group_by`.

5.  Repeat exercise 4 for males.

6.  We can actually combine both these summaries into one line of code.
    This is because `group_by` permits us to group by more than one
    variable. Obtain one big summary table using `group_by(AgeDecade,
    Gender)`.

7.  For males between the ages of 40-49, compare systolic blood pressure
    across race as reported in the `Race1` variable. Order the resulting
    table from lowest to highest average systolic blood pressure.
