# IPL Data Analysis
 
This project is an end-to-end data analysis on Indian Premier League cricket data. It is structured in three progressive layers — starting from raw NumPy array operations, moving into a full Pandas pipeline, and finally building a composite scoring model to predict the Orange Cap winner. The data used covers ball-by-ball delivery records and match-level metadata across multiple IPL seasons.
 
---
 
## Project Structure
 
```
IPL-DATA_ANALYSIS/
│
├── data/
│   ├── deliveries.csv
│   └── matches.csv
│
├── numpy/
│   └── data_analysis_numpy.ipynb
│
├── pandas/
│   ├── data_analysis_pandas.ipynb
│   ├── output/
│   
│
├── trend_analysis/
│   └── boundary_analysis.ipynb
│
└── README.md
```
 
---
 
## Datasets
 
The project uses two CSV files stored in the `data/` folder.
 
**deliveries.csv** contains ball-by-ball data — every delivery bowled across all matches, including the batter, bowler, runs scored, extras, wicket information, and over number.
 
**matches.csv** contains match-level information — teams playing, venue, city, season, winner, result margin, and the player of the match.
 
---
 
## NumPy Analysis
 
**`numpy/data_analysis_numpy.ipynb`**
 
This notebook does the entire analysis using raw NumPy arrays without any Pandas. The goal was to build a strong foundation in vectorised operations, masking, and array-based aggregation.
 
The tasks covered include:
 
- Loading both CSVs using `np.genfromtxt` and handling missing values
- Calculating total runs scored per match using boolean masking and `np.sum`
- Finding the top 5 batters by total runs using `np.bincount` and `np.argsort`
- Computing strike rates for every batter (runs divided by balls, multiplied by 100)
- Computing economy rates for every bowler (runs conceded per over)
- Analysing average runs scored in each of the 20 overs across all matches
- Counting total fours and sixes, and finding the team with the most boundaries
- Isolating death over (overs 16–20) performance by runs and team
- Identifying the single highest-scoring match in the dataset
- Breaking down runs scored per team per match
 
---
 
## Pandas Analysis
 
**`pandas/data_analysis_pandas.ipynb`**
 
This notebook builds a proper data pipeline from scratch — ingestion, cleaning, transformation, and analysis — all using Pandas.
 
### Data Ingestion
 
Both CSVs are loaded using `pd.read_csv`. The shapes, column names, and data types are inspected before any processing begins.
 
### Data Cleaning
 
The raw data had several issues that needed to be handled:
 
- Null values in columns like `player_dismissed`, `dismissal_kind`, and `fielder` were filled with `'none'`
- Team names were inconsistent across seasons — for example, `Delhi Daredevils` and `Delhi Capitals` refer to the same franchise. These were standardised across both files
- Null city values were resolved using a manual venue-to-city mapping for Dubai and Sharjah stadiums
- Match ID integrity was validated to confirm every delivery record links to a valid match
 
### Data Transformation
 
Before analysis, several new columns were engineered — total runs calculated independently, pure batting runs, and a ball number column that converts over and ball into a single sequential index. The two DataFrames were then merged into a single unified `df_final` on `match_id`.
 
### Analysis
 
- **Total runs per match** — grouped by match ID and summed
- **Runs per team per match** — multi-key groupby on match and batting team
- **Top 10 batters** — total runs scored across all matches, sorted descending
- **Strike rates** — computed on valid balls only (excluding wides, byes, leg byes, and penalties)
- **Top bowlers by economy** — runs conceded divided by overs, filtered for qualified bowlers
- **Most consistent batter** — average runs per match for players with at least 30 appearances
 

---
 
## IPL Boundary Forecaster: A Data-Driven Approach

The Pitch
Can we figure out which IPL franchise will hit the most boundaries (4s and 6s) next season before a single ball is bowled?

This project uses pure data analytics and historical statistics to forecast a team's boundary-hitting potential for the upcoming season. Instead of relying on complex "black-box" machine learning models, this project uses a highly interpretable, heuristic approach: analyzing the actual players drafted in the auction, stadium factors, and recent historical momentum.

---

#### How It Works (The Logic)
Predicting the future in sports requires smoothing out the noise. This project calculates a team's potential using three heavy-hitting data points:

The "Player DNA" (Career Squad Metrics): It doesn't just look at the team name; it looks at the roster. By inputting the exact post-auction squad, the script calculates the career boundary percentages and strike rates of those specific batters. If a team buys three power-hitters, their projected boundaries instantly scale up!

Home Venue Advantage: The data mathematically understands that a team playing half their matches at the M. Chinnaswamy Stadium (historically yielding ~50 boundaries/match) will inherently score differently than a team playing on a slower pitch.

The 3-Year Rolling Average: To account for a team's general form and team culture, it uses a 3-year average of past boundaries. This acts as a reliable baseline, smoothing out the variance of a single lucky or unlucky season.

---
 
## Tech Stack
 
- **Python 3**
- **NumPy** — array operations, masking, bincount
- **Pandas** — data cleaning, transformation, groupby analysis
- **Matplotlib** — visualisation
- **Jupyter Notebook** — interactive development environment
