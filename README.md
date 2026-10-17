# Finding Flying Aircraft in SAR Imagery

This repository contains the code, notebooks, and supporting files required to reproduce the results, figures, and tables presented in the article. All workflows are provided as Jupyter notebooks and can be run directly in **VS Code** or any other Jupyter-compatible environment.

## Project Overview

The project combines:

- **Sentinel-1 SAR imagery** from the Copernicus Data Space Ecosystem
- **OpenSky ADS-B aircraft data**
- Aircraft trajectory interpolation
- Doppler displacement estimation
- Visualization of aircraft locations in SAR imagery

For each Sentinel-1 acquisition, the workflow:

1. Downloads the SAR scene.
2. Retrieves aircraft positions from OpenSky.
3. Interpolates aircraft trajectories during the SAR acquisition.
4. Computes the expected Doppler-induced displacement.
5. Projects aircraft positions into SAR image coordinates.
6. Generates annotated SAR images showing:
   - ADS-B aircraft positions
   - Aircraft tracks
   - Aircraft headings
   - Doppler search areas
   - The observed SAR signatures

## Example: BAW14K

An example is included for the aircraft **BAW14K**. By providing valid:

- Copernicus Data Space credentials
- OpenSky/Trino credentials

and running the notebooks, the workflow will automatically generate the SAR image containing:

- The ADS-B aircraft position
- The predicted Doppler search area
- The aircraft track and heading
- The corresponding aircraft signature in the SAR image

## Reproducing Other Results

To reproduce other examples from the article, simply modify the acquisition dates in the notebook configuration section. The workflow will then download Sentinel-1 scenes for the selected period, retrieve matching aircraft data, and generate the corresponding SAR visualizations.

## Requirements

- Python 3.x
- Jupyter Notebook or VS Code
- Copernicus Data Space Ecosystem account
- OpenSky Network account with Trino access

### Account Registration

**Copernicus Data Space Ecosystem**

<https://dataspace.copernicus.eu/>

**OpenSky Network**

<https://opensky-network.org/>

## Repository Structure

```text
├── Smart_download.ipynb        # Sentinel-1 download workflow
├── SAR_processing.ipynb        # Aircraft processing and Doppler calculations
├── Example_Data/               # Example outputs
├── Figures/                    # Generated figures
└── README.md
```

## Citation

(When the article comes out there will be a citation to it here)
