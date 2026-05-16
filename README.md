# Spotify Tracks Analysis — Power BI Dashboard

An interactive single-page Power BI dashboard built on the Kaggle Spotify Tracks Dataset, exploring audio features, popularity trends, genre profiles, and artist statistics across 114,000 tracks.

---

## Project overview

This project was completed as an academic data analysis exercise. The goal was to take a raw flat CSV file, clean and model it using Power Query and DAX, and build a structured dashboard that communicates meaningful insights about music trends across genres.

---

## Dataset

- **Source:** [Kaggle — Spotify Tracks Dataset](https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset)
- **Size:** 114,000 rows × 22 columns
- **Coverage:** 114 genres, 1,000 tracks per genre
- **Key columns:** `track_id`, `artists`, `track_name`, `track_genre`, `popularity`, `danceability`, `energy`, `loudness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, `tempo`, `duration_ms`, `explicit`

---

## Tools used

| Tool | Purpose |
|---|---|
| Power BI Desktop | Dashboard building, DAX measures, data modelling |
| Power Query (M) | Data cleaning and transformation |
| Kaggle | Dataset source |

---

## Data preparation steps

All cleaning was done inside Power Query Editor before loading into the model.

1. **Removed junk columns** — deleted `Unnamed: 0` and `Unnamed: 0.1` (leftover Python row indexes)
2. **Removed null rows** — filtered out 3 rows with missing `track_name`, `artists`, and `album_name`
3. **Fixed data types** — set `explicit` to True/False, numeric columns to correct types
4. **Added `duration_min`** — converted `duration_ms` to minutes (`duration_ms / 60000`)
5. **Added `popularity_tier`** — bucketed popularity score into: Niche (0–25), Moderate (26–50), Popular (51–75), Viral (76–100)
6. **Decoded `key_name`** — mapped integer key values (0–11) to musical note names (C, C#, D…)
7. **Decoded `mode_name`** — mapped 0 → Minor, 1 → Major
8. **Filtered duration outliers** — removed tracks over 10 minutes (`duration_ms > 600,000`) for music-only analysis



---

## DAX measures

```dax
Total Tracks = COUNTROWS('fact_tracks')

Avg Popularity = AVERAGE('fact_tracks'[popularity])

Avg Danceability = AVERAGE('fact_tracks'[danceability])

Avg Energy = AVERAGE('fact_tracks'[energy])

Avg Acousticness = AVERAGE('fact_tracks'[acousticness])

Avg Valence = AVERAGE('fact_tracks'[valence])

Avg Duration (min) = AVERAGE('fact_tracks'[duration_min])

Explicit Track % =
    DIVIDE(
        COUNTROWS(FILTER('fact_tracks', 'fact_tracks'[explicit] = TRUE())),
        COUNTROWS('fact_tracks')
    )
```

---

## Dashboard layout

The dashboard is a single page (1280 × 720) divided into four zones:

| Zone | Content |
|---|---|
| Zone 1 — KPI cards|Total tracks, avg popularity,avg danceability, explicit % |
| Zone 2 — Slicers | Genre, popularity tier, explicit, mode (Major/Minor) |
| Zone 3 — Main charts |  genres by avg popularity (bar), tracks by popularity tier (donut) |
| Zone 4 — Detail charts | Energy & danceability by genre (bar), top artists by track count (bar) |

---

## Key insights

- **Pop, K-pop, and Hip-hop** rank highest in average popularity across all 114 genres
- **Danceability** clusters strongly between 0.6–0.8 for the most popular genres
- Around **24.6%** of all tracks in the dataset are marked explicit
- High-energy genres do not always correlate with high danceability — classical and ambient genres show high energy with low danceability scores
- The majority of tracks (~60%) fall into the **Niche** popularity tier (score 0–25), suggesting most tracks on Spotify have limited reach


---
