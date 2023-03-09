---
title: 'Tame your data'
description: 'In this chapter, you will learn some basics of data taming, like how to tame your variable types, names, and values.'
attachments:
    slides_link: 'https://s3.amazonaws.com/assets.datacamp.com/production/course_6012/slides/chapter2.pdf'
---

## Cast column types

```yaml
type: VideoExercise
key: 47a144a957
lang: r
xp: 50
skills: 1
```

`@projector_key`
86d6aef57ef874954a2d2f82892a7915

---

## Cast a character to a number

```yaml
type: BulletExercise
key: 9640a5ad62
lang: r
xp: 100
```

In the video, we saw a good workflow for parsing columns using `readr`:

- Use `parse_number()` to practice, then
- Use `col_number()` to cast. 

But sometimes you'll need to start with casting, then diagnose parsing problems using a new `readr` function called [`problems()`](https://www.rdocumentation.org/packages/readr/versions/1.1.1/topics/problems). 

Let's practice this with `"desserts.csv"`, which includes data about the judges' ranks for each baker in the `technical` challenge, for every episode.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/4417ffb42e1882b1102c1ed64a46121406214e56/desserts.csv", 
              destfile = "desserts.csv", quiet = TRUE)
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
key: 30a415e65a
xp: 30
```

`@instructions`
Cast the `technical` variable as a number. You will get a parsing error! Read the red text carefully.

`@hint`
Within the `cols()` argument, the bare variable name goes on the left of the equal sign. Don't forget the open/close parentheses after the column type on the right of the equal sign- no arguments are needed inside the parentheses.

`@sample_code`
```{r}
# Try to cast technical as a number
desserts <- read_csv("desserts.csv",
                      col_types = cols(
                        ___ = ___
                     ))
```

`@solution`
```{r}
# Try to cast technical as a number
desserts <- read_csv("desserts.csv",
                      col_types = cols(
                        technical = col_number()
                     ))
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "desserts") %>% check_equal(),
	{
    check_function(., "read_csv") %>% {
        check_arg(., "file") %>% check_equal()
        check_function(., "cols") %>% check_arg(., "technical") %>% check_equal(incorrect_msg = "Did you use `col_number()` properly?", append = FALSE)
      }
	}
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 3b5a180fa3
xp: 30
```

`@instructions`
Pass the `desserts` tibble to the `problems()` function- which value could not be parsed (hint: `actual`)?

`@hint`
The argument to the `problems()` function is the name of your problematic data frame: `problems(tibble_name_here)`. Remember you can use `?problems` to get help.

`@sample_code`
```{r}
# Try to cast technical as a number
desserts <- read_csv("desserts.csv",
                      col_types = cols(
                        technical = col_number())
                     )

# View parsing problems

```

`@solution`
```{r}
# Try to cast technical as a number
desserts <- read_csv("desserts.csv",
                      col_types = cols(
                        technical = col_number())
                     )

# View parsing problems
problems(desserts)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "desserts") %>% check_equal(),
	{
    check_function(., "read_csv") %>% {
        check_arg(., "file") %>% check_equal()
        check_function(., "cols") %>% check_arg(., "technical") %>% check_equal(incorrect_msg = "Did you use `col_number()` properly?", append = FALSE)
      }
	}
)

ex() %>% check_function("problems") %>% check_arg(., "x") %>% check_equal()

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: c87f585c34
xp: 40
```

`@instructions`
Adapt your code to add the value that `read_csv()` had trouble parsing to the character vector of missing values.

`@hint`
The argument name is `na`, and your list of missing values should now include `"N/A"`.

`@sample_code`
```{r}
# Edit code to fix the parsing error 
desserts <- read_csv("desserts.csv",
                      col_types = cols(
                        technical = col_number()),
                        ___ = c("", "NA", ___) 
                     )

# View parsing problems
problems(desserts)
```

`@solution`
```{r}
# Edit code to fix the parsing error 
desserts <- read_csv("desserts.csv",
                      col_types = cols(
                        technical = col_number()),
                        na = c("", "NA", "N/A")
                     )

# View parsing problems
problems(desserts)
```

`@sct`
```{r}
ex() %>% check_function(., "read_csv") %>% {
    check_arg(., "file") %>% check_equal()
    check_arg(., "col_types")
  	check_arg(., "na") %>% check_equal()
    check_function(., "cols") %>% 
    	check_arg(., "technical") %>% check_equal(incorrect_msg = "Did you use `col_number()` properly?", append = FALSE)
}

ex() %>% check_object("desserts") %>% check_equal()

ex() %>% check_function("problems") %>% check_arg(., "x") %>% check_equal()

ex() %>% check_error()

success_msg("Great work dealing with a tricky parsing problem!")
```

---

## Cast a character to a date

```yaml
type: TabExercise
key: 59fc96f4d1
lang: r
xp: 100
```

As we saw in the video, a good workflow for parsing dates using `readr` is to, for example:

- Use `parse_date("2012-14-08", format = "%Y-%d-%m")` first, then
- Use `col_date(format = "%Y-%d-%m")` within a `col_types()` argument to `read_csv()`. 

In `"desserts.csv"`, the variable `uk_airdate` is formatted like "17 August 2010". Let's parse, then cast this variable!

Later on in Chapter 4, we'll showcase some functions from the `lubridate` package to help you extract data from dates, once they are cast properly.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/4417ffb42e1882b1102c1ed64a46121406214e56/desserts.csv", 
              destfile = "desserts.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
desserts <- read_csv("desserts.csv",
                    na = c("", "NA", "N/A"),
                    col_types = cols(
                      technical = col_number()
                    ))
```

***

```yaml
type: NormalExercise
key: 5446ef8229
xp: 30
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
test_correct(ex() %>% check_output_expr("parse_date(\"17 August 2010\", format = \"%d %B %Y\")", missing_msg = "Make sure you're using the correct format to parse `uk_airdate`."),
ex() %>% check_function("parse_date") %>% {
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
key: 82a0098606
xp: 30
```

`@instructions`
Edit your code for reading in `"desserts.csv"` from the previous exercise to cast `uk_airdate` as a date.

`@hint`
Remember when you cast a date, you have to specify the `format` argument. The argument value for `format` should be in quotes.

`@sample_code`
```{r}
# Find format to parse uk_airdate 
parse_date("17 August 2010", format = "%d %B %Y")

# Edit to cast uk_airdate
desserts <- read_csv("desserts.csv", 
                     na = c("", "NA", "N/A"),
                     col_types = cols(
                       technical = col_number(),
                       ___ = ___
                     ))
```

`@solution`
```{r}
# Find format to parse uk_airdate 
parse_date("17 August 2010", format = "%d %B %Y")

# Edit to cast uk_airdate
desserts <- read_csv("desserts.csv", 
                     na = c("", "NA", "N/A"),
                     col_types = cols(
                       technical = col_number(),
                       uk_airdate = col_date(format = "%d %B %Y")
                     ))
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

ex() %>% check_function("read_csv") %>% {
    check_arg(., "file") %>% check_equal()
    check_arg(., "na") %>% check_equal()
    check_arg(., "col_types")
    check_function(., "cols") %>% {
        check_arg(., "technical") %>% check_equal(incorrect_msg = "Did you use `col_number()` properly?", append = FALSE)
        check_arg(., "uk_airdate")
        check_function(., "col_date") %>% {
            check_arg(., "format") %>% check_equal(incorrect_msg = "Did you specify the format in date, month, and then year?", append = FALSE)
            check_result(.) %>% check_equal(incorrect_msg = "Make sure you used the correct argument in `col_date`.", append = FALSE)
            }
        }
    }

ex() %>% check_object("desserts") %>% check_equal()

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 061408efdf
xp: 40
```

`@instructions`
Sort `desserts` in descending order by `uk_airdate`- when did the last episode air in the UK?

`@hint`
You use `arrange()` from `dplyr`; use `desc(variable)` inside the `arrange()` to sort in descending order.

`@sample_code`
```{r}
# Find format to parse uk_airdate 
parse_date("17 August 2010", format = "%d %B %Y")

# Edit to cast uk_airdate
desserts <- read_csv("desserts.csv", 
                     na = c("", "NA", "N/A"),
                     col_types = cols(
                       technical = col_number(),
                       uk_airdate = col_date(format = "%d %B %Y")
                     ))

# Print by descending uk_airdate
```

`@solution`
```{r}
# Find format to parse uk_airdate 
parse_date("17 August 2010", format = "%d %B %Y")

# Edit to cast uk_airdate
desserts <- read_csv("desserts.csv", 
                     na = c("", "NA", "N/A"),
                     col_types = cols(
                       technical = col_number(),
                       uk_airdate = col_date(format = "%d %B %Y")
                     ))

# Print by descending uk_airdate
desserts %>% 
    arrange(desc(uk_airdate))
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


ex() %>% check_function("read_csv") %>% {
    check_arg(., "file") %>% check_equal()
    check_arg(., "na") %>% check_equal()
    check_arg(., "col_types")
    check_function(., "cols") %>% {
        check_arg(., "technical") %>% check_equal(incorrect_msg = "Did you use `col_number()` properly?", append = FALSE)
        check_arg(., "uk_airdate")
        check_function(., "col_date") %>% {
            check_arg(., "format") %>% check_equal(incorrect_msg = "Did you specify the format in date, month, and then year?", append = FALSE)
            check_result(.) %>% check_equal(incorrect_msg = "Make sure you used the correct argument in `col_date`.", append = FALSE)
            }
        }
    }
ex() %>% check_object("desserts") %>% check_equal()

ex() %>% check_correct(
	check_output_expr(., "desserts %>% arrange(desc(uk_airdate))", missing_msg = "Make sure you're using `desc()` inside `arrange()` to sort `uk_airdate `correctly."), 
	check_function(., "arrange") %>% {
    	check_arg(., '.data') %>% check_equal(incorrect_msg = "Are you piping desserts into `arrange()`?", append = FALSE)
    	check_result(.) %>% check_equal(eq_condition = "identical", incorrect_msg = "Make sure you're using `desc()` inside `arrange()` to sort `uk_airdate` correctly.", append = FALSE)
    }
)

ex() %>% check_error()

success_msg("Looking good! Notice that `us_airdate` didn't need to be cast - this is because the date format `%Y-%m-%d` is unambiguous, so it is automatically parsed as a date by `readr`.")
```

---

## Cast a variable as a factor

```yaml
type: NormalExercise
key: 0204cb461a
lang: r
xp: 100
```

Factors are categorical variables, where the possible values are a fixed and known set. For example, take a simple factor like `bake` below:

```{r}
bake <- c("pie", "cake", "neither") 
parse_factor(bake, levels = NULL) 
```
```
[1] pie  cake neither
Levels: pie cake neither
```

Remember to use `?parse_factor` to read more about the `levels` argument. And remember that for every `parse_*` function, there is a `col_*` function for casting.

This is the last time you'll read in `"desserts.csv"`!

`@instructions`
Adapt your code from the previous exercise to cast `result` as a factor; detect the factor levels based on the unique values present in each variable. Use `glimpse()` to check your work.

`@hint`
Use `?parse_factor` to see how `levels` needs to be specified within `col_factor()` to satisfy the condition stated in the instructions.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/4417ffb42e1882b1102c1ed64a46121406214e56/desserts.csv", 
              destfile = "desserts.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
```

`@sample_code`
```{r}
# Cast result a factor
desserts <- read_csv("desserts.csv", 
                     na = c("", "NA", "N/A"),
                     col_types = cols(
                       technical = col_number(),
                       uk_airdate = col_date(format = "%d %B %Y"),
                       ___ = ___(levels = ___)
                     ))
                    
# Glimpse to view
```

`@solution`
```{r}
# Cast result a factor
desserts <- read_csv("desserts.csv", 
                     na = c("", "NA", "N/A"),
                     col_types = cols(
                       technical = col_number(),
                       uk_airdate = col_date(format = "%d %B %Y"),
                       result = col_factor(levels = NULL)
                     ))

# Glimpse to view
glimpse(desserts)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "desserts") %>% check_equal(),
  	check_function(., "read_csv") %>% {
    	check_arg(., "file") %>% check_equal()
    	check_arg(., "na") %>% check_equal()
    	check_arg(., "col_types")
    check_function(., "cols") %>% {
        check_arg(., "technical") %>% check_equal(incorrect_msg = "Did you use `col_number()` properly?", append = FALSE)
        check_arg(., "uk_airdate")
        check_function(., "col_date") %>% {
            check_arg(., "format") %>% check_equal(incorrect_msg = "Did you specify the format in date, month, and then year?", append = FALSE)
            check_result(.) %>% check_equal(incorrect_msg = "Make sure you used the correct argument in `col_date`.", append = FALSE)
            }
        check_arg(., "result")
        check_function(., "col_factor") %>% check_arg(., "levels") %>% check_equal(eval=FALSE)
        }
    }
)

ex() %>% check_function("glimpse") %>% check_arg("x") %>% check_equal()

ex() %>% check_error()

success_msg("You've casted like a champ! While it takes a lot of work up front, casting column types when you import can make your analyses easier to reproduce (for yourself too!).")
```

---

## Recode values

```yaml
type: VideoExercise
key: 1f979b4484
lang: r
xp: 50
skills: 1
```

`@projector_key`
0fd2069420e16aaacdf2c8d9d99eb2fd

---

## Recode a character variable

```yaml
type: TabExercise
key: cb06292c87
lang: r
xp: 100
```

In this exercise, you'll [`recode`](https://www.rdocumentation.org/packages/dplyr/versions/0.7.3/topics/recode) the `nut` variable in the `desserts` data. This is a character variable that tells us, for each bake, whether a nut was a key ingredient and if so, what kind of nut!

Remember to use `?function_name_here` to read more about arguments and how to use them.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/6b30a6b2920d8c1fe29bf8b30ebf144706ee8c89/desserts_tidy.csv", 
              destfile = "desserts.csv", quiet = TRUE)
library(dplyr)
library(readr)
library(stringr)
desserts <- read_csv("desserts.csv",
                    na = c("", "NA", "N/A"),
                    col_types = cols(
                      technical = col_number(),
                      uk_airdate = col_date(format = "%d %B %Y"),
                      result = col_factor(levels = NULL)
                    ))
```

***

```yaml
type: NormalExercise
key: c42e0cfd57
xp: 30
```

`@instructions`
- Count the distinct values in the `nut` variable, sorting based on the number of rows. How many bakes included filberts as the nut?

`@hint`
- Use `?count` to remind yourself about how to sort in descending order.

`@sample_code`
```{r}
# Count rows grouping by nut variable
desserts %>%
```

`@solution`
```{r}
# Count rows grouping by nut variable
desserts %>% 
    count(nut, sort = TRUE)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_output_expr(., "desserts %>% count(nut, sort = TRUE)", missing_msg = "Did you `count()` `nut` in the proper order?"),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping desserts into `count()`.", append = FALSE)
    	check_arg(., "sort") %>% check_equal(incorrect_msg = "Set sort equal to TRUE.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Your first argument inside `count()` should be nut.", append = FALSE)
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 2b04e8fc34
xp: 30
```

`@instructions`
- What is a `"filbert"` you may ask? Recode `"filbert"` to `"hazelnut"`, and count again.

`@hint`
- Remember to use `recode` within a `mutate()`. Both the old and the new replacement values are in quotes: old goes first, new goes last.

`@sample_code`
```{r}
# Count rows grouping by nut variable
desserts %>% 
    count(nut, sort = TRUE)
    
# Recode filberts as hazelnuts
desserts_2 <- desserts %>% 
  ___(nut = ___(nut, ___ = ___))

# Count rows again 
desserts %>% 
    count(nut, sort = TRUE)
```

`@solution`
```{r}
# Count rows grouping by nut variable
desserts %>% 
    count(nut, sort = TRUE)
    
# Recode filberts as hazelnuts
desserts_2 <- desserts %>% 
  mutate(nut = recode(nut, "filbert" = "hazelnut"))

# Count rows again
desserts_2 %>% 
    count(nut, sort = TRUE)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_output_expr(., "desserts %>% count(nut, sort = TRUE)", missing_msg = "Did you `count()` `nut` in the proper order?"),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping desserts into `count()`.", append = FALSE)
    	check_arg(., "sort") %>% check_equal(incorrect_msg = "Set sort equal to TRUE.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Your first argument inside `count()` should be `nut`.", append = FALSE)
    }
)

ex() %>% check_correct(
	check_object(., "desserts_2") %>% check_equal(eq_condition = "equal"),
	{
    check_function(., "recode") %>% {
        check_arg(., ".x") %>% check_equal(incorrect_msg = "Did you specify that you want to make changes within `nut`?", append = FALSE, eval = FALSE)
        check_expr(., "any('hazelnut' %in% names(table(desserts$nut)))") %>% check_result() %>% check_equal()
        }
    check_function(., "mutate") %>% {
        check_arg(., '.data') %>% check_equal(incorrect_msg = "Make sure you're piping desserts into `mutate()`.", append = FALSE)
        check_arg(., "nut") %>% check_equal(incorrect_msg = "Set the recoded column equal to `nut`.", append = FALSE, eval = FALSE)
        check_result(.) %>% check_equal()
        }
    }
)

ex() %>% check_correct(
	check_output_expr(., "desserts_2 %>% count(nut, sort = TRUE)", missing_msg = "Did you `count()` `nut` in the proper order?"),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping desserts_2 into `count()`.", append = FALSE)
    	check_arg(., "sort") %>% check_equal(incorrect_msg = "Set sort equal to TRUE.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Your first argument inside `count()` should be `nut`.", append = FALSE)
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 1a4159ad78
xp: 40
```

`@instructions`
- Edit your code to also recode "no nut" as a missing value, and count again to check your work.

`@hint`
- For character variables, you can recode missing values `NA` as `NA_character_`.

`@sample_code`
```{r}
# Count rows grouping by nut variable
desserts %>% 
    count(nut, sort = TRUE)
    
# Edit code to recode "no nut" as missing
desserts_2 <- desserts %>% 
  mutate(nut = recode(nut, "filbert" = "hazelnut", 
                           ___ = ___))

# Count rows again 
desserts_2 %>% 
    count(nut, sort = TRUE)
```

`@solution`
```{r}
# Count rows grouping by nut variable
desserts %>% 
    count(nut, sort = TRUE)
    
# Edit code to recode "no nut" as missing
desserts_2 <- desserts %>% 
  mutate(nut = recode(nut, "filbert" = "hazelnut", 
                           "no nut" = NA_character_))

# Count rows again
desserts_2 %>% 
    count(nut, sort = TRUE)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_output_expr(., "desserts %>% count(nut, sort = TRUE)", missing_msg = "Don't change the sample code."),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping desserts into `count()`.", append = FALSE)
    	check_arg(., "sort") %>% check_equal(incorrect_msg = "Set sort equal to TRUE.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Your first argument inside `count()` should be nut.", append = FALSE)
    }
)

ex() %>% check_correct(
	check_object(., "desserts_2") %>% check_equal(eq_condition = "equal"),
	{
    check_function(., "recode") %>% {
        check_arg(., ".x") %>% check_equal(incorrect_msg = "Did you specify that you want to make changes within `nut`?", append = FALSE, eval = FALSE)
        check_expr(., "any('hazelnut' %in% names(table(desserts$nut)))") %>% check_result() %>% check_equal()
        check_expr(., "!any('no nut' %in% names(table(desserts$nut)))") %>% check_result() %>% check_equal()
        }
    check_function(., "mutate") %>% {
        check_arg(., '.data') %>% check_equal(incorrect_msg = "Make sure you're piping desserts into `mutate()`.", append = FALSE)
        check_arg(., "nut") %>% check_equal(incorrect_msg = "Set the recoded column equal to `nut`.", append = FALSE, eval = FALSE)
        check_result(.) %>% check_equal()
        }
    }
)


ex() %>% check_correct(
	check_output_expr(., "desserts_2 %>% count(nut, sort = TRUE)", missing_msg = "Don't change the sample code."),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping desserts_2 into `count()`.", append = FALSE)
    	check_arg(., "sort") %>% check_equal(incorrect_msg = "Set sort equal to TRUE.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Your first argument inside `count()` should be nut.", append = FALSE)
    }
)

ex() %>% check_error()

success_msg("Nice! `NA` is a logical constant, so it is always helpful to remember the `NA` constants like `NA_character_` and `NA_integer_` when working with strings or numbers. Always know your variable types!")
```

---

## Recode a numeric variable

```yaml
type: TabExercise
key: 84ed5a0f89
lang: r
xp: 100
```

Dummy variables are often used in data analysis to bin a variable into one of two categories to indicate the absence or presence of something. Dummy variables take the value `0` or `1` to stand for, for example, loser or winner. Dummy variables are often factor variables as opposed to numeric- we'll cover more about factors in the last chapter.

In `desserts`, the `technical` variable is a number representing the judges' ranks for each baker in the technical challenge, for every episode (remember to run `glimpse(desserts)` to take a peek).

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/6b30a6b2920d8c1fe29bf8b30ebf144706ee8c89/desserts_tidy.csv", 
              destfile = "desserts.csv", quiet = TRUE)
library(dplyr)
library(readr)
library(stringr)
desserts <- read_csv("desserts.csv",
                    na = c("", "NA", "N/A"),
                    col_types = cols(
                      technical = col_number(),
                      uk_airdate = col_date(format = "%d %B %Y"),
                      result = col_factor(levels = NULL)
                    )) %>% 
  mutate(nut = recode(nut, "filbert" = "hazelnut", 
                             "no nut" = NA_character_))
```

***

```yaml
type: NormalExercise
key: 018f909816
xp: 30
```

`@instructions`
- Create a new dummy variable called `tech_win`: it should be equal to `1` if the baker was ranked first in the technical challenge, and `0` if not.

`@hint`
- Remember to use `recode()` within a `mutate()` and that numeric values to be replaced need to be in backticks. You'll also want to set the `.default` argument here to 0 to avoid typing out all the other place finishes.

`@sample_code`
```{r}
# Create dummy variable: 1 if won, 0 if not
desserts <- desserts %>%
```

`@solution`
```{r}
# Create dummy variable: 1 if won, 0 if not
desserts <- desserts %>% 
  mutate(tech_win = recode(technical, `1` = 1,
                           .default = 0))
```

`@sct`
```{r}
ex() %>% check_correct(
  	check_object(., "desserts") %>% check_equal(eq_condition = "equal"),
	{
        check_function(., "recode") %>% {
        	check_arg(., ".x") %>% check_equal(incorrect_msg = "Did you specify that you want to make changes within `technical`?", append = FALSE, eval = FALSE)
        	check_arg(., ".default") %>% check_equal(incorrect_msg = "Did you set `.default` equal to 0?", append = FALSE)
        	check_code(., "`1` = 1", missing_msg = "Remember to set 1 using backticks equal to 1 and save the changes to `technical`.", append = FALSE)
        }
    check_function(., "mutate") %>% {
        check_arg(., '.data') %>% check_equal(incorrect_msg = "Make sure you're piping `desserts_dummy` into `mutate()`.", append = FALSE)
        check_arg(., "tech_win") %>% check_equal(incorrect_msg = "Set the recoded column equal to `technical`.", append = FALSE, eval = FALSE)
        check_result(.) %>% check_equal()
        }
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 473d68c579
xp: 30
```

`@instructions`
- Use `count` to compare the number of rows where `technical` is equal to `1` versus all distinct values in the new `tech_win` variable.

`@hint`
- Remember that `technical == 1` is a logical and a valid argument for `count()`, which requires a comparison in its second argument, if necessary.

`@sample_code`
```{r}
# Create dummy variable: 1 if won, 0 if not
desserts <- desserts %>% 
  mutate(tech_win = recode(technical, `1` = 1,
                           .default = 0))

# Count to compare values                      
desserts %>% 
  count(___, ___)
```

`@solution`
```{r}
# Create dummy variable: 1 if won, 0 if not
desserts <- desserts %>% 
  mutate(tech_win = recode(technical, `1` = 1,
                           .default = 0))

# Count to compare values                      
desserts %>% 
  count(technical == 1, tech_win)
```

`@sct`
```{r}
ex() %>% check_correct(
  	check_object(., "desserts") %>% check_equal(eq_condition = "equal"),
	{
        check_function(., "recode") %>% {
        	check_arg(., ".x") %>% check_equal(incorrect_msg = "Did you specify that you want to make changes within `technical`?", append = FALSE, eval = FALSE)
        	check_arg(., ".default") %>% check_equal(incorrect_msg = "Did you set `.default` equal to 0?", append = FALSE)
        	check_code(., "`1` = 1", missing_msg = "Remember to set 1 using backticks equal to 1 and save the changes to `technical`.", append = FALSE)
        }
    check_function(., "mutate") %>% {
        check_arg(., '.data') %>% check_equal(incorrect_msg = "Make sure you're piping `desserts_dummy` into `mutate()`.", append = FALSE)
        check_arg(., "tech_win") %>% check_equal(incorrect_msg = "Set the recoded column equal to `technical`.", append = FALSE, eval = FALSE)
        check_result(.) %>% check_equal()
        }
    }
)

ex() %>% check_correct(
	check_output_expr(., "desserts %>% count(technical == 1, tech_win)", missing_msg = "Did you count to compare the number of rows where `technical` is equal to `1` against your new `tech_win` variable?"),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping `desserts` into `count()`.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Your arguments should counting the number of times `technical` is equal to 1, and tech_win, in that order.", append = FALSE)
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 346f4123eb
xp: 40
```

`@instructions`
- You may have noticed that `tech_win` is a numeric variable (a `dbl`). Adapt your code to use [`recode_factor()`](https://www.rdocumentation.org/packages/dplyr/versions/0.7.3/topics/recode) instead of `recode` to convert it to a factor.

`@hint`
- The code for `recode_factor()` is exactly the same as for `recode()`.

`@sample_code`
```{r}
# Edit to recode tech_win as factor
desserts <- desserts %>% 
  mutate(tech_win = recode(technical, `1` = 1,
                           .default = 0))

# Count to compare values                      
desserts %>% 
  count(technical == 1, tech_win)
```

`@solution`
```{r}
# Edit to recode tech_win as factor
desserts <- desserts %>% 
  mutate(tech_win = recode_factor(technical, `1` = 1,
                                  .default = 0))

# Count to compare values                      
desserts %>% 
  count(technical == 1, tech_win)
```

`@sct`
```{r}
ex() %>% check_correct(
  	check_object(., "desserts") %>% check_equal(eq_condition = "equal"),
	{
        check_function(., "recode_factor") %>% {
        	check_arg(., ".x") %>% check_equal(incorrect_msg = "Did you specify that you want to make changes within `technical`?", append = FALSE, eval = FALSE)
        	check_arg(., ".default") %>% check_equal(incorrect_msg = "Did you set `.default` equal to 0?", append = FALSE)
        	check_code(., "`1` = 1", missing_msg = "Remember to set 1 using backticks equal to 1 and save the changes to `technical`.", append = FALSE)
        }
    check_function(., "mutate") %>% {
        check_arg(., '.data') %>% check_equal(incorrect_msg = "Make sure you're piping `desserts_dummy` into `mutate()`.", append = FALSE)
        check_arg(., "tech_win") %>% check_equal(incorrect_msg = "Set the recoded column equal to `technical`.", append = FALSE, eval = FALSE)
        check_result(.) %>% check_equal()
        }
    }
)

ex() %>% check_correct(
	check_output_expr(., "desserts %>% count(technical == 1, tech_win)", missing_msg = "Don't change the sample code."),
	check_function(., "count") %>% {
    	check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're piping `desserts` into `count()`.", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Your arguments should counting the number of times `technical` is equal to 1, and tech_win, in that order.", append = FALSE)
    }
)

ex() %>% check_error()

success_msg("Great job! We'll do more complex recoding and learn more about factors in the last chapter- get excited!")
```

---

## Select variables

```yaml
type: VideoExercise
key: 137bd14c9c
lang: r
xp: 50
skills: 1
```

`@projector_key`
2c90436063e1a843b65ccc7503599b4f

---

## Combine functions with select

```yaml
type: MultipleChoiceExercise
key: b9f54a3c3d
lang: r
xp: 50
skills: 1
```

The `ratings` data tells us about how many UK viewers tuned in to watch each episode of "The Great British Bake-Off." A key question we'll return to in Chapter 3 is: how many more viewers watched the finale than the premiere episode?

Use the data science tools you've learned so far to answer: 

> For series with 10 episodes, which showed the most growth in viewers from the premiere to the finale? Which showed the least?

`@possible_answers`
- Most: series 6, Least = series 7
- Most: series 5, Least = series 8
- Most: series 8, Least = series 3
- Most: series 7, Least = series 5

`@hint`
You'll probably want to use `filter()`, `mutate()`, and `select()` to answer this question.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/aff290489d4bd6d226a52e589b5f9a2b520d0b3a/02.03_messy_ratings.csv", 
              destfile = "ratings.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(stringr)
ratings <- read_csv("ratings.csv",
                    col_types = cols(
                      series = col_factor(levels = NULL))) %>% 
  select(-contains("_28day"), -contains("airdate")) %>% 
  rename_at(vars(contains("_7day")), funs(str_remove(., "_7day")))
```

`@sct`
```{r}
msg1 = "Incorrect."
msg2 = "Correct! Series 5 on BBC One showed the most, while series 8 on Channel 4 showed the least."
msg3 = "Incorrect."
msg4 = "Incorrect."

ex() %>% check_mc(2, feedback_msgs = c(msg1, msg2, msg3, msg4))
```

---

## Recode factor to plot

```yaml
type: TabExercise
key: 9edc227e90
lang: r
xp: 100
skills: 1
```

In the last exercise, you may have used `mutate()` to find the difference between the number of premiere and finale episode viewers for each series. This variable, which I created and called `viewer_growth`, is now available in `ratings`, and feel free to check out it. 

Now let's practice using `select()` to check the result of another `mutate` with `recode_factor()`.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/aff290489d4bd6d226a52e589b5f9a2b520d0b3a/02.03_messy_ratings.csv", 
              destfile = "ratings.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(ggplot2)
ratings <- read_csv("ratings.csv",
                    col_types = cols(
                      series = col_factor(levels = NULL))) %>% 
  filter(episodes == 10) %>% 
  select(-contains("airdate")) %>% 
  mutate(viewer_growth = e10_viewers_7day - e1_viewers_7day)
```

***

```yaml
type: NormalExercise
key: a8beebf485
xp: 30
```

`@instructions`
Create a new _factor_ variable `bbc` by recoding `channel` with two levels using `recode_factor()`: `0` if that series aired on "Channel 4" and `1` if that series aired on any BBC station.

`@hint`
Remember to use `recode_factor()` within a `mutate()` and that character values to be replaced need to be in quotations. You'll also want to set the `.default` argument here to 1 to avoid typing out all the other channels.

`@sample_code`
```{r}
# Recode channel as factor: bbc (1) or not (0)
ratings <- ratings %>% 
  ___(bbc = ___(___, 
                ___, 
                ___))
```

`@solution`
```{r}
# Recode channel as factor: bbc (1) or not (0)
ratings <- ratings %>% 
  mutate(bbc = recode_factor(channel, 
                             "Channel 4" = 0,
                             .default = 1))
```

`@sct`
```{r}
ex() %>% check_correct(
  check_object(., "ratings") %>% check_equal(eq_condition = "equal"),
  check_function(., "mutate") %>% {
    check_arg(., '.data') %>% check_equal(incorrect_msg = "Make sure you're piping ratings into `mutate()`.", append = FALSE)
    check_arg(., "bbc") 
    check_function(., "recode_factor") %>% {
      check_arg(., ".x") %>% check_equal(incorrect_msg = "Did you specify that you want to make changes within `channel`?", append = FALSE, eval = FALSE)
      check_arg(., ".default") %>% check_equal(incorrect_msg = "Did you set `.default` equal to 1?", append = FALSE)
      check_code(., '"Channel 4" = 0', missing_msg = "Remember to set Channel 4 using backticks equal to 0 and save the changes to `channel`.", append = FALSE)
    }
  }
)


ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: d4eb74d3dd
xp: 30
```

`@instructions`
Use `select()` to view the `series`, `channel`, `bbc`, and `viewer_growth` variables to check that your recoding worked.

`@hint`
Select takes bare variable names as arguments, so there should be four variables listed within `select()` (no quotes).

`@sample_code`
```{r}
# Recode channel as factor: bbc (1) or not (0)
ratings <- ratings %>% 
  mutate(bbc = recode_factor(channel, 
                             "Channel 4" = 0,
                             .default = 1))

# Select to look at variables to plot next
ratings %>%
```

`@solution`
```{r}
# Recode channel as factor: bbc (1) or not (0)
ratings <- ratings %>% 
  mutate(bbc = recode_factor(channel, 
                             "Channel 4" = 0,
                             .default = 1))
                            
# Select to look at variables to plot next
ratings %>% 
  select(series, channel, bbc, viewer_growth)
```

`@sct`
```{r}
ex() %>% check_correct(
  check_object(., "ratings") %>% check_equal(eq_condition = "equal"),
  check_function(., "mutate") %>% {
    check_arg(., '.data') %>% check_equal(incorrect_msg = "Make sure you're piping ratings into `mutate()`.", append = FALSE)
    check_arg(., "bbc") 
    check_function(., "recode_factor") %>% {
      check_arg(., ".x") %>% check_equal(incorrect_msg = "Did you specify that you want to make changes within `channel`?", append = FALSE, eval = FALSE)
      check_arg(., ".default") %>% check_equal(incorrect_msg = "Did you set `.default` equal to 1?", append = FALSE)
      check_code(., '"Channel 4" = 0', missing_msg = "Remember to set Channel 4 using backticks equal to 0 and save the changes to `channel`.", append = FALSE)
    }
  }
)

ex() %>% check_correct(
	check_output_expr(., "ratings %>% select(series, channel, bbc, viewer_growth)", missing_msg = "Select the four variables mentioned in the instructions."),
	check_function(., "select") %>% {
    	check_arg(., ".data") %>% check_equal(incorrect_msg = "Did you pipe `ratings` into `select()`?", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Did you select the four variables mentioned in the instructions?", append = FALSE)
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: c1160100c1
xp: 40
```

`@instructions`
Make a bar chart where the height of the bars shows `viewer_growth`, with `series` on the x-axis. Use `fill` to highlight when the show switched from the BBC.

`@hint`
Using `geom_col()`, you'll want to map the `fill` aesthetic to the new factor variable, `bbc`, you created earlier.

`@sample_code`
```{r}
# Recode channel as factor: bbc (1) or not (0)
ratings <- ratings %>% 
  mutate(bbc = recode_factor(channel, 
                             "Channel 4" = 0,
                             .default = 1))
                            
# Select to look at variables to plot next
ratings %>% 
  select(series, channel, bbc, viewer_growth)
  
# Make a filled bar chart
ggplot(___, aes(x = ___, y = ___, fill = ___)) +
  geom_col()
```

`@solution`
```{r}
# Recode channel as factor: bbc (1) or not (0)
ratings <- ratings %>% 
  mutate(bbc = recode_factor(channel, 
                             "Channel 4" = 0,
                             .default = 1))
                            
# Select to look at variables to plot next
ratings %>% 
  select(series, channel, bbc, viewer_growth)
  
# Make a filled bar chart
ggplot(ratings, aes(x = series, y = viewer_growth, fill = bbc)) +
  geom_col()
```

`@sct`
```{r}
ex() %>% check_correct(
  check_object(., "ratings") %>% check_equal(eq_condition = "equal"),
  check_function(., "mutate") %>% {
    check_arg(., '.data') %>% check_equal(incorrect_msg = "Make sure you're piping ratings into `mutate()`.", append = FALSE)
    check_arg(., "bbc") 
    check_function(., "recode_factor") %>% {
      check_arg(., ".x") %>% check_equal(incorrect_msg = "Did you specify that you want to make changes within `channel`?", append = FALSE, eval = FALSE)
      check_arg(., ".default") %>% check_equal(incorrect_msg = "Did you set `.default` equal to 1?", append = FALSE)
      check_code(., '"Channel 4" = 0', missing_msg = "Remember to set Channel 4 using backticks equal to 0 and save the changes to `channel`.", append = FALSE)
    }
  }
)

ex() %>% check_correct(
	check_output_expr(., "ratings %>% select(series, channel, bbc, viewer_growth)", missing_msg = "Select the four variables mentioned in the instructions."),
	check_function(., "select") %>% {
    	check_arg(., ".data") %>% check_equal(incorrect_msg = "Did you pipe `ratings` into `select()`?", append = FALSE)
    	check_result(.) %>% check_equal(incorrect_msg = "Did you select the four variables mentioned in the instructions?", append = FALSE)
    }
)

ex() %>% {
    check_function(., "ggplot") %>% check_arg("data") %>% check_equal(incorrect_msg = "Are you using `ggplot()` on ratings?", append = FALSE)
    check_function(., "aes") %>% {
        check_arg(., "x") %>% check_equal(eval=FALSE)
        check_arg(., "y") %>% check_equal(eval=FALSE)
        check_arg(., "fill") %>% check_equal(eval=FALSE)
        }
    check_function(., "geom_col")
}

ex() %>% check_error()

success_msg("Good selections. Remember to look up the other select helper functions using `?select`. What is going on with `viewer_growth` in series 8? Stay tuned to find out!")
```

---

## Select and reorder variables

```yaml
type: BulletExercise
key: 46a42f2a47
lang: r
xp: 100
skills: 1
```

As we have seen, selecting a subset of columns to print can help you check that a `mutate()` worked as expected, and rearranging columns next to each other can help you spot obvious errors in data entry.

The [`select()` helpers](https://www.rdocumentation.org/packages/tidyselect/versions/0.2.4/topics/select_helpers) allow you to select variables based on their names.

In this exercise, you'll work with the `ratings` data to practice combining helpers, and you'll use a new helper function called [`everything()`](https://www.rdocumentation.org/packages/tidyselect/versions/0.2.4/topics/select_helpers) which can be useful when reordering columns. Use `?everything` to read more.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/aff290489d4bd6d226a52e589b5f9a2b520d0b3a/02.03_messy_ratings.csv", 
              destfile = "ratings.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
ratings <- read_csv("ratings.csv",
                    col_types = cols(
                      series = col_factor(levels = NULL))) %>% 
  select(everything(), day_of_week = day, -contains("airdate"))
```

***

```yaml
type: NormalExercise
key: 406426b767
xp: 30
```

`@instructions`
Move `channel` to the front of `ratings`, keeping _all_ other variables using `everything()`.

`@hint`
Be sure to include the parentheses after `everything()` (no arguments needed) when you call `select()`, whose first argument is the desired first variable.

`@sample_code`
```{r}
# Move channel to first column
ratings %>% 
  ___(___, ___)
```

`@solution`
```{r}
# Move channel to first column
ratings %>% 
  select(channel, everything())
```

`@sct`
```{r}
test_correct(ex() %>% check_output_expr("ratings %>% select(channel, everything())", missing_msg = "Move channel to the first column while keeping the rest of the variables."), 
ex() %>% {
    check_function(., "everything")
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Make sure you're piping ratings into `select()`.", append = FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_msg = "For this question, the order of the arguments matter.", append = FALSE)
        }
    }
)
ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 99ce922088
xp: 30
```

`@instructions`
Adapt your code to drop only those variables that end with "day".

`@hint`
You don't need to use `everything()` here. Use `-variable` to drop, combined with the `ends_with("string")` helper. You should have 14 variables total!

`@sample_code`
```{r}
# Drop 7- and 28-day episode ratings
ratings %>% 
  select(___)
```

`@solution`
```{r}
# Drop 7- and 28-day episode ratings
ratings %>% 
  select(-ends_with("day"))
```

`@sct`
```{r}
ex() %>% check_correct(
	check_output_expr(., "ratings %>% select(-ends_with(\"day\"))", missing_msg = "Drop viewer data for each episode."), 
	{
    check_function(., "ends_with") %>% check_arg(., "match") %>% check_equal(incorrect_msg = "Make sure you're dropping the right phrase.", append = FALSE)
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Make sure you're piping ratings into `select()`.", append = FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_msg = "Remember to use - to drop a variable.", append = FALSE)
        }
    }
)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 5c1698d89a
xp: 40
```

`@instructions`
Move `channel` to the first column and keep _all_ other variables _except_ those that end with "day".

`@hint`
You'll want to use the `everything()` function, but this time it can't be the last item in your `select()` call- your last argument should drop variables using `-`.

`@sample_code`
```{r}
# Move channel to front and drop 7-/28-day episode ratings
ratings %>% 
  select(___, ___, ___)
```

`@solution`
```{r}
# Move channel to front and drop 7-/28-day episode ratings
ratings %>% 
  select(channel, everything(), -ends_with("day"))
```

`@sct`
```{r}
ex() %>% check_correct(
	check_output_expr(., "ratings %>% select(channel, everything(), -ends_with(\"day\"))", missing_msg = "Move `channel` to the front and drop the viewer data for each episode."), 
	{
    check_function(., "everything")
    check_function(., "ends_with") %>% check_arg(., "match") %>% check_equal(incorrect_msg = "Make sure you're dropping the right phrase.", append = FALSE)
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Make sure you're piping ratings into `select()`.", append = FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_msg = "The order of the arguments within `select()` matters.", append = FALSE)
        }
    }
)

ex() %>% check_error()

success_msg("`everything()` is a great little helper! If it had been placed at the end, it would have added back in all the columns that end with `\"day\"`. Placing it before deselecting columns, though, is a real time-saver.")
```

---

## Tame variable names

```yaml
type: VideoExercise
key: 353cec5986
lang: r
xp: 50
skills: 1
```

`@projector_key`
7475fe2e464c16ac4b74525e8d054e60

---

## Reformat variables

```yaml
type: BulletExercise
key: f2e82b45e8
lang: r
xp: 100
skills: 1
```

In the video, you saw how to use [`clean_names()`](https://www.rdocumentation.org/packages/janitor/versions/1.0.0/topics/clean_names) from the `janitor` package to convert all variable names to `snake_case`. Here is how:

```{r}
library(janitor)
young_bakers3 <- young_bakers3 %>% 
   clean_names()
```

Let's practice using the `messy_ratings` tibble. Remember you can use `?clean_names` to see the function arguments and how to use them.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/aff290489d4bd6d226a52e589b5f9a2b520d0b3a/02.03_messy_ratings.csv", 
              destfile = "messy_ratings.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(stringr)
library(janitor)
messy_ratings <- read_csv("messy_ratings.csv",
                    col_types = cols(
                      series = col_factor(levels = NULL))) %>% 
  select(-contains("airdate")) %>% 
  rename_at(vars(matches("e\\d+_viewers_\\d+day")),
            funs(str_c("GBBO ", .))) %>% 
  rename_all(funs(str_replace(., "_", "."))) %>% 
  rename_all(funs(str_replace(., "_", "~"))) %>% 
  rename_at(vars(contains("viewers")),
            funs(str_to_title(.))) %>% 
  rename_at(vars(contains("series")),
            funs(str_to_upper(.)))
```

***

```yaml
type: NormalExercise
key: 89c007a26f
xp: 30
```

`@instructions`
Use `glimpse()` to view the variable names in `messy_ratings`, and load the `janitor` package.

`@hint`
The only argument for `glimpse()` is the name of the tibble, `messy_ratings`.

`@sample_code`
```{r}
# Glimpse to see variable names


# Load janitor
```

`@solution`
```{r}
# Glimpse to see variable names
glimpse(messy_ratings)

# Load janitor
library(janitor)
```

`@sct`
```{r}
ex() %>% check_function("glimpse") %>% check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're just using `glimpse()` on messy_ratings.", append = FALSE)

ex() %>% check_library("janitor")

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 251e5672f1
xp: 30
```

`@instructions`
Convert all variable names to "lower_camel" case in a new tibble called `ratings`; `glimpse()` to view again.

`@hint`
Use `?clean_names` to see the default case, and the other possible values for the case argument.

`@sample_code`
```{r}
# Glimpse to see variable names
glimpse(messy_ratings)

# Load janitor
library(janitor)

# Reformat to lower camelcase
ratings <- messy_ratings %>%
  ___(___)
    
# Glimpse new tibble
```

`@solution`
```{r}
# Glimpse to see variable names
glimpse(messy_ratings)

# Load janitor
library(janitor)

# Reformat to lower camelcase
ratings <- messy_ratings %>% 
  clean_names("lower_camel")

# Glimpse new tibble
glimpse(ratings)
```

`@sct`
```{r}
ex() %>% check_output_expr("glimpse(messy_ratings)", missing_msg = "Do not change the sample code here.")

ex() %>% check_library("janitor")

ex() %>% check_correct(
	check_object(., "ratings") %>% check_equal(),
	check_function(., "clean_names") %>% {
    	check_arg(., "case") %>% check_equal()
    	check_result(.) %>% check_equal(incorrect_msg = "`clean_names()` should only have one argument here.")
    }
)

ex() %>% check_function("glimpse") %>% check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're just using `glimpse()` on ratings.", append = FALSE)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: ea1dd4682a
xp: 40
```

`@instructions`
Adapt your code to convert all variable names to "snake_case" instead; `glimpse()` to view the new variable names.

`@hint`
Remember you could leave the parentheses empty, or you could specify `"snake"`.

`@sample_code`
```{r}
# Glimpse to see variable names
glimpse(messy_ratings)

# Load janitor
library(janitor)

# Reformat to snake case
ratings <- messy_ratings %>%  
  clean_names("lower_camel")

# Glimpse cleaned names

```

`@solution`
```{r}
# Glimpse to see variable names
glimpse(messy_ratings)

# Load janitor
library(janitor)

# Reformat to snake case
ratings <- messy_ratings %>% 
  clean_names()
    
# Glimpse cleaned names
glimpse(ratings)
```

`@sct`
```{r}
ex() %>% check_library("janitor")

ex() %>% check_correct(
	check_object(., "ratings") %>% check_equal(),
	check_function(., "clean_names") %>% check_arg(., "dat") %>% check_equal(incorrect_msg = "Make sure you're not changing the first `glimpse()` call from the sample code.", append=FALSE)
)

ex() %>% check_function("glimpse") %>%check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're just using `glimpse()` on ratings.", append = FALSE)

ex() %>% check_error()

success_msg("Great job! You have a ton of case options with the `clean_names` function- choose wisely!")
```

---

## Rename and subset variables

```yaml
type: TabExercise
key: f94b58bcaf
lang: r
xp: 100
skills: 1
```

In the next chapter, when we reshape data to be tidy, you'll see the importance of having tame variable names. 

Now that our variable names in `ratings` have been converted to snake case, you'll practice combining renaming with `select()` helper functions to take advantage of the variable name structure.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/aff290489d4bd6d226a52e589b5f9a2b520d0b3a/02.03_messy_ratings.csv", 
              destfile = "ratings.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(stringr)
library(janitor)
ratings <- read_csv("ratings.csv",
                    col_types = cols(
                      series = col_factor(levels = NULL))) %>% 
            rename_at(vars(matches("e\\d+_viewers_\\d+day")), funs(str_c("gbbo_", .)))
```

***

```yaml
type: NormalExercise
key: 4aa52bd7a6
xp: 50
```

`@instructions`
Select `series` and variables with 7-day viewer data from `ratings`; `glimpse()` to view the result.

`@hint`
You'll want to look at the `ends_with()` helper function here for the 7-day viewer data.

`@sample_code`
```{r}
# Select 7-day viewer data by series
viewers_7day <- ratings %>%


# Glimpse

```

`@solution`
```{r}
# Select 7-day viewer data by series
viewers_7day <- ratings %>% 
    select(series, ends_with("7day"))

# Glimpse
glimpse(viewers_7day)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "viewers_7day") %>% check_equal(eq_condition = "identical"),
	{
    check_function(., "ends_with") %>% check_arg(., "match") %>% check_equal(incorrect_msg = "Make sure you're only selecting for variables that end with \"7 day\".", append = FALSE)
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe ratings into `select()`.", append = FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_msg = "The order of the two arguments matters.", append = FALSE)
        }
    }
)

ex() %>% check_function("glimpse") %>% check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're just using `glimpse()` on viewers_7day.", append = FALSE)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 7857537614
xp: 50
```

`@instructions`
Adapt your code to rename each 7-day viewers variable to `viewers_7day_<episode>`, where `<episode>` takes on all the possible episode values.

`@hint`
Remember that the `episode` number will take care of itself; it will get inserted automatically after the last `_`. Here, you are still looking for the 7-day viewers, just assigning them to a different variable name.

`@sample_code`
```{r}
# Adapt code to also rename 7-day viewer data
viewers_7day <- ratings %>% 
    select(series, ends_with("7day"))

# Glimpse
glimpse(viewers_7day)
```

`@solution`
```{r}
# Adapt code to also rename 7-day viewer data
viewers_7day <- ratings %>% 
    select(series, viewers_7day_ = ends_with("7day"))

# Glimpse
glimpse(viewers_7day)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "viewers_7day") %>% check_equal(eq_condition = "identical"),
	{
    check_function(., "ends_with") %>% check_arg(., "match") %>% check_equal(incorrect_msg = "Make sure you're only selecting for variables that end with \"7 day\".", append = FALSE)
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe ratings into `select()`.", append = FALSE)
        check_arg(., "viewers_7day_") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_msg = "The order of the two arguments matters.", append = FALSE)
        }
    }
)

ex() %>% check_function("glimpse") %>% check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're just using `glimpse()` on viewers_7day.", append = FALSE)

ex() %>% check_error()

success_msg("Great job batch renaming! Using `select` helper functions is a powerful way to tame your variable names. Tame names make it easier to tidy your data (see Chapter 3)!")
```

---

## Rename and reorder variables

```yaml
type: BulletExercise
key: 299a294db8
lang: r
xp: 100
skills: 1
```

In the previous exercise, you renamed all the variables in `ratings` that end with "7day" *and* kept only a subset of variables. In this exercise, you'll adapt that code to drop some specific columns, while keeping and reordering other columns.

`@pre_exercise_code`
```{r}
download.file("https://assets.datacamp.com/production/repositories/1613/datasets/aff290489d4bd6d226a52e589b5f9a2b520d0b3a/02.03_messy_ratings.csv", 
              destfile = "ratings.csv", quiet = TRUE)
library <- function(...) {
  suppressPackageStartupMessages(base::library(...))
}
library(readr)
library(dplyr)
library(stringr)
library(janitor)
ratings <- read_csv("ratings.csv",
                    col_types = cols(
                      series = col_factor(levels = NULL))) %>% 
            rename_at(vars(matches("e\\d+_viewers_\\d+day")), funs(str_c("gbbo_", .)))
```

***

```yaml
type: NormalExercise
key: 36081dd93a
xp: 50
```

`@instructions`
Drop the ten columns that contain 28-day viewer data and keep all the `viewers_7day_` variables in the front.

`@hint`
Where you place `everything()` matters- play with the order of arguments within your `select()` call!

`@sample_code`
```{r}
# Adapt code to drop 28-day columns; keep 7-day in front
viewers_7day <- ratings %>% 
    ___(viewers_7day_ = ends_with("7day"),
        ___,
        ___)

# Glimpse
glimpse(viewers_7day)
```

`@solution`
```{r}
# Adapt code to drop 28-day columns; keep 7-day in front
viewers_7day <- ratings %>% 
    select(viewers_7day_ = ends_with("7day"), 
           everything(), 
           -ends_with("28day"))

# Glimpse
glimpse(viewers_7day)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "viewers_7day") %>% check_equal(eq_condition = "identical"),
	{
    check_function(., "ends_with", index = 1) %>% check_arg(., "match") %>% check_equal(incorrect_msg = "Make sure you're only selecting for variables that end with \"7 day\".", append = FALSE)
    check_function(., "everything")
    check_function(., "ends_with", index = 2) %>% check_arg(., "match") %>% check_equal(incorrect_msg = "Make sure you're only selecting for variables that end with \"28 day\".", append = FALSE)    
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe ratings into `select()`.", append = FALSE)
        check_arg(., "viewers_7day_") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_msg = "The order of the three arguments matters, and don't forget that `-` drops  variables.", append = FALSE)
        }
    }
)

ex() %>% check_function("glimpse") %>% check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're just using `glimpse()` on viewers_7day.", append = FALSE)

ex() %>% check_error()
```

***

```yaml
type: NormalExercise
key: 34b976a053
xp: 50
```

`@instructions`
Adapt your code to now keep the original order of the variables, with the conditions in the last part still relevant.

`@hint`
Where you place `everything()` matters- play with the order of arguments within your `select()` call, and don't forget that the dropped variables always go last!

`@sample_code`
```{r}
# Adapt code to keep original order
viewers_7day <- ratings %>% 
    select(viewers_7day_ = ends_with("7day"), 
           everything(), 
           -ends_with("28day"))

# Glimpse
glimpse(viewers_7day)
```

`@solution`
```{r}
# Adapt code to keep original order
viewers_7day <- ratings %>% 
    select(everything(), 
           viewers_7day_ = ends_with("7day"), 
           -ends_with("28day"))

# Glimpse
glimpse(viewers_7day)
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(., "viewers_7day") %>% check_equal(eq_condition = "identical"),
	{
    check_function(., "everything")
    check_function(., "ends_with", index = 1) %>% check_arg(., "match") %>% check_equal(incorrect_msg = "Make sure you're only selecting for variables that end with \"7 day\".", append = FALSE)
    check_function(., "ends_with", index = 2) %>% check_arg(., "match") %>% check_equal(incorrect_msg = "Make sure you're only selecting for variables that end with \"28 day\".", append = FALSE)    
    check_function(., "select") %>% {
        check_arg(., ".data") %>% check_equal(incorrect_msg = "Pipe ratings into `select()`.", append = FALSE)
        check_arg(., "viewers_7day_") %>% check_equal(eval=FALSE)
        check_result(.) %>% check_equal(eq_condition = "identical", incorrect_msg = "The order of the three arguments matters, and don't forget that `-` drops a variable.", append = FALSE)
        }
    }
)

ex() %>% check_function("glimpse") %>% check_arg(., "x") %>% check_equal(incorrect_msg = "Make sure you're just using `glimpse()` on viewers_7day.", append = FALSE)

ex() %>% check_error()

success_msg("You are a master data tamer! You are done with Chapter 2 and ready to start tidying.")
```
