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


#### References & Credits

[Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)


