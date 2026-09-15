# COSMOS-UK Sub-daily ET Analysis

Analysis notebooks for the **COSMOS-UK sub-daily evapotranspiration dataset** (2014–2024): 30-minute eddy covariance flux measurements (ET, LE, H, PE, and related variables) from 45 UK environmental monitoring sites.

## Data

Data is not stored in this repository (see `.gitignore`) — download it from the NERC EDS Environmental Information Data Centre and place it in `data/` (and soil moisture data in `data_sm/`, if used):

> Cooper, H.M.; Crowhurst, D.; Cumming, A.M.J.; Evans, J.G.; Howson, T.; Morrison, R.; Retter, A.; Smith, R.J.; Stanley, S.; Vincent, P.; Fry, M. (2026). *Sub-daily actual evapotranspiration data for 45 monitoring sites (2014-2024) [COSMOS-UK]*. NERC EDS Environmental Information Data Centre. https://doi.org/10.5285/c0b5caf4-c14a-49e8-a720-2401c8d3d135

Some notebooks will use COSMOS-UK soil moisture data as well (place it in `data_sm/`):

> Stanley, S., Antoniou, V., Askquith-Ellis, A., Ball, L., Bennett, E.S., Blake, J.R., Boorman, D.B., Brooks, M., Cirstet, V., Clarke, M.A., Cooper, H.M., Cowan, N.J., Cumming, A., Evans, J.G., Farrand, P., Fry, M., Harvey, D., Houghton-Carr, H., Howson, T., Jiménez-Arranz, G., Keen, Y., Khamis, D., Leeson, S., Lord, W.D., Morrison, R., Nash, G.V., O'Callaghan, F., Retter, A., Rylett, D., Scarlett, P.M., Smith, R.J., St Quintin, P., Swain, O., Szczykulska, M., Teagle, S., Thornton, J.L., Trill, E.J., Vincent, P., Ward, H.C., Warwick, A.C., Winterbourn, J.B. (2026). *Daily and sub-daily hydrometeorological and soil moisture data (2013-2025) [COSMOS-UK]*. NERC EDS Environmental Information Data Centre. https://doi.org/10.5285/dde10e2b-ee1a-4b2b-a247-176db2f7bb31

Licensed under the [Open Government Licence v3](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Attribution: "Contains data supplied by UK Centre for Ecology & Hydrology."

### Quick setup on Binder / Colab / any fresh clone

The flux dataset above is openly downloadable without login, so this one-liner (run from the repo root) fetches and unpacks it into `data/` (and `supporting-documents/`) automatically — a ~400 MB download, ~1.5 GB unpacked, so it takes a few minutes:

```bash
curl -L -o cosmos_fastflux.zip "https://data-package.ceh.ac.uk/data/c0b5caf4-c14a-49e8-a720-2401c8d3d135.zip" && unzip -q -o cosmos_fastflux.zip && rm cosmos_fastflux.zip
```

In a Jupyter/Colab notebook cell, prefix it with `!`:

```
!curl -L -o cosmos_fastflux.zip "https://data-package.ceh.ac.uk/data/c0b5caf4-c14a-49e8-a720-2401c8d3d135.zip" && unzip -q -o cosmos_fastflux.zip && rm cosmos_fastflux.zip
```

This does not include the soil-moisture (`data_sm/`) files used by Sections 11–11b of [02_ET_drydown_example.ipynb](02_ET_drydown_example.ipynb) — see that notebook's Data Access tab.

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

## References:
- Zheng Fu et al. ,Critical soil moisture thresholds of plant water stress in terrestrial ecosystems.Sci. Adv.8,eabq7827(2022).DOI:10.1126/sciadv.abq7827
- Denissen, J. M. C., Teuling, A. J., Reichstein, M., & Orth, R. (2020). Critical soil moisture derived from satellite observations over Europe. Journal of Geophysical Research: Atmospheres, 125, e2019JD031672. https://doi.org/10.1029/2019JD031672





## Requirements

```bash
pip install -r requirements.txt
```

`pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy` — see [requirements.txt](requirements.txt) for the exact versions this was tested with. Each notebook also installs any missing packages automatically in its first cell.

## Running

```bash
jupyter notebook
```
or execute a notebook non-interactively:
```bash
jupyter nbconvert --to notebook --execute --inplace <notebook>.ipynb
```
Or open and run it in an IDE like VS Code or JupyterLab
