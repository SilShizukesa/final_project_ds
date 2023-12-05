---
title: "Exploring Music Through The Years"
output:
  html_notebook:
    toc: true
    toc_float: true
    toc_depth: 2
editor_options: 
  chunk_output_type: inline
---

**Term 2023 Fall**

Team members:

-   Student 1: [Noah Vanscoyoc](noah.vanscoyoc@gmail.com)
-   Student 2: [Brandon Connell](bconnell7730@floridapoly.edu)

**Abstract**

From 1959 to 2017, music has changed immensely, whether it be jazz to rock to pop to edm. Time's have changed and the billboards have too. This project will explore:

  * A songs grade to find the best song in the dataset
  * What types of songs are there? what modes or key modes do they use?
  * How loud are these songs and what does it have to do with other factors?
  * As well as trend patterns from years to years

With these explorations, we should be able to get a good narrative on the shifts in music over the past six decades.

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
url <- "https://raw.githubusercontent.com/SilShizukesa/final_project_ds/master/data/all_billboard_summer_hits.csv"
all_billboard_summer_hits <- read_csv(url)
```
For our report, let us take a quick peak at our dataset:

```{r}
all_billboard_summer_hits
```

# Data Exploration

If we want to explore our dataset, we'll need to get a good feel for the entries in it as well as some key details for each observation. Let's summarise the dataset and see what the variables are:

```{r}
summary(all_billboard_summer_hits)
```

Here are a few samples to show what a data entry looks like:

```{r}
sample_n(all_billboard_summer_hits, 5)
```
# What is the best Summer Billboard Hit?

This question can seem to be a loaded question, and truthfully very opionated. But we can take some key factors into consideration to decide factually what is the best song?
  *Taking the key factors into consideration, giving a weight to each factor then gives us the chance to multiply it by its respective data point and figure out its overall score once added
  *Once the score has been calculated, all that must be done is put everything into the original table and select the data points we only want to see
  *Finally, once the scores are calculated and given, we then will give each score a "Grade" based on this scale:
  #1 song = The Overall Best Song
  70+ = "S Tier"
  60-69 = "A Tier"
  50-59 = "B Tier"
  40-49 = "C Tier"
  39 and below = "D Tier"
  

```{r}
songs_data <- all_billboard_summer_hits

weights <- c(danceability = .3, 
             liveness = .3, 
             energy = .3, 
             valence = .02, 
             acousticness = .02, 
             speechiness = .04, 
             instrumentalness = .02)

all_billboard_summer_hits %>%
  mutate(score = rowSums(songs_data[, names(weights)] * weights)) %>%
  select(track_name, artist_name, score, year,  danceability, liveness, energy, valence, acousticness, speechiness, instrumentalness) %>%
  arrange(desc(score))


```
This is our function!
```{r}
grade <- function(score){
  if(score>=78){
    return("The Overall Best Song")
  }
  else if (score >= 70 & score <= 77){
    return("S Tier")
  }
  else if(score >= 60 & score <= 69){
    return("A Tier")
  }
  else if(score >= 50 & score <= 59){
    return("B Tier")
  }
  else if(score >= 40 & score <= 49){
    return("C Tier")
  }
  else{
    return("D Tier")
  }
}

```

Lets use our brand new function to find the grades for each songs score:
```{r}


# Function to calculate grade
grade <- function(score) {
  if (score >= .78) {
    return("The Overall Best Song")
  } else if (score >= .70 & score <= .77) {
    return("S Tier")
  } else if (score >= .60 & score <= .70) {
    return("A Tier")
  } else if (score >= .50 & score <= .60) {
    return("B Tier")
  } else if (score >= .40 & score <= .50) {
    return("C Tier")
  } else if (score < .07) {
    return("Worst Song in the Summer Billboard Hits")
  } else {
    return("D Tier")
  }
}

# Apply the grade function to the dataset
result_table <- all_billboard_summer_hits %>%
  mutate(score = rowSums(across(names(weights)) * weights),
         grade = sapply(score, grade)) %>%
  select(track_name, artist_name, score, grade, year,  danceability, liveness, energy, valence, acousticness, speechiness, instrumentalness) %>%
  arrange(desc(score))

# View the result table
print(result_table)

```
With this data exploration, we have now made a formula that will find us the best overall song in the billboard summer hits dataset that is without any opinionated input. It is all solely based on data given to use in numerical observations of songs danceability, liveness, acousticeness, energy, and other factors. 

Given this output, the best song on this list is:
Breaking up is Hard to Do by Neil Sedaka, made in 1962 with a score of .78353200 

# Data comparisons

In this section we want to look at these hits as a whole. Are there any trends we can spot? 

#  {.tabset .tabset-fade .tabset-pills}

## Nominal Data

The first data type we looked at was if a song was in major or minor.

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

Now that we can see that the majority of songs are in the major mode. We didn't feel like it was enough and wanted to know the amount of each different key modes of all the songs over the years. 
<br><br>


Therefore, we will find the specific counts of each key mode in this graph:

```{r}
library(dplyr)

all_billboard_summer_hits %>%
  count(key_mode, mode) %>%
  ggplot(aes(x = reorder(key_mode, n), y = n, fill = mode)) +
  geom_bar(stat = "identity") +
  geom_text(aes(label = n), vjust = 0.5, hjust = 1) +
  labs(title = "Distribution of Key Mode",
       x = "Key Mode",
       y = element_blank()) +
  scale_fill_manual(values = c("#66c2a5", "#fc8d62"), name = "Mode") +
  theme_minimal() + 
  coord_flip() +
  theme(axis.text.x = element_blank(),
        axis.ticks = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        plot.title = element_text(hjust = 0.5))

```
And after a quick look, we can see that most songs have used C major in the billboard summer hits. Maybe this means C major is the best key mode for making the top billboard summer hits?

The table below is just to show some songs that are in the C major key mode:
```{r}
all_billboard_summer_hits %>%
  filter(key_mode == "C major")
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

Here we can see that the majority of songs lay between -10 Db and -5 Db, as well as the data having a left skew.

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
