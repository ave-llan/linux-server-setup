How to install and configure [PostgreSQL](http://www.postgresql.org/) to work with [SQLAlchemy](http://www.sqlalchemy.org/). 

### Goals

- Install PostgreSQL and SQLAlchemy.

- Create a psql user with limited privileges that will interface with the database.

- Configure an SQLAlchemy engine to interface with the database.

# Steps


1.  Install PostgreSQL and SQLAlchemy.
 
    After updating the package index, install `postgresql` and typically used add-ons with `postgresql-contrib`. Then install SQLAlchemy for python with the `python-sqlalchemy` package. 
    ```
    sudo apt-get update
    sudo apt-get install postgresql postgresql-contrib
    sudo apt-get install python-sqlalchemy
    ```


2.  Install and enable mod_wsgi.

    To install:
    ```
    sudo apt-get install libapache2-mod-wsgi python-dev
    ```
    
    To enable mod_wsgi:
    ```
    sudo a2enmod wsgi
    ```


#### References & Credits

Step 1: [Ubuntu](https://help.ubuntu.com/community/PostgreSQL)


