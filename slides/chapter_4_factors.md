---
title: Factors
key: 3263d388847709989da3eff7a205c9d8
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch4_2.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch4_2.mp4
transformations:
    translateX: 55
    translateY: 0
    scale: 1

---
## Factors

```yaml
type: TitleSlide
key: eb880a29e0
```

`@lower_third`
name: Alison Hill
title: Professor & Data Scientist

`@script`

We've already worked with some variables as factors so far, but factors in R often need special handling. 

---
## The `forcats` package

```yaml
type: FullSlide
key: 15a4d876dc
```

`@part1`

```
library(forcats) # once per work session
```

![](https://assets.datacamp.com/production/repositories/1613/datasets/f5a57bf069c908e5f0f0fdb67b38842b5c5d928d/forcats.png)

`@citations`

- http://forcats.tidyverse.org 

`@script`


The forcats package is for working with categorical variables. The functions in forcats can help you solve common problems with factors. But first...

---
## What is a factor?

```yaml
type: FullSlide
key: bcfca02eab
```

`@part1`

> "In R, factors are used to work with categorical variables, variables that have a fixed and known set of possible values."

`@citations`

- Garrett Grolemund & Hadley Wickham, http://r4ds.had.co.nz/factors.html

`@script`

...What is a factor? Factors are categorical variables, where the variables take on a fixed and known set of possible values. 

Factors can be character variables, but they can also be numeric.

---
## Count bakers by generation

```yaml
type: FullSlide
key: d1c8ef5c34
```

`@part1`

```{r}
bakers %>% 
  count(gen, sort = TRUE) %>% 
  mutate(prop = n / sum(n))
# A tibble: 5 x 3
  gen           n   prop
  <chr>     <int>  <dbl>
1 gen_x        40 0.421 
2 millenial    35 0.368 
3 boomer       17 0.179 
4 gen_z         2 0.0211
5 silent        1 0.0105
```{{1}}

`@script`

In the last video, we made a variable called gen, which stands for generation. This variable could take on one of 5 values, which were characters. 

Notice that gen is currently a character variable, but it should be a factor.


---
## Plot bakers by generation

```yaml
type: FullSlide
key: '5376934138'
```

`@part1`

```{r}
ggplot(bakers, aes(x = gen)) +
    geom_bar()
```{{1}}

![](https://assets.datacamp.com/production/repositories/1613/datasets/5bbe5868189ac14e4c48f9a127797517c2a3fc15/04.01-bakers_by_gen.png){{2}}


`@script`

For example, if we plot generation on the x-axis, you can see that because it is a character variable, ggplot sorts the values alphabetically. By treating gen as a factor, we can make our plot make more sense.



---
## Reorder from most to least bakers

```yaml
type: FullSlide
key: 5474b25b36
```

`@part1`


```{r}
ggplot(bakers, aes(x = fct_infreq(gen))) +
  geom_bar()
```{{1}}

![](https://assets.datacamp.com/production/repositories/1613/datasets/0e00f7743411aa198edf6c9d193b893878e46922/04.02-bakers_infreq.png){{2}}


`@script`

To reorder the x axis by decreasing frequency, we can use forcats. All functions in this package start with F-C-T underscore, which I'll pronounce as factor. Here we use factor infreq to make it easier to see that the most bakers came from generation X.



---
## Reorder from least to most bakers

```yaml
type: FullSlide
key: c6a64844ad
```

`@part1`

```{r}
ggplot(bakers, aes(x = fct_rev(fct_infreq(gen)))) +
  geom_bar()
```{{1}}

![](https://assets.datacamp.com/production/repositories/1613/datasets/455301c8b5e430cbbd77ae57943168e8e3b526ae/04.02-bakers_revfreq.png){{2}}

`@script`

Adding factor rev in front then reverses the ordering of the generations on the x axis by increasing frequency, highlighting that the fewest bakers came from the silent generation and generation Z.




---
## Relevel using natural order

```yaml
type: FullSlide
key: da44756466
```

`@part1`


![](https://assets.datacamp.com/production/repositories/1613/datasets/ba7722f8e4c44f2e73c31189011723643f2c89bc/pew_generations.png)

`@citations`

- http://www.pewresearch.org/topics/generations-and-age/

`@script`

Another way to order the bars in our chart is to use the natural chronological order of the five generations, ordering by oldest to youngest along the x-axis. To do this, we need to change the factor levels in the actual data. 

---
## Reorder by hand

```yaml
type: FullSlide
key: 73cef11e31
```

`@part1`

```{r}
bakers <- bakers %>% 
    mutate(gen = fct_relevel(gen, "silent", "boomer", 
                             "gen_x", "millenial", "gen_z"))
```{{1}}
```{r}
bakers %>% 
  dplyr::pull(gen) %>% 
  levels()
[1] "silent"    "boomer"    "gen_x"     "millenial" "gen_z"    
```{{2}}

`@script`

We do this within a mutate using the factor relevel function. We'll order our generations from oldest to youngest.

We can check our new levels with two steps. Starting with the data frame, use dplyr pull to extract the factor variable as a vector. Using the pipe operator next, we use the levels functions, which only works for factor variables. We see that the levels match the order we wanted.

Now let's try our plot out one last time.


---
## Reorder generations chronologically

```yaml
type: FullSlide
key: 0a59b3e5a4
disable_transition: true
```

`@part1`

```{r}
bakers <- bakers %>% 
    mutate(gen = fct_relevel(gen, "silent", "boomer", 
                             "gen_x", "millenial", "gen_z"))
```
```{r}
ggplot(bakers, aes(x = gen)) + geom_bar()
```{{1}}

![](https://assets.datacamp.com/production/repositories/1613/datasets/c9db7a9d89dcaab109e7ca61596053ddcb83d989/04.02-bakers_relevel.png){{2}}

`@script`



So now the xaxis is not ordered by frequency, but based on the natural order of the generation variable. 

To do this, we had to manually relevel the generation variable by hand. Because we changed the underlying factor levels in our actual data frame using dplyr mutate, when we went to make the plot, the changes we made to the gen variable were already saved.

Let's make one final tweak to our plot...



---
## Fill fail

```yaml
type: FullSlide
key: 388529a0a9
```

`@part1`

```{r}
ggplot(bakers, aes(x = gen, fill = series_winner)) +
    geom_bar()
```

![](https://assets.datacamp.com/production/repositories/1613/datasets/72aebeb07dd34c8d6409be997e5d9cbfb5babfc4/04.02-bakers_fillfail.png)

`@script`


Let's fill the bars to see how many series winners came from each generation. To do that, we add a fill aesthetic and map it onto the series winner variable. This variable is a 1 if the baker is a winner and a zero if not.

So why does this plot fail? The answer is that series winner is also a factor but ggplot is treating it like a continuous number. Let's cast it as a factor instead.


---
## Fill win!

```yaml
type: FullSlide
key: ca2593b7b8
```

`@part1`

```{r}
bakers <- bakers %>% 
    mutate(series_winner = as.factor(series_winner))
```
```{r}
ggplot(bakers, aes(x = gen, fill = series_winner)) + geom_bar()
```{{1}}

![](https://assets.datacamp.com/production/repositories/1613/datasets/d102937e2a809df5052454ef445b4dadfeb1f342/04.02-bakers_fillwin.png){{1}}

`@script`

To cast a variable as a factor, you can use a mutate and edit an existing variable by replacing it. On the right side of the equal sign, you use the function as dot factor then the variable name you want to cast as a factor.

Now fill works like we want, and we can see that even though more generation x bakers have been on the show, there have been far more millenial winners.


---
## Fill win!

```yaml
type: FullSlide
key: 3a8a4c4737
disable_transition: true
```

`@part1`

```{r}
ggplot(bakers, aes(x = gen, fill = as.factor(series_winner))) +
    geom_bar()
```{{1}}

![](https://assets.datacamp.com/production/repositories/1613/datasets/d102937e2a809df5052454ef445b4dadfeb1f342/04.02-bakers_fillwin.png)

`@script`

You can also use as dot factor when you map your aesthetics on to variables with ggplot.  


---
## Let's practice!

```yaml
type: FinalSlide
key: 1c4eaf2a31
```

`@script`

Time to put this into practice.


