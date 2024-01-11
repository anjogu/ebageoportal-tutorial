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