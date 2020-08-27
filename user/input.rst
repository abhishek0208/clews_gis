==========
Input Data
==========


Raster Component
----------

Input data consist of whichever variables are needed for the CLEWs model in **RASTER** format. Their extent may be global or custom, country or region specific. 

The global raster inputs so far have been acquired from sources like NASA and other research institutes, consisting of satellite acquired data as well as research based models. Current inventory of global raster inputs are as follows:

* `Landcover`_
* `Rainflux`_
* `Water Table Depth`_


In addition, some inputs in consideration are: 

* CarbonFlux (Net Ecosystem Excchange) (link)
* Solar Horizontal Irradiation (Link)
* Wind Power Density Potential (link)

The raw input data from your sources may consist of a variety of raster formats (netCDF, HDF, TIFF etc). However this CLEWs process is designed for simpler raster formats, therefore all inputs were converted to TIFF. QGIS offers simple processes for such conversions.



Vector Shapefile Component
---------

This project uses administrative boundaries shapefiles that are used to clip the raster components to specific countries.

The Global Administrative Unit Layers GAUL_ is implemented by FAO.


.. _Landcover: https://lpdaac.usgs.gov/products/mcd12q1v006/
.. _Rainflux: https://ldas.gsfc.nasa.gov/FLDAS/
.. _Water Table Depth: https://gmd.copernicus.org/articles/12/2401/2019/#section6
.. _GAUL: http://www.fao.org/geonetwork/srv/en/metadata.show?id=12691

