# June 7, 2017 R Club
Allie Gaudinier  
6/6/2017  

##11.2.2 Exercises


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

1. What function would you use to read a file where fields were separated with
“|”?
```
read_delim()
```

2. Apart from file, skip, and comment, what other arguments do read_csv() and read_tsv() have in common?
?read_csv()
?read_tsv()
```
col_names, col_types, locale, na, quoted_na, quote, trim_ws, n_max, guess_max, progress
```

3. What are the most important arguments to read_fwf()?
?read_fwf()

```
You can specify column positions in several ways:
 1. Guess based on position of empty columns
read_fwf(fwf_sample, fwf_empty(fwf_sample, col_names = c("first", "last", "state", "ssn")))
 2. A vector of field widths
read_fwf(fwf_sample, fwf_widths(c(20, 10, 12), c("name", "state", "ssn")))
 3. Paired vectors of start and end positions
read_fwf(fwf_sample, fwf_positions(c(1, 30), c(10, 42), c("name", "ssn")))
 4. Named arguments with start and end positions
read_fwf(fwf_sample, fwf_cols(name = c(1, 10), ssn = c(30, 42)))
 5. Named arguments with column widths
read_fwf(fwf_sample, fwf_cols(name = 20, state = 10, ssn = 12))
```

4. Sometimes strings in a CSV file contain commas. To prevent them from causing problems they need to be surrounded by a quoting character, like " or '. By convention, read_csv() assumes that the quoting character will be ", and if you want to change it you’ll need to use read_delim() instead. What arguments do you need to specify to read the following text into a data frame?

"x,y\n1,'a,b'"

```r
read_csv("x,y\n1,'a,b'", col_names = F)
```

```
## Warning: 1 parsing failure.
## row col  expected    actual         file
##   2  -- 2 columns 3 columns literal data
```

```
## # A tibble: 2 × 2
##      X1    X2
##   <chr> <chr>
## 1     x     y
## 2     1    'a
```


5. Identify what is wrong with each of the following inline CSV files. What happens when you run the code?

```
read_csv("a,b\n1,2,3\n4,5,6")
# 2 columns, 3 factors in the rows

read_csv("a,b,c\n1,2\n1,2,3,4")
# 3 columns, 2 factors in row 2, 4 in row 3


read_csv("a,b\n\"1")
# 2 columns, 1 factor in row 2

read_csv("a,b\n1,2\na,b")
#row 2 is the same as the column names

read_csv("a;b\n1;3")
#creates a 1x1 matrix
```

##11.3.5 Exercises

1. What are the most important arguments to locale()?
?locale()
```
locale(date_names = "en", date_format = "%AD", time_format = "%AT",
  decimal_mark = ".", grouping_mark = ",", tz = "UTC",
  encoding = "UTF-8", asciify = FALSE)
```

2. What happens if you try and set decimal_mark and grouping_mark to the same character? What happens to the default value of grouping_mark when you set decimal_mark to “,”? What happens to the default value of decimal_mark when you set the grouping_mark to “.”?

```r
##locale(decimal_mark = ".", grouping_mark = ".") error
locale(decimal_mark = ",")
```

```
## <locale>
## Numbers:  123.456,78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```

```r
locale(grouping_mark = ".")
```

```
## <locale>
## Numbers:  123.456,78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```



3. I didn’t discuss the date_format and time_format options to locale(). What do they do? Construct an example that shows when they might be useful.
```
date_format, time_format	
Default date and time formats.
```

```r
locale()
```

```
## <locale>
## Numbers:  123,456.78
## Formats:  %AD / %AT
## Timezone: UTC
## Encoding: UTF-8
## <date_names>
## Days:   Sunday (Sun), Monday (Mon), Tuesday (Tue), Wednesday (Wed),
##         Thursday (Thu), Friday (Fri), Saturday (Sat)
## Months: January (Jan), February (Feb), March (Mar), April (Apr), May
##         (May), June (Jun), July (Jul), August (Aug), September
##         (Sep), October (Oct), November (Nov), December (Dec)
## AM/PM:  AM/PM
```

```r
parse_date("1 janvier 2015", "%d %B %Y", locale = locale("fr"))
```

```
## [1] "2015-01-01"
```

```r
parse_date("mayo 05 2017", "%B %d %Y", locale = locale("es"))
```

```
## [1] "2017-05-05"
```

4. If you live outside the US, create a new locale object that encapsulates the settings for the types of file you read most commonly.
```
...
```

5. What’s the difference between read_csv() and read_csv2()?
```
The function read_csv uses a comma, while read_csv2 uses a semi-colon (;)
```

6. What are the most common encodings used in Europe? What are the most common encodings used in Asia? Do some googling to find out.
```
Latin based languages, Chinese, Japanese, Korean... 
```

Generate the correct format string to parse each of the following dates and times:


```r
#d1 <- "January 1, 2010"
d1 <- parse_date("January 01, 2010", "%B %d, %Y", locale = locale("en"))
d1
```

```
## [1] "2010-01-01"
```

```r
#d2 <- "2015-Mar-07"
d2 <- parse_date("2015-Mar-07", "%Y-%b-%d", locale = locale("en"))
d2
```

```
## [1] "2015-03-07"
```

```r
#d3 <- "06-Jun-2017"
d3 <- parse_date("06-Jun-2017", "%d-%b-%Y", locale = locale("en"))
d3
```

```
## [1] "2017-06-06"
```

```r
#d4 <- c("August 19 (2015)", "July 1 (2015)")
d4 <- parse_date(c("August 19 (2015)", "July 1 (2015)"), "%B %d (%Y)", locale = locale("en"))
d4
```

```
## [1] "2015-08-19" "2015-07-01"
```

```r
#d5 <- "12/30/14" # Dec 30, 2014
d5 <- parse_date("12/30/14", "%m/%d/%y")
d5
```

```
## [1] "2014-12-30"
```

```r
d6 <- parse_date("Dec 30, 2014", "%b %d, %Y")               
d6
```

```
## [1] "2014-12-30"
```

```r
#install.packages("hms")
library("hms")
#t1 <- "1705"
t1 <- parse_time("17:05")
t1
```

```
## 17:05:00
```

```r
#t2 <- "11:15:10.12 PM"
t2 <- parse_time("11:15:10.12 PM")
t2
```

```
## 23:15:10
```


