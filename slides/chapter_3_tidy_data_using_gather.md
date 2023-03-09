---
title: Gather
key: e74bbfbd8364c8f60e5109620419c429
video_link:
    hls: https://s3.amazonaws.com/videos.datacamp.com/transcoded/6012_working_with_data_in_the_tidyverse/v1/hls-6012_ch3_2.master.m3u8
    mp4: https://s3.amazonaws.com/videos.datacamp.com/transcoded_mp4/6012_working_with_data_in_the_tidyverse/v1/6012_ch3_2.mp4
transformations:
    translateX: 58
    translateY: 0
    scale: 0.9

---
## Gather

```yaml
type: TitleSlide
key: c499219f47
```

`@lower_third`
name: Alison Hill
title: Professor & Data Scientist

`@script`

Now that we know what tidy data is, let's learn about how to get our data from untidy to tidy.

---
## The `tidyr` package

```yaml
type: FullSlide
key: 15a4d876dc
```

`@part1`

```
library(tidyr) # once per work session
```

![](https://assets.datacamp.com/production/repositories/1613/datasets/5f8c22ec53a1ac61684f3e8d59c623d09227d6b9/hex-tidyr.png)

`@citations`

- http://tidyr.tidyverse.org 

`@script`

One of the most important functions for tidying data in the `tidyr` package is called gather.




---

```yaml
type: FullSlide
key: e6e5223c49
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/a5f76caca56340c0f1ab773376a5d5879a426879/03.01_gather_overview.png)

`@script`

The gather function collapses multiple columns into two columns. Using gather is sometimes referred to as reshaping your data from wide to long, because it often reduces the number of columns and increases the number of rows.

---
## Gather: usage

```yaml
type: FullSlide
key: b0cb4c7c19
```

`@part1`

```{r}
?gather
```

![](http://s3.amazonaws.com/assets.datacamp.com/production/course_7012/datasets/03.01_gather_usage.png)

`@script`

The arguments we need are data, key, value, and we see those ellipses again.


---
## Gather: arguments

```yaml
type: FullSlide
key: a4ac178fde
```

`@part1`

```{r}
?gather
```

![](https://assets.datacamp.com/production/repositories/1613/datasets/28c19ee0ebb5e43750093c70f345bd58bc2ae7ae/03.01_gather_args.png)

`@script`

A key and a value are required, and we'll come back to these. The ellipses is where we name the columns to gather using bare variable names, but we can also use our `dplyr::select()` helpers. We'll use the colon operator to select a range of consecutive columns.


---
## Gathering juniors

```yaml
type: FullSlide
key: f8b5af7de9
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/71ce4091569af6f7b9e5c922e7b5c7a6483a0031/03.01_gather-01.png)

`@script`

Using the juniors data, the key and value arguments highlighted in green are our two NEW variable names: "spice" and "correct." The two new columns you want are the key and value columns. Since these two variables need to be created, their names go in quotes. The key column will contain the original column names. The value column will contain the original cells. Finally, we select the columns to gather, highlighted in blue. You can use the colon operator to select a range of existing, consecutive variables to gather by name.

---
## Gathering what you have into what you want

```yaml
type: FullSlide
key: e19095e7c5
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/397588e75d00cb2e9e776ad1c7bdee4b638c3c2c/03.01_gather_sketch.png)

`@script`


To gather what you have into what you want, start with the data frame, which goes before the pipe operator. Next declare the key and value columns- argument names go on the left and the argument values go in quotes on the right. So, spice and correct are the columns we want. The columns we have to gather go last. Since they already exist in the data, these column names don't need to be in quotes. Let's break down the key column.

---
## The key column 

```yaml
type: FullSlide
key: b55123bcb9
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/373dacae6a29dd96dcd5dc5e6d947992389d6994/03.01_gather-02.png)

`@script`

Key is the new variable that will hold the column of our original column names. Think of turning a key in a lock- the columns names from your original data are turned from a single row into a single column. The values in this column are repeated as needed. Here, cinnamon underscore 1 gets repeated four times.


---
## The key column

```yaml
type: FullSlide
key: 2bbae51c35
disable_transition: TRUE
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/2e8ce5eb15eb6ae46e1ae251c9a7b3f47f138745/03.01_gather-03.png)

`@script`

We also have four observations of the cardamom smell test. These get stacked in the spice column too.

---
## The key column

```yaml
type: FullSlide
key: 30ef5168e2
disable_transition: TRUE
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/9cf1485bad183284cdd85553cd1b4244d7892231/03.01_gather-04.png)

`@script`

And same for nutmeg. 


---
## The value column

```yaml
type: FullSlide
key: 8a19c96db3
disable_transition: TRUE
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/8b38f4620856c91ee551df2aafb64325896cb7af/03.01_gather-05.png)

`@script`

Value is the new variable that will hold all of the original cells in a single column. The first column selected to gather goes to the top of the stack, so we start with cinnamon underscore one.

---
## The value column

```yaml
type: FullSlide
key: 2d3591a750
disable_transition: TRUE
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/b625c2a9dec0db990edd6a759abb0a97355d3925/03.01_gather-06.png)

`@script`

We gather three columns total, so cardamom underscore two gets stacked next in the same column.


---
## The value column

```yaml
type: FullSlide
key: 5b5207e3dd
disable_transition: TRUE
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/e18ad6278596630feb27500b97cc095c5a5c070f/03.01_gather-07.png)

`@script`

And same with nutmeg underscore three.


---
## A little trick

```yaml
type: FullSlide
key: 47a599d621
disable_transition: TRUE
```

`@part1`

![](https://assets.datacamp.com/production/repositories/1613/datasets/1f6d0022193c11b9dfc668f9afe0a74416fb0469/03.01_gather-08.png)

`@script`

One trick is to start with key equals key and value equals value. The worst thing that can happen is that you end up with poorly named columns! In other words, columns where you can't remember what the cells contain, or column names that are so generic that you can't remember them...But sometimes you need to see how the columns look after gathering to be able to name them. Once your code for gathering works, you can always change the key and value column names to be meaningful.

---
## Let's get to work!

```yaml
type: FinalSlide
key: d6106a86f0
```

`@script`

Now let's get busy gathering.

