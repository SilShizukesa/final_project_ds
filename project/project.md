---
title: "Exploring Music Through The Years"
output:
  html_notebook:
    toc: true
    toc_float: true
    toc_depth: 2
---
**Term 2023 Fall**

Team members: 

- Student 1: [Noah Vanscoyoc](noah.vanscoyoc@gmail.com) 
- Student 2: [Brandon Connell](bconnell7730@floridapoly.edu)


**Summary**

Our project investigates the main characteristics of Billboard hits from the year 1958, all the way to 2017. Among the dataset is characteristics such as "Danceability" or "acousticness"
We will be using the data available at: all_billboard_summer_hits.csv


# Introduction
Everyone listens to music

# Prerequisites
Loading the packages needed
```{r, message=FALSE, warning=FALSE}
library(tidyverse)
library(ggplot2)
library(dplyr)
```




# Dataset
Importing the data set from Github, which can be found [here](https://github.com/reisanar/datasets/blob/master/all_billboard_summer_hits.csv).
```{r}
#(https://github.com/reisanar/datasets/blob/master/all_billboard_summer_hits.csv)
#summer_billboard <- read_csv(bb_path)
```




# Data Exploration
A quick view of the data set:

```{r}
summary(all_billboard_summer_hits)
```



Here are a few samples to show what a data entry looks like:

```{r}
sample_n(all_billboard_summer_hits, 5)
```

# Data comparisons

# {.tabset .tabset-fade .tabset-pills}


## Nominal Data

Mode

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = mode, fill = mode)) +
  geom_bar(,stat = "count", show.legend = FALSE) +
  geom_text(stat = "count", aes(label = stat(count)), vjust = -.5) +
  labs(title = "Distribution of Mode",
       x = "Mode",
       y = element_blank()) +
  theme_minimal() + 
  theme(axis.text.y  =  element_blank(),
        axis.ticks = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        plot.title = element_text(hjust = .5))
```



```{r}
ggplot(data = all_billboard_summer_hits, aes(x = key_mode, fill = mode)) +
  geom_bar(stat = "count", show.legend = FALSE) +
  geom_text(stat = "count", aes(label = stat(count)),hjust = 1.2) +
  labs(title = "Distribution of Key Mode",
       x = "Key Mode",
       y = element_blank()) +
  theme_minimal() + 
  coord_flip() +
  theme(axis.text.x  =  element_blank(),
        axis.ticks = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        plot.title = element_text(hjust = .5))
```



## Loudness

Let's look at Loudness:
```{r}
ggplot(data = all_billboard_summer_hits, aes(x = loudness)) +
  geom_histogram(binwidth = .05, fill = "#3498db", color = "#2c3e50", alpha = .08) +
  labs(title = "Distribution of Loudness",
     x = "Loudness",
     y = "Frequency") +
  theme_minimal()
```




Comparing 

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = danceability, y = loudness, color = year)) +
  geom_point() +
  geom_smooth(color = "orange")
```


```{r}
ggplot(data = all_billboard_summer_hits, aes(x = energy, y = loudness, color = year)) +
  geom_point() +
  geom_smooth(color = "orange")
```



## Danceability



Let's look at Danceability:

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = danceability)) +
  geom_histogram(binwidth = .05, fill = "#3498db", color = "#2c3e50", alpha = .08) +
  labs(title = "Distribution of Danceability",
     x = "Danceability",
     y = "Frequency") +
  theme_minimal()
```



Are there any correlations with Danceability?

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = energy, y = danceability, color = year)) +
  geom_point() +
  geom_smooth(color = "orange")
```




```{r}
ggplot(data = all_billboard_summer_hits, aes(x = energy, y = danceability, color = year)) +
  geom_point() +
  geom_smooth(color = "orange")
```


## Valence


How happy are songs?

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = valence)) +
  geom_histogram(binwidth = .05, fill = "#3498db", color = "#2c3e50", alpha = .08) +
  labs(title = "Distribution of Valence",
     x = "Valence",
     y = "Frequency") +
  theme_minimal()
```


```{r}
ggplot(data = all_billboard_summer_hits, aes(x = energy, y = valence, color = year)) +
  geom_point() +
  geom_smooth(color = "orange")
```




# Artist insight

# {.tabset .tabset-fade .tabset-pills}

## Exploration

What artist has the most summer hits? 

```{r}
all_billboard_summer_hits %>% 
  group_by(artist_name) %>% 
  summarise(count = n()) %>% 
  arrange(desc(count)) %>% 
  slice_head(n = 1)

```

And what were her songs?
```{r}
rihanna_songs <- all_billboard_summer_hits %>% 
  filter(artist_name == "Rihanna")

rihanna_songs %>% 
select(track_name, year)
```

```{r}
rihanna_songs
```

## Rihanna Nominal Graphs

Here are some graphs to show countable metrics:

```{r}
ggplot(data = rihanna_songs) +
  geom_bar(aes(x = mode, fill = mode), color = "black", alpha = 0.8, stat = "count") +
  labs(title = "Distribution of Modes in Rihanna's Songs",
       x = "Mode",
       y = "Count",
       fill = "Mode") +
  scale_fill_manual(values = c("#3498db", "#e74c3c")) +
  theme_minimal() +
  theme(axis.text.x = element_text()) +
  guides(fill = FALSE)
```
This needs to be better. Maybe think of something different?

```{r}
ggplot(data = rihanna_songs, aes(x = key_mode, fill = mode)) +
  geom_bar(stat = "count", show.legend = FALSE) +
  geom_text(stat = "count", aes(label = stat(count)),hjust = 1.2) +
  labs(title = "Distribution of Key Mode",
       x = "Key Mode",
       y = element_blank()) +
  theme_minimal() + 
  coord_flip() +
  theme(axis.text.x  =  element_blank(),
        axis.ticks = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        plot.title = element_text(hjust = .5))
```


## Rihanna Comparisons

```{r}
ggplot(data = rihanna_songs) +
  geom_point(aes(x = valence, y = loudness))
```




# Conclusion
Write our conclusion here


# Glossary

Danceability: A measure of how suitable a track is for dancing based on factors like tempo, rhythm stability, beat strength, and overall regularity.

Energy: Represents the intensity and activity level of a song. High energy songs might be more lively and dynamic, while low energy songs may be more calm and subdued.

Key: Indicates the key signature of the song, representing the tonal center or home base of the music.

Loudness: Refers to the volume of the song. It's usually measured in decibels (dB). Higher values indicate louder songs.

Mode: Describes the modality of the music, i.e., whether it's in a major or minor key. Major keys often sound more uplifting, while minor keys can give a more melancholic feel.

Speechiness: Measures the presence of spoken words in a track. Songs with higher speechiness values may contain more spoken words than singing.

Acousticness: Indicates the likelihood that a track is acoustic, meaning it doesn't heavily rely on electronic or synthesized elements.

Instrumentalness: Measures the likelihood that a track is instrumental, without vocal content.

Liveness: Indicates the presence of an audience in the recording. A higher value suggests the track was likely recorded live.

Valence: Describes the musical positiveness of a track. Higher valence values suggest a more positive, happy, or cheerful mood.

Tempo: Represents the speed or pace of a song, typically measured in beats per minute (BPM).

Duration_ms: The duration of the song in milliseconds.

Time Signature: Describes the number of beats in a bar and which note value gets the beat.

Key Mode: A combination of key and mode, indicating both the tonal center and the modality of the music.


