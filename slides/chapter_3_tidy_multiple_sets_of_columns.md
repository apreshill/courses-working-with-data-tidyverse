---
title: Tidy multiple sets of columns
key: 61848b3d31d212772f224b6cdc14dfbf
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch3_5.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch3_5.mp4
transformations:
    translateX: 55
    translateY: 0
    scale: 1

---
## Tidy multiple sets of columns

```yaml
type: TitleSlide
key: 76baa656c6
```

`@lower_third`
name: Alison Hill
title: Professor & Data Scientist

`@script`

Now that we've learned how to gather, separate, unite, and spread, we're going to tackle an untidy data problem that requires us to use all our `tidyr` tools.


---
## Multiple sets to gather

```yaml
type: FullSlide
key: 434aff0ad2
```

`@part1`

```{r}
students
```{{1}}
```
# A tibble: 4 x 5
  student math_num chem_num math_let chem_let
  <chr>      <int>    <int> <chr>    <chr>   
1 Emma          80       98 B-       A+      
2 Harry         90       75 A-       C       
3 Ruby          95       70 A        C-      
4 Zainab        85       90 B        A-    
```{{2}}

```{r}
patients
```{{3}}
```
# A tibble: 4 x 5
  patient height_1 height_2 weight_1 weight_2
  <chr>      <int>    <int>    <int>    <int>
1 Emma          54       59       72       95
2 Harry         55       58       68       90
3 Ruby          55       60       70       94
4 Zainab        53       58       71       95
```{{4}}

`@script`

A common tidying problem is having multiple sets of columns that are untidy. 

For example, you could have numeric and letter grades per student for both math and chemistry.

Or you could have height and weight per patient measured at two visits.

The common thread in these scenarios is that you have repeated measures of more than one variable, and each repeat in one variable has a meaningful link to the repeat in another variable. But this wide, untidy format makes those links hard to see.


---
## Tidied students

```yaml
type: FullSlide
key: dac18ff792
```

`@part1`


```{r}
students
```
```
# A tibble: 4 x 5
  student math_num chem_num math_let chem_let
  <chr>      <int>    <int> <chr>    <chr>   
1 Emma          80       98 B-       A+      
2 Harry         90       75 A-       C       
3 Ruby          95       70 A        C-      
4 Zainab        85       90 B        A-    
```


```
# A tibble: 8 x 4
  student subject let     num
  <chr>   <chr>   <chr> <int>
1 Emma    chem    A+       98
2 Emma    math    B-       80
3 Harry   chem    C        75
4 Harry   math    A-       90
5 Ruby    chem    C-       70
6 Ruby    math    A        95
7 Zainab  chem    A-       90
8 Zainab  math    B        85
```{{1}}

`@script`

Tidying students lets us see letter and number grades within the same class.

---
## Tidied patients

```yaml
type: FullSlide
key: 1a4978edc4
```

`@part1`


```{r}
patients
```
```
# A tibble: 4 x 5
  patient height_1 height_2 weight_1 weight_2
  <chr>      <int>    <int>    <int>    <int>
1 Emma          54       59       72       95
2 Harry         55       58       68       90
3 Ruby          55       60       70       94
4 Zainab        53       58       71       95 
```


```
# A tibble: 8 x 4
  patient visit height weight
  <chr>   <chr>  <int>  <int>
1 Emma    1         54     72
2 Emma    2         59     95
3 Harry   1         55     68
4 Harry   2         58     90
5 Ruby    1         55     70
6 Ruby    2         60     94
7 Zainab  1         53     71
8 Zainab  2         58     95
```{{1}}

`@script`

Tidying patients lets us see height and weight measures for the same visit.



---
## Untidy vs. tidy

```yaml
type: TwoRows
key: 71a20d1d70
```

`@part1`

```{r}
juniors_multi
```
```
# A tibble: 4 x 7
  baker  score_1 score_2 score_3 guess_1  guess_2  guess_3 
  <chr>    <int>   <int>   <int> <chr>    <chr>    <chr>   
1 Emma         1       0       1 cinnamon cloves   nutmeg  
2 Harry        1       1       1 cinnamon cardamom nutmeg  
3 Ruby         1       0       1 cinnamon cumin    nutmeg  
4 Zainab       0      NA       0 cardamom NA       cinnamon
```

`@part2`

```{r}
juniors_tidy %>% slice(6)
```{{1}}
```
# A tibble: 12 x 4
   baker  order guess    score
   <chr>  <int> <chr>    <chr>
 1 Emma       1 cinnamon 1    
 2 Emma       2 cloves   0    
 3 Emma       3 nutmeg   1    
 4 Harry      1 cinnamon 1    
 5 Harry      2 cardamom 1    
 6 Harry      3 nutmeg   1    
```{{1}}

`@script`

This kind of untidy data comes up a lot. Let's go back to our Juniors data. Here, our two sets of untidy columns are scores and guesses. The score and guess variables are linked by a common trial number. So score underscore one and guess underscore one are both about the first trial.

A tidy version of this data would have:
one row per observation- so one row per baker and trial,
one column for scores, and
one column for guesses.

---
## Step 1: `gather` 

```yaml
type: FullSlide
key: 7250f50ad3
```

`@part1`


```{r}
juniors_multi %>% 
  gather(key = "key", value = "value", score_1:guess_3)
```
```
# A tibble: 24 x 3
   baker key     value   
   <chr> <chr>   <chr>   
 1 Emma  guess_1 cinnamon
 2 Emma  guess_2 cloves  
 3 Emma  guess_3 nutmeg  
 4 Emma  score_1 1       
 5 Emma  score_2 0       
 6 Emma  score_3 1       
 7 Harry guess_1 cinnamon
 8 Harry guess_2 cardamom
 9 Harry guess_3 nutmeg  
10 Harry score_1 1       
# ... with 14 more rows
```
  

`@script`

The first step is to gather all columns into one. This means that temporarily, we will have made our data even less tidy because now we have values like the spice names and the numeric scores in the same column.

We know that later on we will need to spread, but first we have to create the variable we want to spread by. 

Our two sets of variables are guesses and scores. So we need a single column that only contains the word "guess" or "score". We are almost there with the current key column, but we'll have to separate the current key column to get there.

---
## Step 2: `separate`

```yaml
type: FullSlide
key: 8b26fa15d2
```

`@part1`

```{r}
juniors_multi %>% 
  gather(key = "key", value = "value", score_1:guess_3) %>% 
  separate(key, into = c("var", "order"), convert = TRUE) 
```
```
# A tibble: 24 x 4
   baker var   order value   
   <chr> <chr> <int> <chr>   
 1 Emma  guess     1 cinnamon
 2 Emma  guess     2 cloves  
 3 Emma  guess     3 nutmeg  
 4 Emma  score     1 1       
 5 Emma  score     2 0       
 6 Emma  score     3 1       
 7 Harry guess     1 cinnamon
 8 Harry guess     2 cardamom
 9 Harry guess     3 nutmeg  
10 Harry score     1 1       
# ... with 14 more rows
```{{2}}

`@script`

We pipe the output of gather to separate next. We separate the key column we just made into two new columns: var and order. Recall that separate by default splits the column at a non-alphanumeric character. The original key column had an underscore separating the values guess and 1 for example, so the result of separate gives us the column we need. 



---
## Before and after spread

```yaml
type: TwoColumns
key: 90d66b094a
```

`@part1`

```
# A tibble: 24 x 4
   baker var   order value   
   <chr> <chr> <int> <chr>   
 1 Emma  guess     1 cinnamon
 2 Emma  guess     2 cloves  
 3 Emma  guess     3 nutmeg  
 4 Emma  score     1 1       
 5 Emma  score     2 0       
 6 Emma  score     3 1       
 7 Harry guess     1 cinnamon
 8 Harry guess     2 cardamom
 9 Harry guess     3 nutmeg  
10 Harry score     1 1       
# ... with 14 more rows
```

`@part2`

```
# A tibble: 12 x 4
   baker  order guess    score
   <chr>  <int> <chr>    <chr>
 1 Emma       1 cinnamon 1    
 2 Emma       2 cloves   0    
 3 Emma       3 nutmeg   1    
 4 Harry      1 cinnamon 1    
 5 Harry      2 cardamom 1    
 6 Harry      3 nutmeg   1    
 7 Ruby       1 cinnamon 1    
 8 Ruby       2 cumin    0    
 9 Ruby       3 nutmeg   1    
10 Zainab     1 cardamom 0    
11 Zainab     2 NA       NA   
12 Zainab     3 cinnamon 0 
```{{1}}


`@script`

That column is called "var", and we want to use it to spread the last column called "value". The var column only contains "guess" or "score".

This means the guesses will get stored in one column and the numeric scores will get stored in the second new column.

---
## Step 3: `spread`

```yaml
type: FullSlide
key: 0fd41dc022
```

`@part1`

```{r}
juniors_multi %>% 
  gather(key = "key", value = "value", score_1:guess_3) %>% 
  separate(key, into = c("var", "order"), convert = TRUE) %>% 
  spread(var, value) 
```
```
# A tibble: 12 x 4
   baker  order guess    score
   <chr>  <int> <chr>    <chr>
 1 Emma       1 cinnamon 1    
 2 Emma       2 cloves   0    
 3 Emma       3 nutmeg   1    
 4 Harry      1 cinnamon 1    
 5 Harry      2 cardamom 1    
 6 Harry      3 nutmeg   1    
 7 Ruby       1 cinnamon 1    
 8 Ruby       2 cumin    0    
 9 Ruby       3 nutmeg   1    
10 Zainab     1 cardamom 0    
11 Zainab     2 NA       NA   
12 Zainab     3 cinnamon 0 
```

`@script`

Here is the final sequence of code. Remember that the first argument for spread is the variable determines how many new columns you create. The second argument is the variable that determines what values go into those new columns. Our tidy result here has four columns total, and each stores only one variable type.



---
## Untidy to tidy

```yaml
type: TwoRows
key: 81fee2d7f7
```

`@part1`

```{r}
juniors_multi
```
```
# A tibble: 4 x 7
  baker  score_1 score_2 score_3 guess_1  guess_2  guess_3 
  <chr>    <int>   <int>   <int> <chr>    <chr>    <chr>   
1 Emma         1       0       1 cinnamon cloves   nutmeg  
2 Harry        1       1       1 cinnamon cardamom nutmeg  
3 Ruby         1       0       1 cinnamon cumin    nutmeg  
4 Zainab       0      NA       0 cardamom NA       cinnamon
```

`@part2`

```{r}
juniors_tidy %>% slice(6)

# A tibble: 12 x 4
   baker  order guess    score
   <chr>  <int> <chr>    <chr>
 1 Emma       1 cinnamon 1    
 2 Emma       2 cloves   0    
 3 Emma       3 nutmeg   1    
 4 Harry      1 cinnamon 1    
 5 Harry      2 cardamom 1    
 6 Harry      3 nutmeg   1    
```{{1}}

`@script`

What is also important is what stayed the same: the order column is still here, which is key to this whole operation working. Recall that order used to be in the original variable names, like guess underscore 1, guess underscore 2, and so on. Now order is a column, and we can see that we have 3 trials for each baker. 

But now it is clearer that each row corresponds to a single trial per baker, and we have two variables about that observation: the guess and the score.

---
## Let's practice!

```yaml
type: FinalSlide
key: 3fc70e14ea
```

`@script`

Now it's your turn.

