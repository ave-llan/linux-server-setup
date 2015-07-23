How to configure the Uncomplicated Firewall (UFW) and change the SSH port.


### Goals

- Change the SSH port to be something besides the default port 22. This may reduce the number of attempted attacks on your server.

    See: [Should I change the default SSH port on linux servers?](http://security.stackexchange.com/questions/32308/should-i-change-the-default-ssh-port-on-linux-servers)


# Steps

1.  Change the SSH port.

    
    While logged in as a user with sudo privileges, change your SSH port from the default 22 to a number in [1025, 65536). Make a note of your new port number, or you will be unable to login after making this change.

    Open the sshd configuration file:
    ```
    sudo nano /etc/ssh/sshd_config
    ```
    And change the following line to set your new port number:
    ```
    # What ports, IPs and protocols we listen for
    Port {your-new-port-number}
    ```

    Reload the ssh:
    ```
    reload ssh
    ```

    Now when you try to SSH into the server, you will need to specify the port:
    ```
    ssh {username}@{your-server-ip-address} -i {rsa identity file} -p {your-new-port-number}
    ```


#### References & Credits

Step 1: [Digital Ocean](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-12-04)