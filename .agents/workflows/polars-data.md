---
description: Use this skill for ALL data wrangling, cleaning, transformation, EDA, and analysis tasks using the Polars library.   Triggers: "load csv polars", "polars dataframe", "filter polars", "group_by polars", "join polars",   "merge datasets", "data cleanin
---

---
name: polars-data
description: >
  Use this skill for ALL data wrangling, cleaning, transformation, EDA, and analysis tasks using the Polars library.
  Triggers: "load csv polars", "polars dataframe", "filter polars", "group_by polars", "join polars",
  "merge datasets", "data cleaning python", "EDA polars", "polars vs pandas", "reshape data",
  "aggregate data", "polars expressions", "lazy frame", "polars select", "polars with_columns",
  "drop nulls polars", "polars describe", "data transformation", "combine CSVs", "polars group aggregate",
  "unicef data", "indicator data", "metadata join", "polars for data analysis assignment".
  ALWAYS use this skill when working with Polars for any data task.
---

# Polars Data Master

You are an expert data analyst using **Polars** — the fast, expressive DataFrame library for Python.
Your style:
- **Lazy evaluation aware** — know when to use `LazyFrame` vs `DataFrame`
- **Expression-first** — use `pl.col()` everywhere, avoid Python loops on data
- **EDA-thorough** — always explore before transforming
- **Assignment-ready** — tie every transformation to the narrative/story

---

## CORE PHILOSOPHY

> Polars is not pandas. Don't translate pandas habits. Think in **expressions**.

```python
import polars as pl

# The Polars way
df.filter(pl.col("year") >= 2010).group_by("country").agg(pl.col("value").mean())

# NOT the pandas way
df[df["year"] >= 2010].groupby("country")["value"].mean()
```

---

## MODULE 1: LOADING & INSPECTING

```python
# Load
df = pl.read_csv("file.csv")
df = pl.read_csv("file.csv", null_values=["", "NA", "N/A"])

# Inspect — ALWAYS do this first
df.shape          # (rows, cols)
df.dtypes         # column types
df.columns        # column names
df.head(5)
df.describe()     # stats: mean, std, min, max, null count
df.null_count()   # nulls per column
```

---

## MODULE 2: SELECTING & RENAMING

```python
# Select specific columns
df.select(["country", "alpha_3_code", "time_period", "obs_value"])

# Select by type
df.select(pl.col(pl.Float64))

# Rename
df.rename({"obs_value": "sanitation_pct", "time_period": "year"})

# Cast column type
df.with_columns(pl.col("year").cast(pl.Int32))
```

---

## MODULE 3: FILTERING

```python
# Single condition
df.filter(pl.col("sex") == "Total")
df.filter(pl.col("year") >= 2010)

# Multiple conditions (AND)
df.filter(
    (pl.col("sex") == "Total") &
    (pl.col("year") >= 2000)
)

# OR condition
df.filter(
    (pl.col("country") == "India") |
    (pl.col("country") == "Nigeria")
)

# Filter by list of values
df.filter(pl.col("country").is_in(["India", "Nigeria", "Brazil"]))

# Drop nulls
df.drop_nulls(subset=["obs_value"])
df.filter(pl.col("obs_value").is_not_null())
```

---

## MODULE 4: ADDING / TRANSFORMING COLUMNS

```python
# with_columns — add or modify columns
df.with_columns(
    pl.col("obs_value").round(2).alias("value_rounded"),
    (pl.col("obs_value") / 100).alias("value_fraction"),
    pl.lit("Africa").alias("region")
)

# Conditional column (like if/else)
df.with_columns(
    pl.when(pl.col("obs_value") >= 50)
    .then(pl.lit("High"))
    .otherwise(pl.lit("Low"))
    .alias("coverage_level")
)

# String operations
df.with_columns(pl.col("country").str.to_lowercase())
```

---

## MODULE 5: GROUPING & AGGREGATING

```python
# Basic group_by + agg
df.group_by("country").agg(
    pl.col("obs_value").mean().alias("avg_value"),
    pl.col("obs_value").max().alias("max_value"),
    pl.col("obs_value").count().alias("n_years")
)

# Multiple group keys
df.group_by(["country", "year"]).agg(
    pl.col("obs_value").first()
)

# Sort after grouping
df.group_by("country").agg(
    pl.col("obs_value").mean().alias("avg_value")
).sort("avg_value", descending=True)

# Top N
df.group_by("country").agg(
    pl.col("obs_value").mean().alias("avg_value")
).sort("avg_value", descending=True).head(15)
```

---

## MODULE 6: JOINING DATASETS

```python
# Left join — keep all rows from left, match from right
df1.join(metadata, on=["alpha_3_code", "year"], how="left")

# Join on different column names
df1.join(df2, left_on="alpha_3_code", right_on="iso3", how="left")

# Inner join — only rows that match in both
df1.join(df2, on="country", how="inner")

# Common pattern: indicator + metadata
sanitation = (
    pl.read_csv("unicef_indicator_1.csv")
    .filter(pl.col("sex") == "Total")
    .rename({"obs_value": "sanitation_pct", "time_period": "year"})
    .select(["country", "alpha_3_code", "year", "sanitation_pct"])
    .with_columns(pl.col("year").cast(pl.Int32))
)

metadata = (
    pl.read_csv("unicef_metadata.csv")
    .rename({"GDP per capita (constant 2015 US$)": "gdp_per_capita"})
    .select(["alpha_3_code", "year", "gdp_per_capita", "Life expectancy at birth, total (years)"])
)

merged = sanitation.join(metadata, on=["alpha_3_code", "year"], how="left")
```

---

## MODULE 7: EDA CHECKLIST

Before building any chart, run this:

```python
# 1. Shape and types
print(df.shape)
print(df.dtypes)

# 2. Null summary
print(df.null_count())

# 3. Descriptive stats
print(df.describe())

# 4. Unique values in key columns
print(df["country"].n_unique())
print(df["year"].unique().sort())

# 5. Value distribution of target column
print(df["obs_value"].describe())

# 6. Check for duplicates
print(df.is_duplicated().sum())
```

**In your report narrative, mention specific values you found:**
> "After removing null values, the dataset covers 154 countries from 2000–2024. The average sanitation coverage is X%, with a minimum of Y% (country Z) and maximum of W%."

---

## MODULE 8: CONVERTING TO PANDAS (FOR PLOTNINE)

plotnine works with pandas DataFrames, so convert at the last step:

```python
# Convert for plotting
df_plot = df.to_pandas()

# Keep polars for all transformations, convert only when needed
```

---

## COMMON MISTAKES

| Mistake | Fix |
|---|---|
| Using pandas syntax on polars | Use `pl.col()`, not `df["col"]` |
| Forgetting `.alias()` after transformations | Always name new columns |
| Joining on mismatched types (str vs int) | Cast with `.cast(pl.Int32)` before joining |
| Not filtering `sex == "Total"` | Indicator files have Male/Female rows too — always filter |
| Modifying without reassigning | `df = df.filter(...)` — polars is immutable |
| Forgetting `.to_pandas()` before plotnine | plotnine needs pandas, not polars |

---

## ASSIGNMENT-SPECIFIC PATTERNS

```python
# World map prep — one row per country, average across all years
country_avg = (
    df.group_by(["country", "alpha_3_code"])
    .agg(pl.col("sanitation_pct").mean().alias("avg_sanitation"))
    .drop_nulls()
    .to_pandas()
)

# Bar chart prep — latest year, top N countries
latest = (
    df.filter(pl.col("year") == df["year"].max())
    .sort("obs_value", descending=True)
    .head(15)
    .to_pandas()
)

# Time series prep — select specific countries
ts = (
    df.filter(pl.col("country").is_in(["India", "Nigeria", "Bangladesh", "Ethiopia", "Pakistan"]))
    .sort(["country", "year"])
    .to_pandas()
)

# Scatterplot prep — join indicator + GDP, one row per country-year
scatter = (
    sanitation
    .join(metadata, on=["alpha_3_code", "year"], how="inner")
    .drop_nulls(subset=["sanitation_pct", "gdp_per_capita"])
    .to_pandas()
)
```
