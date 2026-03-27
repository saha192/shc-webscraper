# Soil Health Card KML Scraper

This repository contains a Python script to download and organize Soil Health Card (SHC) geospatial data from the SHC endpoint, including:

- state metadata
- district metadata
- layer metadata
- KML files
- parsed feature JSON files

## What the script does

The main script:

1. fetches the list of states
2. fetches districts for each state
3. fetches layer metadata for each district
4. downloads KML files for each SHC layer
5. parses the KML files into merged `features.json`

## Repository structure

```text
soil-health-kml-scraper/
├── download_shc_kml.py
├── README.md
└── utils/
    ├── __init__.py
    ├── get_layer_info.py
    └── fetch_features.py
```

## Output directory structure

When you run the script, output will look like this:

```text
data/
└── KML_FILES/
    ├── getStates.json
    └── states/
        ├── TELANGANA/
        │   ├── getDistricts.json
        │   └── MANCHERIAL/
        │       ├── getLayers.json
        │       ├── features.json
        │       ├── <statecode>_<districtcode>_shc_<layer1>.kml
        │       ├── <statecode>_<districtcode>_shc_<layer2>.kml
        │       └── ...
        ├── MADHYA PRADESH/
        │   ├── getDistricts.json
        │   └── BARWANI/
        │       ├── getLayers.json
        │       ├── features.json
        │       └── ...
        └── ...
```

## Requirements

Install the required packages:

```bash
pip install requests tqdm
```

Also make sure your project contains these utility modules:

- `utils/get_layer_info.py`
- `utils/fetch_features.py`

These should expose:

- `get_layer_info(...)`
- `fetch_kml(...)`
- `parse_kml(...)`

## How to run

### Run for all states and districts

```bash
python download_shc_kml.py
```

### Run for one state only

```bash
python download_shc_kml.py --state TELANGANA
```

### Run for one state and one district only

```bash
python download_shc_kml.py --state TELANGANA --district MANCHERIAL
```

### Use a custom data directory

```bash
python download_shc_kml.py --data-dir "H:\shc_data"
```

This will create output inside:

```text
H:\shc_data\KML_FILES
```

## Notes

- State and district filters should be passed in uppercase to match the internal matching logic.
- Existing JSON/KML files are skipped if they are already present.
- The script assumes the SHC endpoint and GraphQL queries remain available in their current form.
- If your utilities return unexpected output types, update the helper functions accordingly.

## Example commands

```bash
python download_shc_kml.py --state "MADHYA PRADESH" --district BARWANI
python download_shc_kml.py --state TELANGANA --district MANCHERIAL
```

## Suggested `.gitignore`

If you do not want to push downloaded data to GitHub, add this to `.gitignore`:

```gitignore
data/
__pycache__/
*.pyc
```

## Optional improvement

If you want, you can also add a `requirements.txt` file like this:

```text
requests
tqdm
```
