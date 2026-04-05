---
name: plotnine-viz
description: >
  Use this skill for ALL data visualization tasks using the plotnine library (Python's ggplot2).
  Triggers: "plotnine chart", "plotnine world map", "ggplot2 python", "geom_map", "geom_point",
  "geom_col", "geom_bar", "geom_line", "geom_smooth", "linear regression chart", "scatterplot plotnine",
  "time series plotnine", "bar chart plotnine", "choropleth map python", "world map plotnine",
  "theme_minimal plotnine", "aes plotnine", "facet_wrap", "scale_fill", "scale_color",
  "plotnine custom theme", "plotnine labels", "plotnine save", "assignment visualisations",
  "4 charts plotnine", "unicef visualisations", "UNICEF charts python".
  ALWAYS use this skill when building visualisations with plotnine.
---

# Plotnine Visualisation Master

You are an expert data visualisation engineer using **plotnine** — Python's implementation of the Grammar of Graphics (ggplot2).

Your style:
- **Grammar-first** — every chart is `ggplot(data, aes(...)) + geom_* + scale_* + labs() + theme()`
- **Design-aware** — never leave default colours; always choose a palette intentional to the story
- **Narrative-tied** — every chart must earn its place in the report
- **Quarto-ready** — outputs must work inside `.qmd` code cells

---

## CORE GRAMMAR

```python
from plotnine import *

(
    ggplot(df, aes(x="col_x", y="col_y", color="group_col"))  # mapping
    + geom_point()              # geometric object
    + scale_color_brewer(type="qual", palette="Set2")          # scale
    + labs(title="...", x="...", y="...", color="...")          # labels
    + theme_minimal()           # base theme
    + theme(figure_size=(10, 6))                               # overrides
)
```

**Rules:**
- `aes()` maps data columns to visual properties — never hardcode values inside `aes()`
- Hardcoded values go OUTSIDE `aes()` → `geom_point(size=2, alpha=0.6)`
- Chained with `+`, not `.`

---

## CHART 1: WORLD MAP (CHOROPLETH)

```python
from plotnine import *
from plotnine.data import world  # built-in world geometry

# Merge your aggregated country data with world geometry
world_df = world.copy()
# world has columns: name, continent, iso_a3 (check with world_df.columns)

merged = world_df.merge(
    country_avg_pandas,      # your aggregated data with 'alpha_3_code' and 'avg_value'
    left_on="iso_a3",
    right_on="alpha_3_code",
    how="left"
)

(
    ggplot(merged)
    + geom_map(aes(fill="avg_sanitation"), color="white", size=0.1)
    + scale_fill_gradient(
        low="#fee5d9",
        high="#a50f15",
        na_value="#d9d9d9",
        name="Coverage (%)"
    )
    + theme_void()
    + theme(
        figure_size=(14, 7),
        plot_title=element_text(size=14, weight="bold", ha="center"),
        legend_position="bottom"
    )
    + labs(title="Global Sanitation Coverage (2000–2024 Average)")
)
```

**Common map mistakes:**
- `world` iso codes are `iso_a3` — match with your `alpha_3_code`
- Missing countries show as grey — set `na_value="#d9d9d9"` explicitly
- Use `theme_void()` to remove axes/grid from maps
- Always use `color="white"` border on `geom_map` for country outlines

---

## CHART 2: BAR CHART

```python
(
    ggplot(top15_df, aes(x="reorder(country, obs_value)", y="obs_value"))
    + geom_col(fill="#c0392b", alpha=0.85)
    + coord_flip()                          # horizontal bars — always more readable for country names
    + geom_text(
        aes(label="obs_value"),
        ha="left", size=8, nudge_y=0.5      # value labels on bars
    )
    + labs(
        title="Top 15 Countries by Refugee Burden",
        subtitle="Refugees hosted per USD of GNI per capita (latest year)",
        x="", y="Refugees per USD GNI per capita"
    )
    + theme_minimal()
    + theme(
        figure_size=(10, 7),
        panel_grid_major_y=element_blank(),
        plot_title=element_text(weight="bold")
    )
)
```

**Tips:**
- `reorder(country, obs_value)` sorts bars by value — always do this
- `coord_flip()` makes country names readable
- Remove horizontal gridlines with `panel_grid_major_y=element_blank()`

---

## CHART 3: SCATTERPLOT WITH LINEAR REGRESSION

```python
(
    ggplot(scatter_df.dropna(subset=["gdp_per_capita", "sanitation_pct"]),
           aes(x="gdp_per_capita", y="sanitation_pct"))
    + geom_point(aes(color="region"), size=2.5, alpha=0.6)
    + geom_smooth(method="lm", color="#2c3e50", se=True, linetype="dashed")
    + scale_x_log10(labels=lambda l: [f"${x:,.0f}" for x in l])
    + scale_color_brewer(type="qual", palette="Set2")
    + labs(
        title="Sanitation Coverage vs GDP per Capita",
        subtitle="Each point = one country-year | log scale x-axis",
        x="GDP per Capita (constant 2015 USD, log scale)",
        y="Population with Sanitation Access (%)",
        color="Region"
    )
    + theme_minimal()
    + theme(
        figure_size=(11, 7),
        legend_position="right",
        plot_title=element_text(weight="bold")
    )
)
```

**Tips:**
- `scale_x_log10()` is almost always needed for GDP — the distribution is very skewed
- `geom_smooth(method="lm", se=True)` draws the regression line + confidence band
- Color by region to reveal patterns beyond the regression line

---

## CHART 4: TIME SERIES

```python
countries = ["India", "Nigeria", "Bangladesh", "Ethiopia", "Pakistan", "Afghanistan"]

(
    ggplot(ts_df[ts_df["country"].isin(countries)],
           aes(x="year", y="sanitation_pct", color="country", group="country"))
    + geom_line(size=1.1)
    + geom_point(size=2, alpha=0.7)
    + scale_color_brewer(type="qual", palette="Dark2")
    + scale_x_continuous(breaks=range(2000, 2025, 4))
    + labs(
        title="Sanitation Coverage Over Time: Selected Countries",
        subtitle="2000–2024 | % population using on-site sanitation facilities",
        x="Year", y="Coverage (%)", color="Country"
    )
    + theme_minimal()
    + theme(
        figure_size=(12, 6),
        panel_grid_minor=element_blank(),
        plot_title=element_text(weight="bold")
    )
)
```

**Tips:**
- Always set `group="country"` alongside `color="country"` so lines connect
- `geom_point()` on top of `geom_line()` marks actual data observations
- `scale_x_continuous(breaks=range(...))` controls x-axis tick intervals

---

## CONSISTENT THEME ACROSS ALL CHARTS

Define once and reuse:

```python
my_theme = (
    theme_minimal()
    + theme(
        figure_size=(11, 6),
        plot_title=element_text(size=13, weight="bold"),
        plot_subtitle=element_text(size=10, color="#666666"),
        axis_title=element_text(size=10),
        legend_title=element_text(size=9, weight="bold"),
        panel_grid_minor=element_blank()
    )
)

# Use as:
chart = ggplot(...) + geom_*() + labs(...) + my_theme
```

---

## SAVING CHARTS

```python
# Save as PNG
chart.save("world_map.png", dpi=150, width=14, height=7)

# In Quarto — just let the cell output render it inline
# The chart object will auto-display when it's the last expression in the cell
```

---

## COLOUR PALETTE REFERENCE

| Use Case | Palette | Code |
|---|---|---|
| Sequential (low→high) | Red gradient | `scale_fill_gradient(low="#fee5d9", high="#a50f15")` |
| Sequential (diverging) | Blue-Red | `scale_fill_gradient2(low="blue", mid="white", high="red", midpoint=50)` |
| Categorical (regions) | ColorBrewer Set2 | `scale_color_brewer(type="qual", palette="Set2")` |
| Categorical (dark) | ColorBrewer Dark2 | `scale_color_brewer(type="qual", palette="Dark2")` |
| Single fill (bars) | UNICEF blue | `fill="#1cabe2"` |

---

## COMMON MISTAKES

| Mistake | Fix |
|---|---|
| Hardcoding values inside `aes()` | `aes(color="blue")` is WRONG — use `geom_*(color="blue")` |
| Forgetting `group=` in line charts | Lines won't connect without `aes(group="country")` |
| Not converting polars → pandas first | `df.to_pandas()` before passing to `ggplot()` |
| No `coord_flip()` on bar charts with long names | Always flip for country names |
| Default grey background | Always use `theme_minimal()` or `theme_bw()` at minimum |
| Not using log scale for GDP | GDP distributions are skewed — use `scale_x_log10()` |
| Ignoring `na_value` on maps | Set `na_value="#d9d9d9"` or missing countries look broken |
