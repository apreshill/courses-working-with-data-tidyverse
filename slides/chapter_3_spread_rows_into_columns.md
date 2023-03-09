---
title: Spread
key: bcd81c3ec4664b352d286e8e174e8f1c
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch3_4.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch3_4.mp4
transformations:
    translateX: 55
    translateY: 0
    scale: 1

---
## Spread

```yaml
type: TitleSlide
key: 9e64f7f323
```

`@lower_third`
name: Alison Hill
title: Professor & Data Scientist

`@script`

We've learned all about gathering, separating, and uniting columns. Now we'll learn how to do the opposite of gather, which is called spread.

---
## Gather

```yaml
type: FullSlide
key: 02a96d3f8e
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/a5f76caca56340c0f1ab773376a5d5879a426879/03.01_gather_overview.png)

`@script`

Recall that we used gather to reshape our data from wide to long. Gathering transforms multiple columns into exactly two columns: a key column contains the original column names, and a value column contains the original cell values. Spread does just the opposite...

---
## Spread

```yaml
type: FullSlide
key: d9b92aec25
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/0561d2bf37a68fac312b4eea126f80de0d5ce202/03.04_spread_overview.png)

`@script`

Spreading reshapes your data from long to wide, because it typically adds columns and shrinks the number of rows. You can think of gather as a tool for tidying messy columns, while spread is a tool for tidying messy rows. 


---
## Spread

```yaml
type: FullSlide
key: f928a71443
disable_transition: TRUE
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/b8a2fd29b5c0eaf3571964825ca1bd218dc8208e/03.04_spread_juniors.png)

`@script`

For example, here is some jumbled up juniors data that needs spreading. Look at the value column first. We have three different variables in this single column- some are numbers, some are character strings. If you look at the key column, you can decipher the value column better. What you see is that value is either the bakers' age, their outcome on the series, or their score on the spice guessing challenge. Spreading the value column by the key column creates three new columns- one for each distinct value in our current key column. These three new columns will contain the cells from the current value column.



---
## Spread: usage

```yaml
type: FullSlide
key: d1db7cbacb
```

`@part1`

```{r}
?spread
```

![](http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/03.03_spread_usage.png)

`@script`

...To use spread, we need 3 arguments: data, key, and value. There are other named arguments that have default settings, too. Let's look at how key and value are defined in the arguments.

---
## Spread: arguments

```yaml
type: FullSlide
key: 270276aa02
```

`@part1`

```{r}
?spread
```

![](http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/03.03_spread_args.png)

`@script`

The arguments tell us that key and value are column names or positions. This isn't really helpful on its own, so let's just use it.

---
## Using spread

```yaml
type: TwoColumns
key: af092ea311
```

`@part1`


```{r}
juniors_jumbled
```
```
# A tibble: 12 x 3
   baker  key     value   
   <chr>  <chr>   <chr>   
 1 Emma   age     11      
 2 Harry  age     10      
 3 Ruby   age     11      
 4 Zainab age     10      
 5 Emma   outcome finalist
 6 Harry  outcome winner  
 7 Ruby   outcome finalist
 8 Zainab outcome finalist
 9 Emma   spices  2       
10 Harry  spices  3       
11 Ruby   spices  2       
12 Zainab spices  0   
```

`@part2`

```{r}
juniors_jumbled %>% 
  spread(key = key, value = value)
```{{1}}
```
# A tibble: 4 x 4
  baker  age   outcome  spices
  <chr>  <chr> <chr>    <chr> 
1 Emma   11    finalist 2     
2 Harry  10    winner   3     
3 Ruby   11    finalist 2     
4 Zainab 10    finalist 0 
```{{2}}

`@script`

...We start with the data frame first, then use spread with two arguments. In this simple example, we called the key column key and the value column value. For any other column names, you would enter those on the right of the equal sign. Those variable names won't need to be in quotes, because they are already columns that exist in your dataset.

The side-by-side variables that get created are taken from the distinct values in the key column: age, outcome, and spices. So this looks pretty good, but notice that age and spices are still character variables, even though the values are clearly numeric.

---
## Spread and convert

```yaml
type: TwoColumns
key: e8c37ab23a
disable_transition: TRUE
```

`@part1`


```{r}
juniors_jumbled
```
```
# A tibble: 12 x 3
   baker  key     value   
   <chr>  <chr>   <chr>   
 1 Emma   age     11      
 2 Harry  age     10      
 3 Ruby   age     11      
 4 Zainab age     10      
 5 Emma   outcome finalist
 6 Harry  outcome winner  
 7 Ruby   outcome finalist
 8 Zainab outcome finalist
 9 Emma   spices  2       
10 Harry  spices  3       
11 Ruby   spices  2       
12 Zainab spices  0   
```

`@part2`

```{r}
juniors_jumbled %>% 
  spread(key = key, value = value, 
          convert = TRUE)
```{{1}}
```
# A tibble: 4 x 4
  baker    age outcome  spices
  <chr>  <int> <chr>     <int>
1 Emma      11 finalist      2
2 Harry     10 winner        3
3 Ruby      11 finalist      2
4 Zainab    10 finalist      0
```{{1}}

`@script`

...We can set the convert argument in spread to TRUE to automatically cast new variable types for us. Using the convert equals TRUE argument, age and spices are now both integers instead of characters.


---
## Spread review

```yaml
type: FullSlide
key: 8ab7fcd827
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/b8a2fd29b5c0eaf3571964825ca1bd218dc8208e/03.04_spread_juniors.png)

`@script`

To recap, use spread to untangle messy rows. Some good clues that you need to spread are when it is difficult to come up with a good column name, or when you see different variable types mixed together in the same column. You'll want to make sure that the values in your key column are tame first- remember that these become your new column names! You may want to use dplyr recode to tame those values first before spreading.


---
## Let's practice!

```yaml
type: FinalSlide
key: 9b08511b84
```

`@script`

Now it's your turn to test out the spread function!

