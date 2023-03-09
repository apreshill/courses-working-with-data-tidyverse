---
title: Final thoughts
key: 524585d2791a9ae709a7b9e62e884f71
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch4_5.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch4_5.mp4
transformations:
    translateX: 55
    translateY: 0
    scale: 1

---
## Final thoughts

```yaml
type: TitleSlide
key: b048254293
```

`@lower_third`
name: Alison Hill
title: Professor & Data Scientist

`@script`

Congratulations on completing another course in DataCamp's "Learn the Tidyverse" track. In these four chapters, you've learned how to use data science tools from the tidyverse to work with data. These skills will pave the way for you to use R to do the analyses and create the visualizations you want, using the data that you have, which can often be a lot of work!


---
## Explore your data

```yaml
type: TwoColumns
key: ed1dfdf8cf
```

`@part1`

```{r}
bakeoff <- read_csv("bakeoff.csv")
```
```{r}
glimpse(bakeoff)
```{{1}}
```{r}
skim(bakeoff)
```{{2}}
```{r}
bakeoff %>% 
  count(series, baker) %>% 
  count(series)
```{{3}}
```{r}
ggplot(bakeoff, aes(episode)) + 
    geom_bar() + 
    facet_wrap(~series)
```{{4}}
```{r}
?read_csv
```{{5}}

`@part2`

![](https://assets.datacamp.com/production/repositories/1613/datasets/aa2ee75ec6d5ec16c3a5c2758c7b6bdb3705ce92/ch1_bakers-1.png){{4}}

`@script`

You started with reading data using the readr package, and exploring tibbles with functions like 
glimpse, 
skim, 
count, 
and ggplot. Along the way, you also learned about functions, missing values, variable types, and how to use the help reference docs.

---
## Tame your data

```yaml
type: FullSlide
key: 03a88eb5d8
```

`@part1`

```{r}
ratings <- read_csv("ratings.csv",
                    col_types = cols(
                      series = col_factor(levels = NULL))) %>%
           clean_names()
viewers_7day <- ratings %>% 
  mutate(bbc = recode(channel, "Channel 4" = 0,
                      .default = 1)) %>% 
  select(series, bbc, viewers_7day_ = ends_with("7day"))
```

`@script`

Next you learned to tame variable types, names, and values, to whip your data into the right shape for tidying.

---
## Tidy your data

```yaml
type: TwoRowsTwoColumns
key: 048aeec055
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/256f184aac6fd436423fe0b7179a2d16f6ba38dd/ch3_ratings_bar-1.png)

`@part2`

![](https://assets.datacamp.com/production/repositories/1613/datasets/c1e49f49de34e03ce344b83c083ec377bd00742c/ch3_ratings_line-1.png)

`@part3`

![](https://assets.datacamp.com/production/repositories/1613/datasets/f4183adb70ae2526513696a060b3e224f4595ac7/ch3_ratings_dumbbell-1.png)

`@part4`

![](https://assets.datacamp.com/production/repositories/1613/datasets/008dc7cae5dde948642f431aa780a56349d9102d/ch3_ratings_percent-1.png)

`@script`

You then completed a masterclass on tidying... drawing on all the skills you learned so far plus gathering, separating, and spreading... to create visualizations that tell a story. 


---
## Transform your data

```yaml
type: FullSlide
key: de167d42c0
```

`@part1`

```{r}
bakers <- bakers %>% 
  mutate(gen = case_when(
    between(birth_year, 1928, 1945) ~ "silent",
    between(birth_year, 1946, 1964) ~ "boomer",
    between(birth_year, 1965, 1980) ~ "gen_x",
    between(birth_year, 1981, 1996) ~ "millenial",
    TRUE ~ "gen_z"
    ))
```
```{r}
bakers <- bakers %>% 
    mutate(gen = fct_relevel(gen, "silent", "boomer", 
                             "gen_x", "millenial", "gen_z"))
```
```{r}
ggplot(bakers, aes(x = gen)) + geom_bar()
```
```{r}
bakers <- bakers %>% 
  mutate(last_date_appeared_us = dmy(last_date_appeared_us),
         occupation = str_to_lower(occupation), 
         student = str_detect(occupation, "student"))
```

`@script`

Finally, you got a taste for transforming data using the dplyr, forcats, lubridate, and stringr packages.

---
## On your own

```yaml
type: FullSlide
key: f792621bca
```

`@part1`

![](https://www.rstudio.com/wp-content/uploads/2014/07/RStudio-Logo-Blue-Gradient.png)

https://www.datacamp.com/courses/working-with-the-rstudio-ide-part-1

`@script`

I hope this course has inspired you to work with your own data using R and the tidyverse. When you do, I recommend using the RStudio Integrated Development Environment. This makes a huge difference when working with data on your own. DataCamp has a course to help get you started.

---
## R Projects in RStudio

```yaml
type: FullSlide
key: 643aec5f88
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/9aa87d996faf266c4afe1ad6fbf5e8a1dae3e8cf/04.04_projects.png)

https://www.datacamp.com/courses/working-with-the-rstudio-ide-part-1

`@script`

Even better, create a new R Project in RStudio every time you want to work with a new data set.


---
## Project-oriented workflows

```yaml
type: FullSlide
key: 63856cc51f
```

`@part1`

```
bakeoff
├── bakeoff.Rproj
├── data
|   └──  bakers.csv <-- this is my file!
└── figures
```

```{r}
# install.packages("here")
library(here)
bakers <- read_csv(here("data", "bakers.csv"))
```

The `here` package: https://here.r-lib.org/

`@script`

Finally, when working on a local computer, I recommend using the here package when reading data files into R. Used within a project, the main function, also named "here", will make working with your file paths just work easier without the pain of messing with working directories.


---
## What's next?

```yaml
type: FullSlide
key: 6e745db85a
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/364e9774a11329d605404e601962ff23a16e490a/dc_tidyverse.png) {{1}}

`@script`

We've given you a glimpse of things you can do in the tidyverse, but there is always more to learn! 

---
## What's next?

```yaml
type: TwoRowsTwoColumns
key: 01e414919d
```

`@part1`
![](https://assets.datacamp.com/production/repositories/1613/datasets/926e5e55d513481cb8a0f5a2d38a04c416216270/dc_working_dates_times.png) {{1}}

`@part2`
![](https://assets.datacamp.com/production/repositories/1613/datasets/3e5dee45c7315357ba7687f7022df1383ea446f7/dc_stringr.png) {{2}}

`@part3`
![](https://assets.datacamp.com/production/repositories/1613/datasets/ae5b3d8360f6c489edb3c7c9f8d3364b2cf6385e/dc_qualitative.png) {{3}}

`@part4`
![](https://assets.datacamp.com/production/repositories/1613/datasets/4e095168ea491fa7821bd334ee00c3839fde1120/dc_communicating.png) {{4}}

`@script`
DataCamp instructors can teach you more about working with dates and times, strings, and categorical variables. We also have courses to help you level up your ggplot2 skills to create custom visualizations, and how to do data story-telling with tools like R Markdown. 
---
## What's next?

```yaml
type: TwoRowsTwoColumns
key: a2198eedde
disable_transition: TRUE
```

`@part1`
![](https://assets.datacamp.com/production/repositories/1613/datasets/926e5e55d513481cb8a0f5a2d38a04c416216270/dc_working_dates_times.png) 

`@part2`
![](https://assets.datacamp.com/production/repositories/1613/datasets/3e5dee45c7315357ba7687f7022df1383ea446f7/dc_stringr.png)

`@part3`
![](https://assets.datacamp.com/production/repositories/1613/datasets/ae5b3d8360f6c489edb3c7c9f8d3364b2cf6385e/dc_qualitative.png)

`@part4`
![](https://assets.datacamp.com/production/repositories/1613/datasets/452262d28e9ad874c62500d0003e2c2164e3c196/dc_modeling_data_tidyverse.png)

`@script`
And finally, if you want more statistics in your life, you can learn more about statistical modeling and inference in R and the tidyverse. But for now, you are a star coder- enjoy using your new skills and working with your data...

---
## Congratulations!

```yaml
type: FinalSlide
key: '643e478735'
```

`@script`

Congratulations!


