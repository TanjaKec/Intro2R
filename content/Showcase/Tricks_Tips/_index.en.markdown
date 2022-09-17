---
date: "2016-04-09T16:50:16+02:00"
title: Spatial Data Analysis
output: 
  learnr::tutorial
weight: 3
---

R has a range of packages which provide functionality for handling spatial data and performing complex spatial analysis operations. Spatial data analysis is no longer reserved just for experts with expensive hardware and software. R has impressive geographic capabilities which anyone with the desire for exploration of the geospatial data can use. 

Before we get into geospatial maping in R, let us introduce some basic ideas about spatial data.

Spatial data comprises of:

- coordinates of the object
- the coordinates relate to a physical location on Earth: **coordinate reference system** (**CRS**)

There are **two types of CRS**:

1) Geographical

  * specific locations on the surface is defined by three dimensional model of the Earth. 
  * longitude and latitude

2) Projected

  *	A conversion of the three-dimensional grid onto a two-dimensional plane.

According to the storing technique, spatial data is one of the two types:

1)	**Raster data**: composed of grid cells identified by row and column. The whole geographic area is divided into groups of individual cells, which represent an image (satellite images)

2)	**Vector data** are composed of points, polylines, and polygons. For example hospitals, houses etc. are represented by points, while rivers, roads,etc., are represented by polylines. Villages and towns are represented by polygons.

When doing spatial analysis you will also deal with attribute data, which contain the relevant information about the spatial data. The analysis will be based on attribute data attached to geospatial data. Such data could be:

* nominal
* ordinal
* interval data
* ratio data,

ie. attribute or measured type of the data.

### Static Maps: Shape File 

R has impressive geographic capabilities and can handle different kinds of spatial data file formats including geojson and KML. We will illustrate the use of the ESRI Shapefile format, which stores nontopological geometry and attribute information for the spatial features in a data set. A shapefile consists minimally of a main file, an index file, and a dBASE table.

-	.shp - lists shape and vertices
-	.shx - has index with offsets
-	.dbf - relationship file between geometry and attributes (data)

To import a ESRI shapefile into R correctly, all three files must be present in the directory and named the same (except for the file extension).

Let us start by reading a shape file of Serbian districts boundaries avaliable from [GADM maps and data](https://gadm.org/download_country_v3.html).


```
## If you don't have sf installed yet, uncomment and run the line below
#install.packeges("sf")

library(ggplot2)
library(sf)

#pointed to the shape file
serbia_location <- "gadm36_SRB_1.shp"

#used the st_read() function to import it
serbia_districts <- st_read(serbia_location)

# take a look at the file
View(serbia_districts)

# plot the disstricts 
ggplot(serbia_districts) + 
  geom_sf()
```

![](/day2/SpatialDA/images/serbia.png?width=40pc)

This looks good 😃 Note that plotting the boundaries using ggplot2 might take some time.
Let us add some colour, ie. data to our map.

```
population <- read.csv("Serbian_Pop.csv")
View(population)

library(dplyr)

serbia_pop <- left_join(serbia_districts, population,
                          by=c("NAME_1" = "District"))

View(serbia_pop)


ncol(serbia_districts)

ncol(population)

ncol(serbia_pop)

names(serbia_pop)


ggplot(serbia_pop) +
  geom_sf(aes(fill=total_pop)) +
  scale_fill_distiller(direction = 1, name = "Population") +
  labs(title="Population of Serbia for 2017", caption="Source: Statisticki Zavod SR")
```

![](/day2/SpatialDA/images/SR_pop_2017.png?width=40pc)

```
ggplot(serbia_pop) +
  geom_sf(aes(fill = total_pop)) +
  scale_fill_distiller(direction = 1, name = "Population") +
  labs(title="Population of Serbia for 2017", caption="Source: Statisticki Zavod SR") +
  facet_wrap(~NAME_1)
```

![](/day2/SpatialDA/images/facet_sr.png?width=40pc)

### Interactive Maps: leaflet


```r
## If you don't have leaflet installed yet, uncomment and run the line below
#install.packeges("leaflet")
library(leaflet)
# Initialize and assign us as the leaflet object
us <- leaflet() %>%
  # add tiles to the leaflet object
  addTiles() %>%  
  # setting the centre of the map and the zoom level
  setView(lng = 20.41215, lat = 45.93362, zoom = 15) %>%
  # add a popup marker 
  addMarkers(lng = 20.407040, lat = 45.934230, popup = "<b>Ciao!</b><br><a href='https://www.mokrinhouse.com/'>Mokrin House! 😀</a>")

us
```

<!--html_preserve--><div id="htmlwidget-f6920a36cc5472b8a547" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-f6920a36cc5472b8a547">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addMarkers","args":[45.93423,20.40704,null,null,null,{"interactive":true,"draggable":false,"keyboard":true,"title":"","alt":"","zIndexOffset":0,"opacity":1,"riseOnHover":false,"riseOffset":250},"<b>Ciao!<\/b><br><a href='https://www.mokrinhouse.com/'>Mokrin House! 😀<\/a>",null,null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]}],"setView":[[45.93362,20.41215],15,[]],"limits":{"lat":[45.93423,45.93423],"lng":[20.40704,20.40704]}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


{{% notice note %}}
bilo sta
{{% /notice %}}


## YOUR TURN 👇

Practise by doing the following set of exercises:




##### useful links: 

[Geocomputation with R](https://geocompr.robinlovelace.net/): a book on geographic data analysis, visualization and modeling.

[GADM maps and data](https://gadm.org/download_country_v3.html)

<https://www.oipapio.com/question-5739445>

<https://map-rfun.library.duke.edu/031_thematic_mapping.html>

-----------------------------
© 2019 Tatjana Kecojevic
