Linux (Ubuntu 18.04 / Debian 10 Linux distribution)
===================================================

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

   .. image:: ./screenshots/pgadmin_linux1.png
      :class: inline
      
#. Test opening the PGAdmin interface. The first time, it will prompt for a master password to use, set one, then enter it to see the servers.
