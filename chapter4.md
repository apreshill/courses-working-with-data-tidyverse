---
title: 'Transform your data'
description: 'In this chapter, you will learn how to tame specific types of variables that are known to be tricky to work with, such as dates, strings, and factors. '
attachments:
    slides_link: 'https://s3.amazonaws.com/assets.datacamp.com/production/course_6012/slides/chapter4.pdf'
---

## Complex recoding with case_when

```yaml
type: VideoExercise
key: ac77f91983
lang: r
xp: 50
skills: 1
```

`@projector_key`
a4d8f6d1082082e04e67256a1b2c4cd2

---

## Combine two variables

```yaml
type: TabExercise
key: 490b4cf37d
lang: r
xp: 100
```

In this exercise, you'll use `case_when()` with the `bakers` data to create a new variable based on the number of times each baker was crowned as star baker or the technical challenge winner:

- If star baker is *greater than* technical wins, recode as `super_star`
- If star baker is *less than* technical wins, recode as `high_tech`
- Recode the rest as `well_rounded`

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/159c5829356f90036720b34a4da121717d921266/baker_results.csv", 
              destfile = "bakers.csv", quiet = TRUE)
library(dplyr)
library(readr)
bakers <- read_csv("bakers.csv",
                      col_types = cols(series = col_factor(levels = NULL)))
```

***

```yaml
type: NormalExercise
key: 5446ef8229
xp: 50
```

`@instructions`
Create a new variable with three levels as described above.

`@hint`
Remember that the left hand side (LHS) must evaluate to `TRUE` or `FALSE`. The RHS values should be in quotes.

`@sample_code`
```{r}
# Create skills variable with 3 levels
bakers_skill <- bakers %>% 
  mutate(skill = case_when(
    ___ ~ ___,
    ___ ~ ___,
    ___ ~ ___
  ))
```

`@solution`
```{r}
# Create skills variable with 3 levels
bakers_skill <- bakers %>% 
  mutate(skill = case_when(
    star_baker > technical_winner ~ "super_star",
    star_baker < technical_winner ~ "high_tech",
    TRUE ~ "well_rounded"
  ))
```

`@sct`
```{r}
ex() %>% check_correct({
  check_object(., "bakers_skill") %>% check_equal()
}, {
  check_function(., 'case_when')
  check_function(., 'mutate') %>% check_result() %>% check_equal()
})
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 58e00b5a12
xp: 50
```

`@instructions`
Filter the rows where both `star_baker` and `technical_winner` are zero, then count the number of bakers by `skill`. How many do you see?

`@hint`
Remember to use `==` in your `filter` for "exactly equal to" 0.

`@sample_code`
```{r}
# Create skill variable with 3 levels
bakers_skill <- bakers %>% 
  mutate(skill = case_when(
    star_baker > technical_winner ~ "super_star",
    star_baker < technical_winner ~ "high_tech",
    TRUE ~ "well_rounded"
  ))
  
# Filter zeroes to examine skill variable
bakers_skill %>% 
  filter(___ & ___) %>% 
  count(___)
```

`@solution`
```{r}
# Create skill variable with 3 levels
bakers_skill <- bakers %>% 
  mutate(skill = case_when(
    star_baker > technical_winner ~ "super_star",
    star_baker < technical_winner ~ "high_tech",
    TRUE ~ "well_rounded"
  ))
  
# Filter zeroes to examine skill variable
bakers_skill %>% 
  filter(star_baker == 0 & technical_winner == 0) %>% 
  count(skill)
```

`@sct`
```{r}
ex() %>% check_correct(
    check_object(., "bakers") %>% check_equal(),
    {
        check_function(., "case_when")
        check_function(., "mutate") %>% check_result() %>% check_equal()
    }
)

ex() %>% {
    check_function(., "filter") %>% check_result() %>% check_equal()
    check_function(., "count") %>% check_result() %>% check_equal()
}

success_msg("Whoops- we may not want bakers who never won star baker or the technical challenge to be called 'well-rounded'. Let's add another level to take care of this!")
```

---

## Add another bin

```yaml
type: TabExercise
key: 356647defa
lang: r
xp: 100
```

In this exercise, you'll edit your previous code so that `skill` has four levels:

- If both star baker and technical wins are zero, recode as `NA_character_`
- If star baker is *greater than* technical wins, recode as `super_star`
- If star baker is *less than* technical wins, recode as `high_tech`
- If star baker is *equal to* technical wins, recode as `well_rounded`

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/159c5829356f90036720b34a4da121717d921266/baker_results.csv", 
              destfile = "bakers.csv", quiet = TRUE)
library(dplyr)
library(readr)
library(tidyr)
bakers <- read_csv("bakers.csv",
                      col_types = cols(series = col_factor(levels = NULL)))
```

***

```yaml
type: NormalExercise
key: ae01146771
xp: 30
```

`@instructions`
Edit the `skill` variable you just created to have the four levels as described above.

`@hint`
Remember that the sequences are evaluated in order, from top to bottom. Order matters here, especially for `NA_character_`!

`@sample_code`
```{r}
# Edit skill variable to have 4 levels
bakers_skill <- bakers %>% 
  mutate(skill = case_when(
    star_baker > technical_winner ~ "super_star",
    star_baker < technical_winner ~ "high_tech",
    TRUE ~ "well_rounded"
  ))
```

`@solution`
```{r}
# Edit skill variable to have 4 levels
bakers_skill <- bakers %>% 
  mutate(skill = case_when(
    star_baker > technical_winner ~ "super_star",
    star_baker < technical_winner ~ "high_tech",
    star_baker == 0 & technical_winner == 0 ~ NA_character_,
    star_baker == technical_winner  ~ "well_rounded"
  ))
```

`@sct`
```{r}
msg = "Did you use `NA_character_` on the correct expression?"
ex() %>% check_correct(
    check_object(., "bakers_skill") %>% check_equal(eq_condition = "identical"),
    {
        check_function(., "case_when") %>% {
            check_expr(., "sort(table(bakers_skill$skill))[1]") %>% check_result() %>% check_equal(incorrect_msg = msg, append = FALSE)
            check_expr(., "sort(table(bakers_skill$skill))[2]") %>% check_result() %>% check_equal(incorrect_msg = msg, append = FALSE)
            check_expr(., "sort(table(bakers_skill$skill))[3]") %>% check_result() %>% check_equal(incorrect_msg = msg, append = FALSE)
        }
        check_function(., "mutate") %>% {
            check_arg(., ".data") %>% check_equal()
            check_arg(., "skill") %>% check_equal(eval=FALSE)
            check_result(.) %>% check_equal(incorrect_msg = "There should only be one argument in `mutate()`.", append = FALSE)
        }
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: bb9078f8e6
xp: 30
```

`@instructions`
Add a pipe and use [`drop_na()`](https://www.rdocumentation.org/packages/tidyr/versions/0.8.1/topics/drop_na) from the `tidyr` package to drop all rows where `skill` is `NA`.

`@hint`
Use `?drop_na` to see how this function works- it does *not* need to be within a `mutate`.

`@sample_code`
```{r}
# Add pipe to drop skill = NA
bakers_skill <- bakers %>% 
  mutate(skill = case_when(
    star_baker > technical_winner ~ "super_star",
    star_baker < technical_winner ~ "high_tech",
    star_baker == 0 & technical_winner == 0 ~ NA_character_,
    star_baker == technical_winner  ~ "well_rounded"
  ))

```

`@solution`
```{r}
# Add pipe to drop skill = NA
bakers_skill <- bakers %>% 
  mutate(skill = case_when(
    star_baker > technical_winner ~ "super_star",
    star_baker < technical_winner ~ "high_tech",
    star_baker == 0 & technical_winner == 0 ~ NA_character_,
    star_baker == technical_winner  ~ "well_rounded"
  )) %>% 
  drop_na(skill)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bakers_skill") %>% check_equal(eq_condition = "identical"),
	{
    check_function(., "case_when") %>% {
        check_expr(., "sort(table(bakers_skill$skill))[1]") %>% check_result() %>% check_equal(incorrect_msg = "Did you use `NA_character_` on the correct expression?", append = FALSE)
        check_expr(., "sort(table(bakers_skill$skill))[2]") %>% check_result() %>% check_equal(incorrect_msg = "Did you use `NA_character_` on the correct expression?", append = FALSE)
        check_expr(., "sort(table(bakers_skill$skill))[3]") %>% check_result() %>% check_equal(incorrect_msg = "Did you use `NA_character_` on the correct expression?", append = FALSE)
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal()
        check_arg(., "skill") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "There should only be one argument in `mutate()`.", append = FALSE)
        }
    check_function(., "drop_na") %>% {
        check_arg(., "data") %>% check_equal()
        check_result(.) %>% check_equal(incorrect_msg = "Make sure you have the correct argument in `drop_na()`.", append = FALSE)
        }
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 6a9b93b202
xp: 40
```

`@instructions`
Count the number of bakers in each `skill` level.

`@hint`
You'll want to use `count()` with one variable.

`@sample_code`
```{r}
# Add pipe to drop skill = NA
bakers_skill <- bakers %>% 
  mutate(skill = case_when(
    star_baker > technical_winner ~ "super_star",
    star_baker < technical_winner ~ "high_tech",
    star_baker == 0 & technical_winner == 0 ~ NA_character_,
    star_baker == technical_winner  ~ "well_rounded"
  )) %>% 
  drop_na(skill)
  
# Count bakers by skill


```

`@solution`
```{r}
# Add pipe to drop skill = NA
bakers_skill <- bakers %>% 
  mutate(skill = case_when(
    star_baker > technical_winner ~ "super_star",
    star_baker < technical_winner ~ "high_tech",
    star_baker == 0 & technical_winner == 0 ~ NA_character_,
    star_baker == technical_winner  ~ "well_rounded"
  )) %>% 
  drop_na(skill)
  
# Count bakers by skill
bakers_skill %>% 
  count(skill)
```

`@sct`
```{r}
ex() %>% check_object("bakers_skill") %>% check_equal(eq_condition = "identical", incorrect_msg = "Do not alter `bakers_skill`.", append = TRUE)

ex() %>% check_correct(
	check_output_expr(., "bakers_skill %>% count(skill)", missing_msg = "Are you counting by the right variable?"),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're counting the correct variable from bakers_skill.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "`count()` should be your only function.", append = FALSE)
    }
)

ex() %>% check_error()

success_msg("Very nice- now we only have 15 'well-rounded' bakers, and we dropped the 41 bakers who never won star baker or a technical challenge.")
```

---

## Factors

```yaml
type: VideoExercise
key: 630c00448d
lang: r
xp: 50
skills: 1
```

`@projector_key`
3263d388847709989da3eff7a205c9d8

---

## Cast a factor and examine levels

```yaml
type: TabExercise
key: 61e8b7c76d
lang: r
xp: 100
```

In this exercise, you'll start where you left off in the last series of exercises, working with the `bakers` data to examine your new categorical variable called `skill`, which has three levels: `well_rounded`, `super_star`, and `high_tech`.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/159c5829356f90036720b34a4da121717d921266/baker_results.csv", 
              destfile = "bakers.csv", quiet = TRUE)
library(dplyr)
library(readr)
library(tidyr)
bakers <- read_csv("bakers.csv",
                      col_types = cols(series = col_factor(levels = NULL))) %>% 
  mutate(skill = case_when(
    star_baker > technical_winner ~ "super_star",
    star_baker < technical_winner ~ "high_tech",
    star_baker == 0 & technical_winner == 0 ~ NA_character_,
    star_baker == technical_winner  ~ "well_rounded"
  )) %>% 
  drop_na()
```

***

```yaml
type: NormalExercise
key: b200b155fc
xp: 50
```

`@instructions`
Cast the `skill` variable as a factor.

`@hint`
You'll want to use `as.factor()` within a `mutate()`, and save over the original variable.

`@sample_code`
```{r}
# Cast skill as a factor
bakers <- bakers %>% 
  ___(___ = ___)
```

`@solution`
```{r}
# Cast skill as a factor
bakers <- bakers %>% 
  mutate(skill = as.factor(skill))
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bakers") %>% check_equal(),
	{
    check_function(., "as.factor") %>% check_arg(., "x") %>% check_equal(eval=FALSE)
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Are you piping bakers into `mutate()`?", append = FALSE)
        check_arg(., "skill") %>% check_equal(eval = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "Are you using `as.factor()` on the correct variable?", append = FALSE)
        }
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: d9a25dd91d
xp: 50
```

`@instructions`
Examine the levels of the factor, and if you were to plot it, think about what the order of the levels would be.

`@hint`
Use `pull()` to grab the `skill` variable, then pipe that to the `levels()` function.

`@sample_code`
```{r}
# Cast skill as a factor
bakers <- bakers %>% 
  mutate(skill = as.factor(skill))

# Examine levels



```

`@solution`
```{r}
# Cast skill as a factor
bakers <- bakers %>% 
  mutate(skill = as.factor(skill))

# Examine levels
bakers %>% 
  pull(skill) %>% 
  levels()
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bakers") %>% check_equal(),
	{
    check_function(., "as.factor") %>% check_arg(., "x") %>% check_equal(eval=FALSE)
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Are you piping bakers into `mutate()`?", append = FALSE)
        check_arg(., "skill") %>% check_equal(eval = FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "Are you using `as.factor()` on the correct variable?", append = FALSE)
        }
    }
)
test_correct(ex() %>% check_output_expr("bakers %>% pull(skill) %>% levels()", missing_msg = "Remember to examine the levels of the skill variable within `bakers`."),
ex() %>% check_function("levels") %>% {
    check_arg(., "x") %>% check_equal(eval=FALSE)
    check_result(.) %>% check_equal(incorrect_msg = "Remember to examine the levels of the skill variable within `bakers`.", append = FALSE)
    }
)
ex() %>% check_error()

success_msg("Sometimes factors have a natural order to them- like beginner, intermediate, and advanced- but sometimes they don't. Know your data, and know your factors!")
```

---

## Plot factor counts

```yaml
type: BulletExercise
key: 006ef87d71
lang: r
xp: 100
skills: 1
```

In this exercise, you'll use `ggplot2` to make a bar chart with one per level of the `skill` factor we just made, where the height of the bar shows the number of bakers in each skill level. 

Notice a *big* difference between the data here and the data we have used before to make a bar chart- here, you want the height of the bars to be represent the number of rows in the `bakers` data in each skill level. This is compared to bar charts we made where the height represented values in the data (when you used `geom_col()`).

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/159c5829356f90036720b34a4da121717d921266/baker_results.csv", 
              destfile = "bakers.csv", quiet = TRUE)
library(dplyr)
library(readr)
library(tidyr)
library(ggplot2)
library(forcats)
bakers <- read_csv("bakers.csv",
                      col_types = cols(
                        series = col_factor(levels = NULL),
                        series_winner = col_factor(levels = NULL)
                      )
                  ) %>% 
  mutate(skill = case_when(
    star_baker > technical_winner ~ "super_star",
    star_baker < technical_winner ~ "high_tech",
    star_baker == 0 & technical_winner == 0 ~ NA_character_,
    star_baker == technical_winner  ~ "well_rounded")
         ) %>% 
  drop_na(skill) %>% 
  mutate(skill = as.factor(skill))
```

`@sample_code`
```{r}

```

***

```yaml
type: NormalExercise
key: a19cdb46ba
xp: 50
```

`@instructions`
Using the `bakers` data, plot the counts of bakers by each `skill` level, coloring the bars for series winners.

`@hint`
You'll want to use `geom_bar()` here, and map the `fill` aesthetic onto `series_winner`.

`@sample_code`
```{r}
# Plot counts of bakers by skill, fill by winner


```

`@solution`
```{r}
# Plot counts of bakers by skill, fill by winner
ggplot(bakers, aes(x = skill, fill = series_winner)) +
  geom_bar()
```

`@sct`
```{r}
ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal(incorrect_msg = "Are you using the bakers data?", append = FALSE)
    check_function(., "aes") %>% {
        check_arg(., "x") %>% check_equal(eval = FALSE) 
        check_arg(., "fill") %>% check_equal(eval = FALSE)
        }
    check_function(., "geom_bar")
    }
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: deb496829c
xp: 50
```

`@instructions`
Edit your plot code to reverse the order of the `skill` factor levels along the x-axis.

`@hint`
You'll want to use `fct_rev()` here.

`@sample_code`
```{r}
# Edit to reverse x-axis order
ggplot(bakers, aes(x = skill, fill = series_winner)) +
  geom_bar()
```

`@solution`
```{r}
# Edit to reverse x-axis order
ggplot(bakers, aes(x = fct_rev(skill), fill = series_winner)) +
  geom_bar()
```

`@sct`
```{r}
ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal(incorrect_msg = "Are you using the bakers data?", append = FALSE)
    check_function(., "fct_rev") %>% check_arg(., "f") %>% check_equal(eval = FALSE)
    check_function(., "aes") %>% {
        check_arg(., "x") %>% check_equal(eval = FALSE) 
        check_arg(., "fill") %>% check_equal(eval = FALSE)
        }
    check_function(., "geom_bar")
    }
ex() %>% check_error()
success_msg("Great job! Note that when you use function like `fct_rev()`, `forcats` converts the character to a factor for your plot only.")
```

---

## Dates

```yaml
type: VideoExercise
key: 5bae77b50f
lang: r
xp: 50
skills: 1
```

`@projector_key`
6cd0e5ddc76a5b1455418eeef2a225b0

---

## Cast characters as dates

```yaml
type: TabExercise
key: cb085ac8de
lang: r
xp: 100
```

As we saw in the video, you can use `lubridate` to parse and cast a date variable within a `mutate()`. In this exercise, you'll practice doing this with the `baker_dates` data, and then go even further to extract data from the dates like the labelled month. Remember to use `?month` to read about the `month()` function and its arguments.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/course_7012/datasets/messy_baker_results.csv", 
              destfile = "bakers.csv", quiet = TRUE)
library(dplyr)
library(readr)
library(tidyr)
library(ggplot2)
library(lubridate)
baker_dates <- read_csv("bakers.csv",
                   na = c("not aired in US", "NA"),
                   col_types = cols(age = col_number())) %>% 
  select(series, baker, contains("date"), -first_date_us, -last_date_us)
```

***

```yaml
type: NormalExercise
key: 4e9041ea11
xp: 30
```

`@instructions`
Cast `last_date_appeared_us` as a date.

`@hint`
Remember that the key is the order of the letters: `y`, `d`, `m`; use `?ymd` for help.

`@sample_code`
```{r}
# Cast last_date_appeared_us as a date
baker_dates_cast <-

```

`@solution`
```{r}
# Cast last_date_appeared_us as a date
baker_dates_cast <- baker_dates %>% 
  mutate(last_date_appeared_us = dmy(last_date_appeared_us))
```

`@sct`
```{r}
ex() %>% check_correct(
  	check_object(., "baker_dates_cast") %>% check_equal(eq_statement = "equal"),
	{
    	check_function(., "dmy")
    	check_function(., "mutate") %>% {
        	check_arg(., ".data") %>% check_equal(incorrect_msg = "Make sure you're piping `baker_dates` into `mutate()`.", append = FALSE)
        	check_arg(., "last_date_appeared_us") %>% check_equal(eval = FALSE)
        	check_result(.) %>% check_equal(incorrect_msg = "Make sure you're casting the correct variable as a date.", append = FALSE)
        }
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: eb385ec5e3
xp: 30
```

`@instructions`
Add a line to your code to create a new variable called `last_month_us`, with the month of each bakers' last date appeared in the US as a character string such as "Jan."

`@hint`
Remember to use `?month` to read about how to use the `label` argument.

`@sample_code`
```{r}
# Add a line to extract labeled month
baker_dates_cast <- baker_dates %>% 
  mutate(last_date_appeared_us = dmy(last_date_appeared_us),
        ___ = ___())
```

`@solution`
```{r}
# Add a line to extract labeled month
baker_dates_cast <- baker_dates %>% 
  mutate(last_date_appeared_us = dmy(last_date_appeared_us),
         last_month_us = month(last_date_appeared_us, label = TRUE))
```

`@sct`
```{r}
ex() %>% check_correct(
  	check_object(., "baker_dates_cast") %>% check_equal(eq_statement = "equal"),
	{
    check_function(., "dmy")
    check_function(., "month") %>% {
        check_arg(., "x") %>% check_equal(eval=FALSE)
        check_arg(., "label") %>% check_equal()
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Make sure you're piping `baker_dates` into `mutate()`.", append = FALSE)
        check_arg(., "last_date_appeared_us") %>% check_equal(eval = FALSE)
        check_arg(., "last_month_us") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "Make sure you're casting the correct variable as a month.", append = FALSE)
        }
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: f869bd4f76
xp: 40
```

`@instructions`
Use a bar chart to plot the frequencies of bakers by `last_month_us`- in which month have we said goodbye to the most bakers?

`@hint`
`geom_bar()` is the correct geometric object to add to your `ggplot()` call.

`@sample_code`
```{r}
# Add a line to extract labeled month
baker_dates_cast <- baker_dates %>% 
  mutate(last_date_appeared_us = dmy(last_date_appeared_us),
         last_month_us = month(last_date_appeared_us, label = TRUE))
         
# Make bar chart by last month


```

`@solution`
```{r}
# Add a line to extract labeled month
baker_dates_cast <- baker_dates %>% 
  mutate(last_date_appeared_us = dmy(last_date_appeared_us),
         last_month_us = month(last_date_appeared_us, label = TRUE))
         
# Make bar chart by last month
ggplot(baker_dates_cast, aes(x = last_month_us)) + 
    geom_bar()
```

`@sct`
```{r}
ex() %>% check_object("baker_dates") %>% check_equal(eq_statement = "equal", incorrect_msg = "Do not alter `baker_dates`.", append = FALSE)

ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal()
    check_function(., "aes") %>% check_arg(., "x") %>% check_equal(eval = FALSE)
    check_function(., "geom_bar")
    }

ex() %>% check_error()

success_msg("Success! There are more functions like `year` for getting out other date components- you can even extract fiscal quarters or semesters! Look at the documentation to see more ways.")
```

---

## Calculate timespans

```yaml
type: TabExercise
key: 48f61a76b2
lang: r
xp: 100
skills: 1
```

As we saw in the video, the first step to calculating a timespan in `lubridate` is to make an `interval`, then use division to convert the units to what you want (like `weeks(x)` or `months(x)`). The `x` refers to the number of time units to be included in the period. In this exercise, you'll calculate timespans with the `baker_time` data we just made.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/course_7012/datasets/messy_baker_results.csv", 
              destfile = "bakers.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(lubridate)
baker_time <- read_csv("bakers.csv",
                   na = c("not aired in US", "NA"),
                   col_types = cols(age = col_number())) %>% 
  select(series, baker, contains("date"), -first_date_us, -last_date_us) %>% 
  mutate(first_date_appeared_us = dmy(first_date_appeared_us),
         last_date_appeared_us = dmy(last_date_appeared_us))
```

***

```yaml
type: NormalExercise
key: fb09706609
xp: 30
```

`@instructions`
- Create a new `interval` called `time_on_air`, calculated as the timespan between each baker's first and last date that they appeared in the UK.

`@hint`
- The `lubridate` function to use is `interval()` (be sure to order the date variables as first comma last!)

`@sample_code`
```{r}
# Create interval between first and last UK dates


```

`@solution`
```{r}
# Create interval between first and last UK dates
baker_time <- baker_time %>% 
  mutate(time_on_air = interval(first_date_appeared_uk, last_date_appeared_uk))
```

`@sct`
```{r}
ex() %>% 
    check_function("mutate") %>% 
    {
    check_arg(., "time_on_air") %>% 
    check_equal(eval = FALSE)
    
    check_arg(., ".data") %>% 
    check_equal(eval = FALSE)
    }

ex() %>% check_object("baker_time") 
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 2f2954cee3
xp: 30
```

`@instructions`
- Add another line to your `mutate()` call to create another variable called `weeks_on_air`, which converts `time_on_air` to the number of weeks (time unit = 1).

`@hint`
- You'll want to use `weeks(1)` as the unit.

`@sample_code`
```{r}
# Add a line to create weeks on air variable
baker_time <- baker_time  %>% 
  mutate(time_on_air = interval(first_date_appeared_uk, last_date_appeared_uk),
         ___ = ___)
```

`@solution`
```{r}
# Add a line to create weeks on air variable
baker_time <- baker_time  %>% 
  mutate(time_on_air = interval(first_date_appeared_uk, last_date_appeared_uk),
         weeks_on_air = time_on_air / weeks(1))
```

`@sct`
```{r}
ex() %>% check_object("baker_time") 
ex() %>% 
    check_function("mutate") %>% 
    {
    check_arg(., "time_on_air") %>% 
    check_equal(eval = FALSE)
    
    check_arg(., "weeks_on_air") %>%
    check_equal(eval = FALSE)
    
    check_arg(., ".data") %>% 
    check_equal(eval = FALSE)
    }
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: c757890d4a
xp: 40
```

`@instructions`
- Add another line to your `mutate()` call to create another variable called `months_on_air`, which stores the number of months each baker was "on air" in whole numbers.

`@hint`
- You'll want to use the modulo operator `%/%` with `months(1)` as the unit.

`@sample_code`
```{r}
# Add a line to create whole months on air variable
baker_time <- baker_time  %>% 
  mutate(time_on_air = interval(first_date_appeared_uk, last_date_appeared_uk),
         weeks_on_air = time_on_air / weeks(1),
         ___ = ___)
```

`@solution`
```{r}
# Add a line to create whole months on air variable
baker_time <- baker_time  %>% 
  mutate(time_on_air = interval(first_date_appeared_uk, last_date_appeared_uk),
         weeks_on_air = time_on_air / weeks(1),
         months_on_air = time_on_air %/% months(1))
```

`@sct`
```{r}
ex() %>% check_object("baker_time") 
ex() %>% 
    check_function("mutate") %>% 
    {
    check_arg(., "time_on_air") %>% 
    check_equal(eval = FALSE)
    
    check_arg(., "weeks_on_air") %>%
    check_equal(eval = FALSE)
    
    check_arg(., "months_on_air") %>%
    check_equal(eval = FALSE)
    
    check_arg(., ".data") %>% 
    check_equal(eval = FALSE)
    }
ex() %>% check_error()
success_msg("Here again, `lubridate` gives you many options for working with periods from `years` down to `picoseconds`. You may also end up needing to work with durations too, which have similar functions that always start with a `d` from `dyears` to `dpicoseconds`.")
```

---

## Strings

```yaml
type: VideoExercise
key: ba6b093136
lang: r
xp: 50
skills: 1
```

`@projector_key`
10d08245c8635a3e65a4b860e548786b

---

## Wrangle a character variable

```yaml
type: TabExercise
key: 8b0fcf630e
lang: r
xp: 100
skills: 1
```

In this exercise, we'll wrangle the values in the `position_reached` variable, which looks like this right now:

```
bakers %>% 
  count(position_reached)
# A tibble: 8 x 2
  position_reached     n
  <chr>            <int>
1 Runner up            2
2 Runner Up           12
3 Runner-Up            1
4 Third Place          1
5 winner               2
6 Winner               1
7 WINNER               5
8 NA                  71
```

Let's clean these strings up!

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/course_7012/datasets/messy_baker_results.csv", 
              destfile = "bakers.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(stringr)
bakers <- read_csv("bakers.csv") %>% 
  select(-baker_first, -baker_last, -baker_full, -starts_with("e_"), -contains("date"))
```

***

```yaml
type: NormalExercise
key: f9d0019ec0
xp: 30
```

`@instructions`
Convert all the values in the `position_reached` variable to upper case.

`@hint`
Within a mutate, all `stringr` functions take the original variable as the first argument.

`@sample_code`
```{r}
# Convert to upper case
bakers <- bakers %>% 
  ___(___ = ___(___))
```

`@solution`
```{r}
# Convert to lower case
bakers <- bakers %>% 
  mutate(position_reached = str_to_upper(position_reached))
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bakers") %>% check_equal(),
	{
    check_function(., "str_to_upper") %>% check_arg(., "string") %>% check_equal(eval=FALSE)
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal()
        check_arg(., "position_reached") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "There should only be one argument in `mutate()`.")
        }
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 3850566602
xp: 30
```

`@instructions`
Replace all dashes with a single space (`" "`).

`@hint`
You'll want to use the `str_replace()` function.

`@sample_code`
```{r}
# Add another mutate to replace "-" with " "
bakers <- bakers %>% 
  mutate(position_reached = str_to_upper(position_reached),
         ___ = ____)
```

`@solution`
```{r}
# Add another mutate to replace "-" with " "
bakers <- bakers %>% 
  mutate(position_reached = str_to_upper(position_reached),
         position_reached = str_replace(position_reached, "-", " "))
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bakers") %>% check_equal(),
	{
    check_function(., "str_to_upper") %>% check_arg(., "string") %>% check_equal(eval=FALSE)
    check_function(., "str_replace") %>% {
        check_arg(., "string") %>% check_equal(eval=FALSE)
        check_arg(., "pattern") %>% check_equal(eval=FALSE)
        check_arg(., "replacement") %>% check_equal(eval=FALSE)
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal()
        check_arg(., "position_reached") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "There should only be one argument in `mutate()`.")
        }
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 7e5ef77f1d
xp: 40
```

`@instructions`
Replace "THIRD PLACE" with "RUNNER UP", and count your new `position_reached` variable.

`@hint`
You'll want to use the `str_replace()` function again.

`@sample_code`
```{r}
# Add another mutate to replace "THIRD PLACE" with "RUNNER UP"and count
bakers <- bakers %>% 
  mutate(position_reached = str_to_upper(position_reached),
         position_reached = str_replace(position_reached, "-", " "),
         ___ = ___)

# Count rows
bakers %>%

```

`@solution`
```{r}
# Add another mutate to replace "THIRD PLACE" with "RUNNER UP" and count
bakers <- bakers %>% 
  mutate(position_reached = str_to_upper(position_reached),
         position_reached = str_replace(position_reached, "-", " "),
         position_reached = str_replace(position_reached, "THIRD PLACE", "RUNNER UP"))

# Count rows
bakers %>% 
  count(position_reached)
```

`@sct`
```{r}
ex() %>% check_object("bakers") %>% check_equal(incorrect_msg = "Make sure you're not changing the contents of `baker`.", append = FALSE)

ex() %>% check_correct(
	check_output_expr(., "bakers %>% count(position_reached)", missing_msg = "Are you counting the correct variable?"),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(eval=FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "You should only have one argument in `count()`.")
    }
)
ex() %>% check_error()
success_msg("Great work! Each of the 8 series had one winner and two runner-ups, so now this variable is much easier to work with!")
```

---

## Detect a string pattern

```yaml
type: TabExercise
key: 9c88a42fb6
lang: r
xp: 100
skills: 1
```

In this exercise, we'll wrangle each bakers' `occupation` from the `bakers` data, which is currently a character string like this:


```
bakers %>% 
  select(baker, occupation)
# A tibble: 6 x 2
  baker   occupation               
  <chr>   <chr>                    
1 Jason   Civil Engineering Student
2 Simon   Rugby Coach              
3 Enwezor Business consultant      
4 Martha  Student                  
5 Lee     Pastor                   
6 Liam    Student 
```

We'll create a logical variable (`TRUE` or `FALSE`) indicating whether or not each baker is a student.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/course_7012/datasets/messy_baker_results.csv", 
              destfile = "bakers.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(stringr)
bakers <- read_csv("bakers.csv") %>% 
  select(-baker_first, -baker_last, -baker_full, -starts_with("e_"), -contains("date")) %>% 
  mutate(position_reached = str_to_upper(position_reached),
         position_reached = str_replace(position_reached, "-", " "),
         position_reached = str_replace(position_reached, "THIRD PLACE", "RUNNER UP"))
```

***

```yaml
type: NormalExercise
key: e9826b6745
xp: 30
```

`@instructions`
Convert all the values in the `occupation` variable to lower case.

`@hint`
Within a `mutate()`, all `stringr` functions take the original variable as the first argument.

`@sample_code`
```{r}
# Convert to lower case
bakers <- bakers %>%

```

`@solution`
```{r}
# Convert to lower case
bakers <- bakers %>% 
    mutate(occupation = str_to_lower(occupation))
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bakers") %>% check_equal(),
	{
    check_function(., "str_to_lower") %>% check_arg(., "string") %>% check_equal(eval=FALSE)
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal()
        check_arg(., "occupation") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "There should only be one argument in `mutate()`.")
        }
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 23ffa8a492
xp: 30
```

`@instructions`
Create a new variable called `student` that is `TRUE` if the string `"student"` is present in the `occupation` variable, and `FALSE` if it is not.

`@hint`
Remember that the string "student" needs to be in quotes when you use `str_detect()`.

`@sample_code`
```{r}
# Add a line to create new variable called student
bakers <- bakers %>% 
    mutate(occupation = str_to_lower(occupation),
           ___ = ___)
```

`@solution`
```{r}
# Add a line to create new variable called student
bakers <- bakers %>% 
    mutate(occupation = str_to_lower(occupation), 
           student = str_detect(occupation, "student"))
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bakers") %>% check_equal(),
	{
    check_function(., "str_to_lower") %>% check_arg(., "string") %>% check_equal(eval=FALSE)
    check_function(., "str_detect") %>% {
        check_arg(., "string") %>% check_equal(eval=FALSE)
        check_arg(., "pattern") %>% check_equal(eval=FALSE)
        }
    check_function(., "mutate") %>% {
        check_arg(., ".data") %>% check_equal()
        check_arg(., "occupation") %>% check_equal(eval=FALSE)
        check_arg(., "student") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(incorrect_msg = "There should be two arguments in `mutate()`.")
        }
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 20e11c7f34
xp: 40
```

`@instructions`
Filter all students in the `bakers` data frame, and look at their names, occupations, and confirm that they are all students. Did we mis-classify anyone?

`@hint`
It might help to use both `filter()` and `select()` here.

`@sample_code`
```{r}
# Add a line to create new variable called student
bakers <- bakers %>% 
    mutate(occupation = str_to_lower(occupation), 
           student = str_detect(occupation, "student"))

# Find all students and examine occupations
bakers %>%


```

`@solution`
```{r}
# Add a line to create new variable called student
bakers <- bakers %>% 
    mutate(occupation = str_to_lower(occupation), 
           student = str_detect(occupation, "student"))

# Find all students and examine occupations
bakers %>% 
  filter(student == TRUE) %>% 
  select(baker, occupation, student)
```

`@sct`
```{r}
ex() %>% check_object("bakers") %>% check_equal(incorrect_msg = "Don't alter `bakers`.", append = FALSE)

ex() %>% check_correct(
	check_output_expr(., "bakers %>% filter(student == TRUE) %>% select(baker, occupation, student)", missing_msg = "Are you using `filter()` and `select()` on the correct variables?"),
	{
    check_function(., "filter") %>% {
        check_arg(., ".data") %>% check_equal()
        check_result(.) %>% check_equal(incorrect_msg = "Are you using `filter()` only on students?", append = FALSE)
        }
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal()
        check_result(.) %>% check_equal(incorrect_msg = "Are you using `select()` on the correct variables? There should be three.", append = FALSE)
        }
    }
)
ex() %>% check_error()
success_msg("Rav's occupation was student support, but we classified him as a student. This shows a common problem in working with data- it helps to always check your work!")
```

---

## Final thoughts

```yaml
type: VideoExercise
key: 416fa3a980
lang: r
xp: 50
skills: 1
```

`@projector_key`
524585d2791a9ae709a7b9e62e884f71
