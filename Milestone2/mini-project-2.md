Mini Data Analysis Milestone 2
================

*To complete this milestone, you can edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to your second (and last) milestone in your mini data analysis project!

In Milestone 1, you explored your data, came up with research questions,
and obtained some results by making summary tables and graphs. This
time, we will first explore more in depth the concept of *tidy data.*
Then, you’ll be sharpening some of the results you obtained from your
previous milestone by:

- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 55 points (compared to the 45 points
of the Milestone 1): 45 for your analysis, and 10 for your entire
mini-analysis GitHub repository. Details follow.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

- Understand what *tidy* data is, and how to create it using `tidyr`.
- Generate a reproducible and clear report using R Markdown.
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

# Task 1: Tidy your data (15 points)

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

*Tidy’ing* data is sometimes necessary because it can simplify
computation. Other times it can be nice to organize data so that it can
be easier to understand when read manually.

### 2.1 (2.5 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

``` r
# select eight columns from vancounver_trees to see if the data is tidy or untidy
eight_cols <- vancouver_trees %>%
    select(tree_id, civic_number, std_street, genus_name, species_name, height_range_id, diameter, date_planted)
head(eight_cols)
```

    ## # A tibble: 6 × 8
    ##   tree_id civic_number std_street genus_name specie…¹ heigh…² diame…³ date_pla…⁴
    ##     <dbl>        <dbl> <chr>      <chr>      <chr>      <dbl>   <dbl> <date>    
    ## 1  149556          494 W 58TH AV  ULMUS      AMERICA…       2      10 1999-01-13
    ## 2  149563          450 W 58TH AV  ZELKOVA    SERRATA        4      10 1996-05-31
    ## 3  149579         4994 WINDSOR ST STYRAX     JAPONICA       3       4 1993-11-22
    ## 4  149590          858 E 39TH AV  FRAXINUS   AMERICA…       4      18 1996-04-29
    ## 5  149604         5032 WINDSOR ST ACER       CAMPEST…       2       9 1993-12-17
    ## 6  149616          585 W 61ST AV  PYRUS      CALLERY…       2       5 NA        
    ## # … with abbreviated variable names ¹​species_name, ²​height_range_id, ³​diameter,
    ## #   ⁴​date_planted

For selected eight variables, each row represents an observation of a
tree in Vancouver. Each columns contains one variable and the name of
each variable clearly explain what value the column stores. Each cell
has a value that corresponds to the variable. Hence, the data is tidy.

<!----------------------------------------------------------------------------->

### 2.2 (5 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

The data is tidy, so we untidy the data first and tidy it back to its
original state.

Before: Every two row represents an observation, which makes the data
untidy.

``` r
tidy_data <- eight_cols %>%
    pivot_longer(cols = c(genus_name, species_name), # select the columns that you want to change
                 names_to = 'tree_type', # set the names to new variable "tree_type"
                 values_to = 'tree_type_name') # set the values to new variable "tree_type_name"
print(tidy_data)
```

    ## # A tibble: 293,222 × 8
    ##    tree_id civic_number std_street height_r…¹ diame…² date_pla…³ tree_…⁴ tree_…⁵
    ##      <dbl>        <dbl> <chr>           <dbl>   <dbl> <date>     <chr>   <chr>  
    ##  1  149556          494 W 58TH AV           2      10 1999-01-13 genus_… ULMUS  
    ##  2  149556          494 W 58TH AV           2      10 1999-01-13 specie… AMERIC…
    ##  3  149563          450 W 58TH AV           4      10 1996-05-31 genus_… ZELKOVA
    ##  4  149563          450 W 58TH AV           4      10 1996-05-31 specie… SERRATA
    ##  5  149579         4994 WINDSOR ST          3       4 1993-11-22 genus_… STYRAX 
    ##  6  149579         4994 WINDSOR ST          3       4 1993-11-22 specie… JAPONI…
    ##  7  149590          858 E 39TH AV           4      18 1996-04-29 genus_… FRAXIN…
    ##  8  149590          858 E 39TH AV           4      18 1996-04-29 specie… AMERIC…
    ##  9  149604         5032 WINDSOR ST          2       9 1993-12-17 genus_… ACER   
    ## 10  149604         5032 WINDSOR ST          2       9 1993-12-17 specie… CAMPES…
    ## # … with 293,212 more rows, and abbreviated variable names ¹​height_range_id,
    ## #   ²​diameter, ³​date_planted, ⁴​tree_type, ⁵​tree_type_name

After: Tidy the data back to its original state.

``` r
untidy_data <- tidy_data %>%
    pivot_wider(id_cols = c(tree_id, civic_number, std_street, height_range_id, diameter, date_planted), # select unchanged columns
                 names_from = tree_type, # get names from variable tree_type
                 values_from = tree_type_name) # get values from variable tree_type_name
print(untidy_data)
```

    ## # A tibble: 146,611 × 8
    ##    tree_id civic_number std_street    heigh…¹ diame…² date_pla…³ genus…⁴ speci…⁵
    ##      <dbl>        <dbl> <chr>           <dbl>   <dbl> <date>     <chr>   <chr>  
    ##  1  149556          494 W 58TH AV           2    10   1999-01-13 ULMUS   AMERIC…
    ##  2  149563          450 W 58TH AV           4    10   1996-05-31 ZELKOVA SERRATA
    ##  3  149579         4994 WINDSOR ST          3     4   1993-11-22 STYRAX  JAPONI…
    ##  4  149590          858 E 39TH AV           4    18   1996-04-29 FRAXIN… AMERIC…
    ##  5  149604         5032 WINDSOR ST          2     9   1993-12-17 ACER    CAMPES…
    ##  6  149616          585 W 61ST AV           2     5   NA         PYRUS   CALLER…
    ##  7  149617         4909 SHERBROOKE ST       3    15   1993-12-16 ACER    PLATAN…
    ##  8  149618         4925 SHERBROOKE ST       3    14   1993-12-16 ACER    PLATAN…
    ##  9  149619         4969 SHERBROOKE ST       2    16   1993-12-16 ACER    PLATAN…
    ## 10  149625          720 E 39TH AV           2     7.5 1993-12-03 FRAXIN… AMERIC…
    ## # … with 146,601 more rows, and abbreviated variable names ¹​height_range_id,
    ## #   ²​diameter, ³​date_planted, ⁴​genus_name, ⁵​species_name

<!----------------------------------------------------------------------------->

### 2.3 (7.5 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the next four tasks:

<!-------------------------- Start your work below ---------------------------->

1.  *Which decade saw more trees planted in Vancouver?*
2.  *What is the relationship between the height of a tree and its age?
    Are older trees generally higher?*

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

For research question 1, I would like to compare the number of trees
planted in different period like past twenty years and nearly past ten
years. Also, I would like to explore the height of trees in each decade.

For research question 2, I would like to explore more about the
relationship between the height of a tree and its age within a year or a
month.

<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

<!--------------------------- Start your work below --------------------------->

Modify vancouver_trees to get the appropriate version for following
questions:

``` r
vancouver_trees_updated <- vancouver_trees %>%
    select(tree_id,  genus_name,  height_range_id, diameter, date_planted) %>% # select needed columns
    drop_na() %>% # remove NA values
    filter(date_planted > '1989-12-31') %>% # select observations that after 1989-12-31
    # create a new variable to represent decade
    # assign 1990~1999 label to trees planed in this range
    # assign 2000~2009 label to trees planed in this range
    # assign 2010~2019 label to trees planed in this range
    mutate(year_period = ifelse(date_planted <= '1999-12-31', "1990~1999", 
                       ifelse(date_planted >= '2000-1-1' & date_planted <= '2009-12-31', "2000~2009", 
                       ifelse(date_planted >= '2010-1-1', "2010~2019", NA)))) 
vancouver_trees_updated
```

    ## # A tibble: 69,763 × 6
    ##    tree_id genus_name height_range_id diameter date_planted year_period
    ##      <dbl> <chr>                <dbl>    <dbl> <date>       <chr>      
    ##  1  149556 ULMUS                    2    10    1999-01-13   1990~1999  
    ##  2  149563 ZELKOVA                  4    10    1996-05-31   1990~1999  
    ##  3  149579 STYRAX                   3     4    1993-11-22   1990~1999  
    ##  4  149590 FRAXINUS                 4    18    1996-04-29   1990~1999  
    ##  5  149604 ACER                     2     9    1993-12-17   1990~1999  
    ##  6  149617 ACER                     3    15    1993-12-16   1990~1999  
    ##  7  149618 ACER                     3    14    1993-12-16   1990~1999  
    ##  8  149619 ACER                     2    16    1993-12-16   1990~1999  
    ##  9  149625 FRAXINUS                 2     7.5  1993-12-03   1990~1999  
    ## 10  149626 TILIA                    2     7.75 1993-12-03   1990~1999  
    ## # … with 69,753 more rows

<!----------------------------------------------------------------------------->

# Task 2: Special Data Types (10)

For this exercise, you’ll be choosing two of the three tasks below –
both tasks that you choose are worth 5 points each.

But first, tasks 1 and 2 below ask you to modify a plot you made in a
previous milestone. The plot you choose should involve plotting across
at least three groups (whether by facetting, or using an aesthetic like
colour). Place this plot below (you’re allowed to modify the plot if
you’d like). If you don’t have such a plot, you’ll need to make one.
Place the code for your plot below.

<!-------------------------- Start your work below ---------------------------->

Point plot where x = year of planted and y = height_range_id and wrap
using year_period

``` r
tress_in_decades <- vancouver_trees_updated %>%
  ggplot(aes(date_planted, height_range_id, colour = year_period)) + # x = date_planted, y = height_range_id
  geom_point(alpha = 0.1) + # draw point plot with alpha = 0.1
  facet_wrap(~year_period) + # wrap with year_period
  labs(x = "Year of planted", y = "Height id of trees"); # set labels
tress_in_decades
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

<!----------------------------------------------------------------------------->

Now, choose two of the following tasks.

1.  Produce a new plot that reorders a factor in your original plot,
    using the `forcats` package (3 points). Then, in a sentence or two,
    briefly explain why you chose this ordering (1 point here for
    demonstrating understanding of the reordering, and 1 point for
    demonstrating some justification for the reordering, which could be
    subtle or speculative.)

2.  Produce a new plot that groups some factor levels together into an
    “other” category (or something similar), using the `forcats` package
    (3 points). Then, in a sentence or two, briefly explain why you
    chose this grouping (1 point here for demonstrating understanding of
    the grouping, and 1 point for demonstrating some justification for
    the grouping, which could be subtle or speculative.)

3.  If your data has some sort of time-based column like a date (but
    something more granular than just a year):

    1.  Make a new column that uses a function from the `lubridate` or
        `tsibble` package to modify your original time-based column. (3
        points)

        - Note that you might first have to *make* a time-based column
          using a function like `ymd()`, but this doesn’t count.
        - Examples of something you might do here: extract the day of
          the year from a date, or extract the weekday, or let 24 hours
          elapse on your dates.

    2.  Then, in a sentence or two, explain how your new column might be
        useful in exploring a research question. (1 point for
        demonstrating understanding of the function you used, and 1
        point for your justification, which could be subtle or
        speculative).

        - For example, you could say something like “Investigating the
          day of the week might be insightful because penguins don’t
          work on weekends, and so may respond differently”.

<!-------------------------- Start your work below ---------------------------->

**Task Number**: 2

I utilize fct_collapse to group 1990\~1999 and 2000\~2009 together as a
new column and leave 2010\~2019 as a new column alone. In this way, I
can compare the number of trees planted in 1990\~2009 with trees planted
in nearly past ten years (2010\~2019) and also see their height
distribution. Besides, I can also combine other 20 years together to
compare more.

``` r
tress_in_decades_fct <- vancouver_trees_updated %>%
  # combine 1990~1999 and 2000~2009 together
  mutate(new_year_period = fct_collapse(year_period, past_twenty_years = c("1990~1999","2000~2009"), nearly_ten_years = "2010~2019")) %>%
  ggplot(aes(date_planted, height_range_id, colour = new_year_period)) + # x = date_planted, y = height_range_id 
  geom_point(alpha = 0.1) + # point plot with alpha = 0.1
  facet_wrap(~new_year_period) +
  labs(x = "Date of planted", y = "Height id of trees") # set labels
tress_in_decades_fct
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

<!----------------------------------------------------------------------------->
<!-------------------------- Start your work below ---------------------------->

**Task Number**: 3

I utilize functions from ‘lubridate’ to extract year, month, and day
from the date_planted variable. In this way, I can check the
relationship between trees’ height and their age in more detail. For
example, I can compare them in monthly or weekly or daily which gives
more detailed relationship result.

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
vancouver_trees_dates <- vancouver_trees_updated %>%
    mutate(year = year(date_planted)) %>%
    mutate(month = month(date_planted)) %>%
    mutate(day = day(date_planted))

print(vancouver_trees_dates)
```

    ## # A tibble: 69,763 × 9
    ##    tree_id genus_name height_rang…¹ diame…² date_pla…³ year_…⁴  year month   day
    ##      <dbl> <chr>              <dbl>   <dbl> <date>     <chr>   <dbl> <dbl> <int>
    ##  1  149556 ULMUS                  2   10    1999-01-13 1990~1…  1999     1    13
    ##  2  149563 ZELKOVA                4   10    1996-05-31 1990~1…  1996     5    31
    ##  3  149579 STYRAX                 3    4    1993-11-22 1990~1…  1993    11    22
    ##  4  149590 FRAXINUS               4   18    1996-04-29 1990~1…  1996     4    29
    ##  5  149604 ACER                   2    9    1993-12-17 1990~1…  1993    12    17
    ##  6  149617 ACER                   3   15    1993-12-16 1990~1…  1993    12    16
    ##  7  149618 ACER                   3   14    1993-12-16 1990~1…  1993    12    16
    ##  8  149619 ACER                   2   16    1993-12-16 1990~1…  1993    12    16
    ##  9  149625 FRAXINUS               2    7.5  1993-12-03 1990~1…  1993    12     3
    ## 10  149626 TILIA                  2    7.75 1993-12-03 1990~1…  1993    12     3
    ## # … with 69,753 more rows, and abbreviated variable names ¹​height_range_id,
    ## #   ²​diameter, ³​date_planted, ⁴​year_period

<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 2.0 (no points)

Pick a research question, and pick a variable of interest (we’ll call it
“Y”) that’s relevant to the research question. Indicate these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: What is the relationship between the height of a
tree and its age? Are older trees generally higher?

**Variable of interest**: height_range_id

<!----------------------------------------------------------------------------->

## 2.1 (5 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression.

<!-------------------------- Start your work below ---------------------------->

I would like to find the relationship between the height of trees and
the year they are planted to check if older trees will usually higher
where x = year and y = height_range_id.

``` r
height_year_model <- lm(height_range_id ~ year, vancouver_trees_dates) # set up model
summary(height_year_model) # get summary information of the model
```

    ## 
    ## Call:
    ## lm(formula = height_range_id ~ year, data = vancouver_trees_dates)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -2.7784 -0.4231 -0.1220  0.2545  7.8239 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  1.526e+02  7.578e-01   201.4   <2e-16 ***
    ## year        -7.529e-02  3.781e-04  -199.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.7381 on 69761 degrees of freedom
    ## Multiple R-squared:  0.3624, Adjusted R-squared:  0.3624 
    ## F-statistic: 3.964e+04 on 1 and 69761 DF,  p-value: < 2.2e-16

<!----------------------------------------------------------------------------->

## 2.2 (5 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

I will predict the height_range_ids of trees planted in 1979, 1989,
1999, 2009, and 2019

``` r
# a column contains multiple planted years that I want to predict
year_planted_data = tribble(~year, 1979, 1989, 1999, 2009, 2019)
broom::augment(x = height_year_model, newdata = year_planted_data, type.predict = "response") # get predicted height range id of the planted years (year_planted_data)
```

    ## # A tibble: 5 × 2
    ##    year .fitted
    ##   <dbl>   <dbl>
    ## 1  1979   3.61 
    ## 2  1989   2.85 
    ## 3  1999   2.10 
    ## 4  2009   1.35 
    ## 5  2019   0.595

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 3.1 (5 points)

Take a summary table that you made from Milestone 1 (Task 4.2), and
write it as a csv file in your `output` folder. Use the `here::here()`
function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
# Use summary statistics of diameter to help determine which genus of trees are more likely to be thicker.
diameter_summary <- vancouver_trees %>%
  group_by(genus_name) %>% # group rows by genus_name
  drop_na(diameter) %>% # drop NA values
  # calculate min, Q1, median, mean, Q2, max and range of diameter
  summarise(min = min(diameter), Q1 = quantile(diameter, probs = 1/4), median = median(diameter), mean = mean(diameter), Q2 = quantile(diameter, probs = 3/4), max = max(diameter), range = max - mean)
diameter_summary
```

    ## # A tibble: 97 × 8
    ##    genus_name    min    Q1 median  mean    Q2   max  range
    ##    <chr>       <dbl> <dbl>  <dbl> <dbl> <dbl> <dbl>  <dbl>
    ##  1 ABIES           1  3     12    12.9  19.5   42.5  29.6 
    ##  2 ACER            0  3      8    10.6  15    317   306.  
    ##  3 AESCULUS        0 19     25    23.7  30     64    40.3 
    ##  4 AILANTHUS       3 15     19.5  15.9  20.4   21.5   5.62
    ##  5 ALBIZIA         6  6      6     6     6      6     0   
    ##  6 ALNUS           0 12     17.5  17.5  23.4   40    22.5 
    ##  7 AMELANCHIER     0  3      3     3.21  3     20    16.8 
    ##  8 ARALIA          3  4.31   6.12  6.81  8.62  12     5.19
    ##  9 ARAUCARIA       3  4.12   8.5  11.4  12.9   32    20.6 
    ## 10 ARBUTUS         6  7.75  17.5  18.4  28.5   33    14.6 
    ## # … with 87 more rows

``` r
write_csv(diameter_summary, here::here("output", "diameter_summary.csv")) # write csv file
```

<!----------------------------------------------------------------------------->

## 3.2 (5 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 3.1 apply here.

<!-------------------------- Start your work below ---------------------------->

``` r
saveRDS(height_year_model,here::here("output", "objects.rds")) # save rds file
readRDS(here::here("output", "objects.rds")) # read the saved rds file
```

    ## 
    ## Call:
    ## lm(formula = height_range_id ~ year, data = vancouver_trees_dates)
    ## 
    ## Coefficients:
    ## (Intercept)         year  
    ##   152.60990     -0.07529

<!----------------------------------------------------------------------------->

# Tidy Repository

Now that this is your last milestone, your entire project repository
should be organized. Here are the criteria we’re looking for.

## Main README (3 points)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

- In a sentence or two, explains what this repository is, so that
  future-you or someone else stumbling on your repository can be
  oriented to the repository.
- In a sentence or two (or more??), briefly explains how to engage with
  the repository. You can assume the person reading knows the material
  from STAT 545A. Basically, if a visitor to your repository wants to
  explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without them!
They are tremendously helpful.

## File and Folder structure (3 points)

You should have at least four folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (2 points)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output, and all data files
  saved from Task 4 above appear in the `output` folder.
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

PS: there’s a way where you can run all project code using a single
command, instead of clicking “knit” three times. More on this in STAT
545B!

## Error-free code (1 point)

This Milestone 1 document knits error-free, and the Milestone 2 document
knits error-free.

## Tagged release (1 point)

You’ve tagged a release for Milestone 1, and you’ve tagged a release for
Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.