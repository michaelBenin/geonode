PostGis2.0.3 Installation on Ubuntu 12.04
=========================================

**Step 1 – Install dependencies**

Before you can install PostGis 2.0.3, some dependencies have to be installed first. You can do this by using the Linux command *apt-get*. It is recommended to install postgresql-9.2 before you install PostGis 2.0.3. For the installation guide of postgresql-9.2 see ... .

Now the first step is to type the following command into your terminal to install all dependencies:

    sudo apt-get install build-essential postgresql-server-dev-9.2 libxml2-dev libproj-dev libjson0-dev xsltproc docbook-xsl docbook-mathml libgdal1-dev

*Remark*: libgdal1-dev is needed for raster support and is required if you want to build the postgresql extension!



**Step 2 – Build Geos 3.3.2 or higher**

GEOS is used for the topology support and because Postgis 2.0 requires a GEOS version 3.3.2 or higher, you have to build this before you can install postgis itself. (genereally Ubuntu comes with an GEOS version lower than 3.3.2!) Download your favourite version of geos (has to be 3.3.2 or higher!) using the following command line:

    wget http://download.osgeo.org/geos/geos-3.3.8.tar.bz2

Unpack it and go to the unpacked folder::

    tar xvfj geos-3.3.8.tar.bz2
    cd geos-3.3.8

Now you can install geos by using the following command lines (this process may take a while)

    ./configure
    make
    sudo make install
    cd ..


**Step 3 – Install postgis**

The following steps are almost the same like instructed in *Step 2*. Download postgis 2.0.3, unpack it and go to unpacked folder.

    wget http://download.osgeo.org/postgis/source/postgis-2.0.3.tar.gz
    tar xfvz postgis-2.0.3.tar.gz
    cd postgis-2.0.3

Now postgis can be installed:

    ./configure
    make
    sudo make install
    sudo ldconfig
    sudo make comments-install

*Remark*: PostGIS 2.0.3 can be configured to disable topology or raster components using the configure flags

    --without-raster

and/or

    --without-topology.

The default is to build both. Note that the raster component is required for the extension installation method for postgresql!



Create the postgis extension for postgresql
-------------------------------------------

**Common way**

The best way to install the postgis extension is by using templates. After downloading and installing postgresql and postgis you create a database called template_postigsxxx (xxx should be replaced by your version of postgis; in this case postgis 2.0.3 was used).

    sudo -u postgres createdb template_postgis203

Before installing the extension you have to log in to the database

    psql template_postgis203

and now you can create the extension

    create extension postigs;

*Remark*: Do not forget the semicolon at the end! Otherwise this statement will have no effect!

Now you can easily create a new database wich automatically has the postgis extension as well, e.g create a new database called geonode like this

    sudo -u postgres createdb -T template_postgis203 geonode

(Remark: -T stands for template)



**Alternative**

If you already created a db called *geonode*, then you can use this alternative to create the extension for postigs.

By typing

    sudo su postgres

you log in as the user *postgres*.

By using the following command line

    psql geonode

you log in to the database *geonode*.

To install the posgis extension type

    create extension postgis;

Now you should have successfully installed the postgis extension in your geonode database.
