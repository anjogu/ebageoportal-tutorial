Installation
=====

.. toctree::
   :maxdepth: 2
   :caption: Contents:

------------
Recommended Minimum System Requirements
------------


For efficient deployment of the Geoportal, the following bare minimum system requirements need to be satisfied.


• 8GB RAM, (16GB or more preferred for a production deployment)

• 2.2GHz processor with 4 cores. (Additional processing power may be required for multiple concurrent styling renderings)

• 30 GB software disk usage (Reserved to OS and source code only).

• 64 bit architecture is strongly recommended.

NB: 
- Additional disk space for any data hosted with GeoNode, data stored on the DataBase and tiles cached with GeoWebCache.
- For db, spatial data, cached tiles, and “scratch space” useful for administration, a decent baseline size for GeoNode deployments is between 50GB and 100GB but can vary depending on the amount of data one may have.


.. toctree::
   :maxdepth: 2
   :caption: Contents:

------------
Installing on Ubuntu 22.04
------------
This part of the documentation describes the complete setup process for GeoNode on an Ubuntu 22.04LTS 64-bit clean environment (Desktop or Server)

We will be using shell commands/ a remote shell.


.. toctree::
   :maxdepth: 3
   :caption: Contents:

------------
Install the dependancies
------------

In this section we are going to install the basic packages and tools needed for the complete Geoportal installation.

.. toctree::
   :maxdepth: 4
   :caption: Contents:

------------
Upgrade your system packages
------------

Check that your system is already up-to-date with the repository running the following commands:

.. code-block:: console

    sudo add-apt-repository ppa:ubuntugis/ubuntugis-unstable
    sudo apt update -y; sudo apt upgrade -y;


.. toctree::
   :maxdepth: 4
   :caption: Contents:

------------
Packages Installation
------------
First, we are going to install all the system packages needed for the GeoNode setup. Login to the target machine and execute the following commands:


.. code-block:: console
    
    # Install packages from GeoNode core
    sudo apt install -y build-essential gdal-bin \
    python3.10-dev \
    libxml2 libxml2-dev gettext \
    libxslt1-dev libjpeg-dev libpng-dev libpq-dev libgdal-dev \
    software-properties-common build-essential \
    git unzip gcc zlib1g-dev libgeos-dev libproj-dev \
    python3-pip
    
    #sqlite3 spatialite-bin libsqlite3-mod-spatialite libsqlite3-dev

    # Install Openjdk
    sudo apt install openjdk-11-jdk-headless default-jdk-headless -y
    sudo update-java-alternatives --jre-headless --jre --set java-1.11.0-openjdk-arm64

    # Verify GDAL version
    gdalinfo --version
      $> GDAL 3.4.1, released 2021/12/27
      

    # Verify Python version
    python3.10 --version
    $> Python 3.10.12

    which python3.10
    $> /usr/bin/python3.10

    # Verify Java version
    java -version
        $> openjdk version "11.0.21" 2023-10-17
        $> OpenJDK Runtime Environment (build 11.0.21+9-post-Ubuntu-0ubuntu122.04)
        $> OpenJDK 64-Bit Server VM (build 11.0.21+9-post-Ubuntu-0ubuntu122.04, mixed mode)

    # Install VIM
    sudo apt install -y vim

    # Cleanup the packages
    sudo apt update -y; sudo apt upgrade -y; sudo apt autoremove --purge
    
    # install Django==3.2.16
    sudo pip install Django==2.2.12

    # Clone the geoNode-project source code on /opt/django_dev
    cd /opt/django_dev/; git clone https://github.com/GeoNode/geonode-project.git -b 3.3.x
    
    # Generate the project

     django-admin startproject --template=./geonode-project -e py,sh,md,rst,json,yml,ini,env,sample,properties -n monitoring-cron -n Dockerfile eba_bare

    # Install the Python packages
    cd /opt/django_dev/ebageoportal
    cd src
    sudo pip install -r requirements.txt --upgrade
    
    sudo pip install -e . --upgrade

    # Install Gdal utilities
    sudo pip install pygdal=="`gdal-config --version`.*"


.. toctree::
   :maxdepth: 4
   :caption: Contents:

------------
Web Server setup(Apache) 
------------

To deploy the geoportal application on a production environment, one can set it up using docker or as a bare metal setup. For the purpose of this documentation, we will set up the geoportal in a bare metal setup. To do this, we will configure a webserver to run our application. We are going to use Apache webserver. 
To set up and configure Apache, we are going to run the commands below. To check if Apache has been set up on your server, run the code below

.. code-block:: console
   sudo service apache2 status

   #if you get the message below, then apache has not been installed
   >Unit apache2.service could not be found.

Now to install Apache webserver, runn the following commands;


.. code-block:: console
   sudo apt update; sudo apt upgrade -y; sudo apt autoremove --purge; 

   then,

   sudo apt install -y gcc apache2 libapache2-mod-wsgi-py3 libgeos-dev  libjpeg-dev libpng-dev libpq-dev libproj-dev libxml2-dev libxslt1-dev

   # copy .conf file from the scripts directory to the apache sites-available directory
   sudo cp /opt/django_dev/eba_geoportalv2/src/scripts/misc/apache2/geonode.conf.sample /etc/apache2/sites-available/eba.conf

   # activate proxy and wsgi files
   sudo a2enmod proxy; sudo a2enmod proxy_http; sudo a2enmod proxy_ajp; sudo a2enmod wsgi
   sudo a2dissite 000-default.conf; sudo a2ensite eba.conf



   # environment setup
   python3.10 -m venv ~/.virtualenvs/envebadev

   workon envebadev

   