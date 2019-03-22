
-   [R toolbox to find, download and preprocess Sentinel-2 data](#r-toolbox-to-find-download-and-preprocess-sentinel-2-data)
    -   [Installation](#installation)
    -   [Usage](#usage)
    -   [Credits](#credits)

<!-- IMPORTANT: do NOT edit README.Rmd! Edit index.Rmd instead,       -->
<!-- and generate README.Rmd using inst/extdata/code/create_README.sh -->
[![Travis-CI Build Status](https://travis-ci.org/ranghetti/sen2r.svg?branch=master)](https://travis-ci.org/ranghetti/sen2r) [![Docker Build Status](https://img.shields.io/docker/build/ranghetti/sen2r.svg)](https://hub.docker.com/r/ranghetti/sen2r) [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.1240384.svg)](https://doi.org/10.5281/zenodo.1240384) [![CRAN Status](http://www.r-pkg.org/badges/version/sen2r)](https://cran.r-project.org/package=sen2r) [![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0)

<img src="man/figures/sen2r_logo_200px.png" width="200" height="113" align="right" />

R toolbox to find, download and preprocess Sentinel-2 data
==========================================================

<span style="color:#5793dd;vertical-align:top;font-size:90%;font-weight:normal;">sen</span><span style="color:#6a7077;vertical-align:baseline;font-size:115%;font-weight:bolder;">2</span><span style="color:#2f66d5;vertical-align:baseline;font-size:90%;font-weight:bold;">r</span> is an R library which helps to download and preprocess Sentinel-2 optical images. The purpose of the functions contained in the library is to provide the instruments required to easily perform (and eventually automate) all the steps necessary to build a complete Sentinel-2 processing chain, without the need of any manual intervention nor the needing to manually integrate any external tool.

In particular, <span style="color:#5793dd;vertical-align:top;font-size:90%;font-weight:normal;">sen</span><span style="color:#6a7077;vertical-align:baseline;font-size:115%;font-weight:bolder;">2</span><span style="color:#2f66d5;vertical-align:baseline;font-size:90%;font-weight:bold;">r</span> allows to:

-   retrieve the list of available products on a selected area (which can be provided by specifying a bounding box, by loading a vector file or by drawing it on a map) in a given time window;
-   download the required SAFE Level-1C products, or retrieve the required SAFE Level-2A products by downloading them (if available) or downloading the corresponding Level-1C and correcting them with **Sen2Cor**;
-   obtain the required products (Top of Atmosphere radiances, Bottom of Atmosphere reflectances, Surface Classification Maps, True Colour Images) clipped on the specified area (adjacent tiles belonging to the same frame are merged);
-   mask cloudy pixels (using the Surface Classification Map as masking layer);
-   computing spectral indices and RGB images.

Setting the execution of this processing chain is particularly easy using the <span style="color:#5793dd;vertical-align:top;font-size:90%;font-weight:normal;">sen</span><span style="color:#6a7077;vertical-align:baseline;font-size:115%;font-weight:bolder;">2</span><span style="color:#2f66d5;vertical-align:baseline;font-size:90%;font-weight:bold;">r</span> GUI, which allows to set the parameters, to directly launch the main function or to save them in a JSON file which can be used to launch the processing at a later stage.

The possibility to launch the processing with a set of parameters saved in a JSON file (or directly passed as function arguments) makes easy to build scripts to automatically update an archive of Sentinel-2 products. Specific processing operations (i.e. applying **Sen2Cor** on Level-1c SAFE products, merging adjacent tiles, computing spectral indices from existing products) can also be performed using intermediate functions (see [usage](#usage)).

Installation
------------

<span style="color:#5793dd;vertical-align:top;font-size:90%;font-weight:normal;">sen</span><span style="color:#6a7077;vertical-align:baseline;font-size:115%;font-weight:bolder;">2</span><span style="color:#2f66d5;vertical-align:baseline;font-size:90%;font-weight:bold;">r</span> is supported over Linux and Windows operating systems; the support for Mac will be added in future.

### Install locally

The package can be installed from GitHub with the R package **remotes**:

``` r
remotes::install_github("ranghetti/sen2r")
```

For detailed instructions about installing the package (including dependencies), see the [Installation](http://sen2r.ranghetti.info/articles/installation.html) page.

### Run as 
image

A dockerised version of <span style="color:#5793dd;vertical-align:top;font-size:90%;font-weight:normal;">sen</span><span style="color:#6a7077;vertical-align:baseline;font-size:115%;font-weight:bolder;">2</span><span style="color:#2f66d5;vertical-align:baseline;font-size:90%;font-weight:bold;">r</span> is available [here](https://hub.docker.com/r/ranghetti/sen2r).

For detailed instructions about using it, see the page ["Run in a Docker container"](http://sen2r.ranghetti.info/articles/docker.html) page.

Usage
-----

The simplest way to use <span style="color:#5793dd;vertical-align:top;font-size:90%;font-weight:normal;">sen</span><span style="color:#6a7077;vertical-align:baseline;font-size:115%;font-weight:bolder;">2</span><span style="color:#2f66d5;vertical-align:baseline;font-size:90%;font-weight:bold;">r</span> is to execute it in interactive mode:

``` r
library(sen2r)
sen2r()
```

this opens a GUI which allows to set the required processing parameters, and then launches the main function.

<p style="text-align:center;">
<a href="https://raw.githubusercontent.com/ranghetti/sen2r/devel/man/figures/sen2r_gui_sheet1.png" target="_blank"> <img src="man/figures/sen2r_gui_sheet1_small.png"> </a> <a href="https://raw.githubusercontent.com/ranghetti/sen2r/devel/man/figures/sen2r_gui_sheet2.png" target="_blank"> <img src="man/figures/sen2r_gui_sheet2_small.png"> </a> <br/> <a href="https://raw.githubusercontent.com/ranghetti/sen2r/devel/man/figures/sen2r_gui_sheet3.png" target="_blank"> <img src="man/figures/sen2r_gui_sheet3_small.png"> </a> <a href="https://raw.githubusercontent.com/ranghetti/sen2r/devel/man/figures/sen2r_gui_sheet4.png" target="_blank"> <img src="man/figures/sen2r_gui_sheet4_small.png"> </a> <a href="https://raw.githubusercontent.com/ranghetti/sen2r/devel/man/figures/sen2r_gui_sheet5.png" target="_blank"> <img src="man/figures/sen2r_gui_sheet5_small.png"> </a>
</p>
Alternatively, [`sen2r()`](http://sen2r.ranghetti.info/reference/sen2r.html) can be launched with a list of parameters (created with [`s2_gui()`](http://sen2r.ranghetti.info/reference/s2_gui.html)) or passing manually the parameters as arguments of the function (see [the documentation of the function](http://sen2r.ranghetti.info/reference/sen2r.html) for further details).

Other specific functions can be used to run single steps separately:

-   [`s2_list()`](http://sen2r.ranghetti.info/reference/s2_list.html) to retrieve the list of available Sentinel-2 products basing on input parameters;
-   [`s2_download()`](http://sen2r.ranghetti.info/reference/s2_download.html) to download Sentinel-2 products;
-   [`sen2cor()`](reference/sen2cor.html) to correct level-1C products using [Sen2Cor](http://step.esa.int/main/third-party-plugins-2/sen2cor);
-   [`s2_translate()`](http://sen2r.ranghetti.info/reference/s2_translate.html) to convert Sentinel-2 products from SAFE format to a format managed by GDAL;
-   [`s2_merge()`](http://sen2r.ranghetti.info/reference/s2_merge.html) to merge Sentinel-2 tiles which have the same date and orbit;
-   [`gdal_warp()`](http://sen2r.ranghetti.info/reference/gdal_warp.html) to clip, reproject and warp raster files (this is a wrapper to call [gdal\_translate](http://www.gdal.org/gdal_translate.html) or [gdalwarp](http://www.gdal.org/gdalwarp.html) basing on input parameters);
-   [`s2_mask()`](http://sen2r.ranghetti.info/reference/s2_mask.html) to apply a cloud mask to Sentinel-2 products;
-   [`s2_rgb()`](http://sen2r.ranghetti.info/reference/s2_rgb.html) to generate RGB images from Sentinel-2 Surface Reflectance multiband raster files;
-   [`s2_calcindices()`](http://sen2r.ranghetti.info/reference/s2_calcindices.html) to compute maps of spectral indices from Sentinel-2 Surface Reflectance multiband raster files;
-   [`s2_thumbnails()`](http://sen2r.ranghetti.info/reference/s2_thumbnails.html) to generate RGB thumbnails (JPEG or PNG) of the products.

Credits
-------

<a href="http://www.irea.cnr.it" target="_blank"> <img src="man/figures/irea_logo_200px.png" height="100" align="left" style="padding-right: 100px;"/></a> <span style="color:#5793dd;vertical-align:top;font-size:90%;font-weight:normal;">sen</span><span style="color:#6a7077;vertical-align:baseline;font-size:115%;font-weight:bolder;">2</span><span style="color:#2f66d5;vertical-align:baseline;font-size:90%;font-weight:bold;">r</span> is being developed by Luigi Ranghetti and Lorenzo Busetto ([IREA-CNR](http://www.irea.cnr.it)), and it is released under the [GNU General Public License version 3](https://www.gnu.org/licenses/gpl-3.0.html) (GPL‑3).

The [<span style="color:#5793dd;vertical-align:top;font-size:90%;font-weight:normal;">sen</span><span style="color:#6a7077;vertical-align:baseline;font-size:115%;font-weight:bolder;">2</span><span style="color:#2f66d5;vertical-align:baseline;font-size:90%;font-weight:bold;">r</span> logo](https://raw.githubusercontent.com/ranghetti/sen2r/devel/man/figures/sen2r_logo_200px.png), partially derived from the [R logo](https://www.r-project.org/logo), is released under the [Creative Commons Attribution-ShareAlike 4.0 International license](https://creativecommons.org/licenses/by-sa/4.0) (CC-BY-SA 4.0).

The functionalities to search and download SAFE tiles are based on the Python tool [Sentinel-download](https://github.com/olivierhagolle/Sentinel-download) by Olivier Hagolle, released under the [GNU General Public License version 2](https://www.gnu.org/licenses/gpl-2.0.html) (GPL‑2).

To cite this library, please use the following entry:

Ranghetti, L. and Busetto, L. (2019). *sen2r: an R toolbox to find, download and preprocess Sentinel-2 data*. R package version 1.0.0. DOI: [10.5281/zenodo.1240384](https://dx.doi.org/10.5281/zenodo.1240384). URL: <http://sen2r.ranghetti.info>.

``` bibtex
@Manual{sen2r,
  title  = {sen2r: an R toolbox to find, download and preprocess Sentinel-2 data},
  author = {Luigi Ranghetti and Lorenzo Busetto},
  year   = {2019},
  note   = {R package version 1.0.0},
  doi    = {10.5281/zenodo.1240384},
  url    = {http://sen2r.ranghetti.info},
}
```
