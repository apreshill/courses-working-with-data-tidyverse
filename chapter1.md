---
title: 'Explore your data'
description: 'You will start this course by learning how to read data into R. We''ll begin with the readr package, and use it to read in data files organized in rows and columns. In the rest of the chapter, you''ll learn how to explore your data using tools to help you view, summarize, and count values effectively. You''ll see how each of these steps gives you more insights into your data. '
free_preview: true
attachments:
    slides_link: 'https://s3.amazonaws.com/assets.datacamp.com/production/course_6012/slides/chapter1.pdf'
---

## Import your data

```yaml
type: VideoExercise
key: 35cec500c1
lang: r
xp: 50
skills: 1
```

`@projector_key`
5db22793e6d4becbe4b19772996c6c1d

---

## Read a CSV file

```yaml
type: TabExercise
key: 34173742d0
lang: r
xp: 100
```

In this exercise, you'll use `read_csv()` twice. The first time you will only specify the filename, but you'll notice a problem with the imported data. The second time you'll use a new argument called `skip` to fix the problem. Remember to use `?read_csv` to read more about arguments like `skip` and how to use them.

The data you'll work with is from "The Great British Bake-Off." The file `"bakeoff.csv"` contains data for each episode of the show, organized by series and baker.

_This course touches on a lot of concepts you may have forgotten, so if you ever need a quick refresher, download the [Tidyverse Cheat Sheet](https://datacamp-community-prod.s3.amazonaws.com/e63a8f6b-2aa3-4006-89e0-badc294b179c) and keep it handy!_	

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/54e897ce5028a7a0caae2647f5dc3b20ca19b7ba/messy_bakeoff.csv", 
              destfile = "bakeoff.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(ggplot2)
```

***

```yaml
type: NormalExercise
key: 33b44121d6
xp: 25
```

`@instructions`
Load the `readr` package.

`@hint`
You would load the `readr` package with `library(readr)`.

`@sample_code`
```{r}
# Load readr
library(___)
```

`@solution`
```{r}
# Load readr
library(readr)
```

`@sct`
```{r}
ex() %>% check_library("readr")
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: cf085a1dd0
xp: 25
```

`@instructions`
- Use `read_csv()` to read in `"bakeoff.csv"`, and assign it to a new dataset `bakeoff` using the assignment operator (`<-`).

`@hint`
- The only argument to the `read_csv()` function here is the full file name in quotes (including the file extension!).

`@sample_code`
```{r}
# Load readr
library(readr)

# Create bakeoff from "bakeoff.csv"

```

`@solution`
```{r}
# Load readr
library(readr)

# Create bakeoff from "bakeoff.csv"
bakeoff <- read_csv("bakeoff.csv")
```

`@sct`
```{r}
ex() %>% check_library('readr')

ex() %>% check_correct(
  	check_object(., "bakeoff") %>% check_equal(),
	check_function(., "read_csv") %>% check_arg("file") %>% check_equal()
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 2f3d4db414
xp: 25
```

`@instructions`
- Print `bakeoff` to view it. Look at the variable names and the first row of values. Do you notice any problems?

`@hint`
- To print, type `bakeoff` on its own line.

`@sample_code`
```{r}
# Load readr
library(readr)

# Create bakeoff from "bakeoff.csv"
bakeoff <- read_csv("bakeoff.csv")

# Print bakeoff

```

`@solution`
```{r}
# Load readr
library(readr)

# Create bakeoff from "bakeoff.csv"
bakeoff <- read_csv("bakeoff.csv")

# Print bakeoff
bakeoff
```

`@sct`
```{r}
ex() %>% check_library('readr')

ex() %>% check_correct(
  	check_object(., "bakeoff") %>% check_equal(),
	check_function(., "read_csv") %>% check_arg("file") %>% check_equal()
)

ex() %>% check_output_expr("bakeoff") 

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: cf9b8c50f1
xp: 25
```

`@instructions`
- Adapt your code to read in `"bakeoff.csv"` again, but this time, use the `skip` argument to skip the first line before reading the data. Print again to view it. Remember you can use `?read_csv`.

`@hint`
- The default is `skip = 0`, which means to skip zero lines before reading the data. You'll want to set the argument value for `skip` to `1` to skip the first row after the column names.

`@sample_code`
```{r}
# Load readr
library(readr)

# Create bakeoff but skip first row
bakeoff <- read_csv("bakeoff.csv", ___)

# Print bakeoff
bakeoff
```

`@solution`
```{r}
# Load readr
library(readr)

# Create bakeoff but skip first row
bakeoff <- read_csv("bakeoff.csv", skip = 1)

# Print bakeoff
bakeoff
```

`@sct`
```{r}
ex() %>% check_library('readr')

ex() %>% check_correct(
  	check_object(., "bakeoff") %>% check_equal(),
	check_function(., "read_csv") %>% {
		check_arg(., "file") %>% check_equal()
		check_arg(., "skip") %>% check_equal()
	}
)

ex() %>% check_output_expr("bakeoff") 

ex() %>% check_error()

success_msg("Great job! Our original import had 3 problems: we had 1 extra observation, our first row stored the true variable names, and all column types were characters. Adding `skip = 1` worked because the default argument for `col_names` is `TRUE`.")
```

---

## Assign missing values

```yaml
type: TabExercise
key: a39264524f
xp: 100
```

The `read_csv()` function also has an `na` argument, which allows you to specify value(s) that represent missing values in your data. The default values for the `na` argument are `c("", "NA")`, so both are recoded as missing (`NA`) in R. When you read in data, you can add additional values like the string __"UNKNOWN"__ to a vector of missing values using the [`c()`](https://www.rdocumentation.org/packages/base/versions/3.5.0/topics/c) function to combine multiple values into a single vector.

The [`is.na()`](https://www.rdocumentation.org/packages/base/versions/3.5.1/topics/NA) function is also helpful for identifying rows with missing values for a variable.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/7218cffdd84ae28bea74db29f0464ce29b954cc5/messy_bakeoff.csv", 
              destfile = "bakeoff.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
bakeoff <- read_csv("bakeoff.csv", skip = 1)
```

***

```yaml
type: NormalExercise
key: 999e8e80eb
xp: 25
```

`@instructions`
- Load the `dplyr` package.

`@hint`
- You would load the `dplyr` package with `library(dplyr)`.

`@sample_code`
```{r}
# Load dplyr
```

`@solution`
```{r}
# Load dplyr
library(dplyr)
```

`@sct`
```{r}
ex() %>% check_library("dplyr")

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 44aec018cd
xp: 25
```

`@instructions`
- Add a `filter()` line after the pipe (`%>%`) to show only the rows in the `showstopper` variable coded as __"UNKNOWN"__.

`@hint`
- Remember that you use `==` to compare two values- the string __"UNKNOWN"__ should be in all caps and in quotes.

`@sample_code`
```{r}
# Load dplyr
library(dplyr)

# Filter rows where showstopper is UNKNOWN
bakeoff %>%
```

`@solution`
```{r}
# Load dplyr
library(dplyr)

# Filter rows where showstopper is UNKNOWN
bakeoff %>% 
    filter(showstopper == "UNKNOWN")
```

`@sct`
```{r}
ex() %>% check_library("dplyr")

ex() %>% check_correct(
	check_output_expr(., "bakeoff %>% filter(showstopper == \"UNKNOWN\")", missing_msg = "Did you properly filter for \"UNKNOWN\" in `showstopper`?"),
	check_function(., "filter") %>% {
    	check_arg(., ".data") %>% check_equal()
    	check_result(.) %>% check_equal(incorrect_msg = "Did you properly filter for \"UNKNOWN\" in `showstopper`?", append = FALSE)
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: b7534a01c9
xp: 25
```

`@instructions`
- Adapt your code for creating `bakeoff` to add the `na` argument. Define `NA` values as a character string vector that includes the two default values *plus* __"UNKNOWN"__.

`@hint`
- Each of the 3 values inside `c()` need to be in quotes- remember to use `?read_csv` to see how to use arguments like `na`.

`@sample_code`
```{r}
# Load dplyr
library(dplyr)

# Filter rows where showstopper is UNKNOWN
bakeoff %>% 
    filter(showstopper == "UNKNOWN")

# Edit to add list of missing values
bakeoff <- read_csv("bakeoff.csv", 
                    skip = 1,
                    na = c(___, ___, ___))
```

`@solution`
```{r}
# Load dplyr
library(dplyr)

# Filter rows where showstopper is UNKNOWN 
bakeoff %>% 
    filter(showstopper == "UNKNOWN")

# Edit to add list of missing values
bakeoff <- read_csv("bakeoff.csv", 
                    skip = 1,
                    na = c("", "NA", "UNKNOWN"))
```

`@sct`
```{r}
ex() %>% check_library("dplyr")

ex() %>% check_function("filter") %>% {
    check_arg(., ".data") %>% check_equal()
    check_result(.) %>% check_equal(incorrect_msg = "Did you properly filter for \"UNKNOWN\" in `showstopper`, adding the `NA` argument?", append = FALSE)
    }

ex() %>% check_function("read_csv") %>% {
    check_arg(., "file") %>% check_equal()
    check_arg(., "skip") %>% check_equal() 
    check_arg(., "na") %>% check_equal()
    }

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 9e066abaef
xp: 25
```

`@instructions`
- Add a `filter()` line after the pipe (`%>%`) now to show rows where `showstopper` is missing, using the `is.na()` function.

`@hint`
- You'll need to use `is.na(showstopper)` inside your call to `filter`.

`@sample_code`
```{r}
# Load dplyr
library(dplyr)

# Filter rows where showstopper is UNKNOWN 
bakeoff %>% 
    filter(showstopper == "UNKNOWN")

# Edit to add list of missing values
bakeoff <- read_csv("bakeoff.csv", skip = 1,
                    na = c("", "NA", "UNKNOWN"))

# Filter rows where showstopper is NA 
bakeoff %>%
```

`@solution`
```{r}
# Load dplyr
library(dplyr)

# Filter rows where showstopper is UNKNOWN 
bakeoff %>% 
    filter(showstopper == "UNKNOWN")

# Edit to add list of missing values
bakeoff <- read_csv("bakeoff.csv", skip = 1,
                    na = c("", "NA", "UNKNOWN"))

# Filter rows where showstopper is NA 
bakeoff %>% 
    filter(is.na(showstopper))
```

`@sct`
```{r}
ex() %>% check_library("dplyr")

ex() %>% check_function("filter") %>% {
    check_arg(., ".data") %>% check_equal()
    check_result(.) %>% check_equal(incorrect_msg = "Did you properly filter for \"UNKNOWN\" in `showstopper`, adding the `NA` argument?", append = FALSE)
    }

ex() %>% check_object("bakeoff") %>% check_equal()

ex() %>% check_function("read_csv") %>% {
    check_arg(., "file") %>% check_equal()
    check_arg(., "skip") %>% check_equal() 
    check_arg(., "na") %>% check_equal()
    }

ex() %>% check_correct(
	check_output_expr(., "bakeoff %>% filter(is.na(showstopper))", missing_msg = "Use `is.na()` to check for NAs when filtering showstopper here."),
	check_function(., "filter") %>% {
    	check_arg(., ".data") %>% check_equal()
    	check_result(.) %>% check_equal()
    }
)

ex() %>% check_error()
success_msg("Nice! We only had 4 missing values for the showstopper variable to start, but now all 21 are present and accounted for.")
```

---

## Know your data

```yaml
type: VideoExercise
key: 416aa0b520
lang: r
xp: 50
skills: 1
```

`@projector_key`
4cfd91fcbc986579858c55482728e057

---

## Arrange and glimpse

```yaml
type: MultipleChoiceExercise
key: 5c81b9c283
lang: r
xp: 50
skills: 1
```

From here, if we don't ask you to load a package, you can assume it's already loaded.

You can combine [`glimpse()`](http://tibble.tidyverse.org/reference/glimpse.html) with other functions in a sequence using the pipe (`%>%`) operator. For example, you can use other `dplyr` functions like `arrange` first, then use `glimpse` by adding a line after the final pipe (`%>%`):

```{r}
bakers_mini %>% 
  arrange(age) %>% 
  glimpse() # no argument needed here
```

Take a glimpse of the `bakeoff` data we imported in the first set of exercises. On which date did the first episode of the show air in the US?

`@possible_answers`
- `2010-08-17`
- `2014-12-28`
- `2013-08-20`

`@hint`
You'll want to arrange the data by either `us_season` or `us_airdate`. Note that when using `glimpse()` at the end of a sequence, the function won't take any arguments since you are using the `%>%` operator.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/bbe39331b108bf00e3ad1d213e0d8fd8b7cc88ee/bakeoff.csv", 
              destfile = "bakeoff.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
bakeoff <- read_csv("bakeoff.csv", 
                    na = c("", "NA", "UNKNOWN", "N/A"),
                    col_types = cols(
                      result = col_factor(levels = NULL)
                    ))
```

`@sct`
```{r}
msg1 = "Incorrect, this is the date the first episode of the first series aired in the UK."
msg2 = "Correct! The first episode of the first US season aired on December 28, 2014."
msg3 = "Incorrect, this is the UK air date of the first episode of the first US season."

ex() %>% check_mc(2, feedback_msgs = c(msg1, msg2, msg3))
```

---

## Summarize your data

```yaml
type: TabExercise
key: 2443631391
lang: r
xp: 100
```

You can combine [`skim()`](https://www.rdocumentation.org/packages/skimr/versions/1.0.2/topics/skim) with other functions in a sequence using the pipe (`%>%`) operator. For example, you could use other `dplyr` functions like `group_by` first, then use `skim()` by adding a line after the final pipe. 

```{r}
bakers_mini %>% 
  group_by(series) %>% 
  skim() # no argument needed here
```

This will produce summary statistics for each `series`. Let's practice this!

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/bbe39331b108bf00e3ad1d213e0d8fd8b7cc88ee/bakeoff.csv", 
              destfile = "bakeoff.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(skimr)
bakeoff <- read_csv("bakeoff.csv", 
                    na = c("", "NA", "UNKNOWN", "N/A"),
                    col_types = cols(
                      result = col_factor(levels = NULL)
                    ))
```

***

```yaml
type: NormalExercise
key: 8e95f892fb
xp: 30
```

`@instructions`
- Load the `skimr` package.

`@hint`
- You would load the `skimr` package with `library(skimr)`.

`@sample_code`
```{r}
# Load skimr
```

`@solution`
```{r}
# Load skimr
library(skimr)
```

`@sct`
```{r}
ex() %>% check_library("skimr")

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 14e1c00936
xp: 30
```

`@instructions`
- First `filter()` to extract only those observations where `us_season` is not missing, then pipe that to `skim()`. Recall that `!` means "not", so `!is.na()` creates the right logical condition.

`@hint`
- Don't forget the parentheses in `skim()`! The `skim()` function won't take any arguments at the end of a sequence like this, since you are using the `%>%` operator.

`@sample_code`
```{r}
# Load skimr
library(skimr)

# Filter and skim
bakeoff %>%
  ___(!is.na(___)) %>% 
  ___
```

`@solution`
```{r}
# Load skimr
library(skimr)

# Filter and skim
bakeoff %>% 
  filter(!is.na(us_season)) %>% 
  skim()
```

`@sct`
```{r}
ex() %>% check_library("skimr")

ex() %>% check_correct(
	check_output_expr(., "bakeoff %>% filter(!is.na(us_season)) %>% skim()", missing_msg = "Remember to `skim()` the bakeoff data for non-missing `us_season` observations."),
    {
        check_function(., "filter") %>% {
            check_arg(., ".data") %>% check_equal(incorrect_msg = "Make sure you're piping the `bakeoff` dataset into `filter()`.", append = FALSE)
            check_result(.) %>% check_equal(incorrect_msg = "Did you properly specify for everything besides NA values?", append = FALSE)
        }
        check_function(., "skim") %>% check_arg(".data") %>% check_equal()
    }
)
```

***

```yaml
type: NormalExercise
key: b0d92f49cb
xp: 40
```

`@instructions`
- Add a line above the final `skim()` to group by `us_season` first, before getting the summary statistics.

`@hint`
- Keep `filter` first, then `group_by(variable)`, before piping to `skim()`- the `skim()` function won't take any arguments here since you are using the `%>%` operator.

`@sample_code`
```{r}
# Load skimr
library(skimr)

# Edit to filter, group by, and skim
bakeoff %>% 
  filter(!is.na(us_season)) %>% 
  ___  %>% 
  skim()
```

`@solution`
```{r}
# Load skimr
library(skimr)

# Edit to filter, group by, and skim
bakeoff %>% 
  filter(!is.na(us_season)) %>% 
  group_by(us_season) %>% 
  skim()
```

`@sct`
```{r}
ex() %>% check_library("skimr")

ex() %>% check_correct(
	check_output_expr(., "bakeoff %>% filter(!is.na(us_season)) %>% group_by(us_season) %>% skim()", missing_msg = "Did you adapt your code to skim for observations where `us_season` is not missing?"),
    {
        check_function(., "filter") %>% {
            check_arg(., ".data") %>% check_equal(incorrect_msg = "Make sure you're piping the `bakeoff` dataset into `filter()`.", append = FALSE)
            check_result(.) %>% check_equal(incorrect_msg = "Did you properly specify for everything besides NA values?", append = FALSE)
        }
        check_function(., "group_by") %>% {
            check_arg(., ".data") %>% check_equal(incorrect_msg = "Make sure you're piping the filtered data into `group_by().", append = FALSE)
            check_result(.) %>% check_equal("identical", incorrect_msg = "Did you `group_by()` us_season?", append = FALSE)
        }
        check_function(., "skim") %>% check_arg(., ".data") %>% check_equal(incorrect_msg = "Did you `group_by()` us_season?", append = FALSE)
    }
)

success_msg("Nice piping & skimming! Producing quick grouped summaries with the `skim` function is a real time (and sanity) saver.")
```

---

## Know your variable types

```yaml
type: MultipleChoiceExercise
key: 6455237604
lang: r
xp: 50
skills: 1
```

How many variables of each type do we have in the `bakeoff` data? You may use any of the data science tools we've learned for getting to know your data (`dplyr` and `skimr` are loaded for you). You may also want to try piping a skimmed object to `summary()`, also from the `skimr` package:

```{r}
tibble_name %>% 
  skim() %>%  # no argument needed here
  summary() # no argument needed here
```

`@possible_answers`
- Date: 2, character: 3, factor: 1, integer: 4
- Date: 3, character: 3, factor: 1, integer: 3
- Date: 2, character: 3, factor: 1, integer: 3

`@hint`
- You could use `bakeoff %>% skim() %>% summary()` to see a compact list.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/bbe39331b108bf00e3ad1d213e0d8fd8b7cc88ee/bakeoff.csv", 
              destfile = "bakeoff.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(skimr)
bakeoff <- read_csv("bakeoff.csv", 
                    na = c("", "NA", "UNKNOWN", "N/A"),
                    col_types = cols(
                      result = col_factor(levels = NULL)
                    ))
```

`@sct`
```{r}
msg1 = "Correct!"
msg2 = "Incorrect, there are only 2 dates and there is one more numeric variable."
msg3 = "Incorrect, there is one more numeric variable."

ex() %>% check_mc(1, feedback_msgs = c(msg1, msg2, msg3))
```

---

## Count with your data

```yaml
type: VideoExercise
key: f628fd75bd
lang: r
xp: 50
skills: 1
```

`@projector_key`
598f9582872d0d3ee4c733a4a2faa99a

---

## Distinct and count

```yaml
type: BulletExercise
key: d044261e62
lang: r
xp: 100
skills: 1
```

In every episode of "The Great British Bake-Off", bakers complete 3 challenges and the show's judges award the title "Star Baker" to the baker who excelled in that week's challenges (with the exception of the finale). Each baker's result for every episode is stored in `bakeoff`- `result` is a character variable, and the value "SB" stands for star baker.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/bbe39331b108bf00e3ad1d213e0d8fd8b7cc88ee/bakeoff.csv", 
              destfile = "bakeoff.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(skimr)
bakeoff <- read_csv("bakeoff.csv", 
                    na = c("", "NA", "UNKNOWN", "N/A"),
                    col_types = cols(
                      result = col_factor(levels = NULL)
                    ))
```

***

```yaml
type: NormalExercise
key: 27a6414c2e
xp: 30
```

`@instructions`
View the distinct values for the `result` variable. Is "SB" one of them?

`@hint`
You don't need the assignment operator here- you can just add `distinct(variable)` after the pipe (`%>%`) on a new line and read the output that prints to the screen.

`@sample_code`
```{r}
# View distinct results
bakeoff %>%
```

`@solution`
```{r}
# View distinct results
bakeoff %>% 
  distinct(result)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_output_expr(., "bakeoff %>% distinct(result)", missing_msg = "Did you look at the `distinct()` values for `result`?"),
	check_function(., "distinct") %>% {
      check_arg(., ".data") %>% check_equal(incorrect_msg = "Make sure you're piping bakeoff into `distinct()`.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Is your argument inside `distinct()`, `result`?", append = FALSE)
    }
)

ex() %>% check_error()

success_msg("Yes, there are six different results. But how frequent is each result?")
```

***

```yaml
type: NormalExercise
key: fd08b30287
xp: 30
```

`@instructions`
Adapt your code to `count` rows by `result` instead. How many star bakers are there?

`@hint`
You'll want to replace `distinct` with `count`- the usage/arguments stay the same!

`@sample_code`
```{r}
# Count rows for each result
bakeoff %>% 
  distinct(result)
```

`@solution`
```{r}
# Count rows for each result
bakeoff %>% 
  count(result)
```

`@sct`
```{r}
ex() %>% check_correct(
 	check_output_expr(., "bakeoff %>% count(result)", missing_msg = "Remember to `count()` by result."),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping bakeoff into `count()`.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Is your argument inside `count()`, `result`?", append = FALSE)
    }
)

ex() %>% check_error()

success_msg("Awesome counting! There are 61 star bakers.")
```

***

```yaml
type: NormalExercise
key: 8a0356d811
xp: 40
```

`@instructions`
Adapt your code to `count` by a logical condition instead: if the `result` is equal to `"SB"`.

`@hint`
A logical must evaluate to `TRUE` or `FALSE`. Here, the argument to the `count()` function should be `result == "SB"`.

`@sample_code`
```{r}
# Count whether or not star baker
bakeoff %>% 
  count(result)
```

`@solution`
```{r}
# Count whether or not star baker
bakeoff %>% 
  count(result == "SB")
```

`@sct`
```{r}
ex() %>% check_correct(
	check_output_expr(., "bakeoff %>% count(result == \"SB\")", missing_msg = "`count()` where `result` is star baker."),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping bakeoff into `count()`.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Remember to set `result` equal to `SB`.", append = FALSE)
    }
)

ex() %>% check_error()

success_msg("Nice logical detective work! There are 488 results that are not star baker.")
```

---

## Count episodes

```yaml
type: TabExercise
key: 25984c43f5
lang: r
xp: 100
```

In the video, here's how we used `count()` back-to-back to roll up a level of detail across `series`:

```{r}
bakers %>% 
  count(aired_us, series) %>% 
  count(aired_us)
```
```
# A tibble: 2 x 2
  aired_us    nn
  <lgl>    <int>
1 FALSE        4
2 TRUE         4
```

Recall that `bakeoff` includes each baker's result for every episode. Let's practice counting the number of episodes per series.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/bbe39331b108bf00e3ad1d213e0d8fd8b7cc88ee/bakeoff.csv", 
              destfile = "bakeoff.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
bakeoff <- read_csv("bakeoff.csv", 
                    na = c("", "NA", "UNKNOWN", "N/A"),
                    col_types = cols(
                      result = col_factor(levels = NULL)
                    ))
```

***

```yaml
type: NormalExercise
key: 5446ef8229
xp: 50
```

`@instructions`
Use `?parse_date` to find the right format to parse the date "17 August 2010".

`@hint`
The "suspected date" goes in quotes first. Scroll down to the "format specification" section in `?parse_date`, where you should see rules like: "Year: "%Y" (4 digits). "%y" (2 digits)." Find the right format for each of the 3 date elements (day, month, and year).

`@sample_code`
```{r}
# Find format to parse uk_airdate 
parse_date(___, format = "__ __ __")
```

`@solution`
```{r}
# Find format to parse uk_airdate 
parse_date("17 August 2010", format = "%d %B %Y")
```

`@sct`
```{r}
ex() %>% check_correct(
	check_output_expr(., "parse_date(\"17 August 2010\", format = \"%d %B %Y\")", missing_msg = "Make sure you're using the correct format to parse `uk_airdate`."),
	check_function(., "parse_date") %>% {
    	check_arg(., "x") %>% check_equal()
    	check_arg(., "format") %>% check_equal()
    	check_result(.) %>% check_equal()
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 8833457b2b
xp: 50
```

`@instructions`
- Adapt your code to add a second `count()` by series again to count the total number of episodes per series.

`@hint`
- You still don't need the assignment operator- you can just add `count(var)` after the pipe (`%>%`) on a new line and read the output that prints to the screen.

`@sample_code`
```{r}
# Add second count by series
bakeoff %>% 
  count(series, episode) %>%
```

`@solution`
```{r}
# Add second count by series
bakeoff %>% 
  count(series, episode) %>% 
  count(series)
```

`@sct`
```{r}
ex() %>% check_function("count", index = 1) %>% {
    check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping bakeoff into `count()`.", append = FALSE)
    check_result(.) %>% check_equal(incorrect_msg = "Your two arguments inside `count()` should be series and episode, separated by a comma.", append = FALSE)
    }

ex() %>% check_function("count", index = 2) %>% {
    check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping the output of first `count()` into the second one.", append = FALSE)
    check_result(.) %>% check_equal(incorrect_msg = "Did you use `series` as the only argument inside the second call to `count()`?", append = FALSE)
    }

ex() %>% check_error()

success_msg("Great work- you can see that by series 3 we were treated to 10 episodes with each new series!")
```

---

## Count bakers

```yaml
type: TabExercise
key: 2149078e02
lang: r
xp: 100
```

In the last exercise, here's how you used `count()` back-to-back to count episodes by series: 

```{r}
bakeoff %>% 
  count(series, episode) %>% 
  count(series)
```
```
# A tibble: 8 x 2
  series    nn
   <int> <int>
1      1     6
2      2     8
3      3    10
4      4    10
5      5    10
6      6    10
7      7    10
8      8    10
```

Now, using the same `bakeoff` data, you'll practice that again, focusing on counting bakers.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/bbe39331b108bf00e3ad1d213e0d8fd8b7cc88ee/bakeoff.csv", 
              destfile = "bakeoff.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
bakeoff <- read_csv("bakeoff.csv", 
                    na = c("", "NA", "UNKNOWN", "N/A"),
                    col_types = cols(
                      result = col_factor(levels = NULL)
                    ))
```

***

```yaml
type: NormalExercise
key: cf24e59d93
xp: 30
```

`@instructions`
- Create a new tibble by `count`ing rows by `series` and `baker`. Print your new counted tibble.

`@hint`
- Create a new counted tibble by adding a `count(var1, var2)` line after the pipe. Type `bakers_by_series` on its own line to print it.

`@sample_code`
```{r}
# Count the number of rows by series and baker
bakers_by_series <- bakeoff %>%


# Print to view
```

`@solution`
```{r}
# Count the number of rows by series and baker
bakers_by_series <- bakeoff %>% 
  count(series, baker)
  
# Print to view
bakers_by_series
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bakers_by_series") %>% check_equal(),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping bakeoff into `count()`.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Did you use `series, baker` as your arguments to `count()`?", append = FALSE)
    }
)

ex() %>% check_output_expr("bakers_by_series", missing_msg = "Did you print `bakers_by_series`?")

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 6f00cf28e0
xp: 30
```

`@instructions`
- Add a line to `count` the already counted tibble by `series` again. Which series had the most bakers? Which had the least?

`@hint`
- You don't need the assignment operator here- you can just add `count(series)` after the pipe (`%>%`) on a new line and read the output that prints to the screen.

`@sample_code`
```{r}
# Count the number of rows by series and baker
bakers_by_series <- bakeoff %>% 
  count(series, baker)
  
# Print to view
bakers_by_series
  
# Count again by series
bakers_by_series %>%
```

`@solution`
```{r}
# Count the number of rows by series and baker
bakers_by_series <- bakeoff %>% 
  count(series, baker)
  
# Print to view
bakers_by_series
  
# Count again by series
bakers_by_series %>% 
  count(series)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bakers_by_series") %>% check_equal(),
	check_function(., "count", index = 1) %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping bakeoff into `count()`.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Did you use `series, baker` as your arguments to `count()`?", append = FALSE)
    }
)

ex() %>% check_output_expr("bakers_by_series", missing_msg = "Did you print `bakers_by_series`?")

ex() %>% check_correct(
	check_output_expr(., "bakers_by_series %>% count(series)", missing_msg = "Did you call `count()` again to count `bakers_by_series` by `series`?"),
	check_function(., "count", index = 2) %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping bakeoff into `count()`.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Did you use `series` as your only argument to the second call of `count()`?", append = FALSE)
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: d41dc6c8f3
xp: 40
```

`@instructions`
- Now `count` the same tibble by `baker` again and use `sort = TRUE`. What's the most common baker name?

`@hint`
- You still don't need the assignment operator- you can just add `count(var, sort = TRUE)` after the pipe (`%>%`) on a new line and read the output that prints to the screen.

`@sample_code`
```{r}
# Count the number of rows by series and baker
bakers_by_series <- bakeoff %>% 
  count(series, baker)
  
# Print to view
bakers_by_series
  
# Count again by series
bakers_by_series %>% 
  count(series)
  
# Count again by baker
bakers_by_series %>%
```

`@solution`
```{r}
# Count the number of rows by series and baker
bakers_by_series <- bakeoff %>% 
  count(series, baker)
  
# Print to view
bakers_by_series
  
# Count again by series
bakers_by_series %>% 
  count(series)
  
# Count again by baker
bakers_by_series %>%
  count(baker, sort = TRUE)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "bakers_by_series") %>% check_equal(),
	check_function(., "count", index = 1) %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping bakeoff into `count()`.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Did you use `series, baker` as your arguments to `count()`?", append = FALSE)
    }
)

ex() %>% check_output_expr("bakers_by_series", missing_msg = "Did you print `bakers_by_series`?")

ex() %>% check_correct(
	check_output_expr(., "bakers_by_series %>% count(series)", missing_msg = "Did you call `count()` again to count `bakers_by_series` by `series`?"),
	check_function(., "count", index = 2) %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping bakeoff into `count()`.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Did you use `series` as your only argument to the second call of `count()`?", append = FALSE)
    }
)

ex() %>% check_correct(
	check_output_expr(., "bakers_by_series %>% count(baker, sort = TRUE)", missing_msg = "Did you call `count()` a third time to count `bakers_by_series` by `baker` while also enabling the `sort` option?"),
	check_function(., "count", index = 3) %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping bakeoff into `count()`.", append = FALSE)
      	check_arg(., "sort") %>% check_equal()
    	check_result(.) %>% check_equal(incorrect_msg = "Did you use `series` as your only argument to the second call of `count()`?", append = FALSE)
    }
)

ex() %>% check_error()

success_msg("Good work- Series 4 had the most bakers with 13 total (a baker's dozen!). Kate is the most popular name- Kates have appeared in 3 different series of the TV show.")
```

---

## Plot counts

```yaml
type: NormalExercise
key: 2e052c9e9b
lang: r
xp: 100
skills: 1
```

You can learn a lot about your data by counting, but sometimes you can learn even more by plotting counts. This is especially true when you have lots of things to count! With eight series, 74 episodes, and 95 bakers, a plot can be more helpful than a table of numbers. We'll use `ggplot2` (already loaded for you) to visualize the number of bakers across episodes for each series.

`@instructions`
Make a bar chart using `geom_bar` to plot the number of bakers per `episode` along the x-axis. Use `facet_wrap` by `series`.

`@hint`
Map the x-axis to `episode` using `aes(episode)` (or equivalentally `aes(x = episode)`), and include `facet_wrap(~series)`. No variable should be mapped to the y-axis for this plot.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/aa9855972db8d5a2bcdc7635a81f947b491d38b2/messy_challenge_results.csv", 
              destfile = "bakeoff.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(tidyr)
library(ggplot2)
bakeoff <- read_csv("bakeoff.csv", 
                    skip = 1,
                    na = c("", "NA", "UNKNOWN", "N/A"),
                    col_types = cols(
                      technical = col_number(),
                      uk_airdate = col_date(format = "%d %B %Y"),
                      us_airdate = col_date(format = "%d-%b-%y"),
                      us_season = col_factor(levels = NULL)
                    )) %>% 
    separate(episode, into = c("series", "episode"), sep = "-", convert = TRUE)  %>%
    mutate(episode = as.factor(episode)) %>% 
    drop_na(result) %>% 
    select(-uk_premiere) %>% 
    mutate(result = case_when(
           result == "Runner up" ~ "RUNNER UP",
           result == "Runner Up" ~ "RUNNER UP",
           result == "Runner-Up" ~ "RUNNER UP",
           result == "Third Place" ~ "RUNNER UP",
           TRUE ~ as.character(result)
           ))
```

`@sample_code`
```{r}
ggplot(___, aes(___)) + 
    ___ + 
    facet_wrap(~___)
```

`@solution`
```{r}
ggplot(bakeoff, aes(episode)) + 
    geom_bar() + 
    facet_wrap(~series)
```

`@sct`
```{r}
ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal()
    check_function(., "aes") %>% check_arg(., "x") %>% check_equal(eval = FALSE)
    check_function(., "geom_bar")
    check_function(., "facet_wrap") %>% check_arg(., "facets") %>% check_equal(eval = FALSE)
    }

ex() %>% check_error()

success_msg("You made it through the first chapter! We have a lot of work left to do with The Great British Bake Off data, but notice how this plot is a great sanity check: the number of bakers tends to go down with every episode (never up!), with a few exceptions where no one was eliminated. Also, you can see that there are fewer episodes in the first two series. Finally, you can see that there are always three bakers who make it to the series finale.")
```
