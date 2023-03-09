---
title: Dates
key: 6cd0e5ddc76a5b1455418eeef2a225b0
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch4_3.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch4_3.mp4
transformations:
    translateX: 55
    translateY: 0
    scale: 1

---
## Dates

```yaml
type: TitleSlide
key: 43edacae50
```

`@lower_third`
name: Alison Hill
title: Professor & Data Scientist

`@script`

Dates show up in all kinds of datasets- whether you work with data on TV show ratings, clinical patients, storms, sports, stock prices, or politics- chances are you will have to work with date variables.

We'll focus on two common tasks: 
- casting characters as dates, and 
- calculating the difference between two dates.

---
## The lubridate package

```yaml
type: FullSlide
key: 15a4d876dc
```

`@part1`

```
library(lubridate) # once per work session
```

![](https://assets.datacamp.com/production/repositories/1613/datasets/9141815d10c187b9570de666c51d0b3d61860ff2/lubridate.png)

`@citations`

- http://lubridate.tidyverse.org 

`@script`

The tidyverse package lubridate makes it easier to work with dates in R. 


---
## Cast character as a date

```yaml
type: FullSlide
key: 778fe7ffee
```

`@part1`


```{r}
?ymd
```

Usage{{1}}

```
ymd(..., quiet = FALSE, tz = NULL, locale = Sys.getlocale("LC_TIME"),
  truncated = 0)

ydm(..., quiet = FALSE, tz = NULL, locale = Sys.getlocale("LC_TIME"),
  truncated = 0)

mdy(..., quiet = FALSE, tz = NULL, locale = Sys.getlocale("LC_TIME"),
  truncated = 0)

myd(..., quiet = FALSE, tz = NULL, locale = Sys.getlocale("LC_TIME"),
  truncated = 0)

dmy(..., quiet = FALSE, tz = NULL, locale = Sys.getlocale("LC_TIME"),
  truncated = 0)

dym(..., quiet = FALSE, tz = NULL, locale = Sys.getlocale("LC_TIME"),
  truncated = 0)
```{{1}}

`@script`

Lubridate has a family of functions used to parse and cast dates with year, month, and day components.

Which function you use depends on how your original date is formatted. After parsing, all dates are in the same standard numeric format Year dash Month dash Day.



---
## ymd: Arguments

```yaml
type: FullSlide
key: 843aef7252
```

`@part1`

```{r}
?ymd
```

![](https://assets.datacamp.com/production/repositories/1613/datasets/60fcce4c271a0f3a86f582a3debf5623af8693ec/04.01_ymd_args.png)

Examples{{1}}

```
ymd("2010-08-17")
mdy(c("08/17/2010", "January 01, 2018"))
dmy("17 08 2010")
```{{2}}

`@script`

The sole argument for this family of functions is the ellipsis, which is a placeholder for a vector of "suspected dates". What does a vector of suspected dates look like?

A suspected date should be a character in quotes. The month can be spelled out, or there can be dashes, slashes, commas, or spaces in between elements.

---
## Parse Dates

```yaml
type: FullSlide
key: 50d0a14853
```

`@part1`

```{r}
dmy("17 August 2010") # does this work?
[1] "2010-08-17"
```{{1}}
```{r}
mdy("17 August 2010") # what about this?
[1] NA
Warning message:
All formats failed to parse. No formats found. 
```{{2}}
```{r}
ymd("17 August 2010") # what about this?
[1] NA
Warning message:
All formats failed to parse. No formats found. 
```{{3}}

`@script`

Let's take the date 17 August 2010 as an example.

We would use d-m-y to parse. Other functions will fail to parse, and will return an error message. So order matters. And in fact, it is the only thing that matters. 

This parsing method is a bit different from how we parsed dates using the readr package when we imported the data, where we specified how the date elements were formatted, like if the month was the number 8 or if it was spelled out like August. In this way, lubridate is a more flexible way to cast dates.

Let's start working with dates in a data frame.

---
## Dates in a data frame

```yaml
type: FullSlide
key: eaa7bfe62a
```

`@part1`

```{r}
hosts <- tibble::tribble(
  ~host, ~bday, ~premiere, 
  "Mary", "24 March 1935", "August 17th, 2010", 
  "Paul", "1 March 1966", "August 17th, 2010")
```
```{r}
hosts

# A tibble: 2 x 3
  host  bday          premiere            
  <chr> <chr>         <chr>            
1 Mary  24 March 1935 August 17th, 2010
2 Paul  1 March 1966  August 17th, 2010
```{{1}}

`@script`

As an example, I'll make a dataset using the tibble package. The variable bday contains the birthdates of Mary Berry and Paul Hollywood, the original hosts of the Great British Bake Off. This variable is day-month-year. The variable premiere is the date of the first episode of the show. It is month-day-year.

Both of these columns are characters right now, let's cast them as dates.


---
## Cast as dates

```yaml
type: FullSlide
key: 63a3f63823
```

`@part1`

```{r}
hosts
```
```
# A tibble: 2 x 3
  host  bday          premiere           
  <chr> <chr>         <chr>            
1 Mary  24 March 1935 August 17th, 2010
2 Paul  1 March 1966  August 17th, 2010
```
```{r}
hosts <- hosts %>% 
  mutate(bday = dmy(bday),
         premiere = mdy(premiere))
```{{1}}
```{r}
hosts
```{{1}}
```
# A tibble: 2 x 3
  host  bday       premiere     
  <chr> <date>     <date>    
1 Mary  1935-03-24 2010-08-17
2 Paul  1966-03-01 2010-08-17
```{{1}}

`@script`

We use dmy to parse the bday column, and mdy to parse the premiere column. 

Now we have two date variables we can work with!

---
## Types of timespans

```yaml
type: FullSlide
key: 69991e8bb4
```

`@part1`

- `interval`: time spans bound by two real date-times.{{1}}

- `duration`: the exact number of seconds in an interval. 
{{2}}

- `period`: the change in the clock time in an interval.{{3}} 

`@citations`

- Lubridate Reference Manual (http://lubridate.tidyverse.org/reference/timespan.html)


`@script`

Once you have cast your dates, a common task is to calculate differences between two dates. Lubridate offers 3 ways to classify timespans:

- intervals are time spans bound by two real date-times. 
- a duration records the exact number of seconds in an interval. They measure the passage of time, but do not always align with intuition.

- a period records the change in the clock time between two date-times.

We'll focus on periods as they are measured in human units like years, months, days, hours, minutes, and seconds.

---
## Calculating an interval

```yaml
type: FullSlide
key: 865e8578b3
```

`@part1`

```{r}
hosts <- hosts %>% 
  mutate(age_int = interval(bday, premiere))
```{{1}}
```{r}
hosts

# A tibble: 2 x 4
  host  bday       premiere   age_int                       
  <chr> <date>     <date>     <S4: Interval>                
1 Mary  1935-03-24 2010-08-17 1935-03-24 UTC--2010-08-17 UTC
2 Paul  1966-03-01 2010-08-17 1966-03-01 UTC--2010-08-17 UTC
```{{2}}



`@script`


To start, let's create a time interval called age underscore int using the interval function, where the start date is first and end date is second. 

Now, the output of interval is not very helpful. Once you have an interval, you'll often want to convert the values into understandable units you can work with.


---
## Converting units of timespans

```yaml
type: FullSlide
key: 90f1bdbc0c
```

`@part1`

```{r}
years(1)
[1] "1y 0m 0d 0H 0M 0S"
```{{1}}
```{r}
hosts %>% 
  mutate(years_decimal = age_int / years(1),
         years_whole = age_int %/% years(1)) 
# A tibble: 2 x 4
  host  age_int                        years_decimal years_whole
  <chr> <S4: Interval>                         <dbl>       <dbl>
1 Mary  1935-03-24 UTC--2010-08-17 UTC          75.4         75.
2 Paul  1966-03-01 UTC--2010-08-17 UTC          44.5         44.
```{{2}}

`@script`


There are two strategies for converting timespan units. You can use division, with a duration as the divisor. 

You can also use modulo division, which always returns a whole number, for when decimals don't make sense.

---
## Converting units of timespans

```yaml
type: FullSlide
key: c807f52b3b
```

`@part1`

```{r}
hosts %>% 
  mutate(age_y = age_int %/% years(1),
         age_m = age_int %/% months(12))

# A tibble: 2 x 6
  host  bday       premiere   age_int                        age_y age_m
  <chr> <date>     <date>     <S4: Interval>                 <dbl> <dbl>
1 Mary  1935-03-24 2010-08-17 1935-03-24 UTC--2010-08-17 UTC   75.   75.
2 Paul  1966-03-01 2010-08-17 1966-03-01 UTC--2010-08-17 UTC   44.   44.
```{{1}}

`@script`


Lubridate also has functions for months, weeks, days, hours, and seconds. The argument in parentheses is the number of time units. Here we calculate each judge's age in years, and we get the same age in 12 month time units.

---
## Let's practice!

```yaml
type: FinalSlide
key: 75dd30672a
```

`@script`

Time to put this into practice.

