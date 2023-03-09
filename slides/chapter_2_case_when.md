---
title: Complex recoding with case_when
key: a4d8f6d1082082e04e67256a1b2c4cd2
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch4_1.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch4_1.mp4
transformations:
    translateX: 55
    translateY: 0
    scale: 1

---
## Complex recoding with case_when

```yaml
type: TitleSlide
key: ea694aabb7
```

`@lower_third`
name: Alison Hill
title: Professor & Data Scientist

`@script`

Earlier we saw how to use dplyr to recode values for individual variables. However, we need a different function for more complex recoding. This is where dplyr's case underscore when function shines! 

Case underscore when allows you to vectorise multiple if and else if statements. 

Why does this mean and why would you need it?

---
## Generations & age

```yaml
type: FullSlide
key: 15a4d876dc
```

`@part1`


![](https://assets.datacamp.com/production/repositories/1613/datasets/ba7722f8e4c44f2e73c31189011723643f2c89bc/pew_generations.png)

`@citations`

- http://www.pewresearch.org/topics/generations-and-age/

`@script`

We'll use case underscore when to group bakers into generations based on their birth year, using the generation definitions from the Pew Research Center.


---
## 

```yaml
type: FullSlide
key: 41c747d2ec
```

`@part1`

```{r}
?case_when
```{{1}}

Usage{{2}}

```{r}
case_when(...)
```{{2}}

![](http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/02.03_case_when_args.png){{3}}


`@script`

Think of case underscore when as a sequence of if-then pairs. 

The if part is on the left hand side. This side must be a logical, so it must be phrased in a way that the answer is only TRUE or FALSE. 

If true, then the value on the right is used as the replacement value.

At the end, there is an implicit ELSE. The default value for any ELSE is NA.

Let's see how this works in practice.


---
## Bakers

```yaml
type: FullSlide
key: 54c66e5b6a
```

`@part1`

```{r}
bakers
```
```
# A tibble: 10 x 2
   baker   birth_year
   <chr>        <dbl>
 1 Liam         1998.
 2 Martha       1997.
 3 Jason        1992.
 4 Stuart       1986.
 5 Manisha      1985.
 6 Simon        1980.
 7 Natasha      1976.
 8 Richard      1976.
 9 Robert       1959.
10 Diana        1945.
```

`@script`

We're going to use our bakers data again.



---
## Simple `if_else`

```yaml
type: FullSlide
key: fc7da196be
```

`@part1`

```{r}
bakers %>% 
   mutate(gen = if_else(between(birth_year, 1981, 1996), 
                        "millenial",
                        "not millenial"))
```
```
# A tibble: 10 x 3
   baker   birth_year gen          
   <chr>        <dbl> <chr>        
 1 Liam         1998. not millenial
 2 Martha       1997. not millenial
 3 Jason        1992. millenial    
 4 Stuart       1986. millenial    
 5 Manisha      1985. millenial    
 6 Simon        1980. not millenial
 7 Natasha      1976. not millenial
 8 Richard      1976. not millenial
 9 Robert       1959. not millenial
10 Diana        1945. not millenial
```{{1}}


`@script`

Within a mutate, we could use a simple if else to create a new variable called gen, which is "millenial" if a baker was born between 1981 and 1996. Here all other birth years are coded as "not millenial".

Between is a dplyr short cut for testing if a value falls within a given range. The boundaries are always inclusive.

---
## Multiple `if_else` pairs

```yaml
type: FullSlide
key: 181b70f1e1
```

`@part1`


```{r}
bakers %>% 
  mutate(gen = case_when(
    between(birth_year, 1965, 1980) ~ "gen_x",
    between(birth_year, 1981, 1996) ~ "millenial"
    ))
```
```
# A tibble: 10 x 3
   baker   birth_year gen      
   <chr>        <dbl> <chr>    
 1 Liam         1998. NA       
 2 Martha       1997. NA       
 3 Jason        1992. millenial
 4 Stuart       1986. millenial
 5 Manisha      1985. millenial
 6 Simon        1980. gen_x    
 7 Natasha      1976. gen_x    
 8 Richard      1976. gen_x    
 9 Robert       1959. NA       
10 Diana        1945. NA  
```{{1}}

`@script`

For multiple "if else" pairs, case underscore when becomes quite handy. 

Now we can string together multiple if then sequences, separated by commas. 

The left hand side defines the condition, like birth year between 1965 and 1980. A tilde separates the left from the right hand side. On the right is the new value, like gen underscore x.

As you read from left to right, think "if this, then that." If a baker's birth year is between the first range, then they are generation x; if in the second range, they are a millenial.

Notice that all other birth years are NA. Let's add more pairs.



---
## Make multiple bins

```yaml
type: FullSlide
key: 0ebef77282
```

`@part1`

```{r}
bakers %>% 
  mutate(gen = case_when(
    between(birth_year, 1928, 1945) ~ "silent",
    between(birth_year, 1946, 1964) ~ "boomer",
    between(birth_year, 1965, 1980) ~ "gen_x",
    between(birth_year, 1981, 1996) ~ "millenial",
    TRUE ~ "gen_z"
    ))
```{{1}}
```
# A tibble: 10 x 3
   baker   birth_year gen      
   <chr>        <dbl> <chr>    
 1 Liam         1998. gen_z    
 2 Martha       1997. gen_z    
 3 Jason        1992. millenial
 4 Stuart       1986. millenial
 5 Manisha      1985. millenial
 6 Simon        1980. gen_x    
 7 Natasha      1976. gen_x    
 8 Richard      1976. gen_x    
 9 Robert       1959. boomer   
10 Diana        1945. silent  
```{{2}}

`@script`


Each if then sequence is evaluated in order from top to bottom.

Let's walk through this code.

We code each generation based on birth year ranges for millenial, generation x, boomers, and the silent generation.

On the last line, TRUE is a catch-all for cases not meeting any of the earlier conditions. This ends up being all bakers whose birth year was after 1996. We code these as "generation z". 

All the right hand side values need to be of the same type. Here, we have created a character variable with 5 possible values.

---
## List of "if-then" pairs

```yaml
type: FullSlide
key: e6e4aae10c
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/0a840abed2f9dfbab46ac2b0cb6f322caa51c3ca/04.01_case_when_first.png)

`@script`

Here is our final case underscore when code broken down. On the left of the tilde think "if TRUE". On the right think "then replace with" Then go down the list.


---
## The last "if-then" pair

```yaml
type: FullSlide
key: f8558f5304
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/9e15d8956b57a7725f3ce9ba7b1ae5ba94330129/04.01_case_when_last.png)

`@script`

When TRUE is on the left of the last line, think "ELSE". Then the value on the right is the replacement.

---
## Know your new variable!

```yaml
type: FullSlide
key: 8ce11a3f87
```

`@part1`

```{r}
bakers
```{{1}}
```
# A tibble: 95 x 3
   baker     birth_year gen      
   <chr>          <dbl> <chr>    
 1 Liam           1998. gen_z    
 2 Martha         1997. gen_z    
 3 Flora          1996. millenial
 4 Michael        1996. millenial
 5 Julia          1996. millenial
 6 Ruby           1993. millenial
 7 Benjamina      1993. millenial
 8 Jason          1992. millenial
 9 James          1991. millenial
10 Andrew         1991. millenial
# ... with 85 more rows
```{{2}}

`@script`

Now that we have made this new categorical variable, let's explore it using the full bakers data, with 95 bakers across the first 8 series of the show.

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
```{{1}}
```
# A tibble: 5 x 3
  gen           n   prop
  <chr>     <int>  <dbl>
1 gen_x        40 0.421 
2 millenial    35 0.368 
3 boomer       17 0.179 
4 gen_z         2 0.0211
5 silent        1 0.0105
```{{2}}

`@script`

Let's say we want to know the amount of bakers in each generation. Then we can use dplyr's count verb to do this. Adding a mutate after count, we can also calculate the proportion of bakers in each generation. 40 out of 95 bakers have been from Generation X, while only 1 has been from the silent generation.


---
## Plot bakers by generation

```yaml
type: FullSlide
key: '5376934138'
```

`@part1`

```{r}
ggplot(bakers, aes(x = gen)) + geom_bar()
```

![](https://assets.datacamp.com/production/repositories/1613/datasets/5bbe5868189ac14e4c48f9a127797517c2a3fc15/04.01-bakers_by_gen.png)

`@script`

We can plot these counts as well using ggplot2 with geom underscore bar. Generation X has clearly led the way with the most bakers in the first 8 series.


---
## Let's practice!

```yaml
type: FinalSlide
key: 2a194c7551
```

`@script`

Now let's try some examples.

