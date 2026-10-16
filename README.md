# Rental Housing Availability and Tenure Inequality in France

Where in France can people rent a home, and how unevenly is rental housing spread within cities? This project adapts the U.S. **"rental deserts"** framework to France. It measures the rental share of every IRIS (INSEE's neighborhood unit, about 2,000 residents), identifies **rental deserts**, and quantifies **tenure segregation** within the 100 most populous urban units using the divergence index. It then relates that segregation to income and building-age segregation, and tests whether the SRU social-housing law is associated with rental availability.

The approach adapts the Joint Center for Housing Studies working paper *Rental Deserts* (Airgood-Obrycki, Maaoui & Wedeen, 2024), which studies U.S. census tracts.

## Data

| Source | Use |
|---|---|
| INSEE census 2021, IRIS level (housing and population) | Rental share, housing type and population for each IRIS |
| IGN CONTOURS-IRIS boundaries (2020–2022) | IRIS polygons for mapping and spatial joins |
| INSEE FILO 2021, IRIS level | Median declared income (`DEC_MED21`), used for income segregation |
| INSEE urban units (UU2020, 2024 edition) | Urban-unit membership and city / suburb / outside-unit classification |
| SRU inventory 2022 | Commune social-housing (LLS) rate, legal target, and compliance |

Raw data and GIS files are not included in this repository. Small lookup and output tables (`.xlsx`) are included.

## Method

**Tenure classification (IRIS).** Each IRIS is classified by its share of renter households:

| Type | Rental share |
|---|---|
| **Rental desert** | under 20% |
| **Mixed tenure** | 20–80% |
| **High rental** | 80% or more |

**Tenure segregation.** For each IRIS *i* in urban unit *m*, the divergence index (Roberto 2015) compares the local renter/owner mix with the urban unit's mix:

`D_i = r_i · ln(r_i / r_m) + o_i · ln(o_i / o_m)`

where *r* and *o* are the rental and owner shares. The urban-unit index is the population-weighted mean of `D_i`. To test whether tenure segregation moves with other kinds of segregation, the same divergence measure is computed for income, and an entropy index measures how mixed building ages are (shares by construction period).

**Policy analysis.** Commune rental share is regressed on SRU compliance and on the actual social-housing rate, separately, together, and interacted with city / suburb / isolated status.

## Main results

**Rental deserts cover nearly half of IRIS** (48,592 IRIS in total):

| | IRIS | Share | Avg. rental share | Houses (%) | In suburbs |
|---|---|---|---|---|---|
| Rental desert | 23,312 | 48.0% | 12.8% | 95.2% | **87.0%** |
| Mixed tenure | 23,668 | 48.7% | 39.0% | 60.3% | 54.2% |
| High rental | 1,612 | 3.3% | 89.7% | 7.3% | 20.6% |

- **Rental deserts are a suburban, single-family phenomenon.** 87% are in suburbs, and their housing is about 95% houses.
- **Tenure segregation varies widely across urban units.** The most divergent include Creil, Le Havre, Nîmes, Tours, Rouen and Lille. The least divergent are smaller coastal and border units such as Sète, Fréjus and Menton-Monaco. Every top-100 urban unit's overall rental share is at least 28.5%, which supports the 20% threshold.
- **Tenure segregation is linked to other segregation.** Across urban units, tenure divergence correlates with income divergence (r = −0.48, p = 0.014) and with building-age diversity (r = −0.50, p = 0.011), with n = 25.
- **SRU and rental availability.** SRU-compliant communes have rental shares about **15 points higher** (p < 0.001). Each point of social-housing rate goes with 0.86 points of rental share (R² = 0.61). Controlling for the actual social-housing rate shrinks the compliance effect to about 2 points, so the association runs mostly through the social-housing stock itself. The compliance effect is smaller in suburbs (−2.9 points, p = 0.035). N = 2,066 communes.

## Repository contents

All analysis code is in `France/`.

| File | Description |
|---|---|
| `France_FillBlanks.Rmd` / `.html` | **Final tables:** tenure classification, IRIS characteristics (Table 1), urban-unit divergence rankings (Table 2), segregation correlations (Table 3), SRU policy models (Table 4) |
| `France_Figures (1).Rmd` | **Final figures:** income by tenure type, national divergence map, city maps of IRIS divergence, and maps of tenure × income and tenure × building-age segregation. Saved to `France/figures/`. |
| `France_DiverseIndex.Rmd` / `.html` | Divergence-index construction and IRIS-level maps |
| `France_metocities.Rmd` / `.html` | Divergence and rental shares for the top 100 urban units |
| `France_TableFigure.Rmd` / `.html` | Descriptive tables for the top 100 urban units and rental-desert IRIS |
| `1_France_GeographySetup.R`, `2_France_DataOrganization.R`, `France_geographysetup.Rmd` | Geography setup (urban units, city / suburb classification) and IRIS data assembly, adapted from the JCHS scripts |
| `France_visualization.Rmd`, `France_1.Rmd`, `France_DiverseIndex_HTML.Rmd`, `France_Figures.Rmd`, `France_Figures (2).Rmd` | Earlier exploratory versions |
| `France/figures/` | Final figures (PNG) |
| `France_RentalDesert_DescStat.docx` | Descriptive statistics write-up |

## How to run

1. Download the data sources above. Place IRIS boundaries, census and FILO files where the `path_*` variables at the top of each Rmd point. Paths are currently absolute and need to be edited for your machine.
2. Install the R packages: `tidyverse`, `sf`, `data.table`, `readxl`, `broom`, `janitor`, `knitr`, `kableExtra`, `RColorBrewer`, `maps` and `tigris`.
3. Knit `France_FillBlanks.Rmd` for the tables and `France_Figures (1).Rmd` for the figures.

## Author

Dawon Oh: adaptation of the rental-deserts framework to France, data assembly, segregation measures, policy analysis, and mapping.
