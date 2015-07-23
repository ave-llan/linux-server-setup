Update all currently installed packages.


### Goals

- Update all currently installed packages and then remove packages which are no longer necessary.


# Steps


1.  Update the Package Index.

    ```
    sudo apt-get update
    ```


2.  Upgrade the packages.

    ```
    sudo apt-get dist-upgrade
    ```
    In addition to performing the function of `apt-get upgrade`, `apt-get dist-upgrade`will intelligently attempt to upgrade the most important packages at the expense of less important ones. 
    

3.  Remove packages which are no longer needed.

    Clean up by removing previously installed packages that were automatically installed to satisfy dependencies which no longer exist.
    ```
    sudo apt-get autoremove
    ```

4.  If needed, restart the machine.

    You may need to restart the machine if new packages were installed.
    ```
    sudo reboot
    ```


#### References & Credits

[Ubuntu](https://help.ubuntu.com/lts/serverguide/apt-get.html)

[apt-get manual](http://manpages.debian.org/cgi-bin/man.cgi?query=apt-get)

