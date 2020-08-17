==========
Input Data
==========

Raster Component
----------

Input data consist of whichever variables are needed for the CLEWs model in **RASTER** format. Their extent may be global or custom, country or region specific. The global raster inputs so far have been acquired from sources like NASA and research institutes, consisting of satellite acquired data as well as research based models. Current inventory of global raster inputs are as follows:

1. [Land Cover](https://lpdaac.usgs.gov/products/mcd12q1v006/)
2. [Rainflux](https://ldas.gsfc.nasa.gov/FLDAS/)
3. [Water Table Depth](https://gmd.copernicus.org/articles/12/2401/2019/#section6)

The raw input data from your sources may consist of a variety of raster formats. 

Land cover - HDF
Rainflux - netCDF
Groundwater - XYZ

However this CLEWs process is designed for simpler raster formats, therefore the inputs were converted to TIFF. QGIS offers simple processes for such conversions.



Vector Shapefile Component
---------

This project uses administrative boundaries shapefiles that are used to clip the raster components to specific countries.

The Global Administrative Unit Layers (GAUL)](http://www.fao.org/geonetwork/srv/en/metadata.show) is implemented by FAO.

