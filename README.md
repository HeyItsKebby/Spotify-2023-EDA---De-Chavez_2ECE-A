
# Exploratory Data Analysis on Spotify 2023 Dataset

Welcome to this repository, where the project performs an Exploratory Data Analysis (EDA) on the dataset containing information about the most popular Spotify tracks of 2023. The dataset is sourced from [Kaggle's Top Spotify Songs 2023](https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023)
 collection, offering detailed information on song attributes, streams, artists, and platforms. The purpose of this analysis is to uncover insights and trends that can shed light on what makes certain tracks stand out in the world of music streaming.
 
#

## 1. Overview of Dataset
This code imports essential Python libraries for data analysis and visualization. Pandas is used for data manipulation, NumPy for numerical operations, Matplotlib for creating visualizations, and Seaborn for statistical plotting and enhanced graphics.
### Code:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

#
This code loads the spotify-2023.csv file and displays its contents in a pandas DataFrame, while ensuring proper handling of encoding issues. The encoding='ISO-8859-1' parameter ensures that non-ASCII characters are properly interpreted.
### Code:

```python
# Load the data
spotify_data = pd.read_csv('spotify-2023.csv', encoding='ISO-8859-1')
spotify_data
```
### Output:
![1](https://github.com/user-attachments/assets/a9d73f9f-c1a6-483c-85d5-cd820b132bfd)

#
This code provides a quick overview of the dataset's dimensions by displaying the number of rows and columns it contains.
### Code:

```python
print(f"Rows: {spotify_data.shape[0]} \nColumns: {spotify_data.shape[1]}")
```
### Output:
![2](https://github.com/user-attachments/assets/5af25e02-383f-469a-a4aa-409dab6a3321)

#
This is the summary of the DataFrame, including column data types, non-null counts, and memory usage.
### Code:

```python
# Display general info
spotify_data.info()
```
### Output:
![3](https://github.com/user-attachments/assets/73e4e3ea-4077-4135-beba-73aa4a3545ec)

#
This code identifies and displays the columns with missing values, along with the count of missing entries in each column, to help assess the data quality before further analysis.
### Code:

```python
# Checking for missing values
missing_data = spotify_data.isnull().sum()
missing_data = missing_data[missing_data > 0].reset_index()
missing_data.columns = ['Column', 'Missing Count']
missing_data
```
### Output:
![4](https://github.com/user-attachments/assets/d7600ca8-bc83-4060-92b3-018dc93ddc89)

#
## 2. Basic Descriptive Statistics

### Analyzing the Mean, Median, and Standard Deviation of the Streams Column
### Code:

```python
# Convert 'streams' column to numeric, handling commas and non-numeric entries
spotify_data['streams'] = pd.to_numeric(spotify_data['streams'].astype(str).str.replace(',', ''), errors='coerce')

# Calculate mean, median, and standard deviation for 'streams'
stream_mean = spotify_data['streams'].mean()
stream_median = spotify_data['streams'].median()
stream_std_dev = spotify_data['streams'].std()

# Display statistics
stream_statistics = pd.DataFrame({
    'Metric': ['Mean', 'Median', 'Std Dev'],
    'Streams Value': [stream_mean, stream_median, stream_std_dev]
})
stream_statistics.style.format({"Streams Value": "{:.2f}"})
```
### Output:
![5](https://github.com/user-attachments/assets/206b81ba-4fdc-447f-a30a-1d8af3f36012)

The statistical measures for the streams column are:
- Mean: 514,137,424.94
- Median: 290,530,915.00
- Standard Deviation: 566,856,949.04
#

### Exploring the Distribution of Released Year and Artist Count: Trends and Outliers
### Code:

```python
# Distribution of release years
year_distribution = spotify_data['released_year'].value_counts().sort_index()

# Plot distribution with rotated x-axis labels
plt.figure(figsize=(10, 5))
sns.barplot(x=year_distribution.index, y=year_distribution.values, color='lightcoral')
plt.title('Track Distribution by Release Year')
plt.xlabel('Year')
plt.ylabel('Track Count')
plt.xticks(rotation=90)
plt.tight_layout()
plt.show()
```
### Output:
![6](https://github.com/user-attachments/assets/adceb25e-b09f-488a-97da-22016a3e4d08)
Distribution of Released Year:
- Trend: There is a noticeable increase in the number of tracks released in recent years, particularly from 2010 onwards. The number of tracks spikes significantly around 2020-2022.
- Outliers: The years 2020-2022 have a much higher number of tracks compared to earlier years, indicating a recent surge in music production.

#
### Code:

```python
# Artist count distribution plot
plt.figure(figsize=(8, 5))
sns.histplot(spotify_data['artist_count'], binwidth=1, color="lightskyblue", edgecolor="black")
plt.title("Distribution of Tracks by Number of Artists")
plt.xlabel("Artist Count")
plt.ylabel("Track Frequency")
plt.grid(axis='y', linestyle='--', alpha=0.6)
plt.tight_layout()
plt.show()
```
### Output:
![7](https://github.com/user-attachments/assets/d0784c53-47b9-412b-91ff-d4d48e3d2a09)
Distribution of Artist Count:
- Trend: The majority of tracks are produced by a single artist. As the number of artists increases, the number of tracks decreases.
- Outliers: Tracks with more than three artists are relatively rare, with very few tracks involving six or more artists.

#
## 3. Top Performers

### Identifying the Most Streamed Track and Top 5 in the Dataset
### Code:

```python
# Top 5 tracks by streams
top_5_tracks = spotify_data.sort_values(by='streams', ascending=False).head(5).reset_index(drop=True)
top_5_tracks
```
### Output:
![8](https://github.com/user-attachments/assets/311e0390-c6ee-4931-b56c-dc431c0251ba)
The track streams from highest to lowest:

- Blinding Lights by The Weeknd: 3.7 billion+ streams
- Shape of You by Ed Sheeran: 3.6 billion+ streams
- Someone You Loved by Lewis Capaldi: 2.88 billion+ streams
- Dance Monkey by Tones and I: 2.86 billion+ streams
- Sunflower - Spider-Man: Into the Spider-Verse by Post Malone & Swae Lee: 2.81 billion+ streams

So "Blinding Lights" by The Weeknd has the highest number of streams at approximately 3.7 billion+ streams.
#

### Identifying the Top 5 Artists with the Most Tracks in the Dataset
### Code:

```python
# Top 5 artists by track count
most_frequent_artists = spotify_data['artist(s)_name'].str.split(', ').explode().value_counts().head(5).reset_index()
most_frequent_artists.columns = ['Artist', 'Track Count']
most_frequent_artists
```
### Output:
![9](https://github.com/user-attachments/assets/b5143df1-39e5-4d51-9024-706ffed989a1)

Top 5 artists by track count:

    1. Bad Bunny - 40 tracks
    2. Taylor Swift - 38 tracks
    3. The Weeknd - 37 tracks
    4. SZA - 23 tracks
    5. Kendrick Lamar - 23 tracks
Note that SZA and Kendrick Lamar are tied for the 4th position with 23 tracks each.
#
## 4. Temporal Trends

### Analyzing the Number of Tracks Released Per Year
### Code:

```python
# Yearly track release trend
spotify_data['released_year'] = pd.to_numeric(spotify_data['released_year'], errors='coerce')
tracks_by_year = spotify_data['released_year'].value_counts().sort_index()

plt.figure(figsize=(12, 6))
sns.barplot(x=tracks_by_year.index, y=tracks_by_year.values, color='mediumseagreen')
plt.title('Tracks Per Year')
plt.xlabel('Year')
plt.ylabel('Number of Tracks')
plt.xticks(rotation=45, ha='right')
plt.grid(axis='y', linestyle='--', alpha=0.6)
plt.tight_layout()
plt.show()
```
### Output:
![10](https://github.com/user-attachments/assets/4c611cba-903b-405e-a180-ea69c4b2d442)

Analysis of the trends:

- 1930 to 2000: The number of tracks released each year remains relatively low and stable, with minimal fluctuations.

- 2000 to 2010: There is a slight increase in the number of tracks, but the growth is still modest.

- 2010 to 2020: The number of tracks begins to increase more noticeably, indicating a gradual upward trend.

- 2020 to 2023: There is a significant spike in the number of tracks released, with the count exceeding 350 tracks per year by 2023. This period shows the most dramatic increase in track releases.

- 2022 had the most number of tracks released.

#

### Monthly Track Release Patterns: Identifying the Peak Month
### Code:
```python
# Track releases by month
spotify_data['released_month'] = pd.to_numeric(spotify_data['released_month'], errors='coerce')
tracks_by_month = spotify_data['released_month'].value_counts().sort_index()

plt.figure(figsize=(10, 5))
sns.barplot(x=range(1, 13), y=tracks_by_month, color='steelblue')
plt.title('Track Release Distribution by Month')
plt.xlabel('Month')
plt.ylabel('Track Count')
plt.xticks(ticks=range(12), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
plt.show()
```
### Output:
![11](https://github.com/user-attachments/assets/bb53d0da-6b1e-4ce7-9513-dfce64ecd745)

Looking at the distribution, there are some clear patterns:
- The highest number of releases occur in January (120+ tracks), showing this month peaks in the year

There seems to be a seasonal pattern:
- Higher releases in the beginning of the year (January)
- A dip in February
- A spring uptick (March-May)
- Moderate levels through summer (June-July)
- Lowest point in August (40+ tracks)
- Gradual increase through fall/winter months (September-December)
- August has the lowest number of releases at around 40+ tracks, while January has the most at approximately 120+ tracks.

There's a general trend of higher activity in the first half of the year compared to the second half.
#
## 5. Genre and Music Characteristics

### Exploring the Correlation Between Streams and Musical Attributes
### Code:

```python
# Scatter plots for 'streams' vs. attributes
fig, axes = plt.subplots(1, 4, figsize=(24, 6))
scatter_params = {'s': 100, 'edgecolor': 'black'}

sns.scatterplot(ax=axes[3], x='danceability_%', y='streams', data=spotify_data, color='powderblue', **scatter_params)
axes[3].set_title("Streams vs Danceability")

sns.scatterplot(ax=axes[2], x='bpm', y='streams', data=spotify_data, color='palegreen', **scatter_params)
axes[2].set_title("Streams vs BPM")

sns.scatterplot(ax=axes[1], x='energy_%', y='streams', data=spotify_data, color='peachpuff', **scatter_params)
axes[1].set_title("Streams vs Energy")

sns.scatterplot(ax=axes[0], x='valence_%', y='streams', data=spotify_data, color='lightcoral', **scatter_params)
axes[0].set_title("Streams vs Valence")

plt.tight_layout()
plt.show()
```
### Output:
![12](https://github.com/user-attachments/assets/21bc7683-d9d0-46ec-8223-84b1ffa91b41)
![13](https://github.com/user-attachments/assets/d6525e25-e888-47b7-b651-766ca6404e63)

Among these attributes, danceability and energy appear to have the strongest influence on streams, though the correlations are still relatively modest. The relationship isn't strongly linear for any attribute, suggesting that streaming success is likely influenced by multiple factors in combination, rather than any single musical characteristic.

The majority of songs across all metrics cluster in the lower streaming ranges (below 1 billion streams), with a few notable outliers achieving significantly higher streaming numbers regardless of their musical attributes.
#

### Analyzing the Relationship Between Danceability, Energy, Valence, and Acousticness
### Code:

```python
# Categorizing valence into different levels
spotify_data['valence_category'] = pd.cut(spotify_data['valence_%'], bins=[0, 25, 50, 75, 100], labels=['Low', 'Medium', 'High', 'Very High'])

# Categorizing energy into different levels
spotify_data['energy_category'] = pd.cut(spotify_data['energy_%'], bins=[0, 25, 50, 75, 100], labels=['Low', 'Medium', 'High', 'Very High'])

# Creating subplots
fig, axes = plt.subplots(1, 2, figsize=(14, 6))
fig.suptitle('Attribute Distributions Comparison')

# Scatter plot of Danceability % by Energy %
sns.scatterplot(x='energy_%', y='danceability_%', data=spotify_data, hue='energy_category', palette="cool", ax=axes[0])
axes[0].set_title('Danceability % vs Energy %')
axes[0].set_ylabel('Danceability %')
axes[0].set_xlabel('Energy %')

# Scatter plot of Acousticness % by Valence %
sns.scatterplot(x='valence_%', y='acousticness_%', data=spotify_data, hue='valence_category', palette="coolwarm", ax=axes[1])
axes[1].set_title('Acousticness % vs Valence %')
axes[1].set_ylabel('Acousticness %')
axes[1].set_xlabel('Valence %')

plt.tight_layout(rect=[0, 0, 1, 0.96])
plt.show()
```
### Output:
![14](https://github.com/user-attachments/assets/67838841-12e2-4a2a-97b8-4926c5ed2d3e)

Based on these music attribute visualizations, we can see clear relationships between energy-danceability (positive correlation) and valence-acousticness (negative correlation). These patterns suggest that more energetic songs tend to be more danceable, while happier songs (high valence) tend to be less acoustic.

To clarify:
- A positive correlation between danceability and energy (as one increases, the other tends to increase)
- A negative correlation between valence and acousticness (as valence increases, acousticness tends to decrease)
#
## 6. Platform Popularity

### Analyzing Track Distribution: Spotify Playlists, Spotify Charts, and Apple Playlists
### Code:

```python
# Artist statistics in playlists and charts
platform_cols = ['in_spotify_playlists', 'in_apple_playlists', 'in_deezer_playlists']
spotify_data[platform_cols] = spotify_data[platform_cols].apply(pd.to_numeric, errors='coerce')
platform_counts = spotify_data[platform_cols].sum().reset_index()
platform_counts.columns = ['Platform', 'Count']

plt.figure(figsize=(12, 6))
sns.barplot(data=platform_counts, x='Platform', y='Count')
plt.title('Track Popularity Across Platforms')
plt.xlabel('Platform')
plt.ylabel('Track Count')
plt.show()
```
### Output:
![15](https://github.com/user-attachments/assets/80d239e6-71ec-497e-8feb-73dc52b1c765)

Spotify playlists contain dramatically more tracks than the other platforms (Approximately 4 million+ tracks).

#
## 7. Advanced Analysis

### Patterns in Streams: Analyzing Tracks by Key and Mode (Major vs. Minor)
### Code:

```python
# Distribution by key and mode
key_mode_data = spotify_data.groupby(['key', 'mode']).size().reset_index(name='Count')

plt.figure(figsize=(12, 6))
sns.barplot(data=key_mode_data, x='key', y='Count', hue='mode', palette='viridis')
plt.title('Track Distribution by Key and Mode')
plt.xlabel('Key')
plt.ylabel('Track Count')
plt.legend(title='Mode')
plt.show()
```
### Output:
![16](https://github.com/user-attachments/assets/9ccdb91d-2946-4883-bead-7b8843a9c014)

Overall Distribution Patterns:
- Major modes are generally more prevalent in keys C#, D, G, and G#
- Minor modes are more dominant in keys B, C, E and F
- Key F show relatively balanced distribution between major and minor modes

Notable Observations:
- C# has the highest number of tracks in major mode (70+ tracks)
- There's a significant preference for major mode in keys G and G# (around 60+ tracks each)
- Keys like C# and E show a stronger preference for minor mode
- The lowest track counts appear in D# for major mode

#

### Analyzing Artist and Genre Frequency in Playlists and Charts
### Code:

```python
# List of platforms/columns that represent the presence of artists in various playlists and charts
platforms_list = ['in_spotify_playlists', 'in_spotify_charts', 'in_apple_playlists', 'in_apple_charts', 'in_deezer_playlists', 'in_deezer_charts']

# Ensure the columns are numeric, coerce errors into NaN (useful if they contain non-numeric values)
spotify_data[platforms_list] = spotify_data[platforms_list].apply(pd.to_numeric, errors='coerce')

# Group by artist(s) and sum the appearances across the platforms
artist_appearance = spotify_data.groupby("artist(s)_name")[platforms_list].sum().sum(axis=1).sort_values(ascending=False)

# Get the top 10 artists
top_10_artists = artist_appearance.head(10).reset_index()
top_10_artists.columns = ['Artist', 'Appearance Count']

# Plot the top 10 artists by appearance count
plt.figure(figsize=(14, 7))
sns.barplot(data=top_10_artists, x='Appearance Count', y='Artist', palette="Paired", hue='Artist')  # Explicitly add 'hue'
plt.title('Top 10 Artists by Playlist/Chart Appearances', fontsize=16, fontweight='bold')
plt.xlabel('Appearance Count')
plt.ylabel('Artist')
plt.grid(axis='x', linestyle='--', alpha=0.6)
plt.show()
```
### Output:
![17](https://github.com/user-attachments/assets/29625e85-2567-4006-b1a0-c8a63cc3da7e)

Analysis:
- The artists span various genres, including pop, hip-hop, rock, and electronic.
- Pop artists like The Weeknd, Taylor Swift, and Ed Sheeran have high playlist/chart appearances, indicating their broad appeal and popularity.
- The list includes both solo artists and groups, as well as a mix of contemporary and classic artists.

This suggests that pop and mainstream genres tend to dominate playlists and charts, but there is also significant representation from other genres. The analysis indicates that certain artists consistently appear in playlists and charts, particularly in the pop genre.
#

## Opportunities for Further Analysis
Future data analysts can build on the insights gained from this exploratory analysis by diving deeper into more specific aspects of the Spotify dataset. For example, analysts could explore the impact of external factors like seasonality, marketing campaigns, or global events on streaming trends, by correlating release dates and streaming patterns with external data sources such as chart performance or social media activity. Lastly, a predictive analysis could be conducted to identify which musical attributes (e.g., danceability, energy, tempo) are most likely to contribute to a track’s success, providing valuable insights for artists and producers.
