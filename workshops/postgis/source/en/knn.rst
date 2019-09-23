.. _knn:

Nearest-Neighbour Searching
===========================

.. note::

  This section refers to a feature that is only available with PostGIS 2.0 and higher.

What is a Nearest Neighbour Search?
-----------------------------------

A frequently posed spatial query is: "what is the nearest <candidate feature> to <query feature>?"

Unlike a distance search, the "nearest neighbour" search doesn't include any measurement restricting how far away candidate geometries might be, features of any distance away will be accepted, as long as they are the *nearest*. This poses a problem for traditional index-assisted queries, that require a search box, and therefore need some kind of measurement value to build the box.

The naive way to carry out a nearest neighbour query is to order the candidate table by distance from the query geometry, and then take the record with the smallest distance:

.. code-block:: sql

  -- Closest street to Broad Street station is Wall St
  SELECT streets.id, streets.name 
  FROM 
    nyc_streets streets, 
    nyc_subway_stations subways
  WHERE subways.name = 'Broad St'
  ORDER BY ST_Distance(streets.geom, subways.geom) ASC
  LIMIT 1;

The trouble with this approach is that it forces the database to calculate the distance between the query geometry and *every* feature in the table of candidate features, then sort them all. For a large table of  candidate features, it is not a reasonable approach.

One way to improve performance is to add an index constraint to the search. This requires a magic number: what's the smallest box we could search around the query geometry, and still come up with at least one candidate geometry? 

If you turn on timing, you can see the performance difference between the box-assisted query below and the simple query above.

.. code-block:: sql

  -- Closest street to Broad Street station is Wall St
  SELECT streets.id, streets.name 
  FROM 
    nyc_streets streets, 
    nyc_subway_stations subways
  WHERE subways.name = 'Broad St'
  AND streets.geom && ST_Expand(subways.geom, 200) -- Magic number: 200m
  ORDER BY ST_Distance(streets.geom, subways.geom) ASC
  LIMIT 1;

The problem with this approach is the magic number of 200 meters. What if there had not happened to be any roads within 200m? We would have failed to come up with a result: there is always a nearest neighbour, it just might not be within 200m.

Index-based KNN
---------------

"KNN" stands for "K nearest neighbours", where "K" is the number of neighbours you are looking for.

KNN is a pure index based nearest neighbour search. By walking up and down the index, the search can find the nearest candidate geometries without using any magical search radius numbers, so the technique is suitable and high performance even for very large tables with highly variable data densities.

.. note:: 

  The KNN feature is only available on PostGIS 2.0 with PostgreSQL 9.1 or greater.

The KNN system works by evaluating distances between bounding boxes inside the PostGIS R-Tree index.

Because the index is built using the bounding boxes of geometries, the distances between any geometries that are not points will be inexact: they will be the distances between the bounding boxes of geometries.

The syntax of the index-based KNN query places a special "index-based distance operator" in the ORDER BY clause of the query, in this case "<->". There are two index-based distance operators, 

* **<->** means "distance between geometries"
* **<#>** means "distance between bounding boxes"

One side of the index-based distance operator must be a literal geometry value. We can get away with a subquery that returns as single geometry, or we could include a :term:`WKT` geometry instead.

.. code-block:: sql

  -- Closest 10 streets to Broad Street station are ?
  SELECT 
    streets.id, 
    streets.name
  FROM 
    nyc_streets streets
  ORDER BY 
    streets.geom <-> 
    (SELECT geom FROM nyc_subway_stations WHERE name = 'Broad St')
  LIMIT 10;

  -- Same query using a geometry EWKT literal

  SELECT ST_AsEWKT(geom)
  FROM nyc_subway_stations 
  WHERE name = 'Broad St';
  -- SRID=26918;POINT(583571 4506714)

  SELECT 
    streets.id, 
    streets.name,
    ST_Distance(
      streets.geom, 
      'SRID=26918;POINT(583571.905921312 4506714.34119218)'::geometry
      ) AS distance
  FROM 
    nyc_streets streets
  ORDER BY 
    streets.geom <-> 
    'SRID=26918;POINT(583571.905921312 4506714.34119218)'::geometry
  LIMIT 10;

::

    id   |    name     |     distance      
  -------+-------------+-------------------
   17394 | Wall St     | 0.714202224374917
   17399 | Broad St    | 0.872022763400183
   17445 | Nassau St   |  1.29928727926582
   17357 | New St      |  63.9499165490674
   17411 | Pine St     |  75.8461038368021
   17367 | Exchange Pl |    101.6241843136
   17322 | Broadway    |  112.049824188021
   17296 | Rector St   |  114.442000781044
   17478 | William St  |  126.934064759446
   17354 | Cedar St    |  133.009278387597

Remember that all the calculations are being done using geometries. Here's what the map looks like for the results of the query:

.. image:: ./screenshots/knn0.png

We can see that the station falls right on the Wall Street line so the **<->** operator computes the distance between geometries giving the proper answer. Moreover, the :command:`ST_Distance` is in alignment with the order provided by the **<->** operator, proving the knn functionality works!

.. note:: 

  For versions of PostgreSQL below 9.5 the **<->** operator would compute distances using the centroid of the bounding boxes of the geometries, producing sometimes approximations that don't match a nearest neighbour search between geometries.

What about the **<#>** operator? If we calculate the distance between box edges, the station would fall **inside** the Wall Street box, giving it a distance of zero and the first entry in the list, right?

.. code-block:: sql

  -- Closest 10 streets to Broad Street station are ?
  SELECT 
    streets.id, 
    streets.name
  FROM 
    nyc_streets streets
  ORDER BY 
    streets.geom <#> 
    'SRID=26918;POINT(583571.905921312 4506714.34119218)'::geometry
  LIMIT 10;

Unfortunately, no.

::

      id   |                               name                               
    -------+------------------------------------------------------------------
     17315 | Pearl St
     17364 | South St
     17394 | Wall St
     17411 | Pine St
     17378 | FDR Dr
     17236 | 
     17241 | West Side Highway; West St; West Side Highway; West Side Highway
     17322 | Broadway
     17382 | FDR Dr
     17399 | Broad St

There are a number of large street features with big boxes that **also** overlap the station and yield a box distance of zero. 

.. image:: ./screenshots/knn3.jpg

This may not give the results we were expecting but since it operates on bounding boxes, it provides better performance than the query using **<->**. In case we had a very large table we could enhance the query processing by limiting our search first using the operator **<#>** and then use the operator **<->** in the resulting subset to get the accurate nearest neighbours:

.. code-block:: sql

    -- "Closest" 100 streets to Broad Street station are?
  WITH closest_candidates AS (
    SELECT 
      streets.id, 
      streets.name,
      streets.geom
    FROM 
      nyc_streets streets
    ORDER BY 
      streets.geom <#> 
      'SRID=26918;POINT(583571.905921312 4506714.34119218)'::geometry
    LIMIT 100
  )
  SELECT id, name
  FROM closest_candidates
  ORDER BY 
    geom <->
      'SRID=26918;POINT(583571.905921312 4506714.34119218)'::geometry
  LIMIT 1;

::

    id   |  name   
  -------+---------
   17394 | Wall St
  
`knn <-> <http://postgis.net/docs/geometry_distance_knn.html>`_: returns the 2D distance between two geometries.

`knn <#> <http://postgis.net/docs/geometry_distance_box.html>`_: returns the distance between two bounding boxes.
