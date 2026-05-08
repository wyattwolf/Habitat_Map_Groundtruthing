# Habitat_Map_Groundtruthing
This repository contains an R script for comparing ground-truth substrate/habitat points to mapped substrate/habitat polygons.

## What the script does

The script:

- reads a CSV of ground-truth points
- reads a polygon shapefile of mapped substrate
- ignores polygons labeled `shadow`
- checks for matching substrate polygons within a small buffer around each point to reduce the effect of poor GPS accuracy
- if no match is found in the buffer, uses the polygon the point falls in
- if the point does not fall in a polygon, uses the nearest polygon
- exports a CSV of reference vs. predicted classes
- writes a confusion matrix to a text file

## Required R packages

```r
install.packages(c("sf", "dplyr", "readr", "caret", "terra"))
```

## Input files

You need:

- a CSV of points with `X`, `Y`, `OID`, and a substrate field
- a polygon shapefile with a substrate field

## Edit these settings in `Ground_Truth.R`

```r
points_csv <- "your_points.csv"
polygon_shapefile <- "your_polygons.shp"
output_csv <- "Groundtruth_Output.csv"
output_confmatrix <- "ConfusionMatrix.txt"
buffer_m <- 5
target_crs <- 32614
points_substrate_field <- "FC_Primary_Substrate"
polygon_substrate_field <- "Name"
```

## What each setting means

- `points_csv`  
  Path to the input CSV file containing your ground-truth points.

- `polygon_shapefile`  
  Path to the input shapefile containing mapped substrate polygons.

- `output_csv`  
  Name of the CSV file the script will create with the reference and predicted classes.

- `output_confmatrix`  
  Name of the text file the script will create for the confusion matrix output.

- `buffer_m`  
  Buffer distance in meters around each point. This is mainly used to reduce problems caused by poor GPS accuracy.

- `target_crs`  
  EPSG code for the projected coordinate reference system used by the points and polygons.

- `points_substrate_field`  
  Name of the substrate field in the point CSV.

- `polygon_substrate_field`  
  Name of the substrate field in the polygon shapefile.

## How to run

Open R or RStudio and run:

```r
source("Ground_Truth.R")
```

## Output

The script creates:

- a CSV with `Reference` and `Prediction`
- a text file with the confusion matrix

## Notes

- Make sure your point coordinates and polygon data use the correct projected CRS.
- Update the class crosswalk in the script if your substrate codes are different.
