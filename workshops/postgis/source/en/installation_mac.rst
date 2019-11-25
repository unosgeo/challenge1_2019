.. _installation_mac:

Mac OS X
========

For OSX users it is possile to install PostGIS along with PostgreSQL using the Postgres app: http://postgresapp.com/. This is the easiest way to do the installation but you can also use the EnterpriseDb installer: https://www.enterprisedb.com/downloads/postgres-postgresql-downloads or the homebrew installer if you are familiar with it by running the command: ``brew install postgis``.

The following steps will guide you in the installation using Postgres.app:

#. Download the dmg file: https://postgresapp.com/downloads.html.

#. Drag the Postgres icon into your applications folder.

  .. image:: ./screenshots/installosx1.png
   :class: inline

#. Double click the application and allow it to be opened:

  .. image:: ./screenshots/installosx2.png
   :class: inline

#. Click initialize to start the process.

   .. image:: ./screenshots/installosx3.png
      :class: inline
   
#. If successful, you will see the databases available to connect to, this means that the database engine is up and running. By clicking the icons of the databases you will be prompted to a command line but for this course, the user interface pgAdmin 4 will be preferred.

   .. image:: ./screenshots/installosx4.png
      :class: inline
   
Install pgAdmin 4
-----------------

#. Go to: https://www.pgadmin.org/download/ to get pgAdmin 4 for **Mac OS X**. Double click the installer and agree to the terms by clicking **Agree**.

   .. image:: ./screenshots/installpgadmin1.png
     :class: inline
     
#. For **Mac OS X** drag the pgAdmin 4 icon to your applications folder.

   .. image:: ./screenshots/installpgadmin2.png
      :class: inline
 
#. Open the installed application and allow it to run:

   .. image:: ./screenshots/installpgadmin3.png
      :class: inline
   
#. pgAdmin 4 is web-based so a tab will open in your browser window. The first time, it will prompt for a master password to use, set one, then enter it to see the servers.
