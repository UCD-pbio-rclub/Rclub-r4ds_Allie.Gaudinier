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
  group_by(flight) %>%
  summarise(early = mean(arr_delay <= -15), late = mean(arr_delay >= 15)) %>%
  filter(early ==0.5 , late == 0.5)
```

```
## # A tibble: 21 × 3
##    flight early  late
##     <int> <dbl> <dbl>
## 1     107   0.5   0.5
## 2    2072   0.5   0.5
## 3    2366   0.5   0.5
## 4    2500   0.5   0.5
## 5    2552   0.5   0.5
## 6    3495   0.5   0.5
## 7    3505   0.5   0.5
## 8    3518   0.5   0.5
## 9    3544   0.5   0.5
## 10   3651   0.5   0.5
## # ... with 11 more rows
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
  group_by(flight) %>%
  summarise(early = mean(arr_delay <= -30), late = mean(arr_delay >= 30)) %>%
  filter(early ==0.5 , late == 0.5)
```

```
## # A tibble: 3 × 3
##   flight early  late
##    <int> <dbl> <dbl>
## 1   3651   0.5   0.5
## 2   3916   0.5   0.5
## 3   3951   0.5   0.5
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

  

```r
not_cancelled %>% count(dest)
```

```
## # A tibble: 104 × 2
##     dest     n
##    <chr> <int>
## 1    ABQ   254
## 2    ACK   264
## 3    ALB   418
## 4    ANC     8
## 5    ATL 16837
## 6    AUS  2411
## 7    AVL   261
## 8    BDL   412
## 9    BGR   358
## 10   BHM   269
## # ... with 94 more rows
```

```r
##same as above
not_cancelled %>%
group_by(dest) %>%
      summarise(flights = n())
```

```
## # A tibble: 104 × 2
##     dest flights
##    <chr>   <int>
## 1    ABQ     254
## 2    ACK     264
## 3    ALB     418
## 4    ANC       8
## 5    ATL   16837
## 6    AUS    2411
## 7    AVL     261
## 8    BDL     412
## 9    BGR     358
## 10   BHM     269
## # ... with 94 more rows
```


```r
not_cancelled %>% count(tailnum, wt = distance)
```

```
## # A tibble: 4,037 × 2
##    tailnum      n
##      <chr>  <dbl>
## 1   D942DN   3418
## 2   N0EGMQ 239143
## 3   N10156 109664
## 4   N102UW  25722
## 5   N103US  24619
## 6   N104UW  24616
## 7   N10575 139903
## 8   N105UW  23618
## 9   N107US  21677
## 10  N108UW  32070
## # ... with 4,027 more rows
```

```r
##same as above
not_cancelled %>%
  group_by(tailnum) %>%
  summarise(flights = sum(distance))
```

```
## # A tibble: 4,037 × 2
##    tailnum flights
##      <chr>   <dbl>
## 1   D942DN    3418
## 2   N0EGMQ  239143
## 3   N10156  109664
## 4   N102UW   25722
## 5   N103US   24619
## 6   N104UW   24616
## 7   N10575  139903
## 8   N105UW   23618
## 9   N107US   21677
## 10  N108UW   32070
## # ... with 4,027 more rows
```

3. Our definition of cancelled flights (is.na(dep_delay) | is.na(arr_delay) ) is slightly suboptimal. Why? Which is the most important column?
Using two variables is a little sloppy - The best way to show if the flight existed is air_time

4. Look at the number of cancelled flights per day. Is there a pattern? Is the proportion of cancelled flights related to the average delay?


```r
cancelled <- flights %>% 
  filter(is.na(dep_delay), is.na(arr_delay))

library(ggplot2)
cancelled %>%
  group_by(day) %>%
  summarise(avg_delay = mean(arr_delay, na.rm = T))
```

```
## # A tibble: 31 × 2
##      day avg_delay
##    <int>     <dbl>
## 1      1       NaN
## 2      2       NaN
## 3      3       NaN
## 4      4       NaN
## 5      5       NaN
## 6      6       NaN
## 7      7       NaN
## 8      8       NaN
## 9      9       NaN
## 10    10       NaN
## # ... with 21 more rows
```

```r
#ggplot(, mapping = aes(x = flight, y = avg_delay)) + 
#    geom_point(alpha = 1/10)
```

5. Which carrier has the worst delays? Challenge: can you disentangle the effects of bad airports vs. bad carriers? Why/why not? (Hint: think about flights %>% group_by(carrier, dest) %>% summarise(n()))


```r
not_cancelled %>%
  group_by(carrier) %>%
  summarise(mean_arr_delay = mean(arr_delay, na.rm = T)) %>%
  arrange(desc(mean_arr_delay))
```

```
## # A tibble: 16 × 2
##    carrier mean_arr_delay
##      <chr>          <dbl>
## 1       F9     21.9207048
## 2       FL     20.1159055
## 3       EV     15.7964311
## 4       YV     15.5569853
## 5       OO     11.9310345
## 6       MQ     10.7747334
## 7       WN      9.6491199
## 8       B6      9.4579733
## 9       9E      7.3796692
## 10      UA      3.5580111
## 11      US      2.1295951
## 12      VX      1.7644644
## 13      DL      1.6443409
## 14      AA      0.3642909
## 15      HA     -6.9152047
## 16      AS     -9.9308886
```


6. What does the sort argument to count() do. When might you use it?
?count()
count(x, ..., wt = NULL, sort = FALSE) 
Sorts the results ofthe count from highest to lowest

##5.7.1 Exercises

1. Refer back to the lists of useful mutate and filtering functions. Describe how each operation changes when you combine it with grouping.
These function within the groups

2. Which plane (tailnum) has the worst on-time record?

```r
not_cancelled %>%
  group_by(tailnum) %>%
  summarise(avg_arr_delay = mean(arr_delay), na.rm = T) %>%
  arrange(desc(avg_arr_delay))
```

```
## # A tibble: 4,037 × 3
##    tailnum avg_arr_delay na.rm
##      <chr>         <dbl> <lgl>
## 1   N844MH      320.0000  TRUE
## 2   N911DA      294.0000  TRUE
## 3   N922EV      276.0000  TRUE
## 4   N587NW      264.0000  TRUE
## 5   N851NW      219.0000  TRUE
## 6   N928DN      201.0000  TRUE
## 7   N7715E      188.0000  TRUE
## 8   N654UA      185.0000  TRUE
## 9   N665MQ      174.6667  TRUE
## 10  N427SW      157.0000  TRUE
## # ... with 4,027 more rows
```

3. What time of day should you fly if you want to avoid delays as much as possible? - May 7th

```r
not_cancelled %>%
  group_by(hour) %>%
  summarise(avg_arr_delay = mean(arr_delay), na.rm = T) %>%
  arrange((avg_arr_delay))
```

```
## # A tibble: 19 × 3
##     hour avg_arr_delay na.rm
##    <dbl>         <dbl> <lgl>
## 1      7    -5.3044716  TRUE
## 2      5    -4.7969072  TRUE
## 3      6    -3.3844854  TRUE
## 4      9    -1.4514074  TRUE
## 5      8    -1.1132266  TRUE
## 6     10     0.9539401  TRUE
## 7     11     1.4819300  TRUE
## 8     12     3.4890104  TRUE
## 9     13     6.5447397  TRUE
## 10    14     9.1976501  TRUE
## 11    23    11.7552783  TRUE
## 12    15    12.3241920  TRUE
## 13    16    12.5976412  TRUE
## 14    18    14.7887244  TRUE
## 15    22    15.9671618  TRUE
## 16    17    16.0402670  TRUE
## 17    19    16.6558736  TRUE
## 18    20    16.6761098  TRUE
## 19    21    18.3869371  TRUE
```

4. For each destination, compute the total minutes of delay. For each, flight, compute the proportion of the total delay for its destination.

```r
not_cancelled %>%
  group_by(dest) %>%
  summarize(total = sum(arr_delay))
```

```
## # A tibble: 104 × 2
##     dest  total
##    <chr>  <dbl>
## 1    ABQ   1113
## 2    ACK   1281
## 3    ALB   6018
## 4    ANC    -20
## 5    ATL 190260
## 6    AUS  14514
## 7    AVL   2089
## 8    BDL   2904
## 9    BGR   2874
## 10   BHM   4540
## # ... with 94 more rows
```

```r
##look at stacey's code for the second answer
```


5. Delays are typically temporally correlated: even once the problem that caused the initial delay has been resolved, later flights are delayed to allow earlier flights to leave. Using lag() explore how the delay of a flight is related to the delay of the immediately preceding flight.

```r
not_cancelled %>%
  group_by(origin, year, month, day) %>%
  mutate(prec_flight_delay = lag(dep_delay))
```

```
## Source: local data frame [327,346 x 20]
## Groups: origin, year, month, day [1,095]
## 
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
## # ... with 327,336 more rows, and 13 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>, prec_flight_delay <dbl>
```

```r
## look at min-yaos code
```



6. Look at each destination. Can you find flights that are suspiciously fast? (i.e. flights that represent a potential data entry error). Compute the air time a flight relative to the shortest flight to that destination. Which flights were most delayed in the air?


```r
#look at staceys answer
not_cancelled %>%
  group_by(dest) %>%
  mutate(fastest = min(air_time)) %>%
  ungroup() %>%
  arrange(air_time)
```

```
## # A tibble: 327,346 × 20
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
## # ... with 327,336 more rows, and 13 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>, fastest <dbl>
```


7. Find all destinations that are flown by at least two carriers. Use that information to rank the carriers.

```r
flights %>%
  group_by(dest) %>%
  count(carrier) %>%
  summarise(num_carrier = n ()) %>%
  filter(num_carrier >= 2)
```

```
## # A tibble: 76 × 2
##     dest num_carrier
##    <chr>       <int>
## 1    ATL           7
## 2    AUS           6
## 3    AVL           2
## 4    BDL           2
## 5    BGR           2
## 6    BNA           5
## 7    BOS           7
## 8    BQN           2
## 9    BTV           3
## 10   BUF           4
## # ... with 66 more rows
```


8. For each plane, count the number of flights before the first delay of greater than 1 hour.
not_cancelled %>%
  group_by(flight)
  summarize()
  Look at Michelle's answer for this
  
  
ctrl + shift + m = %>%

