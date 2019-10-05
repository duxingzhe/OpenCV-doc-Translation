Geospatial raster data is a heavily used product in Geographic Information Systems and Photogrammetry. Raster data typically can represent imagery and Digital Elevation Models (DEM). The standard library for loading GIS imagery is the Geographic Data Abstraction Library (GDAL). In this example, we will show techniques for loading GIS raster formats using native OpenCV functions. In addition, we will show some an example of how OpenCV can use this data for novel and interesting purposes.

Goals

The primary objectives for this tutorial:

* How to use OpenCV imread to load satellite imagery.
* How to use OpenCV imread to load SRTM Digital Elevation Models
* Given the corner coordinates of both the image and DEM, correllate the elevation data to the image to find elevations for each pixel.
* Show a basic, easy-to-implement example of a terrain heat map.
* Show a basic use of DEM data coupled with ortho-rectified imagery.

To implement these goals, the following code takes a Digital Elevation Model as well as a GeoTiff image of San Francisco as input. The image and DEM data is processed and generates a terrain heat map of the image as well as labels areas of the city which would be affected should the water level of the bay rise 10, 50, and 100 meters.