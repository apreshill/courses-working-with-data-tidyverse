---
title: Recode Values
key: 0fd2069420e16aaacdf2c8d9d99eb2fd
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch2_2.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch2_2.mp4
transformations:
    translateX: 55
    translateY: 0
    scale: 1

---
## Recode Values

```yaml
type: "TitleSlide"
key: "8a011ab6b0"
```

`@lower_third`

name: Alison Hill
title: Professor & Data Scientist


`@script`
Some variables, like categorical factor variables, may need even more taming after casting them. A factor is either a numeric or character variable where the range of possible values is fixed and limited. But sometimes those values need to be replaced.

Think of recoding values like a "find-and-replace" for variables in a data frame.


---
## Find-and-replace

```yaml
type: "TwoColumns"
key: "c6275a3320"
```

`@part1`
```{r}
bakeoff %>% 
    distinct(result)
```{{1}}
```
# A tibble: 6 x 1
  result   
  <fct>    
1 IN       
2 OUT      
3 RUNNER UP
4 WINNER   
5 SB       
6 LEFT
```{{2}}


`@part2`
```{r}
bakeoff %>% 
    distinct(result)
```{{3}}
```
# A tibble: 6 x 1
  result   
  <fct>    
1 IN       
2 OUT      
3 RUNNER UP
4 WINNER   
5 STAR BAKER      
6 LEFT
```{{3}}


`@script`
Here is an example from an exercise you did in chapter 1. These are the distinct values of the result variable in the bakeoff data.

What if we wanted to change all values of SB to STAR BAKER? 

This simple change might make it easier for people to work this variable.


---
## The `dplyr` package

```yaml
type: "FullSlide"
key: "15a4d876dc"
```

`@part1`
```
library(dplyr) # once per work session
```

![](https://assets.datacamp.com/production/repositories/1613/datasets/071952491ec4a6a532a3f70ecfa2507af4d341f9/hex-dplyr.png)


`@citations`
- http://dplyr.tidyverse.org


`@script`
To do this, we use the recode function in the dplyr package.

For more complex recoding, there is a dplyr function called case underscore when, which we'll cover in the last chapter of this course.


---
## Recode: Usage

```yaml
type: "FullSlide"
key: "19cef2355f"
```

`@part1`
```{r}
?recode
```{{1}}

![](http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/02.03_recode_usage.png){{2}}


`@script`
The usage for recode lists dot-x and dot-dot-dot. The dot-dot-dot is called an ellipsis, and it is important to know in R. Generally it means that the function is designed to take any number of named or unnamed arguments.

Finally, we have two named arguments with defaults set to NULL: dot-default and dot-missing.


---
## Recode: Arguments

```yaml
type: "FullSlide"
key: "4c03f01a49"
```

`@part1`
```{r}
?recode
```{{1}}

![](http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/02.03_recode_args.png){{2}}


`@script`
The first argument, dot-x, is the vector we're modifying. 

The ellipsis stands for the replacements. The argument names, the left hand side of each pair, are the current values to be replaced. 

The argument values, the right hand side of each pair, are the new replacement values. Let's see this in action.


---
## Youngest bakers

```yaml
type: "FullSlide"
key: "7d673ea283"
```

`@part1`
```{r}
young_bakers
```{{1}}
```
# A tibble: 10 x 4
   baker       age occupation                            student
   <chr>     <dbl> <chr>                                   <dbl>
 1 Flora       19. art gallery assistant                      0.
 2 Julia       21. aviation broker                            0.
 3 Benjamina   23. teaching assistant                         0.
 4 Martha      17. student                                    1.
 5 Jason       19. civil engineering student                  1.
 6 Liam        19. student                                    1.
 7 Ruby        20. history of art and philosophy student      1.
 8 Michael     20. student                                    1.
 9 James       21. medical student                            2.
10 John        23. law student                                2.
```{{2}}


`@script`
This is some data about our ten youngest bakers on the show. The data frame is called young_bakers.

Let's recode the student variable, which is numeric- it looks like zeroes mean "not a student" and ones and twos indicate types of students.


---
## Recode student

```yaml
type: "FullSlide"
key: "a532ce25f9"
```

`@part1`
```{r}
young_bakers %>% 
  mutate(stu_label = recode(student, `0` = "other",
                                .default = "student"))
```{{1}}
```{r}
# A tibble: 10 x 5
   baker       age occupation                            student stu_label
   <chr>     <dbl> <chr>                                   <dbl> <chr>    
 1 Flora       19. art gallery assistant                      0. other    
 2 Julia       21. aviation broker                            0. other    
 3 Benjamina   23. teaching assistant                         0. other    
 4 Martha      17. student                                    1. student  
 5 Jason       19. civil engineering student                  1. student  
 6 Liam        19. student                                    1. student  
 7 Ruby        20. history of art and philosophy student      1. student  
 8 Michael     20. student                                    1. student  
 9 James       21. medical student                            2. student  
10 John        23. law student                                2. student 
```{{2}}


`@script`
Recode is best used inside a mutate. This means that you can save the recoded variable as a new variable, or you can recode and replace an existing variable.

Here we made a new variable called stu underscore label.

The first argument to recode is the existing variable to recode.

Next come to replacement pairs. On the left is the old value to be replaced. I put the zero here in backticks because it is a number. On the right is the replacement value, which is the string "other".

This is the equivalent of find-and-replace: find the value on the left, and replace it with the value on the right.

We also used the dot default argument, and set it to the string "student". This means that any value that is not 0 will be recoded as student.


---
## Recode with NA

```yaml
type: "FullSlide"
key: "5db23af27f"
```

`@part1`
```{r}
young_bakers %>% 
  mutate(stu_label = recode(student, `0` = NA_character_,
                                .default = "student"))
```{{1}}
```
# A tibble: 10 x 5
   baker       age occupation                            student stu_label
   <chr>     <dbl> <chr>                                   <dbl> <chr>    
 1 Flora       19. art gallery assistant                      0. NA       
 2 Julia       21. aviation broker                            0. NA       
 3 Benjamina   23. teaching assistant                         0. NA       
 4 Martha      17. student                                    1. student  
 5 Jason       19. civil engineering student                  1. student  
 6 Liam        19. student                                    1. student  
 7 Ruby        20. history of art and philosophy student      1. student  
 8 Michael     20. student                                    1. student  
 9 James       21. medical student                            2. student  
10 John        23. law student                                2. student 
```{{2}}


`@script`
We could also have replaced any existing values with NA. Just as you learned how to replace some values with NA at import using read underscore csv, you can also replace values with NA using recode.

Here we recode zeroes as NA. But because our replacement values are characters, we have to use NA underscore character underscore. Here again, values that are not zero are recoded as "student."


---
## Recode multiple values

```yaml
type: "FullSlide"
key: "d947bc2c73"
```

`@part1`
```{r}
young_bakers %>% 
  mutate(stu_label = recode(student, `0` = NA_character_,
                            `2` = "law/med",
                            .default = "student"))
```{{1}}
```{r}
# A tibble: 10 x 5
   baker       age occupation                            student stu_label
   <chr>     <dbl> <chr>                                   <dbl> <chr>    
 1 Flora       19. art gallery assistant                      0. NA       
 2 Julia       21. aviation broker                            0. NA       
 3 Benjamina   23. teaching assistant                         0. NA       
 4 Martha      17. student                                    1. student  
 5 Jason       19. civil engineering student                  1. student  
 6 Liam        19. student                                    1. student  
 7 Ruby        20. history of art and philosophy student      1. student  
 8 Michael     20. student                                    1. student  
 9 James       21. medical student                            2. law/med  
10 John        23. law student                                2. law/med 
```{{2}}


`@script`
We can replace as many values as we want by listing them, with each replacement pair separated by commas. The old value is always on the left, and the new value is always on the right.


---
## Convert to NA only

```yaml
type: "FullSlide"
key: "e129374aaf"
```

`@part1`
```{r}
young_bakers %>% 
  mutate(student = na_if(student, 0))
```{{1}}
```
# A tibble: 10 x 4
   baker       age occupation                            student
   <chr>     <dbl> <chr>                                   <dbl>
 1 Flora       19. art gallery assistant                     NA 
 2 Julia       21. aviation broker                           NA 
 3 Benjamina   23. teaching assistant                        NA 
 4 Martha      17. student                                    1.
 5 Jason       19. civil engineering student                  1.
 6 Liam        19. student                                    1.
 7 Ruby        20. history of art and philosophy student      1.
 8 Michael     20. student                                    1.
 9 James       21. medical student                            2.
10 John        23. law student                                2.
```{{2}}


`@script`
If you only want to convert specific values to NA, but leave all other values as is, you can use the na underscore if function instead. You use it inside a mutate just like recode. 

The first argument is the variable to modify, and the second is the current value to replace with NA. 

Here the only change we made is to replace all zeroes with NA.


---
## Let's practice!

```yaml
type: "FinalSlide"
key: "b3b9333b0b"
```

`@script`
Now let's try some examples.

