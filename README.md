<p align="center">
  <img src="walmartecomm.jpg" alt="Walmart eCommerce" width="400"/>
</p>

# Walmart Sales ETL Pipeline

An end-to-end ETL pipeline in Python and SQL that analyses weekly Walmart store sales around public holidays. It extracts sales data from PostgreSQL, merges it with complementary economic data, cleans and aggregates it, then loads the results to CSV with a validation check.

> **Project type:** DataCamp project, completed as part of my data engineering training.

## Business context

Walmart's sales are strongly affected by public holidays such as the Super Bowl, Labour Day, Thanksgiving and Christmas. The goal is a repeatable pipeline that prepares clean data for analysing supply and demand around those periods.

## Data sources

| Source | Description |
|---|---|
| `grocery_sales` (PostgreSQL table) | Store ID, week of sales and weekly sales per store |
| `extra_data.parquet` | Holiday flag, temperature, fuel price, CPI, unemployment, markdowns, department, store size and type |

> The `grocery_sales` table is hosted in DataCamp's PostgreSQL environment, so the SQL cell only runs inside that workspace.

## Pipeline steps

1. **Extract:** query `grocery_sales` with SQL and merge it with the parquet file on `index`.
2. **Transform:**
   - fill missing numeric values with the column median
   - convert `Date` to datetime and create a `Month` column
   - keep only weeks with weekly sales above $10,000
   - select the final columns: `Store_ID`, `Month`, `Dept`, `IsHoliday`, `Weekly_Sales`, `CPI`, `Unemployment`
3. **Aggregate:** calculate average weekly sales per month.
4. **Load:** write the cleaned data to `clean_data.csv` and the monthly summary to `agg_data.csv`.
5. **Validate:** a `validation()` function confirms both output files exist.

## Results

- `clean_data.csv`: about 106,000 cleaned rows
- `agg_data.csv`: average weekly sales for each of the 12 months
- Average weekly sales peak in **December** (about $39,249), followed by **November** (about $36,594), consistent with holiday-driven demand

## Tech stack

`Python` · `pandas` · `SQL` · `PostgreSQL` · `Parquet` · `Jupyter Notebook`

## Repository contents

```
├── notebook.ipynb      # ETL pipeline: extract, transform, aggregate, load, validate
├── extra_data.parquet  # Complementary data
├── clean_data.csv      # Cleaned output
└── agg_data.csv        # Monthly average weekly sales
```

## What I practised

- Structuring an ETL process as separate, reusable functions
- Merging a SQL table with a parquet file
- Handling missing values and filtering data
- Aggregating with `groupby` and saving outputs
- Adding a basic validation step to a pipeline
