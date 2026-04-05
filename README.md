# Bearing the Burden: Refugees, Wealth & Sanitation

![UNICEF Analytics Report](https://img.shields.io/badge/Status-Complete-success)
![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![Polars](https://img.shields.io/badge/Data%20Wrangling-Polars-orange.svg)
![Plotnine](https://img.shields.io/badge/Visualisation-Plotnine-red.svg)
![Quarto](https://img.shields.io/badge/Rendering-Quarto-blueviolet)

This repository contains the final submission for the **BAA1030 UNICEF Data Analytics Assignment**, designed to expose the compounded inequalities surrounding the global refugee crisis and basic sanitation infrastructure.

🌍 **Live Report:** [View the full interactive HTML report here](https://aayush110410.github.io/unicef-analytics-report/report.html)

---

## 📌 Project Overview

This data-journalism report connects two critical UNICEF indicators across 180 countries and 24 years:
1. **Refugee Burden:** The absolute number of refugees hosted, normalised against national wealth (per $1 GNI per capita).
2. **Sanitation Access:** The percentage of the population with access to safe on-site sanitation.

The underlying narrative addresses **SDG 6 (Clean Water & Sanitation)** and **SDG 10 (Reduced Inequalities)**, demonstrating a structural "Sanitation Trap"—the countries hosting the most refugees are structurally the least equipped to do so, bearing nearly 23x the global average burden.

## 🛠️ Technology Stack & Execution

The report was built using **Python** within a Quarto environment to generate a professional, self-contained HTML document.

- **Data Wrangling:** [Polars](https://pola-rs.github.io/polars/) was used extensively for high-performance data transformations, dealing with missing values, performing `group_by` aggregations, dynamically generating decades, and managing complex inner/left joins between indicator and World Bank metadata datasets.
- **Visualisation Design:** [Plotnine](https://plotnine.readthedocs.io/en/stable/) (based on the Grammar of Graphics) combined with `geopandas` was utilized to construct visually appealing, non-generic plots styled specifically against a designated data-journalism color palette (red/orange/brown).

## 📊 Visualisations Included

The Quarto report contains four core programmatic visualisations aligned perfectly with the data narrative:
1. **World Map Chart:** A choropleth map highlighting the 2022 refugee burden concentration in the Global South (East Africa & South Asia).
2. **Bar Chart:** A horizontal ranking of the Top 10 burdened nations, featuring dynamic textual annotations and a custom global average reference line.
3. **Time Series Chart:** Tracking the trajectory of the 5 most heavily burdened countries from 2001–2024, demonstrating that influx shocks almost never revert to pre-baseline states.
4. **Scatterplot (Regression):** A quadrant-driven mapping of Refugee Burden vs. Sanitation Coverage featuring an OLS regression line and a defined "Critical Zone" for the 10 most severely trapped nations.

## 📁 Repository Structure

```text
📦 unicef-analytics-report
├── report.qmd                   # Core Quarto markdown file containing narrative prose & Python cells
├── report.html                  # Executed, standalone HTML report (Publish output)
├── unicef_indicator_1 (1).csv   # Indicator data 1: Safe sanitation access
├── unicef_indicator_2 (1).csv   # Indicator data 2: Refugees per 1 USD GNI per capita
├── unicef_metadata (1).csv      # World Bank metadata (GDP, Population, Life Expectancy)
├── ne_110m_countries.zip        # Natural Earth geographical shapefile for mapping
└── README.md                    # Project documentation
```

## 🚀 How to Run Locally

If you want to re-render the data report locally rather than looking at the pre-compiled HTML, ensure you have the `quarto` CLI installed.

1. **Clone the repository:**
   ```bash
   git clone https://github.com/aayush110410/unicef-analytics-report.git
   cd unicef-analytics-report
   ```

2. **Install required python dependencies:**
   ```bash
   pip install polars pandas geopandas plotnine
   ```

3. **Render the document via Quarto:**
   ```bash
   quarto render report.qmd
   ```
   *Quarto will automatically execute the embedded Python code and regenerate the standalone `report.html` file.*

---
*Created by **Anshika Garg**.*
