# Working with data in the tidyverse

*Intro video: This course aims to empower you to work with your data in R from start to finish, and to demystify some of the "tidyverse" package pipeline by showing you when each one can be helpful, and how they can be used together. At the end of this course, you will be tidyverse fluent and happy to dive into working with your data in R.*

Datasets:

* Ice skating (separate male/female skater pairs)
* Face ID task (rename range of variables)
* ADHD data (multiple sets of columns to gather)
* Fillers (https://gist.github.com/kylebgorman/77ce12c9167554ade560af9d34565c11)
* Autism BMI data from ATN
* GBBO

## Chapter 1: Reading data into R

You will start this course by learning how to read data into R, using 3 different tidyverse packages. Each package is designed to work with different kinds of data files. We'll begin with the `readr` package, and use it to read in a file where the values are separated by commas, called a CSV file for "comma-separated values." Then we'll introduce 2 specialized packages for reading Excel files (`readxl`) and data exported from statistical packages like SPSS and SAS (`haven`). Finally, we'll highlight some `dplyr` functions that allow you to quickly see your freshly read data in R.

Plan for slides/demo: bakeoff data
* missing values (`c("N/A", "unknown")`)
* character -> date (original air date)
* number -> factor (star baker: yes/no)
* number from character (technical scores)

Plan for slides/demo: sewing bee data
* missing values (`c("N/A", "unknown")`)
* character -> date (original air date)
* number -> factor (best garment: yes/no)
* number from character (technical scores)


* Read a CSV file using `readr`
    * Read the CSV file in using 3 different file paths: working directory, sub-directory (i.e., "data/file.csv"), url (using all default arguments)
    * Specify missing values using the `na = ` argument in `read_csv`
    * Specify column names using the `col_names = ` argument in `read_csv`
* Column types in `readr`
    * Use `col_types` to cast a variable as a date in `readr`
    * Typecast a numeric variable as a factor in `readr`
    * Extract numbers from text (`readr::parse_number`)
    * Use `spec` in `readr` for longer lists of variables
* Read in other file types
    * Read Excel file using `readxl` (specify sheet and range)
    * Set column types within `readxl`
    * Read SPSS file via `haven`
* Preview data using `dplyr`
    * Get a glimpse of your data
    * Use `dplyr::select` to: drop columns, keep columns, rearrange columns with helper functions
    * See distinct rows using `dplyr::distinct`; count distinct rows using `dplyr::count`


## Chapter 2: Taming data 

In this chapter, you'll learn some basics of data taming, like how to make your variable names make sense, how to deal with duplicates and missing data, and how to get your columns to hold the information you actually need.

Plan for slides/demo: bakeoff data (messier)
* rename single variable
* rename variable range
* character -> date (original air date)
* number -> factor (star baker: yes/no)
* number from character (technical scores)

Plan for slides/demo: sewing bee data (messier)
* missing values (`c("N/A", "unknown")`)
* character -> date (original air date)
* number -> factor (best garment: yes/no)
* number from character (technical scores)

* Cleaning up variable names
    * Use `janitor::clean_names`
    * Rename specific variables using `dplyr::rename`
    * Use `dplyr::select` to rename variable range (with `everything` helper function)
* Dealing with missing data
    * Efficient filtering of missing values with `tidyr::drop_na` and `tidyr::replace_na`
* Separating and uniting variables with `tidyr`
    * Separate one column into two columns, use `extra = merge` vs drop, fill too?
    * Unite with default `_` as separator, then specify different `sep = `
    * Use `extract`

## Chapter 3: Taming variables

In this chapter, you'll learn how to tame specific types of variables that are known to be tricky to work with, specifically: dates, strings, and factors. 

* Dates [video]
    * Typecast within a `dplyr::mutate` (after importing)
    * Difference between 2 dates (calculate age using `lubridate::interval`)
* Strings [video]
    * Use `stringr` to split string (`str_split_fixed`?)
    * Use `stringr::str_detect` in a `dplyr::mutate` to make a 0/1 variable
    * Use `stringr::str_replace` & `str_replace_all`
* Factors [video]
    * Use `forcats` to relevel a factor variable
    * Use `dplyr::recode` for simple recoding factor (or `forcats::recode`)
    * Use `dplyr::case_when` to recode a factor variable (lumping/binning)

## Chapter 4: Tidying data

Now that your data has been tamed, it is time to get tidy. In this last chapter, you will get hands-on experience tidying data and combining multiple tidying functions together in a chain using the pipe operator. 

* Tidy data
    * Multiple choice
* Gather a set of columns into one column
    * Use `tidyr::gather` by variable inclusion as a range
    * Use `tidyr::gather` by variable exclusion
    * Gather non-sequential columns by combining with `select` and helper functions (matches, starts_with, ends_with, contains)
* Spread one column into multiple columns
    * Simple where `fill = NA` is default arg
    * Set fill to something other than NA
* Tidy multiple sets of columns
    * Build up to a `gather %>% separate %>% spread` sequence
    * Build up to a `gather %>% separate %>% unite %>% spread` sequence