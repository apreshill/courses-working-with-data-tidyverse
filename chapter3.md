---
title: 'Tidy your data'
description: 'Now that your data has been tamed, it is time to get tidy. In this chapter, you will get hands-on experience tidying data and combining multiple tidying functions together in a chain using the pipe operator. '
attachments:
    slides_link: 'https://s3.amazonaws.com/assets.datacamp.com/production/course_6012/slides/chapter3.pdf'
---

## Introduction to Tidy Data

```yaml
type: VideoExercise
key: de453e4f0f
lang: r
xp: 50
skills: 1
```

`@projector_key`
e92f30b47653c749db6e33cac4d23e1e

---

## Tidy line-up

```yaml
type: MultipleChoiceExercise
key: 8753fbb6fb
lang: r
xp: 50
skills: 1
```

Four data frames are loaded for you. They each contain some data on the number of viewers who watched episodes of the "The Great British Bake-off". Determine which one is tidy.

`@possible_answers`
- `ratings1`
- `ratings2`
- `ratings3`
- `ratings4`

`@hint`
Remember the rules of tidy data: (1) each variable forms a column; (2) each observation forms a row.

`@pre_exercise_code`
```{r}
library(tibble)
ratings4 <- tibble::tribble(
  ~series, ~episode, ~viewers_7day, ~viewers_28day, ~uk_premiere,
        6,        1,         11.62,          11.73, "2015-08-05",
        6,        2,         11.59,          11.84, "2015-08-12",
        6,        3,         12.01,             NA, "2015-08-19",
        6,        4,         12.36,          12.67, "2015-08-26",
        6,        5,         12.39,          12.63, "2015-09-02",
        6,        6,            12,          12.26, "2015-09-09"
  )

ratings3 <- tibble::tribble(
  ~series, ~episode, ~uk_premiere, ~window, ~viewers,
        6,        1, "2015-08-05",  "7day",    11.62,
        6,        1, "2015-08-05", "28day",    11.73,
        6,        2, "2015-08-12",  "7day",    11.59,
        6,        2, "2015-08-12", "28day",    11.84,
        6,        3, "2015-08-19",  "7day",    12.01,
        6,        3, "2015-08-19", "28day",       NA
  )
  
ratings2 <- tibble::tribble(
  ~series, ~e1_viewers_7day, ~e1_viewers_28day, ~e2_viewers_7day, ~e2_viewers_28day,
        1,             2.24,                NA,                3,                NA,
        2,              3.1,                NA,             3.53,                NA,
        3,             3.85,                NA,              4.6,                NA,
        4,              6.6,                NA,             6.65,                NA,
        5,             8.51,                NA,             8.79,                NA,
        6,            11.62,             11.73,            11.59,             11.84,
        7,            13.58,             13.86,            13.45,             13.74,
        8,             9.46,              9.72,             9.23,              9.53
  )
  
ratings1 <- tibble::tribble(
  ~series, ~e1_viewers_7day, ~e2_viewers_7day, ~e3_viewers_7day, ~e4_viewers_7day,
        1,             2.24,                3,                3,              2.6,
        2,              3.1,             3.53,             3.82,              3.6,
        3,             3.85,              4.6,             4.53,             4.71,
        4,              6.6,             6.65,             7.17,             6.82,
        5,             8.51,             8.79,             9.28,            10.25,
        6,            11.62,            11.59,            12.01,            12.36,
        7,            13.58,            13.45,            13.01,            13.29,
        8,             9.46,             9.23,             8.68,             8.55
  )
```

`@sct`
```{r}
msg1 = "Incorrect- this one has a variable, episode number, in the variable names."
msg2 = "Incorrect- this one has two variables, episode number and time window, in the variable names."
msg3 = "Correct! This is the only tidy data in our line-up!"
msg4 = "Incorrect! This one still has a variable, time window in days, in two variable names."

ex() %>% check_mc(3, feedback_msgs = c(msg1, msg2, msg3, msg4))
```

---

## Plot untidy data

```yaml
type: TabExercise
key: e219632ab5
xp: 100
```

We'll start by visualizing the number of show viewers (in millions, cumulative across seven days) across series by episode using the `ratings` data.

`@pre_exercise_code`
```{r}
download.file("http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/messy_ratings.csv", 
              destfile = "ratings.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(ggplot2)
ratings <- read_csv("ratings.csv", col_types = cols(series = col_factor(levels = NULL)))
```

***

```yaml
type: NormalExercise
key: 406426b767
xp: 50
```

`@instructions`
- Use `ggplot2` to make a bar chart that shows the number of viewers for the first episode by series.

`@hint`
- The variable `series` should be mapped onto the x-axis, and `e1` should be mapped on the y-axis, using `geom_col`.

`@sample_code`
```{r}
# Plot of episode 1 viewers by series
```

`@solution`
```{r}
# Plot of episode 1 viewers by series
ggplot(ratings, aes(x = series, y = e1)) +
    geom_col()
```

`@sct`
```{r}
ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal(incorrect_msg = "You should be calling `ggplot()` on ratings.", append = FALSE)
    check_function(., "aes") %>% {
        check_arg(., "x") %>% check_equal(eval=FALSE)
        check_arg(., "y") %>% check_equal(eval=FALSE)
        }
    check_function(., "geom_col")
    }
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 4fa02dd28a
xp: 50
```

`@instructions`
- Adapt your code to make another bar plot just like the first, but this time plot the viewers for the second episode by series.

`@hint`
- The number of viewers for episode two is stored in the `e2` variable.

`@sample_code`
```{r}
# Adapt code to plot episode 2 viewers by series
ggplot(ratings, aes(x = series, y = e1)) +
    geom_col()
```

`@solution`
```{r}
# Adapt code to plot episode 2 viewers by series
ggplot(ratings, aes(x = series, y = e2)) +
    geom_col()
```

`@sct`
```{r}
ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal(incorrect_msg = "You should be calling `ggplot()` on ratings.", append = FALSE)
    check_function(., "aes") %>% {
        check_arg(., "x") %>% check_equal(eval=FALSE)
        check_arg(., "y") %>% check_equal(eval=FALSE)
        }
    check_function(., "geom_col")
    }

ex() %>% check_error()

success_msg("Series 8 was the first with different hosts on a different channel. Do you notice a pattern? To draw any conclusions, we'd want to plot the viewers for every episode though- aren't you glad you weren't asked to make all 10 plots? Wouldn't it be nice if episode was a variable we could use?")
```

---

## Gather

```yaml
type: VideoExercise
key: d69bb57818
lang: r
xp: 50
skills: 1
```

`@projector_key`
e74bbfbd8364c8f60e5109620419c429

---

## Gather by hand

```yaml
type: MultipleChoiceExercise
key: fc5e23d39a
lang: r
xp: 50
skills: 1
```

Type `mini` in the console to see a small un-tidy data frame. On a sheet of paper, sketch out the tidy version of this data. In the tidy version, what is in the "value" column in the sixth row (not counting the header row)?

`@possible_answers`
- IN.
- OUT.
- NA.

`@hint`
To make the `mini` data tidy, you would `gather()` all columns that start with the word "episode".

`@pre_exercise_code`
```{r}
library(tibble)
library(tidyr)
library(dplyr)
mini <- tribble(
    ~baker, ~episode_1, ~episode_2, ~episode_3,
  "Annetha", "IN",   "OUT", NA,
  "David", "IN",   "IN", "IN",
  "Edd", "IN",   "IN", "IN",
  "Jasminder", "IN",   "IN", "IN"
) 
tidy_mini <- mini %>% 
    gather(key = episode, value = result, starts_with("episode"))
```

`@sct`
```{r}
msg1 = "Correct! David was IN in episode 2."
msg2 = "Incorrect. Annetha was OUT in episode 2, but that is in the fifth row of the tidy table."
msg3 = "Incorrect. Annetha was NA in episode 3, but that is in the ninth row of the tidy table."

ex() %>% check_mc(1, feedback_msgs = c(msg1, msg2, msg3))
```

---

## Gather & plot

```yaml
type: TabExercise
key: 669c0b0bf7
xp: 100
```

In a previous frustrating exercise working with untidy data, we attempted to visualize viewers per episode across series with the untidy `ratings` data.

![](https://assets.datacamp.com/production/repositories/1613/datasets/0e686a52da94bfa5373dbe9688240d7b89e8c412/ratings_long_bars.png)

Here is the plot we wanted to create. To make it, we'll tidy the data, then use [`row_number()`](https://www.rdocumentation.org/packages/dplyr/versions/0.7.5/topics/ranking) from the `dplyr` package to create a continuous episode count variable, such that episode 1 from series 1 will have a value of `1`, episode 1 from series 2 will have a value of `7`, and so on. This new variable will be mapped onto the x-axis using `ggplot2`.

`@pre_exercise_code`
```{r}
download.file("http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/messy_ratings.csv", 
              destfile = "ratings.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(ggplot2)
ratings <- read_csv("ratings.csv", col_types = cols(series = col_factor(levels = NULL)))
```

`@sample_code`
```{r}

```

***

```yaml
type: NormalExercise
key: 91f8b9bf22
xp: 25
```

`@instructions`
- Gather all columns but `series`. Name the key `episode` and the value `viewers_7day`. For plotting, store the key values as a factor variable to preserve the original ordering of the episodes, and remove rows that are `NA`.

`@hint`
- To exclude columns, use the `-` operator before the bare variable name: `-variable_name`. Use `?gather` to figure out which argument to use to convert the key column to a factor.

`@sample_code`
```{r}
tidy_ratings <- ___ %>%
    # Gather and convert episode to factor
	gather(key = ___, value = ___, ___, 
           ___ = TRUE, na.rm = ___)
```

`@solution`
```{r}
tidy_ratings <- ratings %>%
    # Gather and convert episode to factor
	gather(key = "episode", value = "viewers_7day", -series, 
           factor_key = TRUE, na.rm = TRUE)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "tidy_ratings") %>% check_equal(),
	check_function(., "gather") %>% {
    	check_arg(., "data") %>% check_equal(incorrect_msg = "You should be piping ratings into `gather()`.", append = FALSE)
    	check_arg(., "key") %>% check_equal()
    	check_arg(., "value") %>% check_equal()
    	check_arg(., "factor_key") %>% check_equal()
    	check_arg(., "na.rm") %>% check_equal()
    	check_result(.) %>% check_equal(incorrect_msg = "Remember to exclude the `series` variable.", append = FALSE)
    }
)    

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: a40d4f839e
xp: 25
```

`@instructions`
- Next, sort the rows by `series` then `episode`.

`@hint`
- You'll want to use `arrange()` to sort in ascending order.

`@sample_code`
```{r}
tidy_ratings <- ratings %>%
	# Gather and convert episode to factor
    gather(key = "episode", value = "viewers_7day", -series, 
           factor_key = TRUE, na.rm = TRUE) %>% 
	# Sort in ascending order by series and episode
    ___(___, ___)
```

`@solution`
```{r}
tidy_ratings <- ratings %>%
	# Gather and convert episode to factor
    gather(key = "episode", value = "viewers_7day", -series, 
           factor_key = TRUE, na.rm = TRUE) %>% 
	# Sort in ascending order by series and episode
    arrange(series, episode)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "tidy_ratings") %>% check_equal(eq_condition = "identical"),
	{
    check_function(., "gather") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "You should be piping ratings into `gather()`.", append = FALSE)
        check_arg(., "key") %>% check_equal()
        check_arg(., "value") %>% check_equal()
        check_arg(., "factor_key") %>% check_equal()
        check_arg(., "na.rm") %>% check_equal()
        check_result(.) %>% check_equal(incorrect_msg = "Remember to exclude the `series` variable.", append = FALSE)
        }
    check_function(., "arrange") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "You should be piping everything from `gather()` directly into `arrange()`.", append = FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_msg = "The order of the two arguments inside `arrange()` matters.", append = FALSE)
        }
    }
)    

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: b5e672e556
xp: 25
```

`@instructions`
- Add a line after the `%>%` to create a new cumulative `episode_count` variable using [`row_number()`](https://www.rdocumentation.org/packages/dplyr/versions/0.7.5/topics/ranking) from the `dplyr` package.

`@hint`
- You'll need to use this function within a `mutate()`; use `row_number()` without any arguments as the argument value.

`@sample_code`
```{r}
tidy_ratings <- ratings %>%
	# Gather and convert episode to factor
    gather(key = "episode", value = "viewers_7day", -series, 
           factor_key = TRUE, na.rm = TRUE) %>% 
	# Sort in ascending order by series and episode
    arrange(series, episode) %>% 
	# Create new variable using row_number()
    ___(___ = ___)
```

`@solution`
```{r}
tidy_ratings <- ratings %>%
	# Gather and convert episode to factor
    gather(key = "episode", value = "viewers_7day", -series, 
           factor_key = TRUE, na.rm = TRUE) %>% 
	# Sort in ascending order by series and episode
    arrange(series, episode) %>%
	# Create new variable using row_number()
    mutate(episode_count = row_number())
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "tidy_ratings") %>% check_equal(eq_condition = "identical"),
	{
    check_function(., "gather") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "You should be piping ratings into `gather()`.", append = FALSE)
        check_arg(., "key") %>% check_equal()
        check_arg(., "value") %>% check_equal()
        check_arg(., "factor_key") %>% check_equal()
        check_arg(., "na.rm") %>% check_equal()
        check_result(.) %>% check_equal(incorrect_msg = "Remember to exclude the `series` variable.", append = FALSE)
        }
    check_function(., "arrange") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "You should be piping everything from `gather()` directly into `arrange()`.", append = FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_msg = "The order of the two arguments inside `arrange()` matters.", append = FALSE)
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "You should be piping everything from `gather()` directly into `arrange()`.", append = FALSE)
        check_arg(., "episode_count") %>% check_equal(incorrect_msg = "Set `episode_count` equal to the function `row_number()`.", append = FALSE, eval = FALSE)
        check_result(.) %>% check_equal(eq_condition = "equal")
        }
    }
)    

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 65429b80a0
xp: 25
```

`@instructions`
- Make a bar chart to visualize 7-day viewers by `episode_count`, filled by `series`.

`@hint`
- You'll need to use `geom_col()`, and map the `fill` aesthetic to the `series` variable.

`@sample_code`
```{r}
tidy_ratings <- ratings %>%
    # Gather and convert episode to factor
	gather(key = "episode", value = "viewers_7day", -series, 
           factor_key = TRUE, na.rm = TRUE) %>%
	# Sort in ascending order by series and episode
    arrange(series, episode) %>% 
	# Create new variable using row_number()
    mutate(episode_count = row_number())

# Plot viewers by episode and series
ggplot(___, aes(x = ___, 
                y = ___, 
                fill = ___)) +
    ___
```

`@solution`
```{r}
tidy_ratings <- ratings %>%
    # Gather and convert episode to factor
	gather(key = "episode", value = "viewers_7day", -series, 
           factor_key = TRUE, na.rm = TRUE) %>% 
	# Sort in ascending order by series and episode
    arrange(series, episode) %>% 
	# Create new variable using row_number()
    mutate(episode_count = row_number())

# Plot viewers by episode and series
ggplot(tidy_ratings, aes(x = episode_count, 
                         y = viewers_7day, 
                         fill = series)) +
    geom_col()
```

`@sct`
```{r}
ex() %>% check_object("tidy_ratings") %>% check_equal(eq_condition = "identical", incorrect_msg = "Do not modify `tidy_ratings`.", append = FALSE)

ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal(incorrect_msg = "You should be calling `ggplot()` on tidy_ratings.", append = FALSE)
    check_function(., "aes") %>% {
        check_arg(., "x") %>% check_equal(eval=FALSE)
        check_arg(., "y") %>% check_equal(eval=FALSE)
        check_arg(., "fill") %>% check_equal(eval=FALSE)
        }
    check_function(., "geom_col")
    }

success_msg("Beautiful plot! In series 8, one of the judges accidentally tweeted out the winner's name 12 hours before the finale aired. Do you think that hurt their finale viewer numbers?")
```

---

## Gather & plot non-sequential columns

```yaml
type: TabExercise
key: e9ae0c29bf
xp: 100
```

In this exercise, you'll combine `tidyr` with [`dplyr::select()`](https://www.rdocumentation.org/packages/dplyr/versions/0.7.5/topics/select) to keep and gather non-sequential columns in the `ratings2` data. You can review the `select` [helper functions here](https://www.rdocumentation.org/packages/dplyr/versions/0.7.5/topics/select). The `ratings2` data includes per-episode data:

- `e*_7day`: viewers within a 7-day window, and
- `e*_28day`: viewers within a 28-day window. 

Here, we'll focus on the 7-day viewers.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/263105bb1109ab2a8cf8e92d6fbf3eecd66140b8/messy_ratings2.csv", 
              destfile = "ratings2.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(ggplot2)
ratings2 <- read_csv("ratings2.csv", col_types = cols(series = col_factor(levels = NULL)))
```

***

```yaml
type: NormalExercise
key: d745b4cfcd
xp: 30
```

`@instructions`
- Create a new data frame called `week_ratings`, selecting the variables you will need from `ratings2` to plot only the 7-day viewers by episode and series.

`@hint`
- You need to use `dplyr::select()` first with the `ends_with()` helper function, and don't forget to select `series`!

`@sample_code`
```{r}
# Select 7-day viewer ratings


```

`@solution`
```{r}
# Select 7-day viewer ratings
week_ratings <- ratings2 %>% 
    select(series, ends_with("7day"))
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "week_ratings") %>% check_equal(eq_condition = "identical"),
	{
    check_function(., "ends_with") %>% check_arg(., 'match') %>% check_equal(incorrect_msg = "Did you specify for only variables that end with `\"7day?\"", append = FALSE)
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Remember to pipe `ratings2` into `select()`.", append = FALSE)
        check_result(.) %>% check_equal(eq_condition = "equal", incorrect_message = "Check to make sure you are only including the necessary arguments inside `select()`.", append = FALSE)
        }
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: bb93df0f77
xp: 30
```

`@instructions`
- Add a line after the `%>%` to gather the 7-day viewer data into one column called `viewers_7day`, removing all rows with missing values and converting the key variable, `episode`, to a factor.

`@hint`
- You can use the `ends_with()` helper function within the `gather()` function. Use `?gather` to find the arguments for removing rows with `NA` values and casting the key variable as a factor.

`@sample_code`
```{r}
week_ratings <- ratings2 %>% 
	# Select 7-day viewer ratings
    select(series, ends_with("7day")) %>%
	# Gather 7-day viewers by episode
	___
```

`@solution`
```{r}
week_ratings <- ratings2 %>% 
	# Select 7-day viewer ratings
    select(series, ends_with("7day")) %>% 
	# Gather 7-day viewers by episode
    gather(episode, viewers_7day, ends_with("7day"), na.rm = TRUE, factor_key = TRUE)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "week_ratings") %>% check_equal(eq_condition = "identical", incorrect_msg = "Make sure you spelled `week_ratings` correctly.", append = FALSE),
	{
    check_function(., "ends_with") %>% check_arg(., 'match') %>% check_equal(incorrect_msg = "Did you specify for only variables that end with `\"7day?\"`", append = FALSE)
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Remember to pipe `ratings2` into `select()`.", append = FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_message = "The order of the two arguments matter.", append = FALSE)
        }
    check_function(., "gather") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Make sure you're piping everything from `select()` into `gather()`.", append = FALSE)
        check_arg(., "key") %>% check_equal(eval=FALSE)
        check_arg(., "value") %>% check_equal(eval=FALSE)
        check_arg(., "na.rm") %>% check_equal(eval=FALSE)
        check_arg(., "factor_key") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical" ,incorrect_msg = "Remember to gather by 7-day viewers.", append = FALSE)
        }
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 9ac90a9c4b
xp: 40
```

`@instructions`
- Make a line plot to visualize the 7-day viewers by `episode`, facetted by `series`. Since the x-axis is a factor, you'll use the group aesthetic to plot one line per `series`.

`@hint`
- You'll want to use `geom_line()`, and map the `group` aesthetic to the `series` variable.

`@sample_code`
```{r}
week_ratings <- ratings2  %>% 
	# Select 7-day viewer ratings
    select(series, ends_with("7day")) %>% 
	# Gather 7-day viewers by episode
    gather(episode, viewers_7day, ends_with("7day"), na.rm = TRUE, factor_key = TRUE)
    
# Plot 7-day viewers by episode and series
ggplot(___, aes(x = ___, 
                y = ___, 
                group = ___)) +
    ___ +
    ___
```

`@solution`
```{r}
week_ratings <- ratings2  %>% 
	# Select 7-day viewer ratings
    select(series, ends_with("7day")) %>% 
	# Gather 7-day viewers by episode
    gather(episode, viewers_7day, ends_with("7day"), na.rm = TRUE, factor_key = TRUE)
    
# Plot 7-day viewers by episode and series
ggplot(week_ratings, aes(x = episode, 
                         y = viewers_7day, 
                         group = series)) +
    geom_line() +
    facet_wrap(~series)
```

`@sct`
```{r}
success_msg("You're done gathering! This plot tells you a lot, but the x-axis labels are a mess! We need to do more tidying to make this plot better.")

ex() %>% check_object("week_ratings") %>% check_equal(eq_condition = "identical", incorrect_msg = "Do not modify `week_ratings`.", append = FALSE)

ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal(incorrect_msg = "Are you plotting the `week_ratings` data?", append = FALSE)
    check_function(., "aes") %>% {
        check_arg(., "x") %>% check_equal(eval = FALSE) 
        check_arg(., "y") %>% check_equal(eval = FALSE)
        check_arg(., "group") %>% check_equal(eval = FALSE)
        }
    check_function(., "geom_line")
    check_function(., "facet_wrap") %>% check_arg(., "facets") %>% check_equal(eval = FALSE)
    }
ex() %>% check_error()
```

---

## Separate

```yaml
type: VideoExercise
key: d9637939b9
lang: r
xp: 50
skills: 1
```

`@projector_key`
7aff726cf37892a9b8450308a33d8ffc

---

## Separate a column

```yaml
type: TabExercise
key: 163526c04b
xp: 100
```

To create the `week_ratings` data, we first selected all the columns in `ratings2` that contain data about the number of 7-day viewers in millions for each series, then we gathered those 10 columns into two columns.

In this exercise, you'll use the `tidyr`, `dplyr`, and `readr` packages (all are loaded) to tidy and plot the `week_ratings` data so we can read the x-axis, which labels each episode!

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/263105bb1109ab2a8cf8e92d6fbf3eecd66140b8/messy_ratings2.csv", 
              destfile = "ratings2.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(ggplot2)
ratings2 <- read_csv("ratings2.csv", col_types = cols(series = col_factor(levels = NULL)))
```

`@sample_code`
```{r}

```

***

```yaml
type: NormalExercise
key: b7c416cb9b
xp: 30
```

`@instructions`
- Add a line after the `%>%` in your code from the last set of gathering exercises to separate the existing column named `episode` into a new column also called `episode`. 
- Drop the extra part that always says "7day". 
- Print to view the result.

`@hint`
- Use the `extra = "drop"` argument to drop the extra column (remember that "drop" needs to be in quotes).

`@sample_code`
```{r}
week_ratings <- ratings2 %>% 
    select(series, ends_with("7day")) %>% 
    gather(episode, viewers_7day, ends_with("7day"), 
           na.rm = TRUE)  %>% 
	# Edit to separate key column and drop extra


# Print to view

```

`@solution`
```{r}
week_ratings <- ratings2 %>% 
    select(series, ends_with("7day")) %>% 
    gather(episode, viewers_7day, ends_with("7day"), 
           na.rm = TRUE) %>% 
	# Edit to separate key column and drop extra
    separate(episode, into = "episode", extra = "drop") 

# Print to view
week_ratings
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "week_ratings") %>% check_equal(eq_condition = "identical"),
	{
    check_function(., "ends_with") %>% check_arg(., 'match') %>% check_equal(incorrect_msg = "Did you specify for only variables that end with `\"7day\"`?", append = FALSE)
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Remember to pipe `ratings2` into `select()`.", append = FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_message = "The order of the two arguments matter.", append = FALSE)
        }
    check_function(., "gather") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Make sure you're piping everything from `select()` into `gather()`.", append = FALSE)
        check_arg(., "key") %>% check_equal(eval=FALSE)
        check_arg(., "value") %>% check_equal(eval=FALSE)
        check_arg(., "na.rm") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical" ,incorrect_msg = "Remember to gather by 7-day viewers.", append = FALSE)
        }
    check_function(., "separate") %>% {
        check_arg(., "data") %>% check_equal()
        check_arg(., "col") %>% check_equal(eval=FALSE)
        check_arg(., "into") %>% check_equal(eval=FALSE)
        check_arg(., "extra") %>% check_equal(eval = FALSE, eq_condition = "identical")
        check_result(.) %>% check_equal(eq_condition = "identical" ,incorrect_msg = "Remember to drop the extras.", append = FALSE)
        }
    }
)
ex() %>% check_output_expr("week_ratings")
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: f88e42218f
xp: 30
```

`@instructions`
- The `episode` column is a character, but it will be easier to work with as a number. Use `readr::parse_number()` to drop the non-numeric characters before the episode number. Print again to view.

`@hint`
- Use `parse_number` within a `dplyr::mutate()` call to make a new variable called `episode`, saving over the existing `episode` variable.

`@sample_code`
```{r}
week_ratings <- ratings2 %>% 
    select(series, ends_with("7day")) %>% 
    gather(episode, viewers_7day, ends_with("7day"), 
           na.rm = TRUE) %>% 
    separate(episode, into = "episode", extra = "drop") %>% 
    # Edit to parse episode number


# Print to view

```

`@solution`
```{r}
week_ratings <- ratings2 %>% 
    select(series, ends_with("7day")) %>% 
    gather(episode, viewers_7day, ends_with("7day"), 
           na.rm = TRUE) %>% 
    separate(episode, into = "episode", extra = "drop") %>%
	# Edit to parse episode number
    mutate(episode = parse_number(episode))

# Print to view
week_ratings
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "week_ratings") %>% check_equal(eq_condition = "identical"),
	{
    check_function(., "ends_with") %>% check_arg(., 'match') %>% check_equal(incorrect_msg = "Did you specify for only variables that end with `\"7day\"`?", append = FALSE)
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Remember to pipe `ratings2` into `select()`.", append = FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_message = "The order of the two arguments matter.", append = FALSE)
        }
    check_function(., "gather") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Make sure you're piping everything from `select()` into `gather()`.", append = FALSE)
        check_arg(., "key") %>% check_equal(eval=FALSE)
        check_arg(., "value") %>% check_equal(eval=FALSE)
        check_arg(., "na.rm") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical" ,incorrect_msg = "Remember to gather by 7-day viewers.", append = FALSE)
        }
    check_function(., "separate") %>% {
        check_arg(., "data") %>% check_equal()
        check_arg(., "col") %>% check_equal(eval=FALSE)
        check_arg(., "into") %>% check_equal(eval=FALSE)
        check_arg(., "extra") %>% check_equal(eval = FALSE, eq_condition = "identical")
        check_result(.) %>% check_equal(eq_condition = "identical" ,incorrect_msg = "Remember to drop the extras.", append = FALSE)
        }
    check_function(., "parse_number") %>% check_arg(., "x") %>% check_equal(eval=FALSE)
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe everything from `separate()` into `mutate()`.", append = FALSE)
        check_arg(., "episode") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "There should only be one argument in `mutate()`.", append = FALSE)
        }
    }
)
ex() %>% check_output_expr("week_ratings")
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 496358347c
xp: 40
```

`@instructions`
- Edit your code for the facetted line plot from the previous exercise to also color the lines by `series`. Using [`guides()`](https://www.rdocumentation.org/packages/ggplot2/versions/3.0.0/topics/guides), remove the `color` guide by setting it equal to `FALSE`, and add `theme_minimal()`.

`@hint`
- Don't forget the parentheses are the end of the `theme_minimal()`!

`@sample_code`
```{r}
# Create week_ratings
week_ratings <- ratings2 %>% 
    select(series, ends_with("7day")) %>% 
    gather(episode, viewers_7day, ends_with("7day"), 
           na.rm = TRUE) %>% 
    separate(episode, into = "episode", extra = "drop") %>% 
    mutate(episode = parse_number(episode))
    
# Edit your code to color by series and add a theme
ggplot(week_ratings, aes(x = episode, y = viewers_7day, 
                         group = series, ___ = ___)) +
    geom_line() +
    facet_wrap(~series) +
    guides(___ = FALSE) +
    ___
```

`@solution`
```{r}
# Create week_ratings
week_ratings <- ratings2 %>% 
    select(series, ends_with("7day")) %>% 
    gather(episode, viewers_7day, ends_with("7day"), 
           na.rm = TRUE) %>% 
    separate(episode, into = "episode", extra = "drop") %>% 
    mutate(episode = parse_number(episode))
    
# Edit your code to color by series and add a theme
ggplot(week_ratings, aes(x = episode, y = viewers_7day, 
                         group = series, color = series)) +
    geom_line() +
    facet_wrap(~series) +
    guides(color = FALSE) +
    theme_minimal()
```

`@sct`
```{r}
ex() %>% check_object("week_ratings") %>% check_equal(eq_condition = "identical", incorrect_msg = "Do not modify `week_ratings`.", append = FALSE)
ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal()
    check_function(., "aes") %>% {
        check_arg(., "x") %>% check_equal(eval = FALSE) 
        check_arg(., "y") %>% check_equal(eval = FALSE)
        check_arg(., "group") %>% check_equal(eval = FALSE)
        check_arg(., "color") %>% check_equal(eval = FALSE)
        }
    check_function(., "geom_line")
    check_function(., "facet_wrap") %>% check_arg(., "facets") %>% check_equal(eval=FALSE)
    check_function(., "guides") %>% check_arg(., "color") %>% check_equal(eval=FALSE)
    check_function(., "theme_minimal")
    }
ex() %>% check_error()
success_msg("Nice work separating and parsing- you are becoming a tidy data pro! Notice the x-axis in this plot: episodes don't actually have decimals! We'll see how to deal with this in the last chapter when we work with factors.")
```

---

## Unite columns

```yaml
type: BulletExercise
key: 2a90a86962
lang: r
xp: 100
skills: 1
```

In the `tidyr` package, the opposite of `separate()` is [`unite()`](https://www.rdocumentation.org/packages/tidyr/versions/0.8.1/topics/unite). Sometimes you need to paste values from two or more columns together to tidy. Here is an example usage for `unite()`:

```
data %>%
    unite(new_var, old_var1, old_var2)
```

The `ratings2` dataset includes these variables:

- `series`: 1-8
- `episode`: 1-10
- `viewers_millions`: whole number of 7-day viewers in millions
- `viewers_decimal`: additional number of 7-days viewers in millions

If `viewers_millions` = 2 and `viewers_decimal` = .6, then the total number of viewers in millions is 2.6. In this exercise, you'll practice uniting these two columns into a single new column.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/263105bb1109ab2a8cf8e92d6fbf3eecd66140b8/messy_ratings2.csv", 
              destfile = "ratings2.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(stringr)
ratings2 <- read_csv("ratings2.csv") %>% 
    select(series, ends_with("7day")) %>% 
    gather(key, viewers_7day, ends_with("7day"), na.rm = TRUE) %>% 
    separate(key, into = "episode", extra = "drop") %>% 
    mutate(episode = parse_number(episode)) %>% 
    arrange(series, episode) %>% 
    mutate(viewers_millions = viewers_7day %/% 1,
           viewers_decimal = round(viewers_7day %% 1, 2),
           viewers_decimal = str_sub(viewers_decimal, 
                                       start = 2L, 
                                       end = -1L)) %>% 
    select(-viewers_7day)
```

`@sample_code`
```{r}

```

***

```yaml
type: NormalExercise
key: eadc2750a8
xp: 30
```

`@instructions`
- Create a new variable by uniting the two viewers data columns into one column called `viewers_7day`; print to view.

`@hint`
- Use help (`?unite`) to figure out the arguments for `unite()`.

`@sample_code`
```{r}
ratings3 <- ratings2 %>% 
	# Unite viewers in millions and decimals together    
	___(___, ___, ___)

# Print to view

```

`@solution`
```{r}
ratings3 <- ratings2  %>% 
	# Unite viewers in millions and decimals together		
	unite(viewers_7day, viewers_millions, viewers_decimal)

# Print to view
ratings3
```

`@sct`
```{r}
ex() %>% check_correct(
  	check_object(., "ratings3") %>% check_equal(eq_statement = "equal"),
	check_function(., "unite") %>% {
    	check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe `ratings2` into `unite()`.")
    	check_arg(., "col") %>% check_equal(eval=FALSE)
    	check_result(.) %>% check_equal(eq_statement = "equal", incorrect_msg = "You should be uniting the two viewers data columns.", append = FALSE)
    }
)

ex() %>% check_output_expr("ratings3")
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: c1601d95a6
xp: 30
```

`@instructions`
- Adapt your `unite()` code to use a new argument, `sep`. The default separator between new columns is `_`. Instead, set `sep = ""`. Print the new data frame to view it.

`@hint`
- The `sep` argument takes the separator in quotes, and `sep = "_"` is the default.

`@sample_code`
```{r}
ratings3 <- ratings2  %>% 
	# Adapt to change the separator
	unite(viewers_7day, viewers_millions, viewers_decimal)

# Print to view
ratings3
```

`@solution`
```{r}
ratings3 <- ratings2  %>% 
	# Adapt to change the separator
	unite(viewers_7day, viewers_millions, viewers_decimal, sep = "")

# Print to view
ratings3
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "ratings3") %>% check_equal(eq_statement = "equal"),
	check_function(., "unite") %>% {
    	check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe ratings2 into `unite()`.")
    	check_arg(., "col") %>% check_equal(eval=FALSE)
    	check_arg(., "sep") %>% check_equal(incorrect_msg = "Your argument sep is incorrect.", append = FALSE)
    	check_result(.) %>% check_equal(eq_statement = "equal", incorrect_msg = "You should be uniting the two viewers data columns.", append = FALSE)
    }
)
ex() %>% check_output_expr("ratings3")
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 9a9cdd966e
xp: 40
```

`@instructions`
- Adapt the provided code to cast the new `viewers_7day` column to a number.

`@hint`
- You'll want to use a mutate with `readr::parse_number()`.

`@sample_code`
```{r}
ratings3 <- ratings2  %>% 
	# Unite and change the separator
	unite(viewers_7day, viewers_millions, viewers_decimal, sep = "") %>%
	# Adapt to cast viewers as a number
	mutate(___ = ___(___))

# Print to view
ratings3
```

`@solution`
```{r}
ratings3 <- ratings2  %>% 
	# Unite and change the separator    
	unite(viewers_7day, viewers_millions, viewers_decimal, sep = "") %>% 
	# Adapt to cast viewers as a number
	mutate(viewers_7day = parse_number(viewers_7day))

# Print to view
ratings3
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "ratings3") %>% check_equal(eq_statement = "equal"),
	{
    check_function(., "unite") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe ratings2 into `unite()`.")
        check_arg(., "col") %>% check_equal(eval=FALSE)
        check_arg(., "sep") %>% check_equal(incorrect_msg = "Your argument sep is incorrect.", append = FALSE)
        check_result(.) %>% check_equal(eq_statement = "equal", incorrect_msg = "You should be uniting the two viewers data columns.", append = FALSE)
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe everything from `unite()` into `mutate()`.")
        check_arg(., "viewers_7day") %>% check_equal(incorrect_msg = "Use `parse_number()` within `mutate()`.", eval = FALSE, append = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "`parse_number()` shouldn't have any arguments.", append = FALSE)
        }
    }
)
ex() %>% check_output_expr("ratings3")
ex() %>% check_error()
```

---

## Spread

```yaml
type: VideoExercise
key: 6d85110e6c
lang: r
xp: 50
skills: 1
```

`@projector_key`
bcd81c3ec4664b352d286e8e174e8f1c

---

## Spread rows into columns

```yaml
type: TabExercise
key: a465f585e6
xp: 100
```

In this exercise, you'll use the `tidyr`, `dplyr`, and `readr` packages to tidy the `ratings2` data after using `dplyr::count()`. The `tidy_ratings_all` data should have four variables to work with:

* `series` (integer: 1-8), 
* `episode` (integer: 1-10), 
* `days` (integer: 7 or 28), and 
* `viewers` (numeric).

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/263105bb1109ab2a8cf8e92d6fbf3eecd66140b8/messy_ratings2.csv", 
              destfile = "ratings2.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
ratings2 <- read_csv("ratings2.csv")
```

`@sample_code`
```{r}

```

***

```yaml
type: NormalExercise
key: 73bd154db7
xp: 30
```

`@instructions`
- Start by gathering all columns that end with "day" into two columns: `episode` and `viewers`. Separate the key column into two columns named `episode` and `days`. Both of these two separated columns need to be parsed as numbers also, within a `mutate`.

`@hint`
- There are two variables that need to be parsed as numbers: `episode` and `days`; `series` and `viewers` are numbers already.

`@sample_code`
```{r}
# Create tidy data with 7- and 28-day viewers
tidy_ratings_all <- ratings2 %>%
	gather(key = ___, value = ___, ends_with(___), na.rm = TRUE) %>% 
    separate(___, into = c(___, ___)) %>%  
    mutate(___ = parse_number(),
           ___ = parse_number())
```

`@solution`
```{r}
# Create tidy data with 7- and 28-day viewers
tidy_ratings_all <- ratings2 %>%
	gather(key = episode, value = viewers, ends_with("day"), na.rm = TRUE) %>% 
    separate(episode, into = c("episode", "days")) %>%  
    mutate(episode = parse_number(episode),
           days = parse_number(days))
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "tidy_ratings_all") %>% check_equal(),
	{
    check_function(., "gather") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe ratings2 into `gather()`.", append = FALSE)
        check_arg(., "key") %>% check_equal(eval = FALSE)
        check_arg(., "value") %>% check_equal(eval = FALSE)
        check_arg(., "na.rm") %>% check_equal()
        check_result(.) %>% check_equal(incorrect_msg = "Remember to select all the columns that end with \"day\".", append = FALSE)
        }
    check_function(., "separate") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe everything from `gather()` into `separate()`.", append = FALSE)
        check_arg(., "col") %>% check_equal(eval = FALSE)
        check_arg(., "into") %>% check_equal(eval = FALSE)
        check_result(.) %>% check_equal()
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe everything from `separate()` into `mutate()`.", append = FALSE)
        check_arg(., "episode") %>% check_equal(eval = FALSE, incorrect_msg = "Use `parse_number()` on episode.", append = FALSE)
        check_arg(., "days") %>% check_equal(eval = FALSE, incorrect_msg = "Use `parse_number()` on days.", append = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "There should only be two arguments within `mutate()`.", append = FALSE)
        }
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 97bd9bd4be
xp: 30
```

`@instructions`
- Using your new tidy data, count the number of viewers grouping by `series` and `days`. **Remember:** you *don't* need to use `group_by` before `count`, but you *do* need to use the `wt = ` argument to sum the values in the `viewers` column.

`@hint`
- For example, after the `%>%`, you would use `count(var2, var2, wt = var3)` to sum the values in the third variable after grouping by the first two variables.

`@sample_code`
```{r}
# Create tidy data with 7- and 28-day viewers
tidy_ratings_all <- ratings2 %>%
    gather(episode, viewers, ends_with("day"), na.rm = TRUE) %>% 
    separate(episode, into = c("episode", "days")) %>%  
    mutate(episode = parse_number(episode),
           days = parse_number(days)) 

tidy_ratings_all %>%
	# Count viewers by series and days
	count(___, ___, wt = ___)
```

`@solution`
```{r}
# Create tidy data with 7- and 28-day viewers
tidy_ratings_all <- ratings2 %>%
    gather(episode, viewers, ends_with("day"), na.rm = TRUE) %>% 
    separate(episode, into = c("episode", "days")) %>%  
    mutate(episode = parse_number(episode),
           days = parse_number(days)) 

tidy_ratings_all %>% 
	# Count viewers by series and days
    count(series, days, wt = viewers)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "tidy_ratings_all") %>% check_equal(),
	{
    check_function(., "gather") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe ratings2 into `gather()`.", append = FALSE)
        check_arg(., "key") %>% check_equal(eval = FALSE)
        check_arg(., "value") %>% check_equal(eval = FALSE)
        check_arg(., "na.rm") %>% check_equal()
        check_result(.) %>% check_equal(incorrect_msg = "Remember to select all the columns that end with \"day\".", append = FALSE)
        }
    check_function(., "separate") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe everything from `gather()` into `separate()`.", append = FALSE)
        check_arg(., "col") %>% check_equal(eval = FALSE)
        check_arg(., "into") %>% check_equal(eval = FALSE)
        check_result(.) %>% check_equal()
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe everything from `separate()` into `mutate()`.", append = FALSE)
        check_arg(., "episode") %>% check_equal(eval = FALSE, incorrect_msg = "Use `parse_number()` on episode.", append = FALSE)
        check_arg(., "days") %>% check_equal(eval = FALSE, incorrect_msg = "Use `parse_number()` on days.", append = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "There should only be two arguments within `mutate()`.", append = FALSE)
        }
    }
)

ex() %>% check_correct(
	check_output_expr(., "tidy_ratings_all %>% count(series, days, wt = viewers)", missing_msg = "Count the number of viewers after applying `group_by()` on the correct variables."), 
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Pipe tidy_ratings_all into `count()`.", append = FALSE)
    	check_arg(., "wt") %>% check_equal(eval = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "`count()` by series and days.")
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 512ccd9e1f
xp: 40
```

`@instructions`
- Add a line after the `%>%` to reshape the output of `count` such that you have 3 columns: `series`, `days_7`, and `days_28`. To do this within [`spread()`](https://www.rdocumentation.org/packages/tidyr/versions/0.8.1/topics/spread), set `sep = "_"` as an argument.

`@hint`
-Use `?spread` to learn more about the `sep` argument. The default value is `NULL`.

`@sample_code`
```{r}
# Create tidy data with 7- and 28-day viewers
tidy_ratings_all <- ratings2 %>%
    gather(episode, viewers, ends_with("day"), na.rm = TRUE) %>% 
    separate(episode, into = c("episode", "days")) %>%  
    mutate(episode = parse_number(episode),
           days = parse_number(days)) 

tidy_ratings_all %>% 
	# Count viewers by series and days
    count(series, days, wt = viewers) %>%
	# Adapt to spread counted values

```

`@solution`
```{r}
# Create tidy data with 7- and 28-day viewers
tidy_ratings_all <- ratings2 %>% 
    gather(episode, viewers, ends_with("day"), na.rm = TRUE) %>% 
    separate(episode, into = c("episode", "days")) %>%  
    mutate(episode = parse_number(episode),
           days = parse_number(days)) 

tidy_ratings_all %>% 
	# Count viewers by series and days
    count(series, days, wt = viewers) %>%
	# Adapt to spread counted values
    spread(days, n, sep = "_")
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "tidy_ratings_all") %>% check_equal(),
	{
    check_function(., "gather") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe ratings2 into `gather()`.", append = FALSE)
        check_arg(., "key") %>% check_equal(eval = FALSE)
        check_arg(., "value") %>% check_equal(eval = FALSE)
        check_arg(., "na.rm") %>% check_equal()
        check_result(.) %>% check_equal(incorrect_msg = "Remember to select all the columns that end with \"day\".", append = FALSE)
        }
    check_function(., "separate") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe everything from `gather()` into `separate()`.", append = FALSE)
        check_arg(., "col") %>% check_equal(eval = FALSE)
        check_arg(., "into") %>% check_equal(eval = FALSE)
        check_result(.) %>% check_equal()
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe everything from `separate()` into `mutate()`.", append = FALSE)
        check_arg(., "episode") %>% check_equal(eval = FALSE, incorrect_msg = "Use `parse_number()` on episode.", append = FALSE)
        check_arg(., "days") %>% check_equal(eval = FALSE, incorrect_msg = "Use `parse_number()` on days.", append = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "There should only be two arguments within `mutate()`.", append = FALSE)
        }
    }
)

ex() %>% check_correct(
	check_output_expr(., "tidy_ratings_all %>% count(series, days, wt = viewers) %>% spread(days, n, sep = \"_\")", missing_msg = "Spread the original output into the three specified columns."),
	{
    check_function(., "count") %>% {
        check_arg(., "x") %>% check_equal(incorrect_msg = "Pipe tidy_ratings_all into `count()`.", append = FALSE)
        check_arg(., "wt") %>% check_equal(eval = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "`count()` by series and days.")
        }
    check_function(., "spread") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe everything from `count()` into `spread()`.", append = FALSE)
        check_arg(., "key") %>% check_equal(eval = FALSE)
        check_arg(., "value") %>% check_equal(eval = FALSE)
        check_arg(., "sep") %>% check_equal()
        check_result(.) %>% check_equal(incorrect_msg = "Spread the output from `count()` into the three specified columns.", eval = FALSE)
        }
    }
)

ex() %>% check_error()

success_msg("Great work! The `sep = \"_\"` argument is especially helpful when you are spreading by a numeric variable so that you don't end up with variable names that start with numbers.")
```

---

## Tidy multiple sets of columns

```yaml
type: VideoExercise
key: cb52e46a83
lang: r
xp: 50
skills: 1
```

`@projector_key`
61848b3d31d212772f224b6cdc14dfbf

---

## Masterclass: Tidy I

```yaml
type: TabExercise
key: e45cdce55b
lang: r
xp: 100
skills: 1
```

In the last three exercises, you'll tidy the `ratings` data to create a scatterplot to see the relationship between the number of premiere and finale UK viewers (7-day only) by series.

`@pre_exercise_code`
```{r}
download.file("http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/messy_ratings.csv", 
              destfile = "ratings.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(ggplot2)
ratings <- read_csv("ratings.csv", col_types = cols(series = col_factor(levels = NULL)))
```

***

```yaml
type: NormalExercise
key: fdc77ca379
xp: 30
```

`@instructions`
- Create a new tibble called `tidy_ratings` by gathering the 7-day viewer data by `episode` (all columns except `series`), removing the rows from output where the value column in `NA`. The key column should be named `episode`, the value should be `viewers`.

`@hint`
Remember to use `?gather` to see the argument for removing `NA` values.

`@sample_code`
```{r}
# Gather viewer columns and remove NA rows
tidy_ratings <- ratings %>%
	gather(key = ___, value = ___, ___, ___)
```

`@solution`
```{r}
# Gather viewer columns and remove NA rows
tidy_ratings <- ratings %>%
	gather(key = episode, value = viewers, -series, na.rm = TRUE)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "tidy_ratings") %>% check_equal(),
	check_function(., "gather") %>% {
    	check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe ratings into `gather()`", append = FALSE)
    	check_arg(., "key") %>% check_equal(eval = FALSE)
    	check_arg(., "value") %>% check_equal(eval = FALSE)
    	check_arg(., "na.rm") %>% check_equal()
    	check_result(.) %>% check_equal(incorrect_msg = "Don't forget to remove series.", append = FALSE)
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 7ccdeed0f4
xp: 30
```

`@instructions`
Using the pipe, add a line of code to convert the `episode` variable into a number.

`@hint`
You'll want to do this within a `mutate()`.

`@sample_code`
```{r}
# Adapt code to parse episode as a number
tidy_ratings <- ratings %>%
    gather(episode, viewers, -series, na.rm = TRUE)

```

`@solution`
```{r}
# Adapt code to parse episode as a number
tidy_ratings <- ratings %>%
    gather(episode, viewers, -series, na.rm = TRUE) %>%
    mutate(episode = parse_number(episode))
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "tidy_ratings") %>% check_equal(),
	{
    check_function(., "gather") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe ratings into `gather()`", append = FALSE)
        check_arg(., "key") %>% check_equal(eval = FALSE)
        check_arg(., "value") %>% check_equal(eval = FALSE)
        check_arg(., "na.rm") %>% check_equal()
        check_result(.) %>% check_equal(incorrect_msg = "Don't forget to remove `series`.", append = FALSE)
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe everything from `gather()` into `mutate()`.", append = FALSE)
        check_arg(., "episode") %>% check_equal(eval = FALSE, incorrect_msg = "Use `parse_number()` on the variable you're converting to a number.", append = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "You should only have one argument in `mutate()`.", append = FALSE)
        }
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: bbb07ff6e6
xp: 40
```

`@instructions`
Fill in the blanks in the provided code to filter rows that contain viewer data for each series' premiere and finale episodes- don't forget to undo the grouping when you are done!

`@hint`
The total number of episodes varies by `series` so you must filter after grouping by `series`- but don't forget to `ungroup()` at the end!

`@sample_code`
```{r}
# Fill in blanks to get premiere/finale data
tidy_ratings <- ratings %>%
    gather(episode, viewers, -series, na.rm = TRUE) %>%
    mutate(episode = parse_number(episode)) %>% 
    group_by(___) %>% 
    filter(___ == 1 | ___ == max(___)) %>% 
    ___
```

`@solution`
```{r}
# Fill in blanks to get premiere/finale data
tidy_ratings <- ratings %>%
    gather(episode, viewers, -series, na.rm = TRUE) %>%
    mutate(episode = parse_number(episode)) %>% 
    group_by(series) %>% 
    filter(episode == 1 | episode == max(episode)) %>% 
    ungroup()
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "tidy_ratings") %>% check_equal(),
	{
    check_function(., "gather") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Pipe ratings into `gather()`", append = FALSE)
        check_arg(., "key") %>% check_equal(eval = FALSE)
        check_arg(., "value") %>% check_equal(eval = FALSE)
        check_arg(., "na.rm") %>% check_equal()
        check_result(.) %>% check_equal(incorrect_msg = "Don't forget to remove series.", append = FALSE)
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe everything from `gather()` into `mutate()`.", append = FALSE)
        check_arg(., "episode") %>% check_equal(eval = FALSE, incorrect_msg = "Use `parse_number()` on the variable you're converting to a number.", append = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "You should only have one argument in `mutate()`.", append = FALSE)
        }
    check_function(., "group_by") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe everything from `mutate()` into `group_by()`", append = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "Are you using `group_by()` on the correct variable?", append = FALSE)
        }
    check_function(., "filter") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe everything from `group_by()` into `filter()`.", append = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "Are you using `filter()` on the correct variable?", append = FALSE)
        }
    check_function(., "ungroup") %>% {
        check_arg(., "x") %>% check_equal("Pipe everything from `filter()` into `ungroup()`.", append = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "There shouldn't be any arguments inside `ungroup()`.", append = FALSE)
        }
    }
)
ex() %>% check_error()
success_msg("Now we have a tidy version of the `ratings` data with only premiere and finale episodes. Let's see if that tweet spoiled the Series 8 finale. You'll see why we needed the last `ungroup()` in the next exercises.")
```

---

## Masterclass: Tidy II

```yaml
type: TabExercise
key: 6a87b0714b
lang: r
xp: 100
skills: 1
```

In the previous exercise, you gathered, then did a grouped filter to make `tidy_ratings`. In this exercise, you'll make two charts in `ggplot2` with this tidy data to see if the judge's tweet spoiled the Series 8 finale ratings. 

The two plots you'll make, a slope chart and a dumbbell chart, are nice ways to show change between two data points combining `geom_point()` and `geom_line()`.

You'll use these to visualize the "finale bump" in viewers between the premiere and finale episodes across series.

`@pre_exercise_code`
```{r}
download.file("http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/messy_ratings.csv", 
              destfile = "ratings.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(ggplot2)
tidy_ratings <- read_csv("ratings.csv", col_types = cols(series = col_factor(levels = NULL))) %>%
    gather(episode, viewers, -series, na.rm = TRUE) %>%
    mutate(episode = parse_number(episode)) %>% 
    group_by(series) %>% 
    filter(episode == 1 | episode == max(episode)) %>% 
    ungroup()
```

***

```yaml
type: NormalExercise
key: c908165025
xp: 30
```

`@instructions`
Recode the `episode` variable as either `"first"` or `"last"`.

`@hint`
You'll want to use a `mutate()`. The last episode number differs by series - remember the `.default` argument for `recode()`!

`@sample_code`
```{r}
# Recode first/last episodes
first_last <- tidy_ratings %>%

```

`@solution`
```{r}
# Recode first/last episodes
first_last <- tidy_ratings %>% 
  mutate(episode = recode(episode, `1` = "first", .default = "last"))
```

`@sct`
```{r}
ex() %>% check_correct(
  check_object(., "first_last") %>% check_equal(),
  check_function(., "mutate") %>% {
    check_arg(., ".data") %>% check_equal(incorrect_msg = "Are you piping tidy_ratings into `mutate()`?", append = FALSE)
    check_arg(., "episode")
    check_function(., "recode") %>% {
      check_arg(., ".x") %>% check_equal(eval = FALSE, incorrect_msg = "Are you recoding the correct variable?", append = FALSE)
      check_arg(., ".default") %>% check_equal(eval=FALSE)
      check_code(., '`1` = "first"', missing_msg = "Are you setting 1 in backticks equal to first?", append = FALSE)
    }
  }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 06ec6e8775
xp: 30
```

`@instructions`
Make a slope chart where the x-axis values are either `"first"` or `"last"`, and the slope of the line connecting the two points shows the change in viewers. Color the points and lines by `series`.

`@hint`
You'll want to plot first/last `episode` along the x-axis and `viewers` along the y-axis.

`@sample_code`
```{r}
# Recode first/last episodes
first_last <- tidy_ratings %>% 
  mutate(episode = recode(episode, `1` = "first", .default = "last")) 

# Fill in to make slope chart
ggplot(___, aes(x = ___, y = ___, color = ___)) +
  geom_point() +
  geom_line(aes(group = series))
```

`@solution`
```{r}
# Recode first/last episodes
first_last <- tidy_ratings %>% 
  mutate(episode = recode(episode, `1` = "first", .default = "last")) 

# Fill in to make slope chart
ggplot(first_last, aes(x = episode, y = viewers, color = series)) +
  geom_point() +
  geom_line(aes(group = series))
```

`@sct`
```{r}
ex() %>% check_object("first_last") %>% check_equal(incorrect_msg = "Do not modify `first_last`.", append = FALSE)
ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal()
    check_function(., "aes", index = 1) %>% {
        check_arg(., "x") %>% check_equal(eval = FALSE) 
        check_arg(., "y") %>% check_equal(eval = FALSE)
        check_arg(., "color") %>% check_equal(eval = FALSE)
        }
    check_function(., "geom_point")
    check_function(., "geom_line")
    }

ex() %>% check_or(
	check_function(., "aes", index = 2) %>% check_arg("group") %>% check_equal(eval=FALSE, incorrect_msg = "Are you using `group` on the correct variable?", append=FALSE), 
	override_solution(., "aes(x = episode, y = viewers, color = series, group = series)") %>% check_function("aes") %>% check_arg("group") %>% check_equal(eval=FALSE, incorrect_msg = "Are you using `group` on the correct variable?", append=FALSE)
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: d48da22a01
xp: 40
```

`@instructions`
To make a dumbbell chart instead, switch the x-axis and color variable mapping. After flipping the axes using `coord_flip()`, the length of the "dumbbell" now illustrates the size of the "finale bump."

`@hint`
Keep the same `geom`s and grouping, but map `series` to the x-axis and `episode` to color.

`@sample_code`
```{r}
# Recode first/last episodes
first_last <- tidy_ratings %>% 
  mutate(episode = recode(episode, `1` = "first", .default = "last")) 

# Switch the variables mapping x-axis and color
ggplot(first_last, aes(x = episode, y = viewers, color = series)) +
  geom_point() + # keep
  geom_line(aes(group = series)) + # keep
  coord_flip() # keep
```

`@solution`
```{r}
# Recode first/last episodes
first_last <- tidy_ratings %>% 
  mutate(episode = recode(episode, `1` = "first", .default = "last")) 

# Switch the variables mapping x-axis and color
ggplot(first_last, aes(x = series, y = viewers, color = episode)) +
  geom_point() + # keep
  geom_line(aes(group = series)) + # keep
  coord_flip() # keep
```

`@sct`
```{r}
ex() %>% check_correct(
  check_object(., "first_last") %>% check_equal(),
  check_function(., "mutate") %>% {
    check_arg(., ".data") %>% check_equal(incorrect_msg = "Are you piping tidy_ratings into `mutate()`?", append = FALSE)
    check_arg(., "episode")
    check_function(., "recode") %>% {
      check_arg(., ".x") %>% check_equal(eval = FALSE, incorrect_msg = "Are you recoding the correct variable?", append = FALSE)
      check_arg(., ".default") %>% check_equal(eval=FALSE)
      check_code(., '`1` = "first"', missing_msg = "Are you setting 1 in backticks equal to first?", append = FALSE)
    }
  }
)
ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal()
    check_function(., "aes", index = 1) %>% {
        check_arg(., "x") %>% check_equal(eval = FALSE) 
        check_arg(., "y") %>% check_equal(eval = FALSE)
        check_arg(., "color") %>% check_equal(eval = FALSE)
        }
    check_function(., "geom_point")
    check_function(., "geom_line")
    check_function(., "coord_flip")
    }

ex() %>% check_or(
	check_function(., "aes", index = 2) %>% check_arg("group") %>% check_equal(eval=FALSE, incorrect_msg = "Are you using `group` on the correct variable?", append=FALSE), 
	override_solution(., "aes(x = episode, y = viewers, color = series, group = series)") %>% check_function("aes") %>% check_arg("group") %>% check_equal(eval=FALSE, incorrect_msg = "Are you using `group` on the correct variable?", append=FALSE)
)    

ex() %>% check_error()

success_msg("Great work- notice how each of these two plots relied on data that is tidy! Series 8 had a pretty small 'finale bump', but we are still looking at raw changes in viewers. Let's visualize percentage changes in viewers instead.")
```

---

## Masterclass: Tidy III

```yaml
type: TabExercise
key: 0c2729990d
lang: r
xp: 100
skills: 1
```

In the previous exercise, you visualized the size of the "finale bump" in viewers for each series. From those charts, we know that finale episodes always garner more viewers than premieres. We also know that the change in viewers for series 8 seemed smaller compared to some previous series. 

But the question of whether the series 8 finale viewers were "spoiled" might be best answered by comparing the relative percentage changes in premiere and finale viewers across series. In this exercise, you'll reshape `first_last` once more to calculate and visualize the *relative finale bump*.

`@pre_exercise_code`
```{r}
download.file("http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/messy_ratings.csv", 
              destfile = "ratings.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(ggplot2)
first_last <- read_csv("ratings.csv", col_types = cols(series = col_factor(levels = NULL))) %>%
    gather(episode, viewers, -series, na.rm = TRUE) %>%
    mutate(episode = parse_number(episode)) %>% 
    group_by(series) %>% 
    filter(episode == 1 | episode == max(episode)) %>% 
    ungroup() %>% 
    mutate(episode = recode(episode, `1` = "first", .default = "last"))
```

***

```yaml
type: NormalExercise
key: 6678095b16
xp: 30
```

`@instructions`
Create a new data frame called `bump_by_series` with three columns: `series`, `first`, and `last`.

`@hint`
You'll want to use a `spread()`- the new `first` and `last` columns carry the viewers in millions.

`@sample_code`
```{r}
# Spread into three columns


```

`@solution`
```{r}
# Spread into three columns
bump_by_series <- first_last %>% 
    spread(episode, viewers)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bump_by_series") %>% check_equal(),
	check_function(., "spread") %>% {
    	check_arg(., "data") %>% check_equal(incorrect_msg = "Are you piping `first_last` into `spread()`?", append = FALSE)
    	check_arg(., "key") %>% check_equal(eval = FALSE)
    	check_arg(., "value") %>% check_equal(eval = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Make sure you're creating the new data frame with the three specified columns.", append = FALSE)
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 938be4758b
xp: 30
```

`@instructions`
Add a line after the `%>%` to create a new variable called `bump` to calculate the difference between finale and premiere viewers in the numerator, divided by the number of premiere viewers.

`@hint`
Mind your parentheses carefully here- you want `(last - first)` in the numerator, don't make the mistake of typing: `last - first/first`!

`@sample_code`
```{r}
# Calculate relative increase in viewers
bump_by_series <- first_last %>% 
    spread(episode, viewers) %>%

```

`@solution`
```{r}
# Calculate relative increase in viewers
bump_by_series <- first_last %>% 
    spread(episode, viewers) %>% 
    mutate(bump = (last - first) / first)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bump_by_series") %>% check_equal(),
 	{
    check_function(., "spread") %>% {
        check_arg(., "data") %>% check_equal(incorrect_msg = "Are you piping `first_last` into `spread()`?", append = FALSE)
        check_arg(., "key") %>% check_equal(eval = FALSE)
        check_arg(., "value") %>% check_equal(eval = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "Make sure you're creating the new data frame with the three specified columns.", append = FALSE)
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Are you piping everything from `spread()` into `mutate()`?", append = FALSE)
        check_arg(., "bump") %>% check_equal(eval = FALSE, incorrect_msg = "Make sure your math expression is correct.", append = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "`mutate()` should have one argument.", append = FALSE)
        }
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 43dd15eee0
xp: 40
```

`@instructions`
Fill in the provided code to make a bar chart with one bar per series, where the height of the bar shows the bump in 7-day viewers from the premiere to the finale episodes. Do any series stand out?

`@hint`
Use `?geom_bar` to find the right geom for the heights of the bars to represent values in the data.

`@sample_code`
```{r}
# Calculate relative increase in viewers
bump_by_series <- first_last %>% 
  spread(episode, viewers) %>%   
  mutate(bump = (last - first) / first)
  
# Fill in to make bar chart of bumps by series
ggplot(___, aes(x = ___, y = ___)) +
  ___ +
  scale_y_continuous(labels = scales::percent) # converts to %
```

`@solution`
```{r}
# Calculate relative increase in viewers
bump_by_series <- first_last %>% 
  spread(episode, viewers) %>%   
  mutate(bump = (last - first) / first)
  
# Fill in to make bar chart of bumps by series
ggplot(bump_by_series, aes(x = series, y = bump)) +
  geom_col() + 
  scale_y_continuous(labels = scales::percent) # converts to %
```

`@sct`
```{r}
ex() %>% check_object("bump_by_series") %>% check_equal(incorrect_msg = "Do not modify `bump_by_series`.", append = FALSE)
ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal(incorrect_msg = "Are you piping bump_by_series into `ggplot()`?", append = FALSE)
    check_function(., "aes") %>% {
        check_arg(., "x") %>% check_equal(eval = FALSE)
        check_arg(., "y") %>% check_equal(eval = FALSE)
        }
    check_function(., "geom_col")
    check_function(., "scale_y_continuous") %>% check_arg("labels") %>% check_equal()
    }
ex() %>% check_error()

success_msg("Series 8 definitely had the lowest 'finale bump' so far- only 6% more viewers watched the finale than the premiere. Notice that we had to `spread` here, because the tidy version of the data depends on the question you want to ask.")
```
