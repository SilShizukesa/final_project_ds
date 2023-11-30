---
title: "Project"
output: html_notebook
---



```{r, message=FALSE, warning=FALSE}
library(tidyverse)
```


# How to do this?
```{r}
#bb_path <- "https://github.com/reisanar/datasets/blob/master/all_billboard_summer_hits.csv"
#summer_billboard <- read_csv(bb_path)
```


A quick view of the data set:

```{r}
all_billboard_summer_hits
```


Just a view of artist, track and what year

```{r}
all_billboard_summer_hits %>% 
  select(artist_name, track_name,year)
```


```{r}
ggplot(data = all_billboard_summer_hits) +
    geom_point(aes(x = energy,
                  y = danceability,
                 color = key)) +
  facet_wrap(vars(year))
```
