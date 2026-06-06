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

### Cleaned Dataset Preview

The first five rows of the cleaned dataset are shown below.

| track_name | artists | popularity | explicit | track_genre | release_year |
|------------|----------|------------|----------|-------------|--------------|
| Comedy | Gen Hoshino | 73 | False | acoustic | 1974.0 |
| Ghost - Acoustic | Ben Woodward | 55 | False | acoustic | NaN |
| To Begin Again | Ingrid Michaelson;ZAYN | 57 | False | acoustic | 1973.0 |
| Can't Help Falling In Love | Kina Grannis | 71 | False | acoustic | NaN |
| Hold On | Chord Overstreet | 82 | False | acoustic | NaN |

### Univariate Analysis

The histogram below shows the distribution of song popularity across all songs in the dataset.

<iframe
    src="projects/proj04/assets/popularity_hist.html"
    width="900"
    height="450"
    frameborder="0">
</iframe>

Most songs have low to medium popularity scores, while highly popular songs are less common. The distribution is right-skewed, indicating that only a relatively small number of songs achieve very high popularity on Spotify.

### Bivariate Analysis

The box plot below compares popularity between explicit and non-explicit songs.

<iframe
    src="projects/proj04/assets/explicit_popularity.html"
    width="900"
    height="450"
    frameborder="0">
</iframe>

Explicit songs appear to have a slightly higher median popularity than non-explicit songs, which provides preliminary evidence that explicit content may be associated with song popularity. However, there is substantial overlap between the two groups, suggesting that additional statistical testing is needed to determine whether the observed difference is significant.

### Interesting Aggregates

To explore how popularity varies across genres, I grouped songs by `track_genre` and computed the average popularity score for each genre. The plot below shows the ten genres with the highest average popularity.

<iframe
    src="projects/proj04/assets/genre_popularity.html"
    width="900"
    height="450"
    frameborder="0">
</iframe>

The results show that popularity differs substantially across genres. Genres such as pop-film and k-pop have some of the highest average popularity scores in the dataset, suggesting that genre may be an important factor when explaining and predicting song popularity.

## Assessment of Missingness

### NMAR Analysis

It is possible that the `tempo` column is NMAR because the reason a tempo value is missing may depend on characteristics of the audio itself that are not fully captured by the observed variables in the dataset. For example, songs with unusual rhythms, ambiguous timing, or recording artifacts may be more difficult for Spotify's tempo estimation process to assign a tempo value. If additional information about how Spotify calculates tempo were available, the missingness might instead be explained by observed variables, making the missingness MAR rather than NMAR.

### Missingness Dependency

The `tempo` column contains 22,114 missing values, making it a suitable candidate for missingness analysis.

The histogram below compares the distribution of energy for songs with missing tempo values and songs without missing tempo values.

<iframe
    src="projects/proj04/assets/energy_missingness.html"
    width="900"
    height="450"
    frameborder="0">
</iframe>

Songs with missing tempo values tend to have lower energy levels on average (0.51) than songs without missing tempo values (0.67).

A permutation test comparing energy and tempo missingness produced a p-value of approximately 0.00. This provides strong evidence that the missingness of tempo depends on energy.

As a comparison, a permutation test using popularity produced a p-value of approximately 0.70. Since this p-value is large, there is insufficient evidence that tempo missingness depends on popularity.

Therefore, the missingness of the `tempo` column appears to depend on `energy` but not on `popularity`. This satisfies the project requirement of identifying one variable on which the missingness depends and one variable on which it does not depend.

## Hypothesis Testing

### Research Question

Do explicit songs tend to have higher popularity than non-explicit songs on Spotify?

### Hypotheses

- **Null Hypothesis (H₀):** Explicit and non-explicit songs have the same popularity distribution. Any observed difference in average popularity is due to random chance.
- **Alternative Hypothesis (H₁):** Explicit songs have a higher average popularity than non-explicit songs.

### Test Statistic

I used the difference in mean popularity:

**(Mean popularity of explicit songs) − (Mean popularity of non-explicit songs)**

This statistic is appropriate because popularity is a quantitative variable and the goal is to compare the average popularity between two groups.

### Significance Level

α = 0.05

### Visualization

The box plot below compares the popularity distributions of explicit and non-explicit songs.

<iframe
    src="projects/proj04/assets/explicit_popularity.html"
    width="900"
    height="450"
    frameborder="0">
</iframe>

### Results

The observed difference in mean popularity was approximately **3.52 popularity points**, with explicit songs having the higher average popularity.

A one-sided permutation test with 500 simulations produced a **p-value less than 0.01**.

Because the p-value is less than the significance level of 0.05, there is sufficient evidence to reject the null hypothesis.

### Conclusion

The results provide evidence that explicit songs tend to have higher average popularity than non-explicit songs in this dataset. However, this does not prove that explicit content causes higher popularity. It only suggests an association between explicit content and song popularity.

## Prediction Problem

The prediction problem for this project is to predict a song's Spotify popularity score using information about the song and its audio characteristics.

This is a **regression problem** because the response variable, `popularity`, is quantitative and can take on many numerical values between 0 and 100.

### Response Variable

The response variable is:

- **popularity**: Spotify's popularity score assigned to a song.

I chose popularity as the response variable because it is one of the most important measures of a song's success on Spotify. Predicting popularity can help identify which song characteristics are associated with higher listener engagement and overall success on the platform.

### Features Available at Time of Prediction

At the time of prediction, information about a song's audio features and metadata would already be available. Therefore, features such as:

- `danceability`
- `energy`
- `acousticness`
- `speechiness`
- `loudness`
- `tempo`
- `duration_ms`
- `valence`
- `explicit`
- `track_genre`

can be used as predictors.

The target variable `popularity` would not be available at prediction time and is therefore excluded from the predictors.

### Evaluation Metric

I will use **Root Mean Squared Error (RMSE)** to evaluate model performance.

RMSE is appropriate because popularity is a continuous numerical variable and RMSE measures prediction error in the same units as the response variable. RMSE also penalizes larger prediction errors more heavily than smaller ones, making it useful for evaluating how accurately the model predicts song popularity.