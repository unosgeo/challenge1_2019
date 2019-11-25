.. _installation:

Installation
============

PostGIS is an extension for the PostgreSQL database to deal with spatial data, so in order to use it we first need an installation of the database engine. The following instructions will guide you using the PostGIS binary installers available from the official project site: https://postgis.net/install/. There are options for multiple platforms.

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

#. Leave the default directories and complete the installation:

   .. image:: ./screenshots/windows20.PNG
     :class: inline

#. Go to **Start** and look for the PostgreSQL folder where pgAdmin 4 will be located. Open it and set a password to login everytime.

Mac OS X
--------

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
   
Install pgAdmin 4 (Mac OS X)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

Linux (Ubuntu 18.04 / Debian 10 Linux distribution)
---------------------------------------------------

#. An installation of PostgreSQL is required before we can add the PostGIS spatial extension. To begin, first update and upgrade the packages from the terminal, you will need to reboot after this.

  ::
   
    sudo apt update
    sudo apt -y upgrade
    sudo reboot

#. Now, you're ready to add the APT repository to install PostgreSQL:

  ::
  
    sudo apt -y install gnupg2
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
    echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list

#. Verify the repository files content:

  ::
  
    cat /etc/apt/sources.list.d/pgdg.list
    
#. Install PostgreSQL 11:

  ::
  
    sudo apt update
    sudo apt -y install postgresql-11
    
#. After the installation of PostgreSQL, we're ready to proceed to install PostGIS.

  ::
  
    sudo apt update
    sudo apt install postgis postgresql-11-postgis-2.5

#. For installing the web interface PGAdmin 4 enter the following commands:

  ::
    
    sudo apt update
    sudo apt install pgadmin4 pgadmin4-apache2

#. During the installation set a password for the ``postgres`` user. You may use ``postgres`` for local purposes.

   .. image:: ./screenshots/installpgadmin2.png
      :class: inline
      
  #. Test opening the PGAdmin interface. The first time, it will prompt for a master password to use, set one, then enter it to see the servers.
