Workshop: Introduction to Data Wrangling III
================

1. Using the resulting dataset from yesterday and functions learned today, mutate the `type` variable so that the two categories are **weighted** and **unweighted** instead of **Predicted (weighted)** and **Unweighted**. Similarly, mutate the `outcome` variable so that the two categories are **all causes** and **excluding covid-19** instead of **All causes** and **All causes, excluding COVID-19**.

**HINT:** Use `str_to_lower`
**HINT:** Use `str_replace` or `str_replace_all`

**Answer**

2. From now on, subset the data with `type = weighted` and `outcome = all causes`. The relative change from a number $A$ to number $B$ is defined as: 

$$\begin{aligned}
B / A - 1
\end{aligned}$$

Using the resulting dataset from yesterday, compute the relative change in weekly observed deaths compared to the average weekly deaths in Puerto Rico starting in March 2020 (the onset of COVID-19). Create a figure with `date` on the *x-axis* and the newly computed percent change on the *y-axis*. Describe what you see?

**OPTIONAL:** Use `geom_smooth` to distinguish the signal from the noise

**Answer**

3. Let us see if the bumps from above match with COVID-19 mortality. Using the `outcome` variable, create a figure with `date` on the *x-axis* and `deaths` from COVID-19 on the *y-axis*. Compare this figure with the one from above. Describe what you see?

**HINT:** Use `pivot_wider`
**HINT:** Don't subset with `outcome = all causes`

**Answer**

4. Devised one metric to summarize cumulative excess mortality in Puerto Rico since March 2020 (the onset of COVID-19) using the data you have. This metric should be useful for comparing Puerto Rico to other states. Compute the metric using the dataset. Provide pros and cons of your metric. Is it useful for comparisons? What other data would be useful for this? Where could you find these data?

**Answer**
