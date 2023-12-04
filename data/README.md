# Data Description: All Billboard Summer Hits

The code provided can be obtained from:
<https://github.com/reisanar/datasets/blob/master/all_billboard_summer_hits.csv>


All_Billboard_summer_hits consists of 600 rows of data along with 22 variables for each. 

Below is a summary of the variables:

## Numerical Variables:

|               | Danceability      | Energy             | Loudness           | Valence            | Tempo             |
|:-------------:|:-----------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Minimum       | 0.2170            | 0.060              | -23.574            | 0.0695             | 62.83             |
| Median        | 0.6480            | 0.6405             | -8.072             | 0.6900             | 120.01            |
| Mean          | 0.6407            | 0.6221             | -8.587             | 0.6488             | 120.48            |
| Maximum       | 0.9800            | 0.9890             | -1.09              | 0.9860             | 210.75            |


|               | Speechiness      | Acousticness      | Instrumentalness  | Liveness          | Duration_ms       |
|:-------------:|:----------------:|:-----------------:|:-----------------:|:------------------:|:------------------:|
| Minimum       | 0.02330          | 0.0000488          | 0.0000000         | 0.02480           | 103386            |
| Median        | 0.04140          | 0.1620000          | 0.0000032         | 0.12400           | 226926            |
| Mean          | 0.06866          | 0.2665156          | 0.0364316         | 0.17979           | 229434            |
| Maximum       | 0.51700          | 0.9870000          | 0.9540000         | 0.98900           | 557293            |



# Categorical Variables:

1. **key Mode:** This is a combination of key and mode, indicating both the tonal center and the modality of the music.

2. **Track URI, Playlist Name, Playlist Image, Track Name, Artist Name, Album Name, Album Image:** These variables are identifiers and names associated with tracks and playlists.

3. **Time Signature:** Describes the number of beats in a bar and which note value gets the beat. For our data options can be 3, 4, or 5.

   | Time Signature | Count |
   |:--------------:|:-----:|
   |       3        |  22   |
   |       4        | 573   |
   |       5        |   5   |

4. **Mode:** Describes the modality of the music, i.e., whether it's in a major or minor key.

   | Mode  | Count |
   |:-----:|:-----:|
   | major |  419  |
   | minor |  181  |

6. **Key:** Indicates the tonal center or home base of a musical composition.

   | Key | Count |
   |:---:|:-----:|
   |  A  |   55  |
   | A#  |   41  |
   |  B  |   35  |
   |  C  |   83  |
   | C#  |   76  |
   |  D  |   47  |
   | D#  |   20  |
   |  E  |   42  |
   |  F  |   63  |
   | F#  |   36  |

7. **Year:** The year at which the song charted, ranges from 1958 to 2017.
