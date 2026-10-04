# Netflix Titles — Data Cleaning

Cleans `netflix_titles.csv`: inspects the raw data, handles missing values
column by column, fixes a mixed-type column, parses dates, and exports a
clean CSV.

## Files

| File | Description |
|---|---|
| `clean_netflix_titles.ipynb` | Jupyter notebook with the full cleaning process, explained step by step, with outputs included. |
| `netflix_titles.csv` | **Not included** — place your own copy in the same folder before running. |
| `clean_netflix_titles.csv` | Output file — created when you run the notebook. |

## Setup

pip install pandas jupyter

Place `netflix_titles.csv` in the same folder as the notebook.

## Running

jupyter notebook clean_netflix_titles.ipynb

or open it directly in VS Code / JupyterLab and run all cells. The cleaned file is saved as `clean_netflix_titles.csv` in the same folder.


## What the notebook does

1. **Inspect** — `.info()`, `.describe(include="all")`, and `.head()` on the raw data to see shape, dtypes, and missing-value counts.
2. **Handle missing values, column by column:**
   - `rating` / `duration` — 3 rows had a duration value (e.g. `"74 min"`) sitting in the `rating` column, a data-entry error rather than a real missing value. Fixed first, before treating either column as missing.
   - `director`, `cast`, `country` — filled with `"Unknown"`. Dropping rows wasn't an option since `director` alone is missing for ~30% of titles.
   - `rating` — any remaining missing values filled with `"Not Rated"`, matching the existing `"NR"`/`"UR"` categories already in the data.
   - `date_added` — 10 rows have no value in the source data. Left as `NaT` (a real null) rather than invented, since a date can't be reasonably guessed.
3. **Fix the mixed-type `duration` column** — it stores two different units as text (`"90 min"` for movies, `"2 Seasons"` for TV shows). Split into two numeric columns, `duration_minutes` and `duration_seasons`, so it's usable in calculations. The original `duration` text column is kept for reference.
4. **Parse `date_added`** — stripped of stray whitespace, then converted from text to a real `datetime` column.
5. **Export** — saved as `clean_netflix_titles.csv`.

## Known limitations

- `director`, `cast`, and `country` can hold multiple values per row, separated by commas (e.g. `"United States, Canada"`), and are left that way rather than split into separate rows or columns.
- `listed_in` (genre) is left untouched — it also holds multiple comma-separated categories per row.

## Dashboard

![Dashboard Screenshot](dashboard.png)

https://roadmap.sh/projects/cleaning-netflix-dataset
