.. Introduction to PostGIS master file.

Introduction to PostGIS
=======================

Getting Started
---------------

* This workshop uses a `data bundle <http://s3.cleverelephant.ca/postgis-workshop-2018.zip>`_. Download and extract this data bundle to a convenient location.
  
* This workshop uses the following software:
  
  * :command:`PostgreSQL` database, along with :command:`PgAdmin` application, and command line tool :command:`psql`.
  * :command:`PostGIS` extension.
  
  To comply with the required software, follow the :ref:`installation` instruction of this workshop, these will guide you while installing PostGIS requirements for different platforms according to the official versions available at the time of making this tutorial.
  
Workshop Materials
------------------

Inside the data bundle, you will find:

**workshop/**
  a directory containing this HTML workshop

**data/**
  a directory containing the shapefiles we will be loading

All the data in the package is public domain and freely redistributable. All the software in the package is open source, and freely redistributable. This workshop is licensed as Creative Commons "`share alike with attribution <http://creativecommons.org/licenses/by-sa/3.0/us/>`_", and is freely redistributable under the terms of that license.

Workshop Modules
----------------

.. toctree::
   :maxdepth: 1
   :numbered:

   welcome
   introduction
   setup
   basic
   advanced
   maintenance

   postgis-functions
   glossary
   license


Links to have on hand
---------------------

* PostGIS - http://postgis.net/

  - Docs - http://postgis.net/docs/

* PostgreSQL - http://www.postgresql.org/

  - Docs - http://www.postgresql.org/docs/
  - Downloads - http://www.postgresql.org/download/
  - JDBC Driver - http://jdbc.postgresql.org/
  - .Net Driver - http://npgsql.projects.postgresql.org/
  - Python Driver - http://www.pygresql.org/
  - C/C++ Driver - http://www.postgresql.org/docs/10/static/libpq.html

* PgAdmin III - http://www.pgadmin.org/

* Open Source Desktop Clients

  - QGIS - http://qgis.org/
  - OpenJUMP - http://openjump.org/
  - uDig - http://udig.refractions.net/
