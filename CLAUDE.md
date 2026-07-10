# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A data analysis workspace for the **COSMOS-UK sub-daily evapotranspiration dataset** (2014–2024), published by NERC EDS / UK Centre for Ecology and Hydrology (DOI: 10.5285/c0b5caf4-c14a-49e8-a720-2401c8d3d135). The dataset covers 45 UK environmental monitoring sites with 30-minute flux measurements derived from eddy covariance (EddyPro v7.0.9).

## Data structure

**`data/`** — one pair of CSVs per site, named `cosmos-uk_<siteid>_fastflux_<level>_sh_<years>.csv`:

- `_l1_` — full set of derived variables (22 columns): sensible heat flux `H`, latent heat `LE`, evapotranspiration `ET`, potential ET `PE`, momentum flux `TAU`, friction velocity `U_STAR`, Monin-Obukhov length `L`, footprint distances `X_PEAK/X_OFFSET/X_90`, and ancillary met variables. No QC flags — contains uncorrected columns (`UN_H`, `UN_TAU`) and spectral correction factors.
- `_l2_` — quality-controlled subset (8 columns): `H`, `H_FLAG`, `LE`, `ET`, `TAU`, `TAU_FLAG`, `SHF` (soil heat flux). Use this for analysis that requires QC'd data.

**`data/cosmos-uk_sitemetadata_2014-2024.csv`** — one row per site with location (BNG easting/northing + WGS84 lat/lon), altitude, soil type, land cover, bulk density, soil organic carbon, and lattice water.

**`supporting-documents/`** — variable dictionaries for L1 and L2 (`cosmos-uk_fastflux_l1/l2_2014-2024_metadata.csv`) and a full methods document (`.docx`).

**`ro-crate-metadata.json`** — RO-Crate provenance/catalogue metadata; not data.

## Key conventions

- Timestamps: ISO 8601 UTC (`DATE_TIME`), end of the 30-minute averaging period.
- Missing/rejected values are encoded as **`-9999`** — filter before any arithmetic.
- Site IDs are 5-character uppercase codes (e.g. `EASTB`, `GLENS`). File names use lowercase versions (e.g. `eastb`, `glens`).
- Units: fluxes in W m⁻², ET/PE in mm (totals over the preceding 30 min), wind in m s⁻¹, temperature in K.

## Working with the data

Load a single site:
```python
import pandas as pd
df = pd.read_csv('data/cosmos-uk_eastb_fastflux_l2_sh_2014-2024.csv',
                 parse_dates=['DATE_TIME'], index_col='DATE_TIME',
                 na_values=-9999)
```

Load all sites into a single frame:
```python
import glob
dfs = [pd.read_csv(f, parse_dates=['DATE_TIME'], na_values=-9999)
       for f in glob.glob('data/*_l2_*.csv')]
df_all = pd.concat(dfs, ignore_index=True)
```

Site metadata join key: `SITE_ID` column present in both data files and `sitemetadata`.
