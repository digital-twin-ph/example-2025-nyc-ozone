# Ozone and child asthma emergency visits, New York City, 2016–2025

A worked example for [Fieldwork](https://github.com/digital-twin-ph/fieldwork): daily asthma emergency department
visits among children aged 5 to 17 in New York City, with ozone, PM2.5, NO₂ and weather, from the reproducibility
package of Lian et al.

**The counts are not in this repository.** They come from NYC Department of Health and Mental Hygiene's syndromic
surveillance, through EpiQuery, and NYC.gov reserves all rights to them. Fieldwork fetches the authors' package from
Zenodo when the example is opened, checks it by digest, and keeps the table on that device. This repository holds the
workflows and the description of where the table comes from.

## The questions

| Workflow | Nodes | What it shows |
| --- | --- | --- |
| What happened during the June 2023 smoke? | 4 | Daily visits, 1 to 14 June 2023, as bars, and the same days with ozone and PM2.5 |
| Ten years of daily visits | 2 | All 3,653 days as a table |
| Did visits move with the air? | 3 | Visits, ozone, PM2.5 and temperature, May to July 2023, one panel each on one date axis, 1 to 14 June shaded. Timing, not effect |
| How much does ozone raise visits? | 3 | The authors' model: Poisson regression with 840 year-month-weekday strata, a temperature spline and HAC intervals. 1.061 (1.035–1.088) per 10 ppb, as published |
| Where was the air measured? | 5 | The study area: the five boroughs, with the six ozone monitors whose daily readings the study averages. The map opens the write-up, under Introduction |
| Did the satellite see the city's NO₂? | 9 | Sentinel-5P's tropospheric NO₂ at the four NO₂ monitors, 1 to 14 June 2023, each day's overpass beside the monitor's own reading, matched on monitor and day; where there is no usable satellite value, which gap it is |

**Opening it:** the table node says its rows are not kept in the project. Choose **Fetch the table**: Fieldwork
downloads the authors' package from Zenodo, about 9 MB, checks both digests and reads the daily table. Then run.

**The fifth workflow is the study area.** The package names no emergency department: the outcome is one citywide daily count. What it does place is the air. Each ozone monitor's position is on every row of the authors' site-day file, so Point data reads the monitors from that table once it is totalled to one row per site, taking x as the longitude and y as the latitude, as geopandas' `points_from_xy` does. The boroughs are New York State's five counties from geoBoundaries (US Census TIGER, 2018, public domain), chosen by name; Kings and Richmond are listed with where they lie, because other states have counties of those names.

**The sixth workflow compares, and does not substitute.** The satellite measures the column of NO₂ above a pixel of
about 3.5 by 5.5 km, in mol m⁻²; a monitor measures the air at its inlet, in ppb. The table puts the two side by side
for each monitor and day and names every gap: no overpass, no retrieval (mostly cloud), or below the product's quality
threshold (`qa_value` 0.75). Only the parts of each file the four pixels need are read, about 10 to 15 MB an
overpass, so the run takes minutes. Queens College 2 has no NO₂ reading for 3 to 5 June; those rows are kept and
named *no ground reading*.

The fourth workflow reproduces the authors' ozone result with Fieldwork's Regression widget (experiment 167): coefficient and HAC standard error equal to their archived R result to 12 significant figures.

## The data

| | |
| --- | --- |
| Days | 3,653, from 2016-01-01 to 2025-12-31 |
| Outcome | `count`: citywide asthma-syndrome ED visits, ages 5 to 17; 123,265 in all |
| Exposures | `o3`, `pm25`, `no2`, `tmean`, `prcp`, `rh`, with ready-made lags |
| Source of the table | `Data/Core/Derived/analysis_master_raw_rebuild.csv` in the package, 2,869,419 bytes |

An asthma visit is one whose chief complaint mentions asthma, wheezing or reactive airway, or whose diagnosis is
J45 or R06.2. Syndrome data are not diagnoses, and the city says they "are not exact measures of morbidity".

## What the authors report, to check against

From their archived `Source_Data/Tables/Main_02.csv`, a multipollutant model of the daily count:

| Pollutant | Contrast | Rate ratio (95% CI) |
| --- | --- | --- |
| O₃ | +10 ppb | 1.061 (1.035–1.088) |
| NO₂ | +10 ppb | 1.081 (1.037–1.126) |
| PM2.5 | +10 µg/m³ | 1.011 (0.963–1.062) |

There is no published or preprint paper; these are the authors' archived outputs. A by-hand check needs no model: on
7 June 2023, during the Canadian smoke, 49 visits against a mean of 25 on 1 to 5 June.

## What this does not establish

- That the city's terms permit more than fetching and analysing the authors' openly published package locally.
- That Zenodo will keep the record, or keep serving it to browsers.

## Files

| File | What it holds |
| --- | --- |
| `fieldwork-project.json.gz` | The Fieldwork project: the workflows, where the tables come from and the five borough outlines; no counts and no monitor readings |
| `provenance.json` | The fetch: URL, the file inside the package, both digests |
| `manual.json` | How to build all six workflows by hand, step by step. Fieldwork shows it in the Manual tab, and its from-scratch test performs it |

## References

- Lian D, Lin C, Wei J, Lin K, Tang Q, et al. Data and code for: Ambient ozone and pediatric asthma emergency
  department visits in New York City: a 10-year time-series study. Version 1.0. Zenodo.
  [doi:10.5281/zenodo.22757330](https://doi.org/10.5281/zenodo.22757330). Authors' materials CC BY 4.0.
- New York City Department of Health and Mental Hygiene. EpiQuery – Syndromic Surveillance.
  https://nyc.gov/health/epiquery
- US Environmental Protection Agency. Air Quality System, daily ozone (parameter 44201), as the authors' package carries it; custodian named in the package's `Metadata/RAW_SOURCE_FILE_MANIFEST.csv`.
- European Space Agency, Copernicus. Sentinel-5P TROPOMI Level 2 tropospheric NO₂, offline stream, read from Microsoft
  Planetary Computer (`sentinel-5p-l2-netcdf`). Contains modified Copernicus Sentinel data 2023.
- Runfola D, et al. geoBoundaries: A global database of political administrative boundaries. PLoS ONE 2020;15(4):e0231866. [doi:10.1371/journal.pone.0231866](https://doi.org/10.1371/journal.pone.0231866). USA ADM2, release 9469f09, from US Census Bureau MAF/TIGER; public domain.
