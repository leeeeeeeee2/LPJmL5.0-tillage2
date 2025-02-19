# Lakes and rivers

The directory contains scripts to derive cell fractions covered by different
types of water bodies.

## Input
The Global Lakes and Wetlands Database (GLWD) is used as source dataset.
You may either use GLWD Level 3 raster data or polygon data of GWLD level 1
and 2.

Processing GLWD Level 3 data is generally faster, but supports only target
resolutions that are an integer multiple of the source resolution.
Using GLWD level 1 and 2 polygon data provides more flexibility in terms of the
target resolution.

Download either Level 3 data or Level 1 and 2 data from
https://www.worldwildlife.org/pages/global-lakes-and-wetlands-database and unzip
in working directory.

This sample application uses 2 grid files generated by the scripts in
`../gadm_paper`, one at 5 minute and the other at 30 minute spatial resolution.
Waterboes are extracted for both resolutions using both GLWD level 3 data and
GLWD level 1 and 2 data to determine the difference between both approaches.

## Software requirements
- `R`
- R packages `lwgeom`, `raster`, `rgdal`, `sf`; to use MPI backend for
  parallelization: `doMPI`, `Rmpi`; or use `doParallel`, `parallel` to run on
  multiple CPUs of a local machine.

## Files included in this directory
  - lakes_rivers_fraction.R: Script to derive cell fractions of cells in grid
    input file covered by different types of water bodies distinguished by GLWD
    level 3
  - lakes_rivers_fraction_SLURM.sh: Example batch script to submit
    lakes_rivers_fraction.R as a job on the PIK high-performance cluster
  - lakes_rivers_polygonbased.R: Script to derive cell fractions of cells in
    grid input file covered by different types of water bodies based on polygon
    data
  - lakes_rivers_polygonbased_SLURM.sh:Example batch script to submit
    lakes_rivers_polygonbased.R as a job on the PIK high-performance cluster
  - lakes_rivers_polygonbased_test_nos2.R: Variation of
    `lakes_rivers_polygonbased.R` run on another system to test effect of
    different versions of the `sf` package on results.
  - paper_analysis.R: Script to analyse generated input files for paper
  - README.md: This file
  - r_modules.sh: Shell script to load necessary modules to run R script on
    PIK high-performance cluster

## Setup for lakes_rivers_fraction.R
  - `glwd_dir`: Directory where outputs will be saved, set to current working
    directory
  - `gridname`: Set path to one of 2 LPJmL grid input files
  - `glwd3_name`: Set path to GLWD level 3 raster file (usually unzipped as
    `glwd_3/hdr.adf`)
  - `glwd3_classes`: Set up which GLWD types of water bodies to extract
    (default: lakes, rivers)
  - `version_string`: Optional version string added to filenames of files
    created by this script. Not used for sample application since file names of
    generated LPJmL input files are distinguished by spatial resolution.

### Additional settings in lakes_rivers_fraction.R
  - `output_format`: Select either one of the native LPJmL input formats
    ("BIN", "RAW") or a raster format supported by the raster package (e.g.
    NetCDF or ASCII grid) for files created by this script. Used "BIN" for
    sample application.
  - `bintype`: Version of `BIN` format used (default: 3)
  - `headername`: Header name used in file header if `BIN` format is used.
    This must match the header name defined in LPJmL source code (default:
    "LPJLAKE")

## Setup for lakes_rivers_polygonbased.R
  - `glwd_dir`: Directory where outputs will be saved, set to current working
    directory
  - `gridname`: Set path to one of 2 LPJmL grid input files
  - `glwd1_name`: Set path to GLWD level 1 polygon shapefile
  - `glwd2_name`: Set path to GLWD level 2 polygon shapefile
  - `glwd_classes`: Set up which GLWD types of water bodies to extract
    (default: lakes, rivers)
  - `version_string`: Optional version string added to filenames of files
    created by this script. Set to "polygon-based" for this sample application
    to distinguish files from files created by `lakes_rivers_fraction.R`

### Additional settings in lakes_rivers_polygonbased.R
  - `output_format`: Set to "BIN".
  - `bintype`: Version of `BIN` format used (default: 3)
  - `headername`: Header name used in file header if `BIN` format is used.
    This must match the header name defined in LPJmL source code (default:
    "LPJLAKE")
