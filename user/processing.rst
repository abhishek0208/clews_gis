=====================
Data Processing Steps
=====================


Raster Component
---------------------

The raw raster input from any source will have their own spatial resolution. This tool requires a uniform spatial resolution of 5 arcmins, or 0.08333 degrees, for the rasters. Therefore, given that your input is a different resolution than 5 arcmins, the first step of the process is to resample the input.

To resample a raster is to change the cell size, while retaining the extent of the dataset. The new values of the cells depends on the technique chosen. For this project, the technique of will be nearest neighbor.

.. code-block:: python

	import rasterio
	from rasterio.enums import Resampling

	upscale_factor = 2 #Downsampling to 1/2 of the resolution can be done with upscale_factor = 1/2.

	with rasterio.open("example.tif") as dataset:

    # resample data to target shape
    data = dataset.read(
        out_shape=(
            dataset.count,
            int(dataset.height * upscale_factor),
            int(dataset.width * upscale_factor)
        ),
        resampling=Resampling.bilinear
    )

    # scale image transform
    transform = dataset.transform * dataset.transform.scale(
        (dataset.width / data.shape[-1]),
        (dataset.height / data.shape[-2])
    )

After resampling your raster input, convert it to *ascii* format (asc.). Currently this is done using QGIS using the *convert raster* tool.


Then the asc files are converted to csv, where the originals pixels of the raster are arranged in csv cells:

.. code-block:: python

	read_file = pd.read_csv (r'example.asc', skiprows=6)
	read_file.to_csv (r'example.csv', index=None)


Then convert csv to Lat/Long Geodataframe:

.. code-block:: python

	df_test = pd.read_csv('example.asc', skiprows = 6)


	df_test = pd.melt(df_test, 
                  id_vars = ['Unnamed: 0'], 
                  value_vars = [x for x in df_test.columns if x != 'Unnamed: 0'], 
                  var_name = 'cols')

	df_test['cols'] = df_test['cols'].astype(int)

	df_test = df_test[df_test['cols']!= -9999]

	df_test.rename(columns = {'Unnamed: 0':'rows'}, inplace = True)

	df_test = df_test.dropna()

	print(df_test)


Create geometry:

.. code-block:: python
	
	df_test['cols'] = df_test['cols']#.astype(int)

	xllcorner = -180.000000000000 ### Are these centroids or edges?
	yllcorner = -60
	cellsize = 0.111

	nrows = len(df_test['rows'].unique())
	ncols = len(df_test['cols'].unique())

	yulcentre = yllcorner + (nrows*cellsize) + (cellsize/2)
	xulcentre = xllcorner + (cellsize/2)

	row_min = df_test['rows'].min()
	col_min = df_test['cols'].min()

	# Calculate lat and lon from row and col values
	df_test['lat'] = yulcentre - (df_test['rows'] - row_min)*cellsize 
	df_test['lon'] = xulcentre + (df_test['cols'] - col_min)*cellsize 
	df_test.drop(['rows', 'cols'], 
             axis = 1, 
             inplace = True)

	geometry_test = [Point(xy) 
                 for xy 
                 in zip(df_test['lon'], 
                        df_test['lat'])]

	gdf_test = GeoDataFrame(df_test, 
                        geometry = geometry_test)

	gdf_test.plot(column = 'value')


Lastly, using the lat/long columns of the csv, create a points shapefile.


Vector Shapefile Component
---------------------

The Global Administrative Unit Layer (GAUL) for admin 0 (country-level) is a single shapefile with each country being a feature in the file.

We will need separate shapefiles for individual countries:

.. code-block:: python

	# Selecting country and exporting into separate shapefile

	import geopandas as gpd
	import matplotlib
	import os
	%matplotlib inline

	dataSrc = gpd.read_file('.\Shapefiles\Global_admin_boundaries\99bfd9e7-bb42-4728-87b5-07f8c8ac631c2020328-1-1vef4ev.lu5nk.shp')
	dataSrc = dataSrc.loc[dataSrc['OBJECTID'] == 25]
	print(dataSrc)
	dataSrc.to_file('Bolivia.shp')


Final Step
---------------

The last step includes integrating the raster and shapefile products, by clipping the points shapefile to the country shapefiles.

.. code-block:: python

	points = #output of previous block
	country = ('country.shp')

	points_clip = gpd.clip(points, country)