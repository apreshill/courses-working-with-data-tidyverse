---
title: Tame Variable Names
key: 7475fe2e464c16ac4b74525e8d054e60
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch2_4.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch2_4.mp4
transformations:
    translateX: 55
    translateY: 0
    scale: 1

---
## Tame Variable Names

```yaml
type: "TitleSlide"
key: "c23d19d1ec"
```

`@lower_third`

name: Alison Hill
title: Professor & Data Scientist


`@script`
We just learned how to use select to keep, drop, and reorder columns in a data frame. Now we'll learn how to use select to rename variables at the same time. Creating variable names that are easier to work with is our last step in data taming.


---
## Select: arguments

```yaml
type: "FullSlide"
key: "1560412e49"
```

`@part1`
```{r}
?select
```{{1}}

![](http://s3.amazonaws.com/assets.datacamp.com/production/course_6012/datasets/02.01_select_args.png){{2}}


`@script`
How do we do this? Look at the ellipsis argument again. In the third chunk of text, it says "use named arguments to rename selected variables."


---
## Select & change variable names

```yaml
type: "FullCodeSlide"
key: "061f7aa5b5"
```

`@part1`
```{r}
young_bakers3
```{{1}}
```
# A tibble: 4 x 6
  baker     student   age  tre1  tre2  tre3
  <chr>       <dbl> <dbl> <dbl> <dbl> <dbl>
1 Ruby           1.   20.   12.    3.    3.
2 Julia          0.   21.    3.    4.    2.
3 Benjamina      0.   23.    6.    3.    6.
4 John           2.   23.   11.    1.    6.
```{{2}}

```{r}
young_bakers3 %>% 
   select(baker, tech_1 = tre1)
```{{3}}
```
# A tibble: 4 x 2
  baker     tech_1
  <chr>      <dbl>
1 Ruby         12.
2 Julia         3.
3 Benjamina     6.
4 John         11.
```{{4}}


`@script`
Here is another mini version of our youngest bakers data. 

The last three variable names are not helpful: these are each baker's technical rank for episodes 1, 2, and 3. These are currently labeled T-R for technical rank then the episode number.

We can rename the first variable, t-r-e-one, as tech underscore one when we select in the same step.

The new name is on the left of the equal sign, and the original name on the right.


---
## Select & change variable names

```yaml
type: "FullCodeSlide"
key: "ea8191bcbe"
disable_transition: true
```

`@part1`
```{r}
young_bakers3
```
```
# A tibble: 4 x 6
  baker     student   age  tre1  tre2  tre3
  <chr>       <dbl> <dbl> <dbl> <dbl> <dbl>
1 Ruby           1.   20.   12.    3.    3.
2 Julia          0.   21.    3.    4.    2.
3 Benjamina      0.   23.    6.    3.    6.
4 John           2.   23.   11.    1.    6.
```

```{r}
young_bakers3 %>% 
   select(baker, tech_ = tre1:tre3)
```{{1}}
```
# A tibble: 4 x 4
  baker     tech_1 tech_2 tech_3
  <chr>      <dbl>  <dbl>  <dbl>
1 Ruby         12.     3.     3.
2 Julia         3.     4.     2.
3 Benjamina     6.     3.     6.
4 John         11.     1.     6.
```{{2}}


`@script`
But we actually have three of these variables here, and in reality there are up to ten episodes. To rename them all, we would have to repeat ourselves over and over again.

Luckily, we can use select helper functions to rename multiple variables at once. Think of it like "batch" renaming. 

We can use the colon operator, for example, to select and rename a range of consecutive variables. 

On the left now is our new variable **prefix**.

In the output, the variable names start with "tech underscore" then a number starting with 1. 

But this method only works if the variables in your range are consecutive, which was not the case with the bakers data.


---
## Change names for a variable range

```yaml
type: "FullCodeSlide"
key: "d6fe000e1a"
disable_transition: true
```

`@part1`
```{r}
young_bakers3
```
```
# A tibble: 4 x 9
  baker       age student  tre1 rse1   tre2 rse2   tre3 rse3 
  <chr>     <dbl>   <dbl> <dbl> <chr> <dbl> <chr> <dbl> <chr>
1 Ruby        20.      1.   12. IN       3. SB       3. IN   
2 Julia       21.      0.    3. IN       4. IN       2. SB   
3 Benjamina   23.      0.    6. IN       3. IN       6. IN   
4 John        23.      2.   11. IN       1. SB       6. IN 
```

```{r}
young_bakers3 %>%
   select(baker, tech_ = starts_with("tr"),
          result_ = starts_with("rs"))
```{{1}}
```
# A tibble: 4 x 7
  baker     tech_1 tech_2 tech_3 result_1 result_2 result_3
  <chr>      <dbl>  <dbl>  <dbl> <chr>    <chr>    <chr>   
1 Ruby         12.     3.     3. IN       SB       IN      
2 Julia         3.     4.     2. IN       IN       SB      
3 Benjamina     6.     3.     6. IN       IN       IN      
4 John         11.     1.     6. IN       SB       IN   
```{{2}}


`@script`
For example, in this version of the data, the technical ranks and the episode results, which start with r-s, are ordered by episode. 

This means that the three technical variables are not next to each other. 

Here, we could use starts_with twice to select and rename. You can use any select helper function in this way.

Notice that the output here is reordered such that all the technical variables are first, and the three result variables are last.


---
## Change names without reordering

```yaml
type: "FullSlide"
key: "347dfb8092"
```

`@part1`
```{r}
young_bakers3
```
```
# A tibble: 4 x 9
  baker       age student  tre1 rse1   tre2 rse2   tre3 rse3 
  <chr>     <dbl>   <dbl> <dbl> <chr> <dbl> <chr> <dbl> <chr>
1 Ruby        20.      1.   12. IN       3. SB       3. IN   
2 Julia       21.      0.    3. IN       4. IN       2. SB   
3 Benjamina   23.      0.    6. IN       3. IN       6. IN   
4 John        23.      2.   11. IN       1. SB       6. IN 
```

```{r}
young_bakers3 %>% 
   rename(tech_1 = t_first, result_1 = r_first)
```{{1}}
```
# A tibble: 4 x 9
  baker       age student tech_1 result_1  tre2 rse2   tre3 rse3 
  <chr>     <dbl>   <dbl>  <dbl> <chr>    <dbl> <chr> <dbl> <chr>
1 Ruby        20.      1.    12. IN          3. SB       3. IN   
2 Julia       21.      0.     3. IN          4. IN       2. SB   
3 Benjamina   23.      0.     6. IN          3. IN       6. IN   
4 John        23.      2.    11. IN          1. SB       6. IN   
```{{2}}


`@script`
To rename without re-ordering variables, you can use the dplyr rename function. 

Keep in mind that the helper functions only work with select, not rename, so rename works best if you only want to rename a handful of variables, and there are no patterns in the existing column names that you can use.


---
## Select & change names without reordering

```yaml
type: "FullCodeSlide"
key: "909a187c06"
disable_transition: true
```

`@part1`
```{r}
young_bakers3
```
```
# A tibble: 4 x 9
  baker       age student  tre1 rse1   tre2 rse2   tre3 rse3 
  <chr>     <dbl>   <dbl> <dbl> <chr> <dbl> <chr> <dbl> <chr>
1 Ruby        20.      1.   12. IN       3. SB       3. IN   
2 Julia       21.      0.    3. IN       4. IN       2. SB   
3 Benjamina   23.      0.    6. IN       3. IN       6. IN   
4 John        23.      2.   11. IN       1. SB       6. IN  
```

```{r}
young_bakers3 %>% 
   select(everything(), tech_ = starts_with("tr"),
          result_ = starts_with("rs"))
```{{1}}
```
# A tibble: 4 x 9
  baker       age student tech_1 result_1 tech_2 result_2 tech_3 result_3
  <chr>     <dbl>   <dbl>  <dbl> <chr>     <dbl> <chr>     <dbl> <chr>   
1 Ruby        20.      1.    12. IN           3. SB           3. IN      
2 Julia       21.      0.     3. IN           4. IN           2. SB      
3 Benjamina   23.      0.     6. IN           3. IN           6. IN      
4 John        23.      2.    11. IN           1. SB           6. IN   
```{{2}}


`@script`
But what if you want to keep the order, and you want to be able to batch rename using select. What can you do?

The answer is everything. We can use select with the everything helper function. If you use everything() first, the order of the columns will stay the same.


---
## What's in a name?

```yaml
type: "FullSlide"
key: "20452d965e"
```

`@part1`
```
i_use_snake_case
otherPeopleUseCamelCase
some.people.use.periods
And_aFew.People_RENOUNCEconvention
```


`@citations`
- R for Data Science (http://r4ds.had.co.nz/workflow-basics.html#whats-in-a-name)


`@script`
Finally, you may have noticed that in all of our examples so far, the column names have been pretty tame to start- no capital letters, punctuation, or spaces. All the variables in our bakers data frame are in "snake case", but you may not always be so lucky!


---
## Clean all variable names

```yaml
type: "FullSlide"
key: "ebf39761bc"
```

`@part1`
```{r}
young_bakers3
```{{1}}
```
# A tibble: 4 x 9
  Baker      Age `Student #` `Tr E1` `Rs E1` `Tr E2` `Rs E2` `Tr E3` `Rs E3`
  <chr>     <dbl>       <dbl>   <dbl> <chr>     <dbl> <chr>     <dbl> <chr>  
1 Ruby        20.          1.     12. IN           3. SB           3. IN     
2 Julia       21.          0.      3. IN           4. IN           2. SB     
3 Benjamina   23.          0.      6. IN           3. IN           6. IN     
4 John        23.          2.     11. IN           1. SB           6. IN  
```{{2}}

```{r}
library(janitor)
young_bakers3 %>% 
   clean_names()
```{{3}}
```
# A tibble: 4 x 9
  baker       age student_number tr_e1 rs_e1 tr_e2 rs_e2 tr_e3 rs_e3
  <chr>     <dbl>          <dbl> <dbl> <chr> <dbl> <chr> <dbl> <chr>
1 Ruby        20.             1.   12. IN       3. SB       3. IN   
2 Julia       21.             0.    3. IN       4. IN       2. SB   
3 Benjamina   23.             0.    6. IN       3. IN       6. IN   
4 John        23.             2.   11. IN       1. SB       6. IN  
```{{4}}


`@script`
Here is a messier, smaller version of our "bakers" data frame. These variables would be much easier to work with in snake case.

To reformat all variables in a data frame, we'll use the janitor package, and a function called "clean underscore names".

Notice that the new column names are snaked- they are lower case and all spaces were replaced by underscores. Also notice that the number symbol in the student variable name was replaced by a word. 

This is usually a great first step when working with very untame variable names.


---
## Let's practice!

```yaml
type: "FinalSlide"
key: "e01fd1ae85"
```

`@script`
Now let's try some examples.

