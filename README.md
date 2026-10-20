# Finding Flying Aircraft in SAR Imagery

This repository contains the code, notebooks, and supporting files required to reproduce the results, figures, and tables presented in the article. All workflows are provided as Jupyter notebooks and can be run directly in VS Code or any other Jupyter-compatible environment.

## Project Overview

The project combines:

- Sentinel-1 SAR imagery from the Copernicus Data Space Ecosystem
- OpenSky ADS-B aircraft data
- Aircraft trajectory interpolation
- Doppler displacement estimation
- Height-induced range correction
- Visualisation of aircraft locations in SAR imagery

For each Sentinel-1 acquisition, the workflow:

1. Retrieves the Sentinel-1 scene.
2. Retrieves aircraft positions from OpenSky.
3. Interpolates aircraft trajectories during the SAR acquisition period.
4. Applies a height-induced range correction.
5. Estimates Doppler-induced displacement.
6. Projects aircraft positions into SAR image coordinates.
7. Generates annotated SAR images showing:
   - Aircraft positions
   - Aircraft tracks
   - Aircraft headings
   - Doppler search regions
   - Candidate aircraft signatures

---

## Requirements

- Python 3.x
- Jupyter Notebook or VS Code
- Copernicus Data Space Ecosystem account
- OpenSky Network account with Trino access

### Account Registration

**Copernicus Data Space Ecosystem**

[https://dataspace.copernicus.eu/](https://dataspace.copernicus.eu/)

**OpenSky Network**

[https://opensky-network.org/](https://opensky-network.org/)

---

# Doppler_Visualisation.ipynb

This notebook reproduces the workflow used to identify and visualise candidate aircraft signatures in Sentinel-1 SAR imagery.

## Configuration

The first notebook cell contains all user-configurable parameters:

```python
ROOT
OUTPUT_DIR
DOWNLOAD_DIR
DATE_FROM
DATE_TO_INCLUSIVE
MARGINS_KM
LIMIT_SCENES
LOCAL_ONLY
OFFLINE
REFRESH_CATALOGUE
BBOX
ORBIT_DIRECTION
polygon_wkt
buffer
inclination
```

Key parameters:

- `DATE_FROM` and `DATE_TO_INCLUSIVE` define the processing period.
- `MARGINS_KM` specifies the search-region margin around aircraft tracks.
- `LIMIT_SCENES` limits the number of scenes processed for testing.
- `BBOX` and `polygon_wkt` define the geographic area of interest.
- `OFFLINE=True` disables all network access and uses only locally available data.

## Authentication

If online processing is enabled (`OFFLINE=False`), run the authentication cell before processing:

```python
if OFFLINE:
    print("Offline mode: using cached ADS-B data only.")
else:
    OPENSKY_USER = login_opensky(OPENSKY_USER)
```

A browser window will open for OpenSky authentication.

## Inventory Mode

Before processing imagery, generate an inventory:

```python
inventory_status = main(notebook_arguments() + ["--inventory"])
```

This step:

- Identifies matching Sentinel-1 acquisitions.
- Checks local XML and TIFF file availability.
- Reports missing products.
- Estimates the number of output images.

## Running the Workflow

Execute:

```python
run_status = main(notebook_arguments())
```

The workflow will automatically:

- Discover Sentinel-1 scenes.
- Download missing Sentinel-1 files when required.
- Retrieve aircraft observations from OpenSky.
- Interpolate aircraft trajectories.
- Apply height and Doppler corrections.
- Generate annotated SAR visualisations.
- Save processing metadata and reports.

## Output Files

Results are written to:

```text
Processed_Images/
```

Typical output structure:

```text
Processed_Images/
│
├── inventory.json
├── run_report.json
├── cache/
├── geometry/
└── margin_5km/
```

### inventory.json

Contains:

- Matching Sentinel-1 scenes
- Local file availability
- Missing products
- Expected image count

### run_report.json

Contains:

- Processing status
- Scene-level information
- Error messages
- Output file metadata

### Geometry Files

Stored in:

```text
cache/geometry/
```

These files contain the processed aircraft-track data used during image generation.

### Output Images

Stored in:

```text
margin_5km/
```

Each image contains:

- Aircraft positions
- Flight direction vectors
- Doppler search regions
- Height-corrected aircraft positions

## Offline Processing

To run entirely from local data:

```python
OFFLINE = True
```

In offline mode:

- No Sentinel-1 downloads are performed.
- No OpenSky authentication is required.
- Cached ADS-B data are used.
- All required Sentinel-1 products must already be available locally.

## Reproducing Results

To reproduce a specific example from the article:

1. Set the desired acquisition period.
2. Configure the study area.
3. Authenticate with OpenSky (if not running offline).
4. Generate an inventory.
5. Execute the processing workflow.
6. Inspect the generated SAR visualisations and reports.

The notebook records processing metadata to support reproducibility and verification of generated outputs.

## Citation

A citation to the associated article will be added here once it becomes available.
