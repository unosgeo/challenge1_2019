.. _creating_db:

Creating a Spatial Database
===========================

PgAdmin
-------

PostgreSQL has a number of administrative front-ends.  The primary one is `psql <http://www.postgresql.org/docs/current/static/app-psql.html>`_ a command-line tool for entering SQL queries.  Another popular PostgreSQL front-end is the free and open source graphical tool `pgAdmin <http://www.pgadmin.org/>`_. All queries done in pgAdmin can also be done on the command line with ``psql``. 

#. Find pgAdmin and start it up. If it is the first time you are running it, it will ask you to set up a master password, this will allow you to log in to pgAdmin everytime and connect to servers.

   .. image:: ./screenshots/pgadmin_00.png
      :class: inline
      
#. Afterwards it will show you a list of servers. Click on **Servers** to view the active servers. 

   .. image:: ./screenshots/pgadminstart.png
     :class: inline

#. If this is the first time you have run pgAdmin, you should have a server entry for **PostGIS (localhost:5432)** already configured in pgAdmin. Click to display the databases in it.

   The PostGIS database has been installed with unrestricted access for local users (users connecting from the same machine as the database is running). That means that it will accept *any* password you provide. If you need to connect from a remote computer, the password for the ``postgres`` user has been set to ``postgres``.


Creating a Database
-------------------

#. Open the Databases tree item and have a look at the available databases.  The ``postgres`` database is the user database for the default postgres user. It is created in the initialization ``ìnitdb`` as a new PostgreSQL database cluster. A database cluster is a collection of databases that are managed by a single server instance. Creating a database cluster consists of creating the directories in which the database data will live, generating the shared catalog tables (tables that belong to the whole cluster rather than to any particular database), and creating the template1 and postgres databases. When you later create a new database, everything in the template1 database is copied. Therefore, anything installed in template1 is automatically copied into each database created later. The postgres database is a default database meant for use by users, utilities and third party applications.

#. Right-click on the ``Databases`` item and select ``New Database``.

   .. image:: ./screenshots/pgadmin_03.png
     :class: inline

#. Fill in the ``New Database`` form as shown below and click **OK**.  

   .. list-table::

     * - **Name**
       - ``nyc``
     * - **Owner**
       - ``postgres``


   .. image:: ./screenshots/pgadmin_02.png
     :class: inline

#. Select the new ``nyc`` database and open it up to display the tree of objects. Where the parent ``nyc`` is the database. The objects contained within it allow to understand the components of this database like showing the extisting `CAST <https://www.postgresql.org/docs/9.2/sql-createcast.html>`_ operators; `Catalogs <https://www.postgresql.org/docs/9.1/catalogs.html>`_, where a relational database management system stores schema metadata, such as information about tables and columns, and internal bookkeeping information; `Triggers <https://www.postgresql.org/docs/11/plpgsql-trigger.html>`_; list the existing `Extensions <https://www.postgresql.org/docs/11/external-extensions.html>`_ and create new ones through the pgAdmin interface; `Foreign Data Wrappers <https://wiki.postgresql.org/wiki/Foreign_data_wrappers>`_ for handling access to remote objects from external SQL databases; see the installed `Procedural Languages <https://www.postgresql.org/docs/11/xplang.html>`_ to interact with the database; and the `Schemas <https://www.postgresql.org/docs/11/ddl-schemas.html>`_, a database contains one or more named schemas, which in turn contain tables. Once you select the ``public`` schema you'll see the tree of objects.

   .. image:: ./screenshots/pgadmin_04.png

#. Click on the SQL query button indicated below (or go to *Tools > Query Tool*).

   .. image:: ./screenshots/pgadmin_05.png

#. Enter the following query into the query text field to load the PostGIS spatial extension:

   .. code-block:: sql

     CREATE EXTENSION postgis;
           
#. Click the **Execute** button in the toolbar (or press **F5**) to "Execute the query." 

#. Now confirm that PostGIS is installed by running a PostGIS function:

   .. code-block:: sql

     SELECT postgis_full_version();

You have successfully created a PostGIS spatial database!!


Function List
-------------

`PostGIS_Full_Version <http://postgis.net/docs/manual-2.5/PostGIS_Full_Version.html>`_: Reports full PostGIS version and build configuration info.
