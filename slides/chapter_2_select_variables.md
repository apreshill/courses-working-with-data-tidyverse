---
title: Select Variables
key: 2c90436063e1a843b65ccc7503599b4f
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch2_3.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch2_3.mp4
transformations:
    translateX: 55
    translateY: 0
    scale: 1

---
## Select Variables

```yaml
type: "TitleSlide"
key: "da2ff79b61"
```

`@lower_third`

name: Alison Hill
title: Professor & Data Scientist


`@script`
Tame data also contains just the variables that you need.

You'll use select from the dplyr package to subset your data, keeping only the variables or columns you want.


---
## Youngest bakers

```yaml
type: "FullSlide"
key: "1ae880eef5"
```

`@part1`
```{r}
young_bakers2
```{{1}}
```
# A tibble: 5 x 5
  baker  star_baker technical_winner series_winner series_runner_up
  <chr>       <dbl>            <dbl>         <dbl>            <dbl>
1 Martha         0.               2.            0.               0.
2 Flora          0.               1.            0.               0.
3 Jason          2.               1.            0.               0.
4 Ruby           3.               2.            0.               1.
5 John           1.               1.            1.               0.
```{{2}}


`@script`
We'll work with our youngest bakers again, except just five of them this time.

Here, we have data about the number of times each baker won the title of star baker, the number of times they came in first on the technical challenge, and whether they were the series winner or runner up.


---
## Select

```yaml
type: "FullSlide"
key: "adfc442f87"
```

`@part1`
```{r}
?select
```{{1}}

**Usage**{{2}}

```
select(.data, ...)
```{{3}}


`@script`
Select is a powerful function when you work with data that contains a lot of variables, but you only want to work with a subset. 

Here is how you use it. The first argument is the .data argument, then dot-dot-dot. Let's look at these arguments more closely.


---
## Select: arguments

```yaml
type: "FullSlide"
key: "eefda81088"
```

`@part1`
```{r}
?select
```{{1}}

![](http://s3.amazonaws.com/assets.datacamp.com/production/course_6012/datasets/02.01_select_args.png){{2}}


`@script`
The first argument is either a data frame or a tbl. This is a key element in working in dplyr and the tidyverse in general: the data frame is the first argument for most functions.

The ellipsis here means "one or more unquoted expressions separated by commas." This means the dots are "bare" variable names without quotes, so let's try it out with our bakers data.


---
## Select variables

```yaml
type: "FullSlide"
key: "a655277d6d"
```

`@part1`
```{r}
young_bakers2
```{{1}}
```
# A tibble: 5 x 5
  baker  star_baker technical_winner series_winner series_runner_up
  <chr>       <dbl>            <dbl>         <dbl>            <dbl>
1 Martha         0.               2.            0.               0.
2 Flora          0.               1.            0.               0.
3 Jason          2.               1.            0.               0.
4 Ruby           3.               2.            0.               1.
5 John           1.               1.            1.               0.
```{{1}}

```{r}
young_bakers2 %>% 
   select(baker, series_winner)
```{{2}}
```
# A tibble: 5 x 2
  baker  series_winner
  <chr>          <dbl>
1 Martha            0.
2 Flora             0.
3 Jason             0.
4 Ruby              0.
5 John              1.
```{{3}}


`@script`
The name of the data frame goes first. In the parentheses after select, we list the variable names to keep separated by commas. The ouptut here is a data frame with 2 columns and the same number of rows.


---
## Select a range of variables

```yaml
type: "FullSlide"
key: "121ad2f1e0"
disable_transition: true
```

`@part1`
```{r}
young_bakers2
```
```
# A tibble: 5 x 5
  baker  star_baker technical_winner series_winner series_runner_up
  <chr>       <dbl>            <dbl>         <dbl>            <dbl>
1 Martha         0.               2.            0.               0.
2 Flora          0.               1.            0.               0.
3 Jason          2.               1.            0.               0.
4 Ruby           3.               2.            0.               1.
5 John           1.               1.            1.               0.
```

```{r}
young_bakers2 %>% 
   select(baker:technical_winner)
```{{1}}
```
# A tibble: 5 x 3
  baker  star_baker technical_winner
  <chr>       <dbl>            <dbl>
1 Martha         0.               2.
2 Flora          0.               1.
3 Jason          2.               1.
4 Ruby           3.               2.
5 John           1.               1.
```{{2}}


`@script`
We can also select a range of consecutive variables using the colon operator. 

Here we select 3 variables: baker, technical underscore winner, and star underscore baker because it is in between baker and technical underscore winner.


---
## Drop variables

```yaml
type: "FullSlide"
key: "6bd03df074"
disable_transition: true
```

`@part1`
```{r}
young_bakers2
```
```
# A tibble: 5 x 5
  baker  star_baker technical_winner series_winner series_runner_up
  <chr>       <dbl>            <dbl>         <dbl>            <dbl>
1 Martha         0.               2.            0.               0.
2 Flora          0.               1.            0.               0.
3 Jason          2.               1.            0.               0.
4 Ruby           3.               2.            0.               1.
5 John           1.               1.            1.               0.
```

```{r}
young_bakers2 %>% 
   select(-technical_winner)
```{{1}}
```
# A tibble: 5 x 4
  baker  star_baker series_winner series_runner_up
  <chr>       <dbl>         <dbl>            <dbl>
1 Martha         0.            0.               0.
2 Flora          0.            0.               0.
3 Jason          2.            0.               0.
4 Ruby           3.            0.               1.
5 John           1.            1.               0.
```{{2}}


`@script`
Use the minus sign to drop variables. Positive values keep variables; negative values drop them. Here, we've dropped the technical winner column.


---
## Select helpers: starts_with()

```yaml
type: "FullSlide"
key: "ed9d3ba420"
disable_transition: true
```

`@part1`
```{r}
young_bakers2
```
```
# A tibble: 5 x 5
  baker  star_baker technical_winner series_winner series_runner_up
  <chr>       <dbl>            <dbl>         <dbl>            <dbl>
1 Martha         0.               2.            0.               0.
2 Flora          0.               1.            0.               0.
3 Jason          2.               1.            0.               0.
4 Ruby           3.               2.            0.               1.
5 John           1.               1.            1.               0.
```

```{r}
young_bakers2 %>% 
   select(baker, starts_with("series"))
```{{1}}
```
# A tibble: 5 x 3
  baker  series_winner series_runner_up
  <chr>          <dbl>            <dbl>
1 Martha            0.               0.
2 Flora             0.               0.
3 Jason             0.               0.
4 Ruby              0.               1.
5 John              1.               0.
```{{2}}


`@script`
The colon operator and the minus sign are two select helper functions. 

There are a number of helper functions you can use within select().

`starts_with()` helps you choose variables that start with a string like the word "series." Use commas between arguments, so here we select baker first, and then two variables that start with series.


---
## Select helper: ends_with()

```yaml
type: "FullSlide"
key: "df71802462"
disable_transition: true
```

`@part1`
```{r}
young_bakers2
```
```
# A tibble: 5 x 5
  baker  star_baker technical_winner series_winner series_runner_up
  <chr>       <dbl>            <dbl>         <dbl>            <dbl>
1 Martha         0.               2.            0.               0.
2 Flora          0.               1.            0.               0.
3 Jason          2.               1.            0.               0.
4 Ruby           3.               2.            0.               1.
5 John           1.               1.            1.               0.
```

```{r}
young_bakers2 %>% 
   select(ends_with("winner"), baker)
```{{1}}
```
# A tibble: 5 x 3
  technical_winner series_winner baker 
             <dbl>         <dbl> <chr> 
1               2.            0. Martha
2               1.            0. Flora 
3               1.            0. Jason 
4               2.            0. Ruby  
5               1.            1. John  
```{{2}}


`@script`
`ends_with()` helps you choose variables that end with a string like the word "winner." This code selects technical winner, series winner, and then the bakers' first names. 

Notice that in this example, I named the variables to keep in a different order than they appear in the original data frame. That is ok and it works! Select allows you to reorder your columns based on the order in which you select them. Here, the bakers' first names becomes the last column.


---
## Select helper: contains()

```yaml
type: "FullSlide"
key: "05b861c4f1"
disable_transition: true
```

`@part1`
```{r}
young_bakers2
```
```
# A tibble: 5 x 5
  baker  star_baker technical_winner series_winner series_runner_up
  <chr>       <dbl>            <dbl>         <dbl>            <dbl>
1 Martha         0.               2.            0.               0.
2 Flora          0.               1.            0.               0.
3 Jason          2.               1.            0.               0.
4 Ruby           3.               2.            0.               1.
5 John           1.               1.            1.               0.
```

```{r}
young_bakers2 %>% 
   select(contains("bake"))
```{{1}}
```
# A tibble: 5 x 2
  baker  star_baker
  <chr>       <dbl>
1 Martha         0.
2 Flora          0.
3 Jason          2.
4 Ruby           3.
5 John           1.
```{{2}}


`@script`
"contains" is even more flexible.

`contains()` helps you choose variables that contain a string anywhere in the variable name. We can use it to select variables that contain the string "bake" anywhere in the variable name, for example.


You'll learn about a few more helpful helper functions that you can use within select() in the exercises.


---
## Combine helper functions

```yaml
type: "FullSlide"
key: "28f6dd3b1a"
disable_transition: true
```

`@part1`
```{r}
young_bakers2
```
```
# A tibble: 5 x 5
  baker  star_baker technical_winner series_winner series_runner_up
  <chr>       <dbl>            <dbl>         <dbl>            <dbl>
1 Martha         0.               2.            0.               0.
2 Flora          0.               1.            0.               0.
3 Jason          2.               1.            0.               0.
4 Ruby           3.               2.            0.               1.
5 John           1.               1.            1.               0.
```

```{r}
young_bakers2 %>% 
   select(contains("bake"), starts_with("series"))
```{{1}}
```
# A tibble: 5 x 4
  baker  star_baker series_winner series_runner_up
  <chr>       <dbl>         <dbl>            <dbl>
1 Martha         0.            0.               0.
2 Flora          0.            0.               0.
3 Jason          2.            0.               0.
4 Ruby           3.            0.               1.
5 John           1.            1.               0.
```{{2}}


`@script`
You can also combine any number of arguments to select.


---
## Filter versus select

```yaml
type: "FullSlide"
key: "2382addc25"
```

`@part1`
```{r}
young_bakers2 %>% 
   filter(series_winner == 1 | series_runner_up == 1)
```{{1}}
```
# A tibble: 2 x 5
  baker star_baker technical_winner series_winner series_runner_up
  <chr>      <dbl>            <dbl>         <dbl>            <dbl>
1 Ruby          3.               2.            0.               1.
2 John          1.               1.            1.               0.
```{{2}}
```{r}
young_bakers2 %>% 
   select(baker, starts_with("series"))
```{{3}}
```
# A tibble: 5 x 3
  baker  series_winner series_runner_up
  <chr>          <dbl>            <dbl>
1 Martha            0.               0.
2 Flora             0.               0.
3 Jason             0.               0.
4 Ruby              0.               1.
5 John              1.               0.
```{{4}}


`@script`
Remember that filter subset your rows, and never changes your columns. Here I have used the OR operator, the vertical line, so this filters for rows where either the baker is the series winner or the series runner up.

Select subsets your columns, and never changes your rows.

In all our examples using select, we always kept all 5 rows in place.


---
## Let's practice!

```yaml
type: "FinalSlide"
key: "ac7d47ebca"
```

`@script`
Now let's try some examples.

