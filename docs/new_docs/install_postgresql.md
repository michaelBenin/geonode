Installation of Postgresql-9.2 on Ubuntu 12.04 
==============================================

Installation
------------

The Installation of Postgresql-9.2 on Ubuntu 12.04 includes two main steps (Generelly, this installation should work for each version of Postgresql!). 



*Step 1*

Create an empty file called *pgdg.list* placed in /etc/apt/sources.list.d/, using these commands::

	cd /etc/apt/sources.list.d/
	
	sudo touch pgdg.list
  
Now the following line has to be included the *pgdg.list* file.

	deb http://apt.postgresql.org/pub/repos/apt/ codename-pgdg main
	
Instead of *codename* write the name of your system. If you do not know this, type

	 lsb_release -c

This will return you the name of your system.

.. warning::
Make sure that you have changed *codename* to the name of your system before you copy the link in the next step!

Now open the *pgdg.list* file using e.g.::

	gksu gedit pdgd.list
	
and copy deb `http://apt.postgresql.org/pub/repos/apt/ codename-pgdg main` into it.


*Step 2*

Now you have to import the repository key from their website. Use the following command to do so:

	wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | sudo apt-key add -

After that you have to update the package lists using

	sudo apt-get update
	
and then you can install postgresql-9.2 and pgadmin3 (if needed)

	sudo apt-get install postgresql-9.2 pgadmin3


Configuration
-------------

**General**

*Step 1 – Change password*

First of all you have to set a new password for the superuser *postgres*. In order to do so, type the following command line into your terminal:

	sudo -u postgres psql postgres
	
Now you are in *psql*, the command interface for postgresql, and in the database *postgres*. In your terminal it looks like this at the moment:

	postgres=#

To change your password, type

	\password postgres
	
and type your new password when asked for it.



*Step 2 – Create a database* (for testing)

If you want to create a db, posgresql has to know, which user you are. Therefore you have to type `-u username` in front of the command `createdb`. If you type the following, it means that you as the user *postgres* want to create a database wich is called *mydb*.

	sudo -u postgres createdb mydb


**For geonode**

*Remark*: If you want to use the *postgis extension* for *postgresql* then you may follow the instructions from this site .... .
If you do not need that (from the beginning) you can follow this instruction. You can even add the *posgis extension* later following the steps from the section *Alternative*. Anyway it is recommended to do the common version. 


*Step 1 – Create a new user (geonode)*

In order to use postgresql in geonode, you have to create a new user *geonode*, which is the owner of a new database, also called *geonode*. So first create a new user by typing

	sudo -u postgres createuser -P geonode

This means, that you as the user *postgres* create a new user called *geonode*. `-P` indicates, that this user will have a password (*geonode*). 



*Step 2 - Create new database*

After creating the new user you can create the db *geonode*:

	sudo -u postgres createdb -O geonode geonode
	
	
