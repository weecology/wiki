---
title: "Geospatial Computing from the Command Line"
linkTitle: "Geospatial Command Line"
type: book
summary: " "
---

## Installation

This commands on this page uses `gdal` and `jq`.
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

One way to combine rasters is to use the `gdal_merge.py` Python script bundled with `gdal`.
We use LZW compression to reduce file sizes while ensuring that the resulting GeoTIFF can be used in all geospatial computing systems.

```bash
gdal_merge.py -co COMPRESS=LZW -o output_file.tif input_file_1.tif input_file_2.tif input_file_3.tif
```
