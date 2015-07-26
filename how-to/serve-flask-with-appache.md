### Goals

- Install Apache and configure it to serve a Python mod_wsgi application.

- Install Flask
# Steps


1.  Install Apache.
 
    After updating the package index, install Apache.
    ```
    sudo apt-get update
    sudo apt-get install apache2
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

3.  Create a directory for the Flask application.

    Move to the `/var/www` directory and create a directory for your application.
    ```
    cd /var/www
    sudo mkdir {your-app-name}
    ```

4.  Clone your Flask application into the new directory. 

    Move inside your app directory and [clone](/how-to/install-git-clone-repository.md) in your Flask application from Github. 
    ```
    cd {your-app-name}
    sudo git clone {your repository clone URL}
    ```

    Rename the directory created by git to `FlaskApp`.
    ```
    sudo mv /{git-repository-name} FlaskApp
    ```

5.  Install Flask and SQLAlchemy inside a virtual environment.

    If pip is not installed, add it now. 
    ```
    sudo apt-get install python-pip
    ```

    Install *virtualenv*.
    ```
    sudo pip install virtualenv
    ```

    Create a virtual environment inside your `FlaskApp` directory.
    ```
    cd FlaskApp
    sudo virtualenv venv
    ```

    Activate the virtual environment and install Flask and SQLAlchemyinside.
    ```
    source venv/bin/activate 
    sudo pip install Flask
    sudo pip install flask-sqlalchemy
    ```

6.  Configure files as needed and test if the installation was successful.
    
    Make sure your database conneciton Url matches the username and password you set up in [installing and configuring PostgreSQL to work with SQLAlchemy](how-to/install-postgres-and-configure.md).

    If your modules reference other files, you will need to alter filepaths.  The path to your FlaskApp directory will be: `/var/www/{your-app-name}/FlaskApp`. 

    Test if the installation is successful and the app can run:
    ```
    sudo python __init__.py
    ```
    If it says `Running on http://0.0.0.0:5000/`, your installation was successful.

7. Configure and enable a new virtual host with Apache.

    Create a configuration file for Apache:
    ```
    sudo nano /etc/apache2/sites-available/FlaskApp.conf
    ```

    And input the configuration:
    ```
    <VirtualHost *:80>
        ServerName {mywebsite.com OR IP address}
        ServerAdmin {admin@mywebsite.com}
        WSGIScriptAlias / /var/www/{your-app-name}/flaskapp.wsgi
        <Directory /var/www/{your-app-name}/FlaskApp/>
            Order allow,deny
            Allow from all
        </Directory>
        Alias /static /var/www/{your-app-name}/FlaskApp/static
        <Directory /var/www/{your-app-name}/FlaskApp/static/>
            Order allow,deny
            Allow from all
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        LogLevel warn
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    ```

    Enable the virtual host.
    ```
    sudo a2ensite FlaskApp
    ```

8. Create a .wsgi file.
    
    Apache uses a .wsgi file to serve the Flask application. Create the file:
    ```
    cd /var/www/{your-app-name}
    sudo nano flaskapp.wsgi
    ```

    Input the configuration:
    ```
    #!/usr/bin/python
    import sys
    import logging
    logging.basicConfig(stream=sys.stderr)
    sys.path.insert(0,"/var/www/{your-app-name}/")

    from FlaskApp import app as application
    application.secret_key = {Add your secret key}
    ```

9. Restart Apache.
    
    ```
    sudo service apache2 restart
    ```
    Open your browser and navigate to the domain name or IP address you entered in the `FlaskApp.conf` configuration file. If all is well, you will see your app.

#### References & Credits

[Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)


