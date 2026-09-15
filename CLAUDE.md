# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A data analysis workspace built on two COSMOS-UK datasets, both published by NERC EDS / UK Centre for Ecology and Hydrology:

- **Sub-daily evapotranspiration** (2014–2024), DOI: [10.5285/c0b5caf4-c14a-49e8-a720-2401c8d3d135](https://doi.org/10.5285/c0b5caf4-c14a-49e8-a720-2401c8d3d135) — 45 UK environmental monitoring sites, 30-minute flux measurements derived from eddy covariance (EddyPro v7.0.9). Lives in `data/`.
- **Daily and sub-daily hydrometeorological and soil moisture data** (2013–2025), DOI: [10.5285/dde10e2b-ee1a-4b2b-a247-176db2f7bb31](https://doi.org/10.5285/dde10e2b-ee1a-4b2b-a247-176db2f7bb31) — 51 sites, includes cosmic-ray neutron probe (`COSMOS_VWC`) and TDT capacitance soil moisture, meteorological, and precipitation variables. Only used by some notebooks (currently only EASTB is present locally, in `data_sm/`).

## Data structure

**`data/`** — one pair of CSVs per site, named `cosmos-uk_<siteid>_fastflux_<level>_sh_<years>.csv`:

- `_l1_` — full set of derived variables (22 columns): sensible heat flux `H`, latent heat `LE`, evapotranspiration `ET`, potential ET `PE`, momentum flux `TAU`, friction velocity `U_STAR`, Monin-Obukhov length `L`, footprint distances `X_PEAK/X_OFFSET/X_90`, and ancillary met variables. No QC flags — contains uncorrected columns (`UN_H`, `UN_TAU`) and spectral correction factors.
- `_l2_` — quality-controlled subset (8 columns): `H`, `H_FLAG`, `LE`, `ET`, `TAU`, `TAU_FLAG`, `SHF` (soil heat flux). Use this for analysis that requires QC'd data.

**`data/cosmos-uk_sitemetadata_2014-2024.csv`** — one row per site with location (BNG easting/northing + WGS84 lat/lon), altitude, soil type, land cover, bulk density, soil organic carbon, and lattice water.

**`supporting-documents/`** — variable dictionaries for L1 and L2 (`cosmos-uk_fastflux_l1/l2_2014-2024_metadata.csv`) and a full methods document (`.docx`).

**`data_sm/`** — hydrometeorological and soil moisture data, one pair of CSVs per site, named `cosmos-uk_<siteid>_hydrosoil_<level>_<years>.csv`:

- `_sh_` — 30-minute resolution.
- `_daily_` — daily resolution (same columns as `_sh_`, aggregated).

Columns: net/incoming/outgoing radiation (`RN`, `LWIN`, `LWOUT`, `SWIN`, `SWOUT`), precipitation (`PRECIP`, `PRECIP_TIPPING`, `PRECIP_RAINE`), meteorology (`PA`, `TA`, `WS`, `WD`, `Q`, `RH`), soil heat flux plates (`G1`, `G2`), up to 10 TDT capacitance probes (`TDT<n>_TSOIL`, `TDT<n>_VWC`), standpipe soil temperature at 5 depths (`STP_TSOIL2/5/10/20/50`), cosmic-ray neutron probe soil moisture (`COSMOS_VWC`, plus `CTS_MOD_CORR` and footprint depth `D86_75M`), snow (`SNOW`, `SNOW_DEPTH`, `SWE`), `ALBEDO`, `PE`, and canopy greenness (`GCC`). Missing values are `-9999`, same convention as `data/`.

Only EASTB's files are currently present locally — the source EIDC dataset covers 51 sites, so other sites' hydrosoil files can be downloaded the same way if needed.

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
