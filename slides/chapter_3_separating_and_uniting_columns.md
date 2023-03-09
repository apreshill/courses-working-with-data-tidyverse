---
title: Separate
key: 7aff726cf37892a9b8450308a33d8ffc
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch3_3.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch3_3.mp4
transformations:
    translateX: 55
    translateY: 0
    scale: 1

---
## Separate

```yaml
type: TitleSlide
key: 82194ad5db
```

`@lower_third`
name: Alison Hill
title: Professor & Data Scientist

`@script`

We just gathered our junior data, but it is not completely tidy just yet.

---
## Gathering the juniors data

```yaml
type: FullSlide
key: a5af99755d
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/71ce4091569af6f7b9e5c922e7b5c7a6483a0031/03.01_gather-01.png)

`@script`


Using the tidyr package, we gathered three columns of data from the spice smelling challenge by selecting every column except the baker column. In tidy data, each variable forms a column. But, our spice column contains two variables: the spice- cinnamon, cardamom, or nutmeg- and the order- 1, 2, or 3. For this data, the spice and the order are always the same- cinnamon was always first. But it didn't have to be that way! ...You can think of variables as either measurement or identification variables. Here, the spice and order are both identification variables- they help index each unique observational unit. In contrast, the column named correct is a measurement variable. Measurement variables provides meaningful data for each observational unit. To finish tidying this data, we need to separate "spice" from a single column into two.


---
## Separate: usage

```yaml
type: FullSlide
key: 0b8d2b564f
```

`@part1`

```{r}
?separate
```

![](http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/03.02_separate_usage.png)

`@script`

Separate requires at least 3 arguments. The first is the data. The second is "col", and the third is "into". There are other named arguments that have default settings, too. Let's look at the col and into arguments.

---
## Separate: arguments

```yaml
type: FullSlide
key: 206cc85289
```

`@part1`

```{r}
?separate
```

![](http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/03.02_separate_args.png)

`@script`

Col is the column name in your original data frame that you want to separate. Into will be the names of the new variables as a character vector. Since these two variables need to be created, their names will go in quotes. If there is more than one, the new variable names will need to be combined into a character vector using the c function.

---
## Separating what you have into what you want

```yaml
type: FullSlide
key: 581d4e5fc0
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/592251c2e45af728ff6795a8ce6da0664b043ac2/03.02_separate_sketch.png)

`@script`

The column we have that we want to separate is named "spice". The col argument takes a bare variable name, because it already exists in the data we have. On the other hand, the into argument creates new variables, so spice and order need to be in quotes since they do not exist in the data we have. The "c" function is used to combine these names into a character vector.


---
## Separate `spice`

```yaml
type: FullSlide
key: 17539a1cd9
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/db4f1b1f9e86b6f95cc882e4e1028a381a127d74/03.02_separate-01.png)

`@script`

With the juniors data, the gather created the new column named "spice". We then separate the spice column in the next step, using the pipe operator, into two columns: spice and order. By default, these two new columns replace the old spice column. 



---
## Reminder: pre-separate

```yaml
type: FullSlide
key: d009eecca9
```

`@part1`

```{r}
juniors_untidy %>% 
  gather(key = spice, value = correct, -baker)
# A tibble: 12 x 3
   baker  spice      correct
   <chr>  <chr>        <int>
 1 Emma   cinnamon_1       1
 2 Harry  cinnamon_1       1
 3 Ruby   cinnamon_1       1
 4 Zainab cinnamon_1       0
 5 Emma   cardamom_2       0
 6 Harry  cardamom_2       1
 7 Ruby   cardamom_2       0
 8 Zainab cardamom_2      NA
 9 Emma   nutmeg_3         1
10 Harry  nutmeg_3         1
11 Ruby   nutmeg_3         1
12 Zainab nutmeg_3         0
``` {{1}}

`@script`

As a reminder, here is what the output of gathering looked like, with 3 columns: baker, spice, and correct.

---
## Gather and separate

```yaml
type: FullSlide
key: ff8ea02ae1
```

`@part1`

```{r}
juniors_untidy %>% 
    gather(key = "spice", value = "correct", -baker) %>% 
    separate(spice, into = c("spice", "order"))

# A tibble: 12 x 4
   baker  spice    order correct
   <chr>  <chr>    <chr>   <int>
 1 Emma   cinnamon 1           1
 2 Harry  cinnamon 1           1
 3 Ruby   cinnamon 1           1
 4 Zainab cinnamon 1           0
 5 Emma   cardamom 2           0
 6 Harry  cardamom 2           1
 7 Ruby   cardamom 2           0
 8 Zainab cardamom 2          NA
 9 Emma   nutmeg   3           1
10 Harry  nutmeg   3           1
11 Ruby   nutmeg   3           1
12 Zainab nutmeg   3           0
```{{1}}

`@script`

Combining gather and separate gives us tidy data. 

But, notice that order is a character variable, when it is actually a number: the only values it can be are 1, 2, or 3.

---
## Gather, separate, and convert types

```yaml
type: FullSlide
key: 2c66a24151
disable_transition: TRUE
```

`@part1`

```{r}
juniors_untidy %>% 
    gather(key = "spice", value = "correct", -baker) %>% 
    separate(spice, into = c("spice", "order"), convert = TRUE)

# A tibble: 12 x 4
   baker  spice    order correct
   <chr>  <chr>    <int>   <int>
 1 Emma   cinnamon     1       1
 2 Harry  cinnamon     1       1
 3 Ruby   cinnamon     1       1
 4 Zainab cinnamon     1       0
 5 Emma   cardamom     2       0
 6 Harry  cardamom     2       1
 7 Ruby   cardamom     2       0
 8 Zainab cardamom     2      NA
 9 Emma   nutmeg       3       1
10 Harry  nutmeg       3       1
11 Ruby   nutmeg       3       1
12 Zainab nutmeg       3       0
```{{1}}

`@script`

The separate function has an argument named convert, which we can use to convert new columns to the right variable types. This is mainly useful if you have a separated column that is numeric.


---
## Before and after separate

```yaml
type: TwoColumns
key: a59148ece6
```

`@part1`

```{r}
# A tibble: 12 x 3
   baker  spice      correct
   <chr>  <chr>        <int>
 1 Emma   cinnamon_1       1
 2 Harry  cinnamon_1       1
 3 Ruby   cinnamon_1       1
 4 Zainab cinnamon_1       0
 5 Emma   cardamom_2       0
 6 Harry  cardamom_2       1
 7 Ruby   cardamom_2       0
 8 Zainab cardamom_2      NA
 9 Emma   nutmeg_3         1
10 Harry  nutmeg_3         1
11 Ruby   nutmeg_3         1
12 Zainab nutmeg_3         0
```


`@part2`

```{r}
# A tibble: 12 x 4
   baker  spice    order correct
   <chr>  <chr>    <int>   <int>
 1 Emma   cinnamon     1       1
 2 Harry  cinnamon     1       1
 3 Ruby   cinnamon     1       1
 4 Zainab cinnamon     1       0
 5 Emma   cardamom     2       0
 6 Harry  cardamom     2       1
 7 Ruby   cardamom     2       0
 8 Zainab cardamom     2      NA
 9 Emma   nutmeg       3       1
10 Harry  nutmeg       3       1
11 Ruby   nutmeg       3       1
12 Zainab nutmeg       3       0
```{{1}}

`@script`

Remember, tidying may be done with identification variables- 

like spice and order here- or measurement variables- like correct. It will always be easier to work with data when each column holds only one variable.

---
## The `sep` argument

```yaml
type: FullSlide
key: 21773c968d
```

`@part1`

```{r}
?separate
```

![](http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/03.02_separate_sep.png)

`@script`

One of the other arguments for separate is sep, which stands for the separator between columns. In our data, there was an underscore between the spice and the order number. Keeping the default value for the sep argument worked because spice and order were separated by a "sequence of non-alphanumeric values." Most of the time, separate works without changing the sep argument- it will separate wherever it finds one or more characters that are not letters or numbers. You can use regular expressions to change this if you ever need to.



---
## Let's practice!

```yaml
type: FinalSlide
key: 2797fe9266
```

`@script`

Now let's try some examples.

