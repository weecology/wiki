---
title: "Geospatial Computing from the Command Line"
linkTitle: "Geospatial Command Line"
summary: " "
---

## Installation

The commands on this page use `gdal` and `jq`.

### Conda

You can install these packages on any operating system using conda/mamba.

```sh
mamba create -n my-gdal-env python=3 gdal jq
```

Then activate the environment every time you want to work with gdal.

```sh
mamba activate my-gdal-env
```

### Linux

On Ubuntu these can be installed using:

```bash
sudo apt install gdal-bin jq
```

## Get information about a raster

`gdalinfo` provides information about raster files.

`gdalinfo myraster.tif` will produce a basic readable output to the screen.

This output can also be written to JSON

```bash
gdalinfo -json myraster.tif
```

Writing to JSON makes it easy to use individual pieces, e.g., to look up the dimensions of the raster (you'll need to install `jq` to do this).

```bash
gdalinfo -json myraster.tif | jq -r .size
```

or just the width of the raster

```bash
gdalinfo -json myraster.tif | jq -r .size[0]
```

## Splitting rasters using gdal

One way to split a raster into pieces is to use the `gdal_retile.py` Python script bundled with `gdal`.

The following command will split `myraster.tif` into 1500x1500 pixel rasters stored in `outputdir`.
The first number is the width (in pixels) and the second is the height (in pixels) of each chunk. 

```bash
gdal_retile.py -ps 1500 1500 -targetDir outputdir myraster.tif
```

Files will be labeled with `_row_col` and so if the original image was 4500x1500 then the above command would produce three output files:

```
myraster_1_1.tif
myraster_2_1.tif
myraster_3_1.tif
```

Representing the top of the original raster (`_1_1`), the middle of the original raster (`_2_1`), and the bottom of the original raster (`_3_1`).

### Split raster into horizontal strips

Our most common usage is to split large rasters into horizontal strips with manageable file sizes (< 3 GB). This can be automated by changing `myraster.tif` to the location of your raster and `outputdir` to the directory you want the split raster pieces stored in and running the code below:

```bash
RASTER=myraster.tif
OUTPUTDIR=outputdir
WIDTH=$(gdalinfo -json $RASTER | jq -r .size[0])
WIDTHPAD=$((WIDTH + 10)) # Padding prevents periodic inclusion of single pixel strip 
HEIGHT=$(expr 1000000000 / $WIDTH)
gdal_retile.py -ps $WIDTHPAD $HEIGHT -targetDir $OUTPUTDIR $RASTER
```

## Combining/merging rasters using gdal

### Merging rasters

One way to combine rasters is to use the `gdal_merge.py` Python script bundled with `gdal`.
We use LZW compression to reduce file sizes while ensuring that the resulting GeoTIFF can be used in all geospatial computing systems.

```bash
gdal_merge.py -o output_file.tif input_file_1.tif input_file_2.tif input_file_3.tif
gdal_translate -co COMPRESS=LZW -co PREDICTOR=2 -co BIGTIFF=YES output_file.tif compressed_file.tif
```

We use this two command approach instead of including compression in the merge command because gdal_merge.py doesn't currently support BigTIFF creation correctly and many of our combined files are greater than the 4GB maximum for regular TIFFs.
`PREDICTOR=2` produces more efficient compression in the presence of spatial autocorrelation, which we often have.

### Virtually combining rasters

Instead of actually merging the rasters you can create a virtual raster in a vrt file.
This file includes metadata on the positions of all of the rasters, which can be loaded into a GIS and viewed like a single raster.

```bash
gdalbuildvrt virtual_combined_raster.vrt *.tif
```

## Removing alpha channels

All of our code works with 3 band RGB rasters.
Occasionally we accidentally produce a raster that contains a 4th alpha channel.
This can be removed using GDAL.

First check to make sure the bands you want are the first three bands (they pretty much always are):

```bash
gdalinfo four_band_ortho.tif
```

This should show something like the following info about channels:

```bash
Band 1 Block=1501x1 Type=Byte, ColorInterp=Red
Band 2 Block=1501x1 Type=Byte, ColorInterp=Green
Band 3 Block=1501x1 Type=Byte, ColorInterp=Blue
Band 4 Block=1501x1 Type=Byte, ColorInterp=Alpha
```

The use `gdal_translate.py` to just keep the first three bands:

```bash
gdal_translate -b 1 -b 2 -b 3 four_band_ortho.tif three_band_ortho.tif
```

[Original source](https://support.skycatch.com/hc/en-us/articles/219178537-Removing-the-Alpha-Channel-from-Your-Orthotiff)
