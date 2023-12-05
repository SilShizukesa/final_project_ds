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
  * Comparisons between variables to other variables to find any correlations?
  * Which artist had the most songs in the top hits?
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
             liveness = .02, 
             energy = .3, 
             valence = .3, 
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
grade <- function(score) {
  if (score >= .7287) {
    return("The Overall Best Song")
  } else if (score >= .70 & score <= .72701942) {
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

```

Lets use our brand new function to find the grades for each songs score:
```{r}


# Function to calculate grade
grade <- function(score) {
  if (score >= .7287) {
    return("The Overall Best Song")
  } else if (score >= .70 & score <= .72701942) {
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
With this data exploration, we have now made a formula that will find us the best overall song in the billboard summer hits dataset that is without any opinionated input. It is all solely based on data given to use in numerical observations of songs danceability, valence, acousticeness, energy, and other factors. 

Given this output, the best song on this list is:
Electric Avennue by Eddie Grant made in 1983 with a score of .72871144

what else can we find about this song that makes it "the best"?

```{r}
all_billboard_summer_hits$duration_minutes <- all_billboard_summer_hits$duration_ms / (1000 * 60)

all_billboard_summer_hits %>%
  filter(track_name == "Electric Avennue") %>%
  select(duration_minutes, track_name, danceability, valence, energy, everything())
```
we can see that this song is 3.2 minutes long, as well as having a very high danceability score. However, the energy seems to be a be low the reason for it being the best overall song is the fact that its valence is right up there with danceability in being so high. And after seeing the scores for its other variables, it can be confirmed as to why Electric Avennue is the overall best song.  


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
  filter(key_mode == "C major") %>%
  select(key_mode, track_name, everything())
```


## Loudness

*Let's take a look at the Loudness scale:* 

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

Here we can see that the majority of songs lay between -10 Db and -5 Db, as well as the data having a left skew. We wanted to make a very simple and easy graph to look at for the loudness scale, making it interactive with plotly allows for users to hover over the bars to see the individual counts for each loudness scale integer. With the highest being 42 songs at a decibel level of -6.


*Now let us take a look at loudness compared with other variables:*

First up is loudness compared to liveness

```{r}
loud_v_live <- ggplot(data = all_billboard_summer_hits, aes(x = liveness, y = loudness)) +
  geom_point(alpha = 0.7, size = 3, color = "#3498db") +
  geom_smooth(method = "lm", color = "#2c3e50", linetype = "dashed") +
  labs(title = "Loudness vs Liveness",
       x = "Liveness",
       y = "Loudness") +
  theme_minimal()
```

```{r}
ggplotly(loud_v_live)
```
This graph shows that there is no relationship between liveliness and loudness. Though, it is important to note that most songs only have a slight live element to them. This is probably the outcome of songs being recorded in recording studios as opposed to live performances.

```{r}
all_billboard_summer_hits %>%
  select(liveness, loudness, track_name)
```

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = energy, y = loudness)) +
  geom_point(alpha = 0.7, size = 3, color = "#3498db") +
  geom_smooth(method = "lm",color = "#2c3e50", linetype = "dashed") +
  labs(title = "Loudness vs Energy",
       x = "Energy",
       y = "Loudness") +
  theme_minimal() +
  theme(legend.position = "top")
```
Energy and loudness have a favorable correlation with one another. This finding supports our earlier theories that suggested that energy levels would increase with music volume.

```{r}
all_billboard_summer_hits %>%
  select(energy, loudness,  track_name)
```

## Danceability

*Next let's take a look at the danceability scale:* 

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = danceability)) +
  geom_histogram(binwidth = .05, fill = "#8e44ad", color = "#008080", alpha = .5) +
  labs(title = "Distribution of Danceability",
     x = "Danceability",
     y = "Frequency") +
  theme_minimal()
```
The song distribution is skewed slightly to the left and follows a standard bell shape. This demonstrates that most of these singles are at least somewhat danceable. 

*Next let us take a look at danceability compared with other variables:*

First we will take a look at Energy compared to Danceability:

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = danceability, y = energy)) +
  geom_point(alpha = 0.5, size = 3, color = "#8e44ad") +  # Adjusted point aesthetics
  geom_smooth(method = "lm", color = "#008080", linetype = "dashed") +
  labs(title = "Danceability vs Energy",
       x = "Danceability",
       y = "Energy") +
  theme_minimal()
```

There is a small correlation between energy and danceability. Though we can see the best range for a song's danceability is from .5 to .75.
```{r}
all_billboard_summer_hits %>%
  select(energy, danceability, track_name)
```


Will we see the same thing with tempo?

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = danceability, y = tempo)) +
  geom_point(alpha = 0.5, size = 3, color = "#8e44ad") +
  geom_smooth(method = "lm", color = "#008080", linetype = "dashed") +
  labs(title = "Danceability vs Tempo",
       x = "Danceability",
       y = "Tempo") +
  theme_minimal()
```
A song's danceability can be viewed as a function of its tempo; this graph indicates that a tempo of between 150 and 100 is ideal.

```{r}
all_billboard_summer_hits %>%
  select(tempo, danceability, track_name)
```

## Valence

To what extent are songs happy? Let us examine the following pair of graphs:


```{r}
ggplot(data = all_billboard_summer_hits, aes(x = "", y = valence)) +
  geom_boxplot(fill = "#2ecc71", color = "#e67e22", alpha = 0.2) +
  labs(title = "Boxplot of Valence",
       y = "Valence") +
  coord_flip() +
  theme_minimal() +
  theme(
    axis.title.y = element_blank())
```


```{r}
ggplot(data = all_billboard_summer_hits, aes(x = valence)) +
  geom_histogram(binwidth = .05, fill = "#2ecc71", color = "#e67e22", alpha = .2) +
  labs(title = "Distribution of Valence",
     x = "Valence",
     y = "Frequency") +
  theme_minimal()
```


According to these graphs, happiness has a significant leftward skew, which indicates that the majority of songs that become hits will sound happy.
<br>
Let's now examine an energy and valence scatter plot:

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = valence, y = energy )) +
  geom_point(alpha = 0.2, size = 3, color = "#2ecc71") +
  geom_smooth(method = "lm", color = "#e67e22", linetype = "dashed") +
  labs(title = "Valence vs Energy",
       x = "Valence",
       y = "Energy") +
  theme_minimal()
```

In any event, I don't think this is all that surprising. It appeared evident that happiness and energy were positively correlated.

```{r}
all_billboard_summer_hits %>%
  select(energy, valence, track_name)
```

# Artist insight
We want to examine the artist with the greatest hits in this section and compare them to their contemporaries. Does said artist adhere to the established trends? Or are they going to overlook them?

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
With seven entries on the list, Rihanna defeated every other artist to take the top spot. Which songs were these?

```{r}
rihanna_songs <- all_billboard_summer_hits %>% 
  filter(artist_name == "Rihanna")

rihanna_songs %>% 
select(track_name, year)
```


The list of songs is visible above. We have listed the songs below along with every characteristic that has been linked to them.


```{r}
rihanna_songs
```

## Rihanna Nominal Graphs

As in the prior sections, let's start by examining Rihanna's nominal statistics:

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
With a higher average of songs in a minor mode, Rihanna beats the mean with a ratio of 4:3.



## Rihanna Comparisons
Let us take a quick look at Rihanna's placements in some of our previous graphs. How will she compare?

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = danceability, y = tempo)) +
  geom_point(alpha = 0.7, size = 3, color = "#3498db") +
  geom_smooth(method = "lm", color = "#e74c3c", linetype = "dashed") +
  
  #add rihannas songs
  geom_point(data = rihanna_songs, aes(x = danceability, y = tempo), alpha = 1, size = 3, color = "#2ecc71") +
  
  labs(title = "Danceability vs Tempo",
       x = "Danceability",
       y = "Tempo") +
  theme_minimal()
```

```{r}
ggplot(data = all_billboard_summer_hits, aes(x = liveness, y = loudness)) +
  geom_point(alpha = 0.7, size = 3, color = "#3498db") +
  geom_smooth(method = "lm", color = "#e74c3c", linetype = "dashed") +
  
  # Add Rihanna's songs
  geom_point(data = rihanna_songs, aes(x = liveness, y = loudness), alpha = 1, size = 3, color = "#2ecc71") +
  
  labs(title = "loudness vs Liveness",
       x = "Liveness",
       y = "Loudness") +
  theme_minimal() +
  theme(legend.position = "top")
```


```{r}
ggplot(data = all_billboard_summer_hits, aes(x = valence, y = energy)) +
  geom_point(alpha = 0.7, size = 3, color = "#3498db") +
  geom_smooth(method = "lm", color = "#e74c3c", linetype = "dashed") +
  
  # Add Rihanna's songs
  geom_point(data = rihanna_songs, aes(x = valence, y = energy), alpha = 1, size = 3, color = "#2ecc71") +
  
  labs(title = "Valence vs Energy",
       x = "Valence",
       y = "Energy") +
  theme_minimal() +
  theme(legend.position = "top")
```


Rihanna's music has above-average danceability and a dynamic touch thanks to its varying tempo. Her studio-centric style, which is positioned on the far left for liveliness, is in line with that of other artists. Her songs are notable for their constant emphasis on volume, which has a striking and captivating effect. Rihanna's music explores a range of energy levels while keeping a central valence, making for a varied and captivating listening experience.


As we have shown Rihanna makes unique music compared to music as a whole. But, lets see if she gets closer to the mean when we look at her songs per year:

```{r}
specific_years <- c(2005, 2006, 2007, 2008, 2012, 2016) # Years that Rihanna charted

filtered_data <- all_billboard_summer_hits %>%
  filter(year %in% specific_years)

ggplot(data = filtered_data, aes(x = danceability, y = tempo)) +
  geom_point(aes(color = "Other"), alpha = 0.7, size = 3) +
  geom_smooth(method = "lm", color = "#e74c3c", linetype = "dashed") +
  
  geom_point(data = rihanna_songs, aes(color = "Rihanna"), alpha = 1, size = 3) +
  
  labs(title = "Danceability and Tempo",
       x = "Danceability",
       y = "Tempo") +
  
  scale_color_manual(values = c("Other" = "#3498db", "Rihanna" = "#2ecc71"), name = "Legend") +
  
  theme_minimal() +
  theme(legend.position = "top") +
  
  # Facet by year
  facet_wrap(~ year, scales = "free")

```

```{r}
specific_years <- c(2005, 2006, 2007, 2008, 2012, 2016) # Years that Rihanna charted

filtered_data <- all_billboard_summer_hits %>%
  filter(year %in% specific_years)

ggplot(data = filtered_data, aes(x = liveness, y = loudness)) +
  geom_point(aes(color = "Other"), alpha = 0.7, size = 3) +
  geom_smooth(method = "lm", color = "#e74c3c", linetype = "dashed") +
  
  geom_point(data = rihanna_songs, aes(color = "Rihanna"), alpha = 1, size = 3) +
  
  labs(title = "Loudness vs Liveness",
       x = "Liveness",
       y = "Loudness") +
  
  scale_color_manual(values = c("Other" = "#3498db", "Rihanna" = "#2ecc71"), name = "Legend") +
  
  theme_minimal() +
  theme(legend.position = "top") +
  
  # Facet by year
  facet_wrap(~ year, scales = "free")
```




```{r}
specific_years <- c(2005, 2006, 2007, 2008, 2012, 2016) # Years that Rihanna charted

filtered_data <- all_billboard_summer_hits %>%
  filter(year %in% specific_years)

ggplot(data = filtered_data, aes(x = valence, y = energy)) +
  geom_point(aes(color = "Other"), alpha = 0.7, size = 3) +
  geom_smooth(method = "lm", color = "#e74c3c", linetype = "dashed") +
  
  geom_point(data = rihanna_songs, aes(color = "Rihanna"), alpha = 1, size = 3) +
  
  labs(title = "Valence vs Energy",
       x = "Valence",
       y = "Energy") +
  
  scale_color_manual(values = c("Other" = "#3498db", "Rihanna" = "#2ecc71"), name = "Legend") +
  
  theme_minimal() +
  theme(legend.position = "top") +
  
  # Facet by year
  facet_wrap(~ year, scales = "free")
```

These graphs demonstrate that Rihanna is, in fact, exceptional. Her songs are typically found on the edge of the uncertainty cone, but there is room for uncertainty with only 10 samples. Her music did seem to deviate the most from the mean in 2007, and was the closest in 2008 & 2012.

## Summary

Rihanna's incredible musical ability, which combines dynamic tempo changes and above-average danceability, fits in with other artists in a studio-centric liveliness. Her songs stand out for emphasizing loudness, which produces an arresting and alluring effect. Although there is some uncertainty due to the small 10-sample size, the graphs show that her music regularly falls on the edge of the uncertainty cone. While her music came very close to the mean in 2008 and 2012, there were some notable departures from the mean in 2007. This combination highlights Rihanna's interesting and varied career in music, both stylistically and numerically.

## For Future Consideration

Does length of song correlate to danceability scores? 
```{r}
df <- all_billboard_summer_hits %>%
  select(danceability, duration_ms, track_name, artist_name) %>%
  arrange(desc(duration_ms))

df$duration_minutes <- df$duration_ms / (1000 * 60)

df %>%
  select(danceability, duration_minutes, track_name, artist_name)



```

In the end, we very briefly looked into what some correlations could be between the length of songs and their danceability score. As we can see above in the table, the longest song (in minutes) is "I want your sex Pts 1 and 2 remastered" by george michaels with a danceability score of .812. But right below it is an 8.95 minute long song with a score of .217. Which to us, makes a lot of sense, if a song is very long then you wouldn't want to dance for too long but then it seems like there could be outliers to this. We wanted to leave this for the end for future consideration to take it further when we are more proficient than we are with R to find out more about this detail. 

# Conclusion

In conclusion, our investigation has explored the dynamic environment of Billboard successes between 1959 and 2017, revealing notable changes in musical styles and traits. We created a formula to objectively identify the greatest song overall in the Billboard summer hits dataset by using a thorough analysis. This formula took into account important parameters like danceability, liveness, energy, and more. We developed a grading system that assigns songs to tiers based on our meticulous study and balancing of these elements, offering an objective evaluation of their general quality. Analyzing nominal data exposed interesting trends, like the dominance of C major key mode and the frequency of songs in the major mode. We also looked into valence, loudness, and danceability, finding patterns and insights into the connections between these musical components. Additional artist-specific research, with a special focus on Rihanna, highlighted her unique musical qualities and the diversity and originality seen in the larger dataset. All things considered, our study provides a thorough examination of the musical landscape over the course of six decades, providing insightful information about the elements that lead to the popularity of Billboard summer successes.

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
