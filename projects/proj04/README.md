# Spotify Song Popularity Analysis

### Joseph Timmy Indra Wijaya

## Introduction

Spotify is one of the largest music streaming platforms in the world, with millions of songs available across many genres and artists. Understanding what factors influence a song's popularity can provide insight into listener preferences and broader music trends.

This project uses the Spotify Music Tracks dataset, which contains **114,000 rows and 25 columns** describing song characteristics. The main question explored in this project is:

**Are explicit songs more popular than non-explicit songs on Spotify?**

This question is interesting because explicit content is common in many popular genres, but it is not clear whether songs with explicit lyrics are actually more popular than songs without explicit content. Understanding this relationship can help explain how song characteristics relate to listener engagement and music consumption patterns.

The dataset contains many variables, but the most relevant columns for this analysis are:

- **popularity**: Spotify popularity score for a song.
- **explicit**: Whether a song contains explicit content.
- **track_genre**: Genre associated with the song.
- **release_year**: Year the song was released.
- **danceability**: How suitable a song is for dancing.
- **energy**: A measure of intensity and activity in a song.

In addition to investigating the relationship between explicit content and popularity, this project also develops machine learning models to predict song popularity using Spotify audio features. Accurate popularity prediction can help identify which characteristics are most strongly associated with successful songs on Spotify.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

Before performing any analysis, I inspected the Spotify Music Tracks dataset and examined the available variables. The dataset contains 114,000 songs and 25 columns describing song characteristics and audio features.

Several preprocessing steps were performed before analysis. An unnecessary index column was removed from the dataset. In addition, a new column called `release_year` was created by extracting the year from the `release_date` column. This makes it easier to analyze songs by release year rather than working with full date values.

I also identified missing values in the dataset. Most columns contained few or no missing values, although the `tempo` column contained **22,114 missing observations**. Since tempo was not directly related to the primary research question, these values were left as missing during exploratory analysis.

### Univariate Analysis

The histogram below shows the distribution of song popularity across all songs in the dataset.

<iframe
    src="assets/popularity_hist.html"
    width="800"
    height="600"
    frameborder="0">
</iframe>

Most songs have low to medium popularity scores, while highly popular songs are less common. The distribution is right-skewed, indicating that only a relatively small number of songs achieve very high popularity on Spotify.

### Bivariate Analysis

The box plot below compares popularity between explicit and non-explicit songs.

<iframe
    src="assets/explicit_popularity.html"
    width="800"
    height="600"
    frameborder="0">
</iframe>

Explicit songs appear to have a slightly higher median popularity than non-explicit songs, which provides preliminary evidence that explicit content may be associated with song popularity. However, there is substantial overlap between the two groups, suggesting that additional statistical testing is needed to determine whether the observed difference is significant.

### Interesting Aggregates

To explore how popularity varies across genres, I grouped songs by `track_genre` and computed the average popularity score for each genre. The plot below shows the ten genres with the highest average popularity.

<iframe
    src="assets/genre_popularity.html"
    width="800"
    height="600"
    frameborder="0">
</iframe>

The results show that popularity differs substantially across genres. Genres such as pop-film and k-pop have some of the highest average popularity scores in the dataset, suggesting that genre may be an important factor when explaining and predicting song popularity.