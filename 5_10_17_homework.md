# 5_10_17_homework
Allie Gaudinier  
5/9/2017  

##5.2.4 Exercises

1. Find all flights that: 
    1. Had an arrival delay of two or more hours

```r
library(nycflights13)
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

```r
filter(flights, arr_delay > 2)
```

```
## # A tibble: 123,096 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      554            558        -4      740
## 5   2013     1     1      555            600        -5      913
## 6   2013     1     1      558            600        -2      753
## 7   2013     1     1      558            600        -2      924
## 8   2013     1     1      559            600        -1      941
## 9   2013     1     1      600            600         0      837
## 10  2013     1     1      602            605        -3      821
## # ... with 123,086 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

  2. Flew to Houston (IAH or HOU)

```r
filter(flights, dest == "IAH" | dest == "HOU")
```

```
## # A tibble: 9,313 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      623            627        -4      933
## 4   2013     1     1      728            732        -4     1041
## 5   2013     1     1      739            739         0     1104
## 6   2013     1     1      908            908         0     1228
## 7   2013     1     1     1028           1026         2     1350
## 8   2013     1     1     1044           1045        -1     1352
## 9   2013     1     1     1114            900       134     1447
## 10  2013     1     1     1205           1200         5     1503
## # ... with 9,303 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

  3. Were operated by United, American, or Delta

```r
filter(flights, carrier %in% c("UA", "AA","DL"))
```

```
## # A tibble: 139,504 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      554            600        -6      812
## 5   2013     1     1      554            558        -4      740
## 6   2013     1     1      558            600        -2      753
## 7   2013     1     1      558            600        -2      924
## 8   2013     1     1      558            600        -2      923
## 9   2013     1     1      559            600        -1      941
## 10  2013     1     1      559            600        -1      854
## # ... with 139,494 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

  4. Departed in summer (July, August, and September)

```r
filter(flights, month %in% c(7,8,9))
```

```
## # A tibble: 86,326 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     7     1        1           2029       212      236
## 2   2013     7     1        2           2359         3      344
## 3   2013     7     1       29           2245       104      151
## 4   2013     7     1       43           2130       193      322
## 5   2013     7     1       44           2150       174      300
## 6   2013     7     1       46           2051       235      304
## 7   2013     7     1       48           2001       287      308
## 8   2013     7     1       58           2155       183      335
## 9   2013     7     1      100           2146       194      327
## 10  2013     7     1      100           2245       135      337
## # ... with 86,316 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

  5. Arrived more than two hours late, but didn’t leave late    

```r
filter(flights, arr_delay > 2, dep_delay == 0)
```

```
## # A tibble: 4,368 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      600            600         0      837
## 2   2013     1     1      635            635         0     1028
## 3   2013     1     1      739            739         0     1104
## 4   2013     1     1      745            745         0     1135
## 5   2013     1     1      800            800         0     1022
## 6   2013     1     1      805            805         0     1015
## 7   2013     1     1      810            810         0     1048
## 8   2013     1     1      823            823         0     1151
## 9   2013     1     1      830            830         0     1018
## 10  2013     1     1      835            835         0     1210
## # ... with 4,358 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

  6. Were delayed by at least an hour, but made up over 30 minutes in flight

```r
filter(flights, dep_delay > 1, air_time)
```

```
## # A tibble: 119,719 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      608            600         8      807
## 5   2013     1     1      611            600        11      945
## 6   2013     1     1      613            610         3      925
## 7   2013     1     1      623            610        13      920
## 8   2013     1     1      632            608        24      740
## 9   2013     1     1      644            636         8      931
## 10  2013     1     1      702            700         2     1058
## # ... with 119,709 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

    7. Departed between midnight and 6am (inclusive)

```r
filter(flights, dep_time > 000, dep_time < 600)
```

```
## # A tibble: 8,730 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      544            545        -1     1004
## 5   2013     1     1      554            600        -6      812
## 6   2013     1     1      554            558        -4      740
## 7   2013     1     1      555            600        -5      913
## 8   2013     1     1      557            600        -3      709
## 9   2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 8,720 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

2. Another useful dplyr filtering helper is between(). What does it do? Can you use it to simplify the code needed to answer the previous challenges?

```r
?between()
##between(flights, dep_time == 000, dep_time == 600)
## no idea how to simplify previous code
```


3. How many flights have a missing dep_time? What other variables are missing? What might these rows represent?

```r
count(flights, dep_time = NA)
```

```
## # A tibble: 1 × 2
##   dep_time      n
##      <lgl>  <int>
## 1       NA 336776
```

4. Why is NA ^ 0 not missing? Why is NA | TRUE not missing? Why is FALSE & NA not missing? Can you figure out the general rule? (NA * 0 is a tricky counterexample!)

```r
NA ^ 0
```

```
## [1] 1
```

```r
NA | TRUE
```

```
## [1] TRUE
```

```r
FALSE & NA
```

```
## [1] FALSE
```

```r
NA * 0
```

```
## [1] NA
```

Everything ^ 0 is 1
NA or TRUE, therefore it is TRUE
FALSE and NA, can be FALSE



##5.3.1 Exercises

1. How could you use arrange() to sort all missing values to the start? (Hint: use is.na()).

```r
df <- tibble(x = c(5, 2, NA))
arrange(df, desc(is.na(x)))
```

```
## # A tibble: 3 × 1
##       x
##   <dbl>
## 1    NA
## 2     5
## 3     2
```

2. Sort flights to find the most delayed flights. Find the flights that left earliest.

```r
arrange(flights, desc(dep_delay))
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     9      641            900      1301     1242
## 2   2013     6    15     1432           1935      1137     1607
## 3   2013     1    10     1121           1635      1126     1239
## 4   2013     9    20     1139           1845      1014     1457
## 5   2013     7    22      845           1600      1005     1044
## 6   2013     4    10     1100           1900       960     1342
## 7   2013     3    17     2321            810       911      135
## 8   2013     6    27      959           1900       899     1236
## 9   2013     7    22     2257            759       898      121
## 10  2013    12     5      756           1700       896     1058
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

```r
arrange(flights, dep_time)
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1    13        1           2249        72      108
## 2   2013     1    31        1           2100       181      124
## 3   2013    11    13        1           2359         2      442
## 4   2013    12    16        1           2359         2      447
## 5   2013    12    20        1           2359         2      430
## 6   2013    12    26        1           2359         2      437
## 7   2013    12    30        1           2359         2      441
## 8   2013     2    11        1           2100       181      111
## 9   2013     2    24        1           2245        76      121
## 10  2013     3     8        1           2355         6      431
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

3. Sort flights to find the fastest flights.

```r
arrange(flights, air_time)
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1    16     1355           1315        40     1442
## 2   2013     4    13      537            527        10      622
## 3   2013    12     6      922            851        31     1021
## 4   2013     2     3     2153           2129        24     2247
## 5   2013     2     5     1303           1315       -12     1342
## 6   2013     2    12     2123           2130        -7     2211
## 7   2013     3     2     1450           1500       -10     1547
## 8   2013     3     8     2026           1935        51     2131
## 9   2013     3    18     1456           1329        87     1533
## 10  2013     3    19     2226           2145        41     2305
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

4. Which flights travelled the longest? Which travelled the shortest?

```r
arrange(flights, desc(distance))
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      857            900        -3     1516
## 2   2013     1     2      909            900         9     1525
## 3   2013     1     3      914            900        14     1504
## 4   2013     1     4      900            900         0     1516
## 5   2013     1     5      858            900        -2     1519
## 6   2013     1     6     1019            900        79     1558
## 7   2013     1     7     1042            900       102     1620
## 8   2013     1     8      901            900         1     1504
## 9   2013     1     9      641            900      1301     1242
## 10  2013     1    10      859            900        -1     1449
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

```r
arrange(flights, distance)
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     7    27       NA            106        NA       NA
## 2   2013     1     3     2127           2129        -2     2222
## 3   2013     1     4     1240           1200        40     1333
## 4   2013     1     4     1829           1615       134     1937
## 5   2013     1     4     2128           2129        -1     2218
## 6   2013     1     5     1155           1200        -5     1241
## 7   2013     1     6     2125           2129        -4     2224
## 8   2013     1     7     2124           2129        -5     2212
## 9   2013     1     8     2127           2130        -3     2304
## 10  2013     1     9     2126           2129        -3     2217
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

##5.4.1 Exercises

1. Brainstorm as many ways as possible to select dep_time, dep_delay, arr_time, and arr_delay from flights.

```r
select(flights, dep_time, dep_delay, arr_time, arr_delay)
```

```
## # A tibble: 336,776 × 4
##    dep_time dep_delay arr_time arr_delay
##       <int>     <dbl>    <int>     <dbl>
## 1       517         2      830        11
## 2       533         4      850        20
## 3       542         2      923        33
## 4       544        -1     1004       -18
## 5       554        -6      812       -25
## 6       554        -4      740        12
## 7       555        -5      913        19
## 8       557        -3      709       -14
## 9       557        -3      838        -8
## 10      558        -2      753         8
## # ... with 336,766 more rows
```

```r
# you could also ! select everything besides these - but that's a lot of unnecessary typing
```

2. What happens if you include the name of a variable multiple times in a select() call?
It only prints it once

```r
select(flights, dep_time, dep_delay, dep_time)
```

```
## # A tibble: 336,776 × 2
##    dep_time dep_delay
##       <int>     <dbl>
## 1       517         2
## 2       533         4
## 3       542         2
## 4       544        -1
## 5       554        -6
## 6       554        -4
## 7       555        -5
## 8       557        -3
## 9       557        -3
## 10      558        -2
## # ... with 336,766 more rows
```

3. What does the one_of() function do? Why might it be helpful in conjunction with this vector?
selects variables based on variables in character vector.
This could be useful if you just want to select a specific year, month, etc

```r
?one_of()
vars <- c("year", "month", "day", "dep_delay", "arr_delay")
```

4. Does the result of running the following code surprise you? How do the select helpers deal with case by default? How can you change that default?
Case doesn't matter in the select function default.

```r
select(flights, contains("TIME", ignore.case = F))
```

```
## # A tibble: 336,776 × 0
```

##5.5.2 Exercises

1. Currently dep_time and sched_dep_time are convenient to look at, but hard to compute with because they’re not really continuous numbers. Convert them to a more convenient representation of number of minutes since midnight.

```r
transmute(flights, dep_time, dep_time_hour = dep_time %/% 100, dep_time_min = dep_time %% 100, dep_time_sincemidnight = dep_time_hour*60 + dep_time_min)
```

```
## # A tibble: 336,776 × 4
##    dep_time dep_time_hour dep_time_min dep_time_sincemidnight
##       <int>         <dbl>        <dbl>                  <dbl>
## 1       517             5           17                    317
## 2       533             5           33                    333
## 3       542             5           42                    342
## 4       544             5           44                    344
## 5       554             5           54                    354
## 6       554             5           54                    354
## 7       555             5           55                    355
## 8       557             5           57                    357
## 9       557             5           57                    357
## 10      558             5           58                    358
## # ... with 336,766 more rows
```

2. Compare air_time with arr_time - dep_time. What do you expect to see? What do you see? What do you need to do to fix it?
air_time is in minutes, arr and dep time is in hours:minutes - need to convert

```r
transmute(flights, air_time, altairtime = arr_time - dep_time)
```

```
## # A tibble: 336,776 × 2
##    air_time altairtime
##       <dbl>      <int>
## 1       227        313
## 2       227        317
## 3       160        381
## 4       183        460
## 5       116        258
## 6       150        186
## 7       158        358
## 8        53        152
## 9       140        281
## 10      138        195
## # ... with 336,766 more rows
```

```r
#this should fix it
transmute(flights, air_time, dep_time_hour = dep_time %/% 100, dep_time_min = dep_time %% 100, dep_time_sincemidnight = dep_time_hour*60 + dep_time_min, 
          arr_time_hour = arr_time %/% 100, arr_time_min = arr_time %% 100, arr_time_sincemidnight = arr_time_hour*60 + arr_time_min, altairtime = arr_time_sincemidnight- dep_time_sincemidnight)
```

```
## # A tibble: 336,776 × 8
##    air_time dep_time_hour dep_time_min dep_time_sincemidnight
##       <dbl>         <dbl>        <dbl>                  <dbl>
## 1       227             5           17                    317
## 2       227             5           33                    333
## 3       160             5           42                    342
## 4       183             5           44                    344
## 5       116             5           54                    354
## 6       150             5           54                    354
## 7       158             5           55                    355
## 8        53             5           57                    357
## 9       140             5           57                    357
## 10      138             5           58                    358
## # ... with 336,766 more rows, and 4 more variables: arr_time_hour <dbl>,
## #   arr_time_min <dbl>, arr_time_sincemidnight <dbl>, altairtime <dbl>
```

3. Compare dep_time, sched_dep_time, and dep_delay. How would you expect those three numbers to be related? dep_delay + sched_dep_time = dep_time

```r
transmute(flights, dep_time, sched_dep_time, dep_delay)
```

```
## # A tibble: 336,776 × 3
##    dep_time sched_dep_time dep_delay
##       <int>          <int>     <dbl>
## 1       517            515         2
## 2       533            529         4
## 3       542            540         2
## 4       544            545        -1
## 5       554            600        -6
## 6       554            558        -4
## 7       555            600        -5
## 8       557            600        -3
## 9       557            600        -3
## 10      558            600        -2
## # ... with 336,766 more rows
```

4. Find the 10 most delayed flights using a ranking function. How do you want to handle ties? Carefully read the documentation for min_rank(). ties.method = "min"

```r
transmute(flights, min_rank(desc(dep_delay)))
```

```
## # A tibble: 336,776 × 1
##    `min_rank(desc(dep_delay))`
##                          <int>
## 1                       114150
## 2                       103893
## 3                       114150
## 4                       144947
## 5                       258934
## 6                       209494
## 7                       234113
## 8                       185276
## 9                       185276
## 10                      163760
## # ... with 336,766 more rows
```

5. What does 1:3 + 1:10 return? Why? 
It cycles through the 1:3 vector for the additions

```r
1:3 + 1:10
```

```
## Warning in 1:3 + 1:10: longer object length is not a multiple of shorter
## object length
```

```
##  [1]  2  4  6  5  7  9  8 10 12 11
```

6. What trigonometric functions does R provide?
R computes the cosine, sine, tangent, arc-cosine, arc-sine, arc-tangent, and the two-argument arc-tangent


