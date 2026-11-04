# Ozone and child asthma emergency visits, New York City, 2016–2025

A worked example for [Fieldwork](https://github.com/digital-twin-ph/fieldwork): daily asthma emergency department
visits among children aged 5 to 17 in New York City, with ozone, PM2.5, NO₂ and weather, from the reproducibility
package of Lian et al.

**The counts are not in this repository.** They come from NYC Department of Health and Mental Hygiene's syndromic
surveillance, through EpiQuery, and NYC.gov reserves all rights to them. Fieldwork fetches the authors' package from
Zenodo when the example is opened, checks it by digest, and keeps the table on that device. This repository holds the
workflows and the description of where the table comes from.

**Not built yet.** The workflows wait for a table fetched at open (experiment 170) and for regression (experiment 167).

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

## References

- Lian D, Lin C, Wei J, Lin K, Tang Q, et al. Data and code for: Ambient ozone and pediatric asthma emergency
  department visits in New York City: a 10-year time-series study. Version 1.0. Zenodo.
  [doi:10.5281/zenodo.22757330](https://doi.org/10.5281/zenodo.22757330). Authors' materials CC BY 4.0.
- New York City Department of Health and Mental Hygiene. EpiQuery – Syndromic Surveillance.
  https://nyc.gov/health/epiquery
