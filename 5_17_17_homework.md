# 5_17_17_homework
Allie Gaudinier  
5/16/2017  



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
delays <- flights %>% 
  group_by(dest) %>% 
  summarise(
    count = n(),
    dist = mean(distance, na.rm = TRUE),
    delay = mean(arr_delay, na.rm = TRUE)
  ) %>% 
  filter(count > 20, dest != "HNL")
delays
```

```
## # A tibble: 96 × 4
##     dest count      dist     delay
##    <chr> <int>     <dbl>     <dbl>
## 1    ABQ   254 1826.0000  4.381890
## 2    ACK   265  199.0000  4.852273
## 3    ALB   439  143.0000 14.397129
## 4    ATL 17215  757.1082 11.300113
## 5    AUS  2439 1514.2530  6.019909
## 6    AVL   275  583.5818  8.003831
## 7    BDL   443  116.0000  7.048544
## 8    BGR   375  378.0000  8.027933
## 9    BHM   297  865.9966 16.877323
## 10   BNA  6333  758.2135 11.812459
## # ... with 86 more rows
```

##Exercises 5.6.7


1. Brainstorm at least 5 different ways to assess the typical delay characteristics of a group of flights. Consider the following scenarios:
 A flight is 15 minutes early 50% of the time, and 15 minutes late 50% of the time.

```r
not_cancelled <- flights %>% 
  filter(!is.na(dep_delay), !is.na(arr_delay))

not_cancelled %>%
  group_by(flight, arr_delay) %>%
  summarise((mean(arr_delay == 15)))
```

```
## Source: local data frame [149,961 x 3]
## Groups: flight [?]
## 
##    flight arr_delay `(mean(arr_delay == 15))`
##     <int>     <dbl>                     <dbl>
## 1       1       -69                         0
## 2       1       -61                         0
## 3       1       -60                         0
## 4       1       -59                         0
## 5       1       -58                         0
## 6       1       -57                         0
## 7       1       -53                         0
## 8       1       -52                         0
## 9       1       -50                         0
## 10      1       -49                         0
## # ... with 149,951 more rows
```
 
 A flight is always 10 minutes late.

```r
 filter(not_cancelled, arr_delay == 10)
```

```
## # A tibble: 3,373 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      624            630        -6      840
## 2   2013     1     1      717            720        -3      850
## 3   2013     1     1      745            745         0     1135
## 4   2013     1     1      805            805         0     1015
## 5   2013     1     1      811            815        -4     1026
## 6   2013     1     1      921            900        21     1237
## 7   2013     1     1     1158           1205        -7     1530
## 8   2013     1     1     1211           1215        -4     1423
## 9   2013     1     1     1455           1459        -4     1655
## 10  2013     1     1     1554           1600        -6     1830
## # ... with 3,363 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

 A flight is 30 minutes early 50% of the time, and 30 minutes late 50% of the time.

```r
not_cancelled %>%
  group_by(flight, arr_delay) %>%
  summarise((mean(arr_delay == 30)))
```

```
## Source: local data frame [149,961 x 3]
## Groups: flight [?]
## 
##    flight arr_delay `(mean(arr_delay == 30))`
##     <int>     <dbl>                     <dbl>
## 1       1       -69                         0
## 2       1       -61                         0
## 3       1       -60                         0
## 4       1       -59                         0
## 5       1       -58                         0
## 6       1       -57                         0
## 7       1       -53                         0
## 8       1       -52                         0
## 9       1       -50                         0
## 10      1       -49                         0
## # ... with 149,951 more rows
```
 99% of the time a flight is on time. 1% of the time it’s 2 hours late.

```r
not_cancelled %>%
  group_by(origin, carrier, flight) %>%
  summarise(quantile(arr_delay > 120, 0.99))
```

```
## Source: local data frame [6,848 x 4]
## Groups: origin, carrier [?]
## 
##    origin carrier flight `quantile(arr_delay > 120, 0.99)`
##     <chr>   <chr>  <int>                             <dbl>
## 1     EWR      9E   3285                                 0
## 2     EWR      9E   3291                                 0
## 3     EWR      9E   3299                                 0
## 4     EWR      9E   3300                                 0
## 5     EWR      9E   3301                                 0
## 6     EWR      9E   3302                                 0
## 7     EWR      9E   3303                                 0
## 8     EWR      9E   3304                                 0
## 9     EWR      9E   3309                                 0
## 10    EWR      9E   3311                                 0
## # ... with 6,838 more rows
```
 
Which is more important: arrival delay or departure delay?
Arrival delay - can make up time in the air. Look at depature delay, arrival delay - not always equal

2. Come up with another approach that will give you the same output as 
not_cancelled %>% count(dest)  


not_cancelled %>%
group_byflight ) %>%
      summarise(dest = sum(dest))

not_cancelled %>% count(tailnum, wt = distance) (without using count()).

3. Our definition of cancelled flights (is.na(dep_delay) | is.na(arr_delay) ) is slightly suboptimal. Why? Which is the most important column?
What happens if the plane  did not have a departure delay or an arrival delay (both equal zero) - I don't think it will count as a flight. The best way to show if the flight existed is airtime

4. Look at the number of cancelled flights per day. Is there a pattern? Is the proportion of cancelled flights related to the average delay?


```r
cancelled <- flights %>% 
  filter(is.na(dep_delay), is.na(arr_delay))

library(ggplot2)
cancelled %>%
  group_by(day) %>%
  summarise(n = n())
```

```
## # A tibble: 31 × 2
##      day     n
##    <int> <int>
## 1      1   246
## 2      2   250
## 3      3   109
## 4      4    82
## 5      5   226
## 6      6   296
## 7      7   318
## 8      8   921
## 9      9   593
## 10    10   535
## # ... with 21 more rows
```

```r
ggplot(data = not_cancelled, mapping = aes(x = flight, y = dep_delay)) + 
    geom_point(alpha = 1/10)
```

![](5_17_17_homework_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

5. Which carrier has the worst delays? Challenge: can you disentangle the effects of bad airports vs. bad carriers? Why/why not? (Hint: think about flights %>% group_by(carrier, dest) %>% summarise(n()))


```r
not_cancelled %>%
  group_by(carrier) %>%
  arrange(desc(dep_delay))
```

```
## Source: local data frame [327,346 x 19]
## Groups: carrier [16]
## 
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
## # ... with 327,336 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```


6. What does the sort argument to count() do. When might you use it?
?count()
count(x, ..., wt = NULL, sort = FALSE) 
wt - If omitted, will count the number of rows. If specified, will perform a "weighted" tally by summing the (non-missing) values of variable wt


##5.7.1 Exercises

1. Refer back to the lists of useful mutate and filtering functions. Describe how each operation changes when you combine it with grouping.
These function within the groups

2. Which plane (tailnum) has the worst on-time record?

```r
not_cancelled %>%
  group_by(tailnum) %>%
  arrange(desc(arr_delay))
```

```
## Source: local data frame [327,346 x 19]
## Groups: tailnum [4,037]
## 
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     9      641            900      1301     1242
## 2   2013     6    15     1432           1935      1137     1607
## 3   2013     1    10     1121           1635      1126     1239
## 4   2013     9    20     1139           1845      1014     1457
## 5   2013     7    22      845           1600      1005     1044
## 6   2013     4    10     1100           1900       960     1342
## 7   2013     3    17     2321            810       911      135
## 8   2013     7    22     2257            759       898      121
## 9   2013    12     5      756           1700       896     1058
## 10  2013     5     3     1133           2055       878     1250
## # ... with 327,336 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

3. What time of day should you fly if you want to avoid delays as much as possible? - May 7th

```r
not_cancelled %>%
  group_by(day) %>%
  arrange((arr_delay))
```

```
## Source: local data frame [327,346 x 19]
## Groups: day [31]
## 
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     5     7     1715           1729       -14     1944
## 2   2013     5    20      719            735       -16      951
## 3   2013     5     2     1947           1949        -2     2209
## 4   2013     5     6     1826           1830        -4     2045
## 5   2013     5     4     1816           1820        -4     2017
## 6   2013     5     2     1926           1929        -3     2157
## 7   2013     5     6     1753           1755        -2     2004
## 8   2013     5     7     2054           2055        -1     2317
## 9   2013     5    13      657            700        -3      908
## 10  2013     1     4     1026           1030        -4     1305
## # ... with 327,336 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

4. For each destination, compute the total minutes of delay. For each, flight, compute the proportion of the total delay for its destination.

```r
#not_cancelled %>%
#  group_by(dest) %>%
#  summarize(arr_delay, 
#    n = n())

not_cancelled %>%
  group_by(dest) %>%
  count(arr_delay)
```

```
## Source: local data frame [19,339 x 3]
## Groups: dest [?]
## 
##     dest arr_delay     n
##    <chr>     <dbl> <int>
## 1    ABQ       -61     1
## 2    ABQ       -58     1
## 3    ABQ       -56     1
## 4    ABQ       -55     2
## 5    ABQ       -52     1
## 6    ABQ       -51     1
## 7    ABQ       -49     1
## 8    ABQ       -45     2
## 9    ABQ       -44     1
## 10   ABQ       -43     3
## # ... with 19,329 more rows
```


5. Delays are typically temporally correlated: even once the problem that caused the initial delay has been resolved, later flights are delayed to allow earlier flights to leave. Using lag() explore how the delay of a flight is related to the delay of the immediately preceding flight.

6. Look at each destination. Can you find flights that are suspiciously fast? (i.e. flights that represent a potential data entry error). Compute the air time a flight relative to the shortest flight to that destination. Which flights were most delayed in the air?


```r
flights %>%
  transmute(origin, dest, distance, air_time, flight) %>%
  group_by(origin, dest, distance, air_time) %>%
  arrange(air_time)
```

```
## Source: local data frame [336,776 x 5]
## Groups: origin, dest, distance, air_time [11,904]
## 
##    origin  dest distance air_time flight
##     <chr> <chr>    <dbl>    <dbl>  <int>
## 1     EWR   BDL      116       20   4368
## 2     EWR   BDL      116       20   4631
## 3     EWR   BDL      116       21   4276
## 4     EWR   PHL       80       21   4619
## 5     EWR   BDL      116       21   4368
## 6     EWR   PHL       80       21   4619
## 7     LGA   BOS      184       21   2132
## 8     JFK   PHL       94       21   3650
## 9     EWR   BDL      116       21   4118
## 10    EWR   BDL      116       21   4276
## # ... with 336,766 more rows
```


7. Find all destinations that are flown by at least two carriers. Use that information to rank the carriers.
flights %>%
  group_by(dest, carrier) %>%
  summarize(carrier >= 2)


8. For each plane, count the number of flights before the first delay of greater than 1 hour.
not_cancelled %>%
  group_by(flight)
  summarize()
