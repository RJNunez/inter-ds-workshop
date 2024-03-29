Workshop: Introduction to Data Wrangling III
================

1.  Complete all lessons and exercises in the <https://regexone.com/>
    online interactive tutorial.

2.  In the `extdata` directory of the **dslabs** package, you will find
    a PDF file containing daily mortality data for Puerto Rico from Jan
    1, 2015 to May 31, 2018. You can find the file like
this:

<!-- end list -->

``` r
fn <- system.file("extdata", "RD-Mortality-Report_2015-18-180531.pdf", package="dslabs")
```

Find and open the file or open it directly from RStudio. On a Mac, you
can type:

``` r
system2("open", args = fn)
```

and on Windows, you can type:

``` r
system("cmd.exe", input = paste("start", fn))
```

Which of the following best describes this file:

    A. It is a table. Extracting the data will be easy.
    B. It is a report written in prose. Extracting the data will be impossible.
    C. It is a report combining graphs and tables. Extracting the data seems possible.
    D. It shows graphs of the data. Extracting the data will be difficult.

**Answer**

3.  We are going to create a tidy dataset with each row representing one
    observation. The variables in this dataset will be year, month, day
    and deaths. Start by installing and loading the pdftools package:

<!-- end list -->

``` r
install.packages("pdftools")
library(pdftools)
```

Now read-in `fn` using the `pdf_text` function, store the results in an
object called `txt`. Describe what you see in `txt`.

    A. A table with the mortality data.
    B. A character string of length 12. Each entry represents the text in each page. The mortality data is in there somewhere.
    C. A character string with one entry containing all the information in the PDF file.
    D. An html document.

**Answer**

4.  Extract the ninth page of the PDF file from the object `txt`, then
    use the `str_split` from the **stringr** package so that you have
    each line in a different entry. Call this string vector `s`. Then
    look at the result and choose the one that best describes what you
    see.
    
    A. It is an empty string. B. I can see the figure shown in page 1.
    C. It is a tidy table. D. I can see the table\! But there is a bunch
    of other stuff we need to get rid of.

**Answer**

5.  What kind of object is `s` and how many entries does it have?

**Answer**

6.  We see that the output is a list with one component. Redefine `s` to
    be the first entry of the list. What kind of object is `s` and how
    many entries does it have?

**Answer**

7.  When inspecting the string we obtained above, we see a common
    problem: white space before and after the other characters. Trimming
    is a common first step in string processing. These extra spaces will
    eventually make splitting the strings hard so we start by removing
    them. We learned about the command `str_trim` that removes spaces at
    the start or end of the strings. Use this function to trim `s`.

**Answer**

8.  We want to extract the numbers from the strings stored in `s`.
    However, there a lot of non-numeric characters that will get in the
    way. We can remove these, but before doing this we want to preserve
    the string with the column header, which includes the month
    abbreviation. Use the `str_which` function to find the rows with a
    header. Save these results to `header_index`. Hint: find the first
    string that matches the pattern `2015` using the `str_which`
    function.

**Answer**

9.  Now we are going to define two objects: `month` will store the month
    and `header` will store the column names. Identify which row
    contains the header of the table. Save the content of the row into
    an object called `header`, then use `str_split` to help define the
    two objects we need. Hints: the separator here is one or more
    spaces. Also, consider using the `simplify` argument.

**Answer**

10. Notice that towards the end of the page you see a *totals* row
    followed by rows with other summary statistics. Create an object
    called `tail_index` with the index of the *totals* entry.

**Answer**

11. Because our PDF page includes graphs with numbers, some of our rows
    have just one number (from the y-axis of the plot). Use the
    `str_count` function to create an object `n` with the number of
    numbers in each each row. Hint: you can write a regex for number
    like this `\\d+`.

**Answer**

12. We are now ready to remove entries from rows that we know we don’t
    need. The entry `header_index` and everything before it should be
    removed. Entries for which `n` is 1 should also be removed, and the
    entry `tail_index` and everything that comes after it should be
    removed as well.

**Answer**

13. Now we are ready to remove all the non-numeric entries. Do this
    using regex and the `str_remove_all` function. Hint: in regex, using
    the `^` inside the `[]` means *not*, like the `!` means not in `!=`.
    To define the regex pattern to catch all non-numbers, you can type
    `[^\\d]`. But remember you also want to keep spaces.

**Answer**

14. To convert the strings into a table, use the `str_split_fixed`
    function. Convert `s` into a data matrix with just the day and death
    count data. Hints: note that the separator is one or more spaces.
    Make the argument `n` a value that limits the number of columns to
    the values in the 4 columns and the last column captures all the
    extra stuff. Then keep only the first four columns.

**Answer**

15. Now you are almost ready to finish. Add column names to the matrix,
    including one called `day`. Also, add a column with the month. Call
    the resulting object `dat`. Finally, make sure the day is an integer
    not a character. Hint: use only the first five columns.

**Answer**

16. Now finish it up by tidying `tab` with the gather function.

**Answer**

17. Make a plot of deaths versus day with color to denote year. Exclude
    2018 since we have no data.

**Answer**
