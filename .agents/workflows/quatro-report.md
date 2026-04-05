---
description: Use this skill for ALL tasks involving Quarto documents, .qmd files, HTML reports, and GitHub Pages publishing.   Triggers: "quarto report", "qmd file", "quarto html", "quarto render", "embed-resources", "code-fold",   "hashpipe options", "quarto YAM
---

---
name: quarto-report
description: >
  Use this skill for ALL tasks involving Quarto documents, .qmd files, HTML reports, and GitHub Pages publishing.
  Triggers: "quarto report", "qmd file", "quarto html", "quarto render", "embed-resources", "code-fold",
  "hashpipe options", "quarto YAML", "quarto theme", "github pages quarto", "quarto publish",
  "quarto narrative", "quarto visualisations", "quarto python", "quarto data analysis report",
  "BAA1030 assignment", "UNICEF quarto report", "data analyst html report", "quarto fig-cap",
  "quarto label", "quarto warning false", "quarto toc", "quarto layout", "quarto callout".
  ALWAYS use this skill when building or debugging any Quarto document.
---

# Quarto Report Master

You are an expert in building polished, publication-grade HTML reports with **Quarto** using Python.

Your style:
- **Structure-first** — YAML → sections → narrative → code → charts
- **Hashpipe-thorough** — every code cell has proper `#|` options
- **Non-AI-sounding** — narrative references specific data values, has a clear point of view
- **GitHub Pages ready** — output is a single self-contained HTML

---

## THE QMD FILE STRUCTURE

```
---
YAML header
---

## Section 1 (narrative + code)

## Section 2 (narrative + code)

...
```

A `.qmd` file is **Markdown + Python code cells**. Render with:
```bash
quarto render report.qmd
quarto preview report.qmd  # live preview in browser
```

---

## MODULE 1: YAML HEADER (Exemplary Level)

```yaml
---
title: "The Silent Crisis: Sanitation, Displacement & Global Inequality"
author: "Your Name"
date: today
format:
  html:
    embed-resources: true      # REQUIRED — single self-contained HTML file
    code-fold: true            # REQUIRED — hides code behind toggle
    theme: cosmo               # pick: cosmo, flatly, lux, darkly, pulse, sketchy, minty
    toc: true                  # table of contents
    toc-depth: 3               # show h2 and h3 in TOC
    toc-title: "Contents"
    number-sections: false
    fig-width: 11
    fig-height: 6
    fig-dpi: 150
    smooth-scroll: true
    highlight-style: github
execute:
  echo: true
  warning: false
  message: false
---
```

**Theme guide:**
| Theme | Feel |
|---|---|
| `cosmo` | Clean, Bootstrap modern |
| `lux` | Elegant, serif-accented |
| `flatly` | Flat design, professional |
| `darkly` | Dark mode |
| `pulse` | Vibrant, bold |
| `sketchy` | Playful, hand-drawn look |

---

## MODULE 2: HASHPIPE OPTIONS (In Every Code Cell)

Every code cell should have at minimum `label` and `fig-cap`:

```python
#| label: fig-world-map
#| fig-cap: "Average sanitation coverage by country (2000–2024). Grey indicates missing data."
#| warning: false
#| message: false
#| echo: true

# your chart code here
```

**All hashpipe options:**

```python
#| label: fig-scatter          # unique label for cross-referencing
#| fig-cap: "Caption here"     # appears below chart
#| fig-width: 12               # override global width for this cell
#| fig-height: 6
#| echo: true                  # show code (overrides global)
#| echo: false                 # hide code entirely for this cell
#| warning: false              # suppress warnings
#| message: false              # suppress messages
#| output: true                # show output
#| eval: true                  # run the code
#| code-fold: false            # don't fold this cell (e.g. for imports)
```

**Cross-reference charts in text:**
```markdown
As shown in @fig-world-map, Sub-Saharan Africa has the lowest coverage...
```

---

## MODULE 3: MARKDOWN FORMATTING

The grader checks for: headings, bold, italic, bullet lists, and that it **reads as personal, data-driven narrative**.

```markdown
# The Silent Crisis
## Background
### A note on data quality

**Key finding:** Over 1.8 billion people lack basic sanitation.
*This is not a static problem* — the data shows clear trajectories.

> "Sanitation is not a luxury. It is a foundation."

- Sub-Saharan Africa: average coverage below 30%
- South Asia: improving but still below 60%
- Latin America: significant regional variation

1. First finding
2. Second finding
3. Third finding
```

**Callout blocks** (Quarto-specific, adds visual interest):
```markdown
::: {.callout-note}
Data note: Indicator 1 covers 154 countries with annual observations from 2000 to 2024.
:::

::: {.callout-warning}
Caution: Countries with fewer than 5 observations were excluded from trend analysis.
:::

::: {.callout-tip}
Key takeaway: The strongest predictor of sanitation access is GDP per capita.
:::
```

---

## MODULE 4: NON-AI NARRATIVE GUIDE

The grader specifically penalises generic AI filler. Here's how to write it yourself:

**Bad (AI-sounding):**
> "Sanitation is a crucial global issue that affects many people worldwide. The data shows various trends across different countries and regions."

**Good (personal, data-driven):**
> "What struck me most in this dataset is the flatness of the curves for the lowest-performing countries. Afghanistan's sanitation coverage in 2024 (9.3%) is almost identical to its 2000 figure — 24 years of virtually no progress. This is not a data error; it reflects a country whose infrastructure development has been paralysed by conflict."

**The formula for each section:**
1. Introduce the question you're asking of the data
2. Describe what the chart shows in one sentence
3. Name a specific country/value/year that surprised you
4. Connect it to the broader story

---

## MODULE 5: FULL FILE TEMPLATE

```python
---
title: "The Silent Crisis: Sanitation, Displacement & Global Inequality"
author: "Your Name"
date: today
format:
  html:
    embed-resources: true
    code-fold: true
    theme: lux
    toc: true
    toc-depth: 3
    fig-width: 11
    fig-height: 6
execute:
  warning: false
  message: false
---

## Introduction

Opening hook with a striking number from your data...

## The Data

Brief description of the three files and what they contain.
Mention the join strategy.

```{python}
#| label: setup
#| code-fold: false

import polars as pl
import pandas as pd
from plotnine import *

# Load data
ind1 = pl.read_csv("unicef_indicator_1.csv")
ind2 = pl.read_csv("unicef_indicator_2.csv")
meta = pl.read_csv("unicef_metadata.csv")

# Filter to Total sex only
ind1 = ind1.filter(pl.col("sex") == "Total")
ind2 = ind2.filter(pl.col("sex") == "Total")

print(f"Indicator 1: {ind1.shape[0]} rows, {ind1['country'].n_unique()} countries")
print(f"Indicator 2: {ind2.shape[0]} rows, {ind2['country'].n_unique()} countries")
print(f"Metadata: {meta.shape[0]} rows")
```

## Global Sanitation Coverage

Narrative text explaining what the world map will show...

```{python}
#| label: fig-world-map
#| fig-cap: "Average sanitation coverage (%) by country, 2000–2024."

# world map code here
```

## Which Countries Bear the Refugee Burden?

Narrative connecting refugee burden to sanitation...

```{python}
#| label: fig-bar-chart
#| fig-cap: "Top 15 countries by refugee burden relative to GNI per capita."

# bar chart code here
```

## Wealth Predicts Access

```{python}
#| label: fig-scatter
#| fig-cap: "Relationship between GDP per capita and sanitation access."

# scatterplot code here
```

## Two Decades of Slow Progress

```{python}
#| label: fig-timeseries
#| fig-cap: "Sanitation coverage trends for selected countries, 2000–2024."

# time series code here
```

## Conclusion

Summary of findings + call to action...
```

---

## MODULE 6: GITHUB PAGES PUBLISHING

**Method 1 — Quarto publish (recommended):**
```bash
quarto publish gh-pages report.qmd
# Creates gh-pages branch automatically
# URL: https://yourusername.github.io/repo-name/
```

**Method 2 — Manual:**
```bash
quarto render report.qmd
# Creates report.html
git add report.html
git commit -m "Add rendered report"
git push origin main
# Then: GitHub repo → Settings → Pages → Deploy from branch → main → /root
```

**Submission URL format:**
```
https://yourusername.github.io/repo-name/report.html
```

---

## COMMON MISTAKES

| Mistake | Fix |
|---|---|
| Missing `embed-resources: true` | HTML won't be self-contained, images break |
| Not using `#| label:` | Can't cross-reference charts in text |
| Same label used twice | Each cell needs a unique label |
| Code cell shows warnings | Add `#| warning: false` |
| Narrative has no specific numbers | Always reference values from `df.describe()` |
| Generic story text | Ask: "would a data analyst actually write this?" |
| Forgetting `quarto publish gh-pages` | Must run this to push to GitHub Pages |

---

## INSTALL CHECKLIST

```bash
pip install polars plotnine geopandas geodatasets mizani
quarto install                   # install Quarto CLI
quarto check                     # verify Python kernel found
```

Verify Python kernel in YAML:
```yaml
jupyter: python3
# or
engine: knitr  # only if using R
```
