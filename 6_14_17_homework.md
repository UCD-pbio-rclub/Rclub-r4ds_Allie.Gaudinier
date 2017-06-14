# 6_14_17_homework
Allie Gaudinier  
6/13/2017  

# 6/14/17 Homework

```r
library(tidyverse)
```

```
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: tidyr
## Loading tidyverse: readr
## Loading tidyverse: purrr
## Loading tidyverse: dplyr
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

###12.2.1 Exercises

```
1. Using prose, describe how the variables and observations are organised in each of the sample tables.
Table 1: Table organized with years separated - the rest of the data is in columns
Table 2: Measurements are put in a "type" column and the data is  in a "count" column
Table 3: similar to table 2, cases/population is combined into a "rate" column
Table 4: separated into 2 tables - one for cases and one for population. columns for each year
```
```
2. Compute the rate for table2, and table4a + table4b. You will need to perform four operations:
    1. Extract the number of TB cases per country per year. 
    2. Extract the matching population per country per year.
    3. Divide cases by population, and multiply by 10000.
    4. Store back in the appropriate place.

Use the commands descibe in the next sections.... 

Which representation is easiest to work with? Which is hardest? Why?
Table 1 is easiest, Table 4 is hardest - data is not all in one place. 

```

```
3. Recreate the plot showing change in cases over time using table2 instead of table1. What do you need to do first?
```

```r
library(ggplot2)
table2 %>%
  filter(type == "cases") %>%
  ggplot(aes(year, count)) + 
  geom_line(aes(group = country), colour = "grey50") + 
  geom_point(aes(colour = country))
```

![](6_14_17_homework_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

###12.3.3 Exercises
``
1. Why are gather() and spread() not perfectly symmetrical?
Carefully consider the following example:
```

```r
stocks <- tibble(
  year   = c(2015, 2015, 2016, 2016),
  half  = c(   1,    2,     1,    2),
  return = c(1.88, 0.59, 0.92, 0.17)
)

stocks %>% 
  spread(year, return) %>% 
  gather("year", "return", `2015`:`2016`)
```

```
## # A tibble: 4 × 3
##    half  year return
##   <dbl> <chr>  <dbl>
## 1     1  2015   1.88
## 2     2  2015   0.59
## 3     1  2016   0.92
## 4     2  2016   0.17
```
```
(Hint: look at the variable types and think about column names.)
They are different depending on what variables are "gathered" or "spread" - this will change the shape of the tibble
```
```
2. Both spread() and gather() have a convert argument. What does it do?
?spread()
convert	If TRUE, type.convert with asis = TRUE will be run on each of the new columns. This is useful if the value column was a mix of variables that was coerced to a string. If the class of the value column was factor or date, note that will not be true of the new columns that are produced, which are coerced to character before type conversion.
```
```
3. Why does this code fail?

table4a %>% 
  gather(1999, 2000, key = "year", value = "cases")
> Error in combine_vars(vars, ind_list): Position must be between 0 and n

This code fails because there year needs backticks
```

```r
table4a %>% 
  gather(`1999`, `2000`, key = "year", value = "cases")
```

```
## # A tibble: 6 × 3
##       country  year  cases
##         <chr> <chr>  <int>
## 1 Afghanistan  1999    745
## 2      Brazil  1999  37737
## 3       China  1999 212258
## 4 Afghanistan  2000   2666
## 5      Brazil  2000  80488
## 6       China  2000 213766
```
  
```
3. Why does spreading this tibble fail? How could you add a new column to fix the problem?

people <- tribble(
  ~name,             ~key,    ~value,
  #-----------------|--------|------
  "Phillip Woods",   "age",       45,
  "Phillip Woods",   "height",   186,
  "Phillip Woods",   "age",       50,
  "Jessica Cordero", "age",       37,
  "Jessica Cordero", "height",   156
)
This are multiple entries for "Phillip Woods" age
```

```
4. Tidy the simple tibble below. Do you need to spread or gather it? What are the variables?
```

```r
preg <- tribble(
  ~pregnant, ~male, ~female,
  "yes",     NA,    10,
  "no",      20,    12
)

preg %>%
  gather(sex, count, male, female)
```

```
## # A tibble: 4 × 3
##   pregnant    sex count
##      <chr>  <chr> <dbl>
## 1      yes   male    NA
## 2       no   male    20
## 3      yes female    10
## 4       no female    12
```

###12.4.3 Exercises

```
1. What do the extra and fill arguments do in separate()? Experiment with the various options for the following two toy datasets.
extra:
If sep is a character vector, this controls what happens when there are too many pieces. There are three valid options:
"warn" (the default): emit a warning and drop extra values.
"drop": drop any extra values without a warning.
"merge": only splits at most length(into) times

fill	
If sep is a character vector, this controls what happens when there are not enough pieces. There are three valid options:
"warn" (the default): emit a warning and fill from the right
"right": fill with missing values on the right
"left": fill with missing values on the left
```

```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
  separate(x, c("one", "two", "three"), extra = "warn")
```

```
## Warning: Too many values at 1 locations: 2
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e     f
## 3     h     i     j
```

```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
  separate(x, c("one", "two", "three"), extra = "drop")
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e     f
## 3     h     i     j
```

```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
  separate(x, c("one", "two", "three"), extra = "merge")
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e   f,g
## 3     h     i     j
```

```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
  separate(x, c("one", "two", "three"), fill = "warn")
```

```
## Warning: Too many values at 1 locations: 2
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e     f
## 3     h     i     j
```

```r
tibble(x = c("a,b,c", "d,e,f,g", "h,i,j")) %>% 
  separate(x, c("one", "two", "three"), fill = "right")
```

```
## Warning: Too many values at 1 locations: 2
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e     f
## 3     h     i     j
```

```r
tibble(x = c("a,b,c", "d,e", "f,g,i")) %>% 
  separate(x, c("one", "two", "three"), fill = "left")
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2  <NA>     d     e
## 3     f     g     i
```
```  
2. Both unite() and separate() have a remove argument. What does it do? Why would you set it to FALSE?
?unite()
remove:	If TRUE, remove input columns from output data frame.
Make false in case you want to keep both the input and output columns.
```

```
3. Compare and contrast separate() and extract(). Why are there three variations of separation (by position, by separator, and with groups), but only one unite?
?extract()
unite combines two columns - you have to directly tell it which columns to use
separate can be applied to any columns that are described by your code
```

```r
tibble(x = c("a,b,c", "d,e,f", "g,h,i")) %>% 
  separate(x, c("one", "two", "three"))
```

```
## # A tibble: 3 × 3
##     one   two three
## * <chr> <chr> <chr>
## 1     a     b     c
## 2     d     e     f
## 3     g     h     i
```

```r
tibble(x = c("a,b,c", "d,e,f", "g,h,i")) %>% 
  separate(x, "one")
```

```
## Warning: Too many values at 3 locations: 1, 2, 3
```

```
## # A tibble: 3 × 1
##     one
## * <chr>
## 1     a
## 2     d
## 3     g
```

```r
tibble(x = c("a,b,c", "d,e,f", "g,h,i")) %>% 
  extract(x, "one")
```

```
## # A tibble: 3 × 1
##     one
## * <chr>
## 1     a
## 2     d
## 3     g
```
