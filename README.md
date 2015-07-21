How to prepare a baseline installation Linux distribution on a virtual machine to host web applications. 

### Steps

1. Launch your virtual machine and SSH into your server.

    ```
    ssh root@{your-server-ip-address}
    ```


2. Create a New User 
    We want to [disable root login via SSH](http://unix.stackexchange.com/questions/82626/why-is-root-login-via-ssh-so-bad-that-everyone-advises-to-disable-it) so we need to create a new user. 

    ```
    adduser {username}
    ```
    Set a password when prompted.


3. Give `username` permission to sudo.
    Edit the sudoers file by calling:
    ```
    visudo
    ```
    Then find the 'User privilege specification' section and add the following line:
    ```
    {username}   ALL=(ALL:ALL) ALL
    ``` 


4. Configure SSH 
    Open the sshd configuration file:
    ```
    nano /etc/ssh/sshd_config
    ```

    If you would like, change the SSH port to be something besides the default port 22. This [may reduce](http://security.stackexchange.com/questions/32308/should-i-change-the-default-ssh-port-on-linux-servers) the number of attempted attacks on your server.
    ```
    # What ports, IPs and protocols we listen for
    Port {your-new-port-number between 1025 and 65536}
    ```

    Disable root login by changing PermitRootLogin to no:
    ```
    # Authentication:
    PermitRootLogin no
    ```

    TODO add permissions for created user to login with rsa key

    Reload the ssh:
    ```
    reload ssh
    ```


#### Helpful Commands

List all available commands you can run: `compgen -c`


#### References
[Steps 1-4](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-12-04)

