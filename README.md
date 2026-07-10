# COSMOS-UK Sub-daily ET Analysis

Analysis notebooks for the **COSMOS-UK sub-daily evapotranspiration dataset** (2014–2024): 30-minute eddy covariance flux measurements (ET, LE, H, PE, and related variables) from 45 UK environmental monitoring sites.

## Data

Data is not stored in this repository (see `.gitignore`) — download it from the NERC EDS Environmental Information Data Centre and place it in `data/` (and soil moisture data in `data_sm/`, if used):

> Cooper, H.M.; Crowhurst, D.; Cumming, A.M.J.; Evans, J.G.; Howson, T.; Morrison, R.; Retter, A.; Smith, R.J.; Stanley, S.; Vincent, P.; Fry, M. (2026). *Sub-daily actual evapotranspiration data for 45 monitoring sites (2014-2024) [COSMOS-UK]*. NERC EDS Environmental Information Data Centre. https://doi.org/10.5285/c0b5caf4-c14a-49e8-a720-2401c8d3d135

Some notebooks will use COSMOS-UK soil moisture data as well.

[TO ADD]

Licensed under the [Open Government Licence v3](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Attribution: "Contains data supplied by UK Centre for Ecology & Hydrology."

See [CLAUDE.md](CLAUDE.md) for a description of the file layout, columns, and conventions (units, QC flags, missing-value handling).

## Notebooks

| Notebook | Description |
|---|---|
| [01_explore_ET_data.ipynb](01_explore_ET_data.ipynb) | Exploratory analysis: data availability, seasonal cycle, energy balance partitioning, diurnal cycle |
| [02_ET_drydown_example.ipynb](02_ET_drydown_example.ipynb) | Drydown event detection and exponential-decay fitting on the evaporative stress factor (α = ET/PE) |
| [03_ET_drydown_ET_fitting.ipynb](03_ET_drydown_ET_fitting.ipynb) | Drydown analysis fitting the decay model directly to ET, compared against the α-based approach |
| [04_subdaily_ET.ipynb](04_subdaily_ET.ipynb) | Sub-daily (30-min) ET dynamics across sites: diurnal shape, phase lag, and hysteresis vs energy/VPD drivers |

## Coming soon:
- sub-hourly ET drydown
- comparing sub-hourly ET and soil moisture


## Requirements

`pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`. Each notebook installs any missing packages automatically in its first cell.

## Running

```bash
jupyter notebook
```
or execute a notebook non-interactively:
```bash
jupyter nbconvert --to notebook --execute --inplace <notebook>.ipynb
```
Or open and run it in an IDE like VS Code or JupyterLab