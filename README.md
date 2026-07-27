# Netflix Content Analysis Dashboard | Power BI

A 3-page Power BI dashboard analyzing Netflix's content catalog — genres, ratings, release trends, and country-level distribution.

## 📊 Dataset
- **Source:** [Netflix Movies and TV Shows](https://www.kaggle.com/datasets/shivamb/netflix-shows) (Kaggle)
- ~8,800 titles with fields: type, title, director, cast, country, date_added, release_year, rating, duration, listed_in (genre), description

## 🧹 Data Cleaning (Power Query)
- Parsed `date_added` into a proper Date type
- Split multi-value columns (`country`, `listed_in`) into separate reference tables (`Genres`, `Countries`) using "Split into Rows," keeping the main `Titles` table at one-row-per-title
- Extracted `duration` into `duration_value` (number) and `duration_type` (min/season)
- Replaced blank `director`/`cast`/`country` values with "Unknown"
- Removed malformed rows caused by a raw-data column shift (a handful of rows had missing fields, misaligning downstream columns)

## 🗂️ Data Model
- **Titles** — fact table, one row per show
- **Genres** — bridge table (show_id → genre), many-to-one to Titles
- **Countries** — bridge table (show_id → country), many-to-one to Titles
- **Date** — calendar table built with `CALENDAR()` in DAX, marked as the official Date table, related to `Titles[date_added]`

## 🧮 Key DAX Measures
```DAX
Total Titles = DISTINCTCOUNT(Titles[show_id])
Movies Count = CALCULATE([Total Titles], Titles[type] = "Movie")
TV Shows Count = CALCULATE([Total Titles], Titles[type] = "TV Show")
Genre Count = COUNTROWS(Genres)
Country Count = COUNTROWS(Countries)
```

## 📄 Report Pages

**1. Overview**
- KPI cards: Total Titles, Movies Count, TV Shows Count
- Donut chart: Movies vs TV Shows split
- Bar chart: Top 10 genres
- Line chart: Titles added by year (shows Netflix's 2016–2019 content expansion)

**2. Deep Dive**
- Bar chart: Title count by content rating
- Clustered column chart: Movies vs TV Shows added per year
- Slicers: Type, Rating, Year (range)

**3. Country Analysis**
- Bar chart: Top 10 countries by title count
- Filled map: Global content distribution by country

## 🎨 Design
Netflix-inspired theme — red accent (`#E50914`) applied across all visuals for a consistent brand feel.

## 🛠️ Tools Used
- Power BI Desktop
- Power Query (M) for data cleaning/transformation
- DAX for measures and the calendar table

## 📸 Preview
*(Add screenshots of each page here before publishing — Overview, Deep Dive, Country Analysis)*

## 🚀 How to Use
1. Download `netflix_titles.csv` from the [Kaggle dataset link](https://www.kaggle.com/datasets/shivamb/netflix-shows)
2. Open the `.pbix` file in Power BI Desktop
3. Home → Refresh (point the data source to your local CSV path if prompted)

---
*Built as a portfolio project to practice Power Query data cleaning, data modeling, DAX, and dashboard design.*
