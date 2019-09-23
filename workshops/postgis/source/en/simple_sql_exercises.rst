.. _simple_sql_exercises:

Simple SQL Exercises
====================

Using the ``nyc_census_blocks`` table, answer the following questions (don't peak at the answers!).

Here is some helpful information to get started.  Recall from the :ref:`About Our Data <about_data>` section our ``nyc_census_blocks`` table definition.

.. list-table::
  :widths: 20 80

  * - **blkid**
    - A 15-digit code that uniquely identifies every census **block**. ("360050001009000")
  * - **popn_total**
    - Total number of people in the census block
  * - **popn_white**
    - Number of people self-identifying as "white" in the block
  * - **popn_black**
    - Number of people self-identifying as "black" in the block
  * - **popn_nativ**
    - Number of people self-identifying as "native american" in the block
  * - **popn_asian**
    - Number of people self-identifying as "asias" in the block
  * - **popn_other**
    - Number of people self-identifying with other categories in the block
  * - **hous_total**
    - Number of housing units in the block
  * - **hous_own**
    - Number of owner-occupied housing units in the block
  * - **hous_rent**
    - Number of renter-occupied housing units in the block
  * - **boroname**
    - Name of the New York borough. Manhattan, The Bronx, Brooklyn, Staten Island, Queens
  * - **geom**
    - Polygon boundary of the block

And, here are some common SQL aggregation functions you might find useful:

* avg() - the average (mean) of the values in a set of records
* sum() - the sum of the values in a set of records
* count() - the number of records in a set of records

Now the questions:

* **"What is the geometry value for the street named 'Atlantic Commons'?"**

  .. code-block:: sql

    SELECT ST_AsText(geom)
      FROM nyc_streets
      WHERE name = 'Atlantic Commons';

  ::

    MULTILINESTRING((586781.701577724 4504202.15314339,586863.51964484 4504215.9881701))

* **"What neighborhood and borough is Atlantic Commons in?"**

  .. code-block:: sql

    SELECT name, boroname
    FROM nyc_neighborhoods
    WHERE ST_Intersects(
      geom,
      ST_GeomFromText('LINESTRING(586782 4504202,586864 4504216)', 26918)
    );

  ::

        name    | boroname
    ------------+----------
     Fort Green | Brooklyn

  .. note::

    "Hey, why did you change from a 'MULTILINESTRING' to a 'LINESTRING'?" Spatially they describe the same shape, so going from a single-item multi-geometry to a singleton saves a few keystrokes.

    More importantly, we also rounded the coordinates to make them easier to read, which does actually change results: we couldn't use the ST_Touches() predicate to find out which roads join Atlantic Commons, because the coordinates are not exactly the same anymore.


* **"What streets does Atlantic Commons join with?"**

  .. code-block:: sql

    SELECT name
    FROM nyc_streets
    WHERE ST_DWithin(
      geom,
      ST_GeomFromText('LINESTRING(586782 4504202,586864 4504216)', 26918),
      0.1
    );

  ::

           name
      ------------------
       Cumberland St
       Atlantic Commons

  .. image:: ./spatial_relationships/atlantic_commons.jpg


* **"Approximately how many people live on (within 50 meters of) Atlantic Commons?"**

  .. code-block:: sql

    SELECT Sum(popn_total)
      FROM nyc_census_blocks
      WHERE ST_DWithin(
       geom,
       ST_GeomFromText('LINESTRING(586782 4504202,586864 4504216)', 26918),
       50
      );

  ::

    1438
 
Function List
-------------

`avg(expression) <http://www.postgresql.org/docs/current/static/functions-aggregate.html#FUNCTIONS-AGGREGATE-TABLE>`_: PostgreSQL aggregate function that returns the average value of a numeric column.

`count(expression) <http://www.postgresql.org/docs/current/static/functions-aggregate.html#FUNCTIONS-AGGREGATE-TABLE>`_: PostgreSQL aggregate function that returns the number of records in a set of records.

`sum(expression) <http://www.postgresql.org/docs/current/static/functions-aggregate.html#FUNCTIONS-AGGREGATE-TABLE>`_: PostgreSQL aggregate function that returns the sum of records in a set of records.
