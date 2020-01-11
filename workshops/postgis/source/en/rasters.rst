.. _working_with_rasters:

In this section you will learn to load a raster, get basic information on the raster, process and analyze it.

Before going further, we should describe what a raster is and what a raster is used for. At the simplest level, a raster is a photo or image with information describing where to place the raster on the Earth's surface. A photograph typically has three sets of values, one set for each primary color (red, green, and blue). A raster also has sets of values, often more than those found in a photograph. Each set of values is known as a band. So, a photograph typically has three bands while a raster has at least one band. As with digital photographs, rasters come in a variety of file formats. Common raster formats you may come across include PNG, JPEG, GeoTIFF, HDF5, and NetCDF. Since rasters can have many bands and even more values, they can be used to store large quantities of data in an efficient manner. Due to their efficiency, rasters are used for satellite and aerial sensors and modeled surfaces, such as weather forecasts.

Some definitions to consider:

#. raster is the PostGIS data type for storing the raster files in PostgreSQL.
#. Tile: This is a small chunk of the original raster file to be stored in one column of a table's row. Each tile has its own set of spatial information and thus is independent of all the other tiles in the same column of the same table, even if the other tiles are from the same original raster file.

For this section let's create a new schema where we will keep the objects for working with rasters. On your PGAdmin *query editor* write:

.. code-block:: sql

   CREATE SCHEMA rasters;
   
The data that we will use in this section is world climate data for the period of 1970-2000 provided by `worldclim <http://worldclim.org/version2>`_ but you can find it in the data folder **raster** of this tutorial.

#. First, let's inspect the ``wc2.0_10m_tmax_01.tif`` raster file using GDAL, you can install GDAL for unix/MAC using the binaries from `this site <https://sandbox.idre.ucla.edu/sandbox/general/how-to-install-and-run-gdal>`_ or using the `OSGeoW <https://trac.osgeo.org/osgeo4w/>`_ suite for Windows, which will provide all the packages you need. You can also open the raster with QGIS and inspect its metadata.

::

  Driver: GTiff/GeoTIFF
  Files: wc2.0_10m_tmax_01.tif
  Size is 2160, 1080
  Coordinate System is:
  GEOGCS["WGS 84",
      DATUM["WGS_1984",
          SPHEROID["WGS 84",6378137,298.257223563,
              AUTHORITY["EPSG","7030"]],
          AUTHORITY["EPSG","6326"]],
      PRIMEM["Greenwich",0],
      UNIT["degree",0.0174532925199433],
      AUTHORITY["EPSG","4326"]]
  Origin = (-180.000000000000000,90.000000000000000)
  Pixel Size = (0.166666666666667,-0.166666666666667)
  Metadata:
    AREA_OR_POINT=Area
  Image Structure Metadata:
    COMPRESSION=DEFLATE
    INTERLEAVE=BAND
  Corner Coordinates:
  Upper Left  (-180.0000000,  90.0000000) (180d 0' 0.00"W, 90d 0' 0.00"N)
  Lower Left  (-180.0000000, -90.0000000) (180d 0' 0.00"W, 90d 0' 0.00"S)
  Upper Right ( 180.0000000,  90.0000000) (180d 0' 0.00"E, 90d 0' 0.00"N)
  Lower Right ( 180.0000000, -90.0000000) (180d 0' 0.00"E, 90d 0' 0.00"S)
  Center      (   0.0000000,   0.0000000) (  0d 0' 0.01"E,  0d 0' 0.01"N)
  Band 1 Block=2160x1 Type=Float32, ColorInterp=Gray
    Min=-39.708 Max=42.235 
    Minimum=-39.708, Maximum=42.235, Mean=1.000, StdDev=1.000
    NoData Value=-3.39999999999999996e+38
    Metadata:
      STATISTICS_MAXIMUM=42.234750080109
      STATISTICS_MEAN=1.#SNAN
      STATISTICS_MINIMUM=-39.707750263214
      STATISTICS_STDDEV=1.#SNAN
      
#. From this information we know the coodinate system of the raster, the limits, pixel size, and some statistics on the values it contains.

#. Now we're ready to load the raster into our database using ``raster2pgsql``.   raster2pgsql -s 4326 -t 100x100 -F -I-C -Y wc2.0_10m_tmax_*.tif rasters.worldclim | psql -d postgis
