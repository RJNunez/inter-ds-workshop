Workshop: Introduction to Data Wrangling II
================

1. Using the resulting dataset from yesterday, create a figure with `date` on the *x-axis* and `deaths` on the *y-axis* for Puerto Rico. To that same figure, add a line with the average number of deaths (`average`) using the color *steelblue3* and a size of 0.80. Only use *unweighted* data. Label the axes accordingly and add a title to the figure. Describe what you see? 

**Answer**

2. To the same figure above, add a line denoting the `threshold` using the color `red3` and a size of 0.80. Describe what you see?

**Answer**

3. **OPTIONAL:** To the same figure above, paint the data points above the threshold red using the color `red3`. Is there a pattern of red data points? Can you described that? 

**Answer**

4. Excess deaths is defined as the difference between the **observed number of deaths** and the **average number of deaths**. The Center for Disease Control and Prevention (CDC) uses the Farrington model to estimate excess deaths. This model provides two excess deaths estimates: 1) an unbiased estimate and 2) a conservative estimate. Denote $Y_{t}$ as the number of deaths at week $t$, $\hat{\mu}_t$

$$\begin{aligned}
\mbox{Unbiased estimate: } Y_t - \hat{\mu}_t \\
\mbox{Conservative estimate: } Y_t - \hat{\gamma}_t
\end{aligned}$$

Compute both of these estimates for Puerto Rico. Only use *unweighted* data.

5. Create a figure with `date` on the *x-axis* and the excess deaths estimates on the *y-axis*. Use color to differentiate between the unbiased and conservative estimates. Label the axes accordingly and add a title to the figure. Describe what you see? 

**HINT:** use `pivot_longer`

**Answer**

6. Due to the nature of the estimates, many weekly excess death estimates are negative. Mutate the newly created variables so that negative weekly excess death estimates are set to zero. Then, recreate the figure from above.

**Answer**

7. What other data would you incorporate into this analysis? What information do you need to have valid comparisons between Puerto Rico and other states?

**Answer**
