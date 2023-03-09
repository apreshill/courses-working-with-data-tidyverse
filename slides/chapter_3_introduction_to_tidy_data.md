---
title: Introduction to Tidy Data
key: e92f30b47653c749db6e33cac4d23e1e
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch3_1.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch3_1.mp4
transformations:
    translateX: 58
    translateY: 0
    scale: 0.9

---
## Introduction to Tidy Data

```yaml
type: TitleSlide
key: bf255f606e
```

`@lower_third`
name: Alison Hill
title: Professor & Data Scientist

`@script`

In the last set of exercises, you tamed the ratings data. Let's take a look at some graphs to explore it.

---
## Graph 1

```yaml
type: FullImageSlide
key: cb343da81f
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/0e686a52da94bfa5373dbe9688240d7b89e8c412/ratings_long_bars.png)

`@script`

Here is a plot of all the epsiode viewers, colored by series. We see a steady increasing trend, until the last series, where viewers seems to dip. This type of plot is helpful for exploring and understanding your data, but to make it, the data needs to be tidy.


---
## Graph 2

```yaml
type: FullImageSlide
key: 2827301ab9
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/078a53fb382fd1673f84f840d87d9fc6eb0dce59/ratings-lines-facet.png)

`@script`

We could also create a line plot of episode viewers, facetted by series. Most finales have a noticeable peak in viewers for the finale, but series 8 looks a little flatter. Again, the data needs to be tidy to make this plot.


---
## The Great British Bake Off Series 8

```yaml
type: FullSlide
key: 4bcc40f1b3
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/2f1a0418d384f985902c4c1a20a580f1abe830c3/gbbo-spoiler-tweet.png){{1}}


`@script`

So, what happened in series 8? For fans of the show, a lot: the show switched channels, and both original hosts plus judge Mary Berry left. Also, the new judge accidentally tweeted out the winner before the finale aired. 

Here is her apology for the spoiler. We'll tidy the ratings data to explore whether this spoiler had an effect on finale viewers. But first, let's talk about tame versus tidy data.

---
## 

```yaml
type: FullSlide
key: 8e01c261a6
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/08e57d61e71c22a756aaa6b646686cbe5476b18a/03.01_tame_v_tidy.png)

`@script`

Tame data is not the same as tidy data. You can have wild data that is tidy but not tame at all. Or you can have very tame data that is untidy. So, what is the difference?

---
## Tame but un-tidy

```yaml
type: FullSlide
key: 7b5b57f00a
```

`@part1`

```{r}
juniors_untidy

# A tibble: 4 x 4
  baker  cinnamon_1 cardamom_2 nutmeg_3
  <chr>       <int>      <int>    <int>
1 Emma            1          0        1
2 Harry           1          1        1
3 Ruby            1          0        1
4 Zainab          0         NA        0
``` {{1}}


`@script`

Here is some tame un-tidy data. This is data from an episode of Junior Bake-off, a spin-off of the Great British Bake Off with kids. In one challenge, the bakers guessed three different spices based on smell alone: cinnamon, cardamom, and nutmeg. If they guessed right, they got a score of 1. This data is tame because:
- It is a rectangle.
- The variable names are easy to work with.
- Each column contains only one variable.
- And the variable types match the values.

But this data is not tidy. Look at the last three variables. We have a column for each of the three trials. The column names are values, not variable names. To be tidy, the spice and trial number should be in columns. ...Next, look at the rows. Each row is a baker, not an observation. Emma's three scores are spread across three columns. To be tidy, these three values should all be in the *same* column, on different rows.

---
## Tidy data

```yaml
type: FullSlide
key: 5bfa21e855
```

`@part1`

```{r}
juniors_tidy

# A tibble: 12 x 4
   baker  spice    order correct
   <chr>  <chr>    <int>   <int>
 1 Emma   cinnamon     1       1
 2 Harry  cinnamon     1       1
 3 Ruby   cinnamon     1       1
 4 Zainab cinnamon     1       0
 5 Emma   cardamom     2       0
 6 Harry  cardamom     2       1
 7 Ruby   cardamom     2       0
 8 Zainab cardamom     2      NA
 9 Emma   nutmeg       3       1
10 Harry  nutmeg       3       1
11 Ruby   nutmeg       3       1
12 Zainab nutmeg       3       0
``` {{1}}


`@script`


Here is the same data tidied. Tidy data tends to be long, whereas un-tidy data is wider. Tidy data has 2 primary features:
- Each variable is one column: baker, spice, order, and correct. Here, spice and order always match, but that doesn't have to be the case.
- Each observation is a row. We have three rows for each baker; one for each trial. This is why tidy data looks more repetitive. 
Emma, Harry, and Ruby all guessed Cinnamon correctly on the first trial, but only Harry guessed Cardamom correctly on the second. And Zainab is the only baker who missed Nutmeg.


---
## Who won? Count it!

```yaml
type: FullSlide
key: b078e1ef45
```

`@part1`

```{r}
juniors_tidy %>% 
  count(baker, wt = correct)

# A tibble: 4 x 2
  baker      n
  <chr>  <int>
1 Emma       2
2 Harry      3
3 Ruby       2
4 Zainab     0
``` {{1}}

`@script`

Using count, we see that Harry guessed all spices correctly, while Zainab guessed none correctly. We use the weight argument here, which sums the values in the correct column instead of counting rows.


---
## Who won? Plot it!

```yaml
type: FullSlide
key: 0792f32d7d
```

`@part1`

```{r}
ggplot(juniors_tidy, aes(baker, correct)) +
  geom_col()
``` {{1}}
![](https://assets.datacamp.com/production/repositories/1613/datasets/0d3ff0e6a9e3fd7801a151d536b8ad9c8c123fc9/03.01_junior_tidy_by_baker.png) {{2}}

`@script`

We could also make a quick bar chart in ggplot using geom underscore col to see total scores per baker.


---
## Which spice was the hardest to guess? Count it!

```yaml
type: FullSlide
key: d301bc78c0
```

`@part1`

```{r}
juniors_tidy %>% 
   count(spice, wt = correct, sort = TRUE)

# A tibble: 3 x 2
  spice        n
  <chr>    <int>
1 cinnamon     3
2 nutmeg       3
3 cardamom     1
``` {{1}}

`@script`

Again using the count function, we can also see that only one baker guessed cardamom correctly.


---
## Which spice was the hardest to guess? Plot it!

```yaml
type: FullSlide
key: 454bf769a7
```

`@part1`

```{r}
ggplot(juniors_tidy, aes(spice, correct)) +
  geom_col()
``` {{1}}

![](https://assets.datacamp.com/production/repositories/1613/datasets/15a9100fa20b4b3f0bb735eb6e7bd68d90e78f92/03.01_junior_tidy_by_spice.png) {{2}}

`@script`


And we can make a bar chart in ggplot to visualize the counts by spice.

---


```yaml
type: FullSlide
key: 7fb70d2cab
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/b25bb9f9dfd00eb1d0da88f75d0abc356a1edaaa/03.01_junior_tame_v_tidy.png)

`@script`

Remember the rules of tidy data:

Each variable forms a column.

Each observation forms a row.

---
## Let's get to work!

```yaml
type: FinalSlide
key: de8b66d81c
```

`@script`

Now it's time to put your tidy data knowledge to the test!

