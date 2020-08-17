=====================
Data Processing Steps
=====================


Vector Shapefile Component
---------------------

The Global Administrative Unit Layer (GAUL) for admin 0 (country-level) is a single shapefile with each country being a feature.

We will need separate shapefiles for each country:


	# Selecting country and exporting into separate shapefile

	import geopandas as gpd
	import matplotlib
	import os
	%matplotlib inline

	dataSrc = gpd.read_file('.\Shapefiles\Global_admin_boundaries\99bfd9e7-bb42-4728-87b5-07f8c8ac631c2020328-1-1vef4ev.lu5nk.shp')
	dataSrc = dataSrc.loc[dataSrc['OBJECTID'] == 25]
	print(dataSrc)
	dataSrc.to_file('Bolivia.shp')