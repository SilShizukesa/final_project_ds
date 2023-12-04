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

-   Student 1: [Noah Vanscoyoc](noah.vanscoyoc@gmail.com)
-   Student 2: [Brandon Connell](bconnell7730@floridapoly.edu)

**Abstract**

This project explores the evolution of Billboard hits from 1958 to 2017, analyzing key musical features such as danceability, energy, and acousticness to unveil patterns and trends, offering a comprehensive visual narrative of the dynamic shifts in popular music over the past six decades.

We will be using the data available at: all_billboard_summer_hits.csv

# Introduction

The musical landscape has undergone transformations over the decades, with Billboard hits acting as a time capsule reflecting the evolving tastes and trends of society. In this exploration, we delve into the rich history of Billboard chart-toppers spanning from 1958 to 2017. By looking at key musical characteristics such as danceability, energy, and acousticness, we aim to uncover nuanced patterns, trends, and the ever-shifting dynamics of popular music. Through visualizations and analyses, we plan to provide a comprehensive overview of how these musical elements have shaped and been shaped by cultural shifts, technological advancements, and the ever-changing musical landscape over the past six decades.

# Prerequisites

Loading the packages needed

```{r, message=FALSE, warning=FALSE}
library(tidyverse)
library(ggplot2)
library(dplyr)
library(plotly)
```

# Dataset

Importing the data set from Github, which can be found [here](https://github.com/reisanar/datasets/blob/master/all_billboard_summer_hits.csv).

```{r}
#(https://github.com/reisanar/datasets/blob/master/all_billboard_summer_hits.csv)
#summer_billboard <- read_csv(bb_path)
```

Let us take a look at the dataframe:

```{r}
all_billboard_summer_hits
```

# Data Exploration

A quick summary of the data set:

```{r}
summary(all_billboard_summer_hits)
```

Here are a few samples to show what a data entry looks like:

```{r}
sample_n(all_billboard_summer_hits, 5)
```

# Data comparisons

In this section we want to look at these hits as a whole. Are there any trends we can spot? 

#  {.tabset .tabset-fade .tabset-pills}

## Nominal Data

Mode

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = mode, fill = mode)) +
  geom_bar(stat = "count", show.legend = FALSE, color = "black", alpha = 0.8) +
  geom_text(stat = "count", aes(label = after_stat(count)), vjust = -0.5) +
  labs(title = "Distribution of Mode",
       x = element_blank(),
       y = element_blank()) +
  scale_fill_manual(values = c("#56B4E9", "#E69F00")) +
  theme_minimal() + 
  theme(axis.text.y = element_blank(),
        axis.ticks = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        plot.title = element_text(hjust = 0.5))
```


We can see that the majority of songs are in the major mode.
<br><br>


I want to make this go from largest to smallest

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = key_mode, fill = mode)) +
  geom_bar(stat = "count", show.legend = FALSE) +
  geom_text(stat = "count", aes(label = after_stat(count)), hjust = -0.01) +
  labs(title = "Distribution of Key Mode",
       x = "Key Mode",
       y = element_blank()) +
  scale_fill_manual(values = c("#66c2a5", "#fc8d62"), name = "Mode") +  # Nice color theme
  theme_minimal() + 
  coord_flip() +
  theme(axis.text.x = element_blank(),
        axis.ticks = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        plot.title = element_text(hjust = 0.5))
```

## Loudness

*Let's look at Loudness:* 

```{r}
dis_loud <- ggplot(data = all_billboard_summer_hits, aes(x = loudness)) +
  geom_histogram(binwidth = .5, fill = "#3498db", color ="#2c3e50", alpha = .5) +
  labs(title = "Distribution of Loudness",
     x = "Loudness",
     y = "Frequency") +
  theme_minimal()
```

```{r}
ggplotly(dis_loud)
```

Comparing



```{r}
loud_v_dance <- ggplot(data = all_billboard_summer_hits, aes(x = danceability, y = loudness)) +
  geom_point(alpha = 0.7, size = 3, color = "darkgrey") +
  geom_smooth(method = "lm", color = "orange", linetype = "dashed") +
  labs(title = "Scatter Plot of Danceability and Loudness",
       x = "Danceability",
       y = "Loudness") +
  theme_minimal()
```

```{r}
ggplotly(loud_v_dance)
```

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = energy, y = loudness)) +
  geom_point(alpha = 0.7, size = 3) +
  geom_smooth(method = "lm",color = "#e74c3c", linetype = "dashed") +
  labs(title = "Scatter Plot of Energy and Loudness by Year",
       x = "Energy",
       y = "Loudness") +
  theme_minimal() +
  theme(legend.position = "top")
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
ggplot(data = all_billboard_summer_hits, aes(x = danceability, y = energy)) +
  geom_point(alpha = 0.7, size = 3, color = "#3498db") +  # Adjusted point aesthetics
  geom_smooth(method = "lm", color = "orange", linetype = "dashed") +
  labs(title = "Scatter Plot of Energy and Danceability",
       x = "Danceability",
       y = "Energy") +
  theme_minimal()
```




Dance vs Tempo

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = danceability, y = tempo)) +
  geom_point(alpha = 0.7, size = 3, color = "#3498db") +  # Adjusted point aesthetics
  geom_smooth(method = "lm", color = "orange", linetype = "dashed") +
  labs(title = "Scatter Plot of Tempo and Danceability",
       x = "Danceability",
       y = "Tempo") +
  theme_minimal()
```




## Valence

How happy are songs?



```{r}
ggplot(data = all_billboard_summer_hits, aes(x = "", y = valence)) +
  geom_boxplot(fill = "#3498db", color = "#2c3e50", alpha = 0.5) +
  labs(title = "Boxplot of Valence",
       y = "Valence") +
  coord_flip() +
  theme_minimal() +
  theme(
    axis.title.y = element_blank())
```


```{r}
ggplot(data = all_billboard_summer_hits, aes(x = valence)) +
  geom_histogram(binwidth = .05, fill = "#3498db", color = "#2c3e50", alpha = .08) +
  labs(title = "Distribution of Valence",
     x = "Valence",
     y = "Frequency") +
  theme_minimal()
```

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = energy, y = valence)) +
  geom_point(alpha = 0.7, size = 3, color = "blue") +  # Adjusted point aesthetics
  geom_smooth(method = "lm", color = "orange", linetype = "dashed") +
  labs(title = "Scatter Plot of Energy and valence",
       x = "Energy",
       y = "Valence") +
  theme_minimal()
```

# Artist insight

#  {.tabset .tabset-fade .tabset-pills}

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
  labs(title = "Distribution of Modes in Rihanna's Songs") +
  scale_fill_manual(values = c("#3498db", "#e74c3c")) +
  theme_minimal() +
  theme(
  legend.position = "none",
  axis.title.y = element_blank(),
  axis.title.x = element_blank())
```




## Rihanna Comparisons

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = energy, y = loudness)) +
  geom_point(alpha = 0.7, size = 3) +
  geom_smooth(method = "lm", color = "#e74c3c", linetype = "dashed") +
  
  # Add Rihanna's songs
  geom_point(data = rihanna_songs, aes(x = energy, y = loudness), alpha = 0.7, size = 3, color = "blue") +
  
  labs(title = "Energy vs Loudness",
       x = "Energy",
       y = "Loudness") +
  theme_minimal() +
  theme(legend.position = "top")
```

We can see that Rihanna makes loud music, but the energy of her songs reaching both ends of the graph. 



Rihanna dance vs liveness
```{r}
ggplot(data = all_billboard_summer_hits, aes(x = danceability, y = liveness)) +
  geom_point(alpha = 0.7, size = 3, color = "skyblue") +  # Adjusted point aesthetics
  geom_smooth(method = "lm", color = "orange", linetype = "dashed") +
  
  geom_point(data = rihanna_songs, aes(x = danceability, y = energy), alpha = 0.7, size = 3, color = "limegreen") +
  
  labs(title = "Scatter Plot of Energy and Danceability",
       x = "Danceability",
       y = "Liveness") +
  theme_minimal()
```

Rihanna's songs range from middle to above average in dance, while her Liveness is well above the mean. 



# Conclusion

Write our conclusion here:

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
