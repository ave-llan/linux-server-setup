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


2. Create a PostgreSQL role (user) with limited permissions to access your databases.

    `postgres` is the default superuser for PostgreSQL, so we will need to run the `createuser` command as postgres using `sudo -u postgres`.
    Then call `createuser` with the following flags:
    - `-D` will allow the user to create databases. 
    - `-P` will issue a prompt to create a password.
    ```
    sudo -u postgres createuser {username} -D -P
    ```

3. Create a database and give ownership to your newly created user role. 

4. Require a password from the newly created user.

    Open the configuration file.
    ```
    sudo nano /etc/postgresql/9.3/main/pg_hba.conf
    ```

    Scroll down and add a record for the newly created user with its method set to `md5` which is a double-hashed password. 

    ```
    # TYPE  DATABASE        USER            ADDRESS                 METHOD
    local   all             {username}                              md5
    ```

#### References & Credits

Step 1: [Ubuntu](https://help.ubuntu.com/community/PostgreSQL)
Step 2: [PostgreSQL: app-createuser](http://www.postgresql.org/docs/9.4/static/app-createuser.html)
Step 4: [PostgreSQL: auth-pg-hba-conf](http://www.postgresql.org/docs/9.4/static/auth-pg-hba-conf.html)


