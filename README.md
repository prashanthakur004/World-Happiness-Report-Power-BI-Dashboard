# World Happiness Report — Power BI Dashboard

An interactive **Power BI** report that explores the **World Happiness Report** dataset across **156 countries**, analyzing how six key contributing factors — GDP per capita, social support, healthy life expectancy, freedom, generosity, and perceptions of corruption — drive national happiness scores.

The report is designed for policy researchers, data analysts, and curious readers who want to understand the global distribution of well-being and which levers correlate most strongly with it.

---

## Table of Contents

- [Overview](#overview)
- [Live Dashboard Preview](#live-dashboard-preview)
- [Dataset](#dataset)
- [Repository Structure](#repository-structure)
- [Key Insights Uncovered](#key-insights-uncovered)
- [Power BI Report Walkthrough](#power-bi-report-walkthrough)
- [Getting Started](#getting-started)
- [Technologies Used](#technologies-used)
- [Data Source & Attribution](#data-source--attribution)
- [Future Enhancements](#future-enhancements)

---

## Overview

The World Happiness Report is an annual landmark publication that ranks countries by how happy their citizens perceive themselves to be. This project takes a snapshot of that report — 156 countries, 9 columns — and turns it into a multi-page Power BI dashboard.

The dashboard answers questions such as:

- Which countries top the global happiness ranking, and what factors do they share?
- Which of the six contributing factors has the strongest correlation with the overall happiness score?
- Are there countries that "punch above their weight" — high happiness despite low GDP, or vice versa?
- How does corruption perception relate to well-being?
- Which regions of the world cluster together in happiness tiers?

Every page is built to stand alone — KPIs on top, supporting visuals below, with slicers that let you cross-filter on the fly.

---

## Live Dashboard Preview

> Open `World Happiness Report.pbix` in Power BI Desktop to interact with the report.

| Page | Focus |
|------|-------|
| **Global Overview** | KPI cards (Top Country, Bottom Country, Avg Score, Countries Analyzed) + ranked bar of Top/Bottom 15 |
| **Factor Analysis** | Scatter plots of each factor vs. Happiness Score, with trend line and R² annotation |
| **Regional Comparison** | Map visual + regional aggregation (using inferred region grouping) |
| **Outlier Spotlight** | Countries where GDP rank differs sharply from Happiness rank — over- and under-performers |

---

## Dataset

The raw dataset (`Happiness Score Data.csv`) contains **156 country records** and **9 columns**. It corresponds to the World Happiness Report 2019 edition.

### Schema

| Column | Type | Description |
|--------|------|-------------|
| `Overall rank` | Integer | Global happiness rank (1 = happiest) |
| `Country or region` | String | Country name (lowercased in source) |
| `Score` | Float | National happiness score, 0–10 scale. Range: 2.853 – 7.769 |
| `GDP per capita` | Float | Economic production contribution (normalized). Range: 0 – 1.684 |
| `Social support` | Float | Having someone to count on (normalized). Range: 0 – 1.624 |
| `Healthy life expectancy` | Float | Life expectancy contribution (normalized). Range: 0 – 1.141 |
| `Freedom to make life choices` | Float | Freedom perception (normalized). Range: 0 – 0.631 |
| `Generosity` | Float | Charitable contribution residual (normalized). Range: 0 – 0.566 |
| `Perceptions of corruption` | Float | Corruption perception (normalized). Range: 0 – 0.453 |

### Quick Stats

| Metric | Value |
|--------|-------|
| Countries ranked | 156 |
| Happiest country | Finland (Score: 7.769) |
| Least happy country | South Sudan (Score: 2.853) |
| Average global score | 5.407 |
| Top-3 region | Nordic — Finland, Denmark, Norway |
| Bottom-3 region | Sub-Saharan Africa + conflict zones — South Sudan, CAR, Afghanistan |
| Average GDP contribution | 0.905 |
| Average social support | 1.209 |

---

## Repository Structure

```
World-Happiness-Report/
│
├── World Happiness Report.pbix     # Power BI report file
├── Happiness Score Data.csv         # Raw dataset (156 rows × 9 columns)
├── README.md                        # This file
└── LICENSE                          # MIT License
```

---

## Key Insights Uncovered

A few of the headline insights surfaced by the dashboard:

1. **The Nordic bloc dominates the top of the table.** Finland, Denmark, Norway, and Iceland all land in the global top 4 — and they score consistently high across **all six factors**, not just GDP. Their distinct advantage is in `Social support` and `Freedom to make life choices`, which together explain much of the gap over similarly wealthy nations.

2. **GDP per capita is the single strongest driver of the happiness score** — the scatter plot of GDP vs. Score shows a tight positive trend. However, the relationship plateaus above a GDP contribution of ~1.2, suggesting that beyond a certain economic baseline, other factors (social support, freedom, low corruption) become the differentiators.

3. **Corruption perception acts as a strong negative moderator.** Countries with `Perceptions of corruption` below 0.1 are overwhelmingly clustered in the top half of the ranking, while high-corruption countries rarely break into the top 80.

4. **Several countries punch above their GDP rank** — Costa Rica, New Zealand, and Uzbekistan post happiness scores significantly higher than what their GDP alone would predict, pointing to strong non-economic drivers. Conversely, some wealthy Gulf states underperform on happiness vs. their GDP rank.

5. **Sub-Saharan Africa and conflict-affected states form a clear bottom cluster** — South Sudan, Central African Republic, and Afghanistan all share both low GDP *and* low social support, demonstrating that the unhappiest countries are typically deprived on multiple axes simultaneously, not just one.

> Open the `.pbix` file to verify each claim interactively and drill into specific countries.

---

## Power BI Report Walkthrough

### Data Preparation (Power Query)
- Loaded `Happiness Score Data.csv` and promoted headers.
- Trimmed whitespace and title-cased `Country or region` for display (kept source as lowercase).
- Converted all numeric columns to fixed-decimal type.
- Created a `Region` column using a country-to-region mapping table (manual lookup) so visuals can group by continent.
- Created a `Happiness Tier` calculated column:
  - **Thriving** (Score ≥ 6.5)
  - **Content** (5.0 ≤ Score < 6.5)
  - **Struggling** (4.0 ≤ Score < 5.0)
  - **Suffering** (Score < 4.0)

### DAX Measures
- `Total Countries = COUNTROWS(Happiness)`
- `Average Score = AVERAGE(Happiness[Score])`
- `Top Country Score = MAXX(TOPN(1, Happiness, Happiness[Score], DESC), Happiness[Score])`
- `Bottom Country Score = MINX(TOPN(1, Happiness, Happiness[Score], ASC), Happiness[Score])`
- `Correlation GDP Score = CORREL(Happiness[GDP per capita], Happiness[Score])` *(as a quick stat card)*
- `Overperformers = Countries where Overall rank < GDP-only predicted rank by ≥ 20 places`

### Visuals by Page
- **Global Overview** — 4 KPI cards, horizontal bar chart of Top 15 by score (with country flag colors), mirror bar chart of Bottom 15.
- **Factor Analysis** — 6-tile scatter grid: each factor vs. `Score` with regression trendline + R² annotation.
- **Regional Comparison** — Filled map colored by score, regional averages matrix.
- **Outlier Spotlight** — Scatter of GDP rank vs. Happiness rank with 45° reference line; over/under-performers labeled.

---

## Getting Started

### Prerequisites
- [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free, Windows-only). Mac users can run Power BI Desktop inside a Windows VM or use [Power BI Service](https://app.powerbi.com/) in the browser.

### Steps
1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/World-Happiness-Report.git
   cd World-Happiness-Report
   ```
2. Open `World Happiness Report.pbix` in Power BI Desktop.
3. If prompted, allow the report to use the bundled `Happiness Score Data.csv` as the data source (the path is relative and should resolve automatically).
4. Interact with slicers, hover over visuals, and drill into specific countries or factors.

> **Tip:** To refresh with a newer edition of the World Happiness Report, replace `Happiness Score Data.csv` and click **Home → Refresh** in Power BI Desktop.

---

## Technologies Used

| Tool | Purpose |
|------|---------|
| **Power BI Desktop** | Report authoring, dashboard visualization |
| **Power Query (M)** | ETL — cleaning, type conversion, region mapping |
| **DAX** | Calculated columns and KPI measures |
| **CSV** | Source data format |
| **Git / GitHub** | Version control and project hosting |

---

## Data Source & Attribution

This project uses the publicly available **World Happiness Report** dataset, which is widely redistributed on Kaggle.

- **Original Report:** [worldhappiness.report](https://worldhappiness.report/) — published annually by the UN Sustainable Development Solutions Network.
- **Common Kaggle mirror:** [World Happiness Report](https://www.kaggle.com/datasets/unsdsn/world-happiness) (Kaggle Datasets)
- **Authors of the report:** Helliwell, J., Layard, R., & Sachs, J.

> Please review the dataset's license on Kaggle before reusing the raw CSV for commercial purposes. The dataset is included in this repository for reproducibility of the Power BI report.

If you use this dataset in your own work, please credit the original World Happiness Report authors.

---

## Future Enhancements

- [ ] Add multi-year trend by ingesting 2015–2024 editions of the report and building a year slicer.
- [ ] Build a Python analysis layer (`pandas` + `plotly`) as an alternative to Power BI for cross-platform users.
- [ ] Compute partial correlations to control GDP when assessing the strength of social support, freedom, etc.
- [ ] Add a "Mover" page showing year-over-year rank changes once multi-year data is loaded.
- [ ] Deploy a Streamlit or Flask dashboard so non-Power-BI users can explore the report in a browser.

Contributions are welcome — feel free to open an issue or submit a pull request.

---
