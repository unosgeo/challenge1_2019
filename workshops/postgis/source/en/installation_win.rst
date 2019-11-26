.. _installation_win:

Windows
-------

For Windows, there is the EnterpriseDB PostgreSQL distributions: https://www.postgresql.org/download/windows/, an interactive installer that will provide the option for installing PostGIS in an installation dialog with "Stack Builder":

#. First install PostgreSQL with the EDB installer:

   .. image:: ./screenshots/windows01.PNG
      :class: inline

#. Follow the installation process leaving the directories as they are by default and make sure all components (postgreSQL Server, pgAdmin 4, Stack Builder and Command Line Tools) are selected to install.

   .. image:: ./screenshots/windows03.PNG
      :class: inline

#. Set a password that you will use later for login. You may use ``postgres``.

   .. image:: ./screenshots/windows05.PNG
      :class: inline

#. Leave the default port to 5432:

   .. image:: ./screenshots/windows06.PNG
      :class: inline

#. At the end of the PostgreSQL installation check the option to launch Stack Builder. This will continue to install PostGIS.

   .. image:: ./screenshots/windows09.PNG
     :class: inline

#. Select the recent PostgreSQL installation:

   .. image:: ./screenshots/windows10.PNG
     :class: inline
     
#. Search for PostGIS under **Spatial Extensions** and check it.
     
   .. image:: ./screenshots/windows11.PNG
     :class: inline
     
#. Follow the installation process and click on **I Agree** in the License Agreement step:

   .. image:: ./screenshots/windows14.PNG
     :class: inline

#. Choose the PostGIS component:

   .. image:: ./screenshots/windows15.PNG
     :class: inline

#. Leave the default directories, **agree to registering the GDAL_DATA environment variable, enable raster drivers, and complete** the installation:

   .. image:: ./screenshots/windows20.PNG
     :class: inline

#. Go to **Start** and look for the PostgreSQL folder where pgAdmin 4 will be located. Open it and set a password to login everytime.
