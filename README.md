# Foundation Soils data

Foundation Soils (foundationsoils.com) is a reference site on the soil and water under US houses, launched in 2026 and built from USDA, NOAA, FEMA and Census data.

This repository holds the numbers behind the site as four CSV files: one row per US county, Texas city, Ohio city and
soil series. The soil shares are computed from USDA's soil survey the same way on every row, so places can be
compared with each other. Foundation Soils is not part of any foundation repair or waterproofing company. Articles titled "Foundation Soils" on contractor websites, including Foundation Recovery Systems and AquaGuard Foundation Solutions, belong to those companies.

| File | Rows | One row per |
|---|---|---|
| `counties.csv` | 3,144 | county or county-equivalent in the 50 states and DC |
| `tx-cities.csv` | 1,227 | active incorporated place in Texas |
| `oh-cities.csv` | 922 | active incorporated place in Ohio |
| `soil-series.csv` | 160 | soil series under the most US housing |

As of: soil survey versions to August 5, 2026; Drought Monitor map of September 22, 2026; FEMA flood records
refreshed September 9, 2026; NOAA rain through August 2026; Census ACS 2020-2024 5-year.

`page_url` links the place's page on foundationsoils.com. It is filled for the 170 county, city and soil series pages open now (70 of them counties); the rest are added as their pages open.

## Columns

Shares are fractions from 0 to 1 of the place's mapped land (0.51 = 51%). A blank cell means the source has no value
for that place, or the site doesn't show one (see the notes below).

**Soil (all four files)**
- `expansive_clay_share`: share of mapped land where USDA's national interpretation "Dwellings without basements" lists
  shrink-swell as a limiting feature with a rating of 0.5 or more on its 0-to-1 scale.
- `high_water_table_share`: share of mapped land where USDA's "Dwellings with basements" interpretation lists depth to
  a saturated zone as a limiting feature with a rating of 0.5 or more (ground water about 2.5 to 6 feet down).
- `septic_very_limited_share`: share rated "very limited" for septic absorption fields.
- `lep_weighted_avg_pct` (places) and `lep_median_pct` (series): linear extensibility, the percent change in a soil's
  length between moist and oven-dry (USDA classes: under 3% low, 3-6% moderate, 6-9% high, 9% and up very high).
- `top_soil_series`, `top_soil_series_share`: the soil series covering the most land, and its share.
- `soil_rated`: false when USDA's soil survey maps too little of the place to rate it; the soil columns are blank.
- `mapped_acres`, `soil_as_of`: the land the shares are computed over, and the newest survey version used.

**Drought**: `drought_d1_plus_pct` is the percent of the county's area in D1 (moderate drought) or worse on the map of
`drought_map_date`; `drought_weeks_d1_plus_last_52` counts the weeks in the last 52 with any D1 or worse. City files
carry the county's figure (`county_drought_d1_plus_pct`).

**Flood losses (counties)**: `flood_records_since_2016` counts NFIP loss records with a date of loss from January 1,
2016; `flood_paid_records_since_2016` and `flood_paid_usd_since_2016` count the records with a payment and the amount
paid for building, contents and compliance costs (gross, as reported); `flood_records_per_10k_homes` divides by
housing units; `flood_latest_loss` is the newest loss on record for the county.

**Rain (counties)**: `rain_normal_annual_in` is the 1991-2020 average calendar-year total; `rain_last12_in` and
`rain_last12_vs_normal_in` are the 12 months ending `rain_last12_end` and their difference from that average.

**Homes**: `housing_units`, `median_year_built`, `pre1980_share` (built in 1979 or earlier) and `median_home_value_usd`
from the Census Bureau's American Community Survey. City figures are left blank for places with fewer than 100
housing units.

**Soil series**: `mapped_acres` and `states` where the series is mapped, `clay_median_pct`, `drainage`,
`hydrologic_group` and `taxonomic_class` from SSURGO, and a link to USDA's series description.

## How the soil shares are computed

The Soil Survey Geographic Database (SSURGO), queried through USDA's Soil Data Access service. Each map unit's acres
are weighted by each soil component's percentage of that map unit. Every component counts in the denominator,
including urban land and water, which USDA doesn't rate, so a heavily built county can show lower shares than its
soils alone would. County acres come from SSURGO's county overlap tables; where those can't be used (Connecticut's
planning regions, split Alaska boroughs, survey areas without overlap acres) the Census county boundary is clipped
against the soil map. City shares clip each Texas and Ohio city's Census boundary against the soil map with Soil Data
Access spatial queries.

A soil rating describes the ground, not the condition of any house. No public dataset records foundation failures by
place, and reinforced slab-on-ground construction has markedly reduced the shrink-swell limitation for small
buildings (USDA Soil Survey Manual). Full method, checks against hand-confirmed values, and USDA's own wording:
https://foundationsoils.com/sources/

## Sources

- Soil: USDA NRCS SSURGO via Soil Data Access, https://sdmdataaccess.sc.egov.usda.gov/
- Drought: US Drought Monitor, https://droughtmonitor.unl.edu/. The U.S. Drought Monitor is jointly produced by the
  National Drought Mitigation Center at the University of Nebraska-Lincoln, the United States Department of
  Agriculture, and the National Oceanic and Atmospheric Administration.
- Flood losses: FEMA OpenFEMA, NFIP Redacted Claims v3, API endpoint https://www.fema.gov/api/open/v3/NfipClaims, refreshed by FEMA on
  September 9, 2026 and retrieved on September 28, 2026 at 21:10 UTC.
  This product uses the Federal Emergency Management Agency's OpenFEMA API, but is not endorsed by FEMA. The Federal Government or FEMA cannot vouch for the data or analyses derived from these data after the data have been retrieved from the Agency's website(s).
- Rain: NOAA NCEI county precipitation series (nClimDiv).
- Homes: US Census Bureau, American Community Survey (ACS 2020-2024 5-year).
- Boundaries: Census Bureau Gazetteer files and TIGERweb.

## Reuse

The federal source data (USDA NRCS, FEMA, NOAA NCEI and the Census Bureau) are works of the US government and in the
public domain in the United States. The Drought Monitor figures carry the credit line above. The compiled tables in
this repository are released under CC BY 4.0 (`LICENSE`): reuse them freely, with credit to Foundation Soils
(foundationsoils.com). `CITATION.cff` has the citation.

About the site and how it is paid for: https://foundationsoils.com/about/
