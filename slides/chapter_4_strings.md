---
title: Strings
key: 10d08245c8635a3e65a4b860e548786b
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch4_4.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch4_4.mp4
transformations:
    translateX: 55
    translateY: 0
    scale: 1

---
## Strings

```yaml
type: TitleSlide
key: 7ff94541ec
```

`@lower_third`
name: Alison Hill
title: Professor & Data Scientist

`@script`

This is our final instructional video for this course. You've learned a lot about how to work with data in R, and in this last video, I'll introduce you to tools for working with strings. But first, let's review some tools we have already covered for string wrangling.

---
## String wrangling

```yaml
type: FullSlide
key: a5f9c96946
```

`@part1`

```{r}
series5
# A tibble: 7 x 3
  baker   about                           showstopper                     
  <chr>   <chr>                           <chr>                           
1 Chetna  35 years, Fashion designer      Fusion Tiered Pies              
2 Luis    42 years, Graphic designer      Four Fruity Seasons Tower       
3 Martha  17 years, Student               Three Little Pigs Pie           
4 Nancy   60 years, Retired manager       Trio of Apple Pies              
5 Richard 38 years, Builder               Three Course Autumn Pie Feast   
6 Norman  66 years, Retired naval officer Pieful Tower                    
7 Kate    41 years, Furniture restorer    Rhubarb, Prune & Apple Pork Pies
```{{1}}

`@script`

One function we learned was from the tidyr package, called separate. This function is for splitting one column into two or more columns. 

In this data from series 5, separate can help us wrangle the second column named about. It has both the bakers' ages and their occupations.

---
## tidyr::separate

```yaml
type: FullSlide
key: ce56ef1ebc
```

`@part1`

```{r}
series5 <- series5 %>%  
  separate(about, into = c("age", "occupation"), sep = ", ")
  
series5
# A tibble: 7 x 4
  baker   age      occupation            showstopper                     
  <chr>   <chr>    <chr>                 <chr>                           
1 Chetna  35 years Fashion designer      Fusion Tiered Pies              
2 Luis    42 years Graphic designer      Four Fruity Seasons Tower       
3 Martha  17 years Student               Three Little Pigs Pie           
4 Nancy   60 years Retired manager       Trio of Apple Pies              
5 Richard 38 years Builder               Three Course Autumn Pie Feast   
6 Norman  66 years Retired naval officer Pieful Tower                    
7 Kate    41 years Furniture restorer    Rhubarb, Prune & Apple Pork Pies
```{{1}}

`@script`

Using separate, we can make two new columns. 

The first argument to the separate function is the name of the column we wish to separate, which is about. Next, we set the into argument, naming our two new columns in quotes with the combine function. Finally, we specify the separator in quotes using the sep argument. We use a comma and a space as the place to split the single column into two.

But we are still not done yet! We can also transform the age column into a numeric column using the readr package.


---
## readr::parse_number

```yaml
type: FullSlide
key: ee8066c09c
disable_transition: TRUE
```

`@part1`

```{r}
series5 <- series5 %>% 
  separate(about, into = c("age", "occupation"), sep = ", ") %>% 
  mutate(age = parse_number(age))
  
series5
# A tibble: 7 x 4
  baker     age occupation            showstopper                     
  <chr>   <dbl> <chr>                 <chr>                           
1 Chetna    35. Fashion designer      Fusion Tiered Pies              
2 Luis      42. Graphic designer      Four Fruity Seasons Tower       
3 Martha    17. Student               Three Little Pigs Pie           
4 Nancy     60. Retired manager       Trio of Apple Pies              
5 Richard   38. Builder               Three Course Autumn Pie Feast   
6 Norman    66. Retired naval officer Pieful Tower                    
7 Kate      41. Furniture restorer    Rhubarb, Prune & Apple Pork Pies
```{{1}}

`@script`

Using parse number within a mutate, we can extract out the bakers ages and drop the word years from all rows. Now we have a numeric age variable.

But, we can do even more with strings using a new package from the tidyverse.

---
## The `stringr` package

```yaml
type: FullSlide
key: 15a4d876dc
```

`@part1`

```
library(stringr) # once per work session
```

![](https://assets.datacamp.com/production/repositories/1613/datasets/39246f1d6442cdc8ac9661a29bffaea03710019e/hex-stringr.png)

`@citations`

- http://stringr.tidyverse.org 

`@script`

The `stringr` package makes it easier to work with strings in R. 

To transform variables that are character strings within a dataframe, you'll typically want to use stringr functions within a mutate.

Let's go over some basic functions.


---
## String Basics

```yaml
type: FullSlide
key: 3afa52a108
```

`@part1`

```{r}
series5 <- series5 %>% 
  mutate(baker = str_to_upper(baker),
         showstopper = str_to_lower(showstopper))
series5
# A tibble: 7 x 4
  baker     age occupation            showstopper                     
  <chr>   <dbl> <chr>                 <chr>                           
1 CHETNA    35. Fashion designer      fusion tiered pies              
2 LUIS      42. Graphic designer      four fruity seasons tower       
3 MARTHA    17. Student               three little pigs pie           
4 NANCY     60. Retired manager       trio of apple pies              
5 RICHARD   38. Builder               three course autumn pie feast   
6 NORMAN    66. Retired naval officer pieful tower                    
7 KATE      41. Furniture restorer    rhubarb, prune & apple pork pies
```{{1}}

`@script`

One of the quickest things to do with strings is to change the case of a string's content. Here, we can convert the baker names to upper case and the showstopper bakes to lower case using their respective functions.

---
## Detect String Patterns

```yaml
type: FullSlide
key: 5b1b7465e5
```

`@part1`

```{r}
series5 %>% 
  mutate(pie = str_detect(showstopper, "pie"))

# A tibble: 7 x 5
  baker     age occupation            showstopper                      pie  
  <chr>   <dbl> <chr>                 <chr>                            <lgl>
1 CHETNA    35. Fashion designer      fusion tiered pies               TRUE 
2 LUIS      42. Graphic designer      four fruity seasons tower        FALSE
3 MARTHA    17. Student               three little pigs pie            TRUE 
4 NANCY     60. Retired manager       trio of apple pies               TRUE 
5 RICHARD   38. Builder               three course autumn pie feast    TRUE 
6 NORMAN    66. Retired naval officer pieful tower                     TRUE 
7 KATE      41. Furniture restorer    rhubarb, prune & apple pork pies TRUE 
```{{1}}

`@script`

All functions in this package start with S-T-R underscore, which I'll pronounce as string.

To detect the presence or absence of a specific string pattern like "pie", we use str detect. This function always returns a logical, so it will either be TRUE or FALSE. 

This code uses string detect within a mutate to create this new logical variable named pie.

We can see that Luis' showstopper bake, called four fruity seasons tower, is the only one that didn't contain the word "pie."

---
## Replace String Patterns

```yaml
type: FullSlide
key: cbad1f5bf8
```

`@part1`

```{r}
series5 %>% 
  mutate(showstopper = str_replace(showstopper, "pie", "tart"))

# A tibble: 7 x 4
  baker     age occupation            showstopper                      
  <chr>   <dbl> <chr>                 <chr>                            
1 CHETNA    35. Fashion designer      fusion tiered tarts              
2 LUIS      42. Graphic designer      four fruity seasons tower        
3 MARTHA    17. Student               three little pigs tart           
4 NANCY     60. Retired manager       trio of apple tarts              
5 RICHARD   38. Builder               three course autumn tart feast   
6 NORMAN    66. Retired naval officer tartful tower                    
7 KATE      41. Furniture restorer    rhubarb, prune & apple pork tarts
```{{1}}

`@script`

Instead of only detecting a specific string pattern like "pie", we can also detect it and replace it with something else. Think of this like find and replace for strings.

Using the function string replace, we can replace all instances of "pie" with "tart" instead. Now Norman's pieful tower becomes a tartful tower.


---
## Remove String Patterns

```yaml
type: FullSlide
key: e548529573
```

`@part1`

```{r}
series5 %>% 
  mutate(showstopper = str_remove(showstopper, "pie"))

# A tibble: 7 x 4
  baker     age occupation            showstopper                  
  <chr>   <dbl> <chr>                 <chr>                        
1 CHETNA    35. Fashion designer      fusion tiered s              
2 LUIS      42. Graphic designer      four fruity seasons tower    
3 MARTHA    17. Student               "three little pigs "         
4 NANCY     60. Retired manager       trio of apple s              
5 RICHARD   38. Builder               three course autumn  feast   
6 NORMAN    66. Retired naval officer ful tower                    
7 KATE      41. Furniture restorer    rhubarb, prune & apple pork s
```{{1}}

`@script`

We could also remove a string all together instead of replacing it. Using the string remove function, we look for all occurrences of "pie" in the showstopper column and remove them. 

Notice that only Martha's creation, the three little pigs pie, now has quotes around it. This is because when we removed pie, there was trailing whitespace at the end of that string. We can remove that using another function called string trim.



---
## Trim white space

```yaml
type: FullSlide
key: 459db2c547
```

`@part1`

```{r}
series5 %>% 
  mutate(showstopper = str_remove(showstopper, "pie"),
         showstopper = str_trim(showstopper))

# A tibble: 7 x 4
  baker     age occupation            showstopper                  
  <chr>   <dbl> <chr>                 <chr>                        
1 CHETNA    35. Fashion designer      fusion tiered s              
2 LUIS      42. Graphic designer      four fruity seasons tower    
3 MARTHA    17. Student               three little pigs            
4 NANCY     60. Retired manager       trio of apple s              
5 RICHARD   38. Builder               three course autumn  feast   
6 NORMAN    66. Retired naval officer ful tower                    
7 KATE      41. Furniture restorer    rhubarb, prune & apple pork s
```{{1}}

`@script`

This function trims whitespace at the beginning or end of a string.

---
## Let's practice!

```yaml
type: FinalSlide
key: c3661c8a7f
```

`@script`

Now it's your turn.

