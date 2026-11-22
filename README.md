# Netflix-Assignment
This assignment requires you to submit the complete Jupyter Notebook developed during the Netflix Data Analysis project. The notebook should demonstrate the full analytics workflow from data loading through insights and conclusions.
Netflix-
# Netflix ETL Pipeline

A Python-based ETL (Extract, Transform, Load) pipeline that combines
Netflix-style user, movie, and watch-history CSV data, calculates
viewing and engagement metrics, and exports analytical reports.

## Overview
The pipeline runs five stages:

1.  **Extract**reads the raw CSV files into pandas DataFrames.
2.  **lean**removes duplicate records, drops rows missing key
    identifiers, fills missing genres, and standardizes selected data
    types.
3.  **Transform**joins watch history with user and movie
    information and calculates viewing completion percentages.
4.  **Analyze** calculates overall KPIs and grouped statistics.
5.  **Load and report** saves the transformed warehouse dataset and
    CSV reports, then prints and saves an executive summary.

**Project structure **

project/
├── data/
│   ├── Users.csv
│   ├── Movies.csv
│   └── WatchHistory.csv
├── output/                  # Created automatically when the pipeline runs
├── analysis.py
├── clean.py
├── extract.py
├── main.py
├── report.py
├── transform.py
└── README.md


**Requirements**
-   Python 3
-   pandas

**Input data**
The transformation and analysis stages also use the corresponding fields
from these datasets. In particular, completion percentage is calculated
when both `watch_duration_mins` and `duration_mins` are available.

## Run the pipeline

**From the project root, run:**
bash
python main.py

The entry point calls `run_pipeline()` in `main.py`. The input directory
is configured as `data`, and the output directory is configured as
`output`.

**The cleaning stage:**

-   Keeps the first record for each `user_id` in the users data and
    removes rows without a `user_id`.
-   Keeps the first record for each `movie_id` in the movies data and
    removes rows without a `movie_id` or `title`.
-   Removes duplicate watch-history rows and rows missing `user_id` or
    `movie_id`.
-   Replaces missing movie genres with `Unknown` when the `genre` column
    exists.
-   Converts `watch_duration_mins` to numeric values, coercing invalid
    values and filling them with `0`.
-   Converts `watch_date` to datetime values when that column exists;
    invalid dates become missing values.

The transformation stage uses inner joins from watch history to users
and movies. As a result, sessions without matching user or movie records
are not included in the transformed dataset.

When both viewing duration and movie duration are present,
`completion_pct` is calculated as:

**(watch_duration_mins / duration_mins) * 100**

The result is capped at 100 and rounded to two decimal places.

## Analysis and reports

The analysis stage calculates:

-   Total sessions, unique users, and unique movies
-   Total watch time in minutes and hours
-   Average and median session duration
-   Average completion percentage
-   Number of sessions with a completion percentage of at least 90%
-   Top 10 movies by total watch time (with views used as a secondary
    sort)
-   Viewing and watch-time statistics by genre

Additional breakdowns are produced when the relevant columns are
available:

-   Subscription-plan statistics
-   The five most active users
-   Engagement statistics by country
-   Reporting-period start and end dates when valid watch dates are
    available

## Output files

The pipeline creates the `output` directory if it does not already
exist.

  Output And Description
  
  `netflix_warehouse.csv`             
  The transformed, joined dataset

  `overview.csv`                      
  Not generated as a CSV; the overview is included in the executive summary

  `top_movies.csv`                    
  Top movie statistics

  `genre_stats.csv`                  
   Genre-level statistics

  `plan_stats.csv`                   
   Subscription-plan statistics, when available

  `top_users.csv`                    
   Top-user statistics, when available

  `country_stats.csv`                 
  Country-level statistics, when available

  `executive_summary.txt`
   Human-readable summary printed to the console and saved to a text file


Only analysis results represented as pandas DataFrames are exported as
individual CSV reports. The `overview` result is a dictionary and is
therefore included in the executive summary rather than saved as a CSV.

## Notes

-   Run the command from the project root so the relative `data` and
    `output` paths resolve as expected.
-   The pipeline expects the CSV filenames shown above.
-   Optional report sections depend on the presence of their required
    columns.
-   The code does not create or download source datasets; provide the
    input CSV files yourself.
