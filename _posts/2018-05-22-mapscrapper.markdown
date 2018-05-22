﻿---
layout: post
title:  "Scrapping data with R"
date:   2018-05-22 17:24:10 +0200
categories: R packages
---
##Preliminary steps
-----------------

### Installing a usable web browser

Before we can actually scrap data from the website, we need to setup a web browser that will run in the background. I recommand using Chrome (mainly because I could not manage doing it with Firefox but you're free to try it).

### Installing Rtools

We also needs an external software to create some external files, called RTools. The installer can be downloaded [here](https://cran.r-project.org/bin/windows/Rtools/index.html). During the instalation tick the box "Add Rtools to system path"

### Installing the mapscrapper package

A brand new, not super efficient and certainly not optimized package to scrap the opentransportmap website!

``` r
devtools::install_github("vpellissier/mapscrapper", ref = "pipes")
```

### Installing and starting a Selenium server

There is several ways to do this, but here we are going to use a standalone server. THe server can be downloaded from the [Selenium Project](http://selenium-release.storage.googleapis.com/index.html). Once downloaded, the server can be started using command line (either windows + cmd, or Tools/Shell from RStudio):

`> java  -jar path/file_name`

Where path is the path were you have downloaded the file, and file the name of the file (should end with a .jar extension)

##Scrapping the website
---------------------

Downloading all the zip file for a given country is quite straightforward. First, we need to know the name of the countries in the first dropdownlist

### Gathering the names of the countries (NUTS0)

The dropdown\_values function can list all the possible values of a given NUTS level (here, 0):

``` r
library(mapscrapper, quietly = T)
names_nuts0 <- dropdown_values(nuts = 0, remDr = NULL)
```

If this return an error message, you need to re-run the previous line of code!

The function returns a vector containing the names of the NUTS0 as they are in the website:

    ## [1] "Austria"        "Belgium"        "Bulgaria"       "Croatia"       
    ## [5] "Cyprus"         "Czech Republic"

### Downloading the maps for a given country

The zip files are going to be downloaded in a folder with the name of the country inside the root path (if it does not exist, the folder will be created):

``` r
download_map_country("Estonia", root_path = "C:/Maps")
```