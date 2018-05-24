---
layout: post
title:  "Scrapping data with R"
date:   2018-05-22 17:24:10 +0200
categories: R packages
---

The website [Open Transport Map](http://opentransportmap.info/) hosts very cool maps of road traffic volume across the whole EU.
Unfortunately, the data are downloadable for each NUTS3 and there is no option to download all the data at once, or even download a selection of NUTS3, so I came together with this package which allow downloading all the maps for a given country.

Preliminary steps
-----------------

### Installing a usable web browser

Before we can actually scrap data from the website, we need to setup a web browser that will run in the background. I recommand using Chrome (mainly because I could not manage doing it with Firefox but you're free to try it).

### Installing Webdriver

The package used to navigate through the website needs additionnal drivers to interface with the browser.
The driver for chrome is called [chromedriver]() while the driver for Firefox is called [gekodriver]().
Download the version suitable for your system and extract the .exe somewhere on your computer (for example in directories C:/gekdriver and C:/chromdriver).
Then, you need to add these path the the Environment variables of your computer:
	1. From the desktop, right-click My Computer and click Properties.
	2. In the System Properties window, click on the Advanced tab
	3. In the Advanced section, click the Environment Variables button.
	4. Finally, in the Environment Variables window , highlight the Path variable in the Systems Variable section and click the Edit button. Add or modify the path lines with the paths you wish the computer to access. Each different directory is separated with a semicolon as shown below.
	 ` C:\gekodriver;C:\chromedriver`

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

Scrapping the website
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
