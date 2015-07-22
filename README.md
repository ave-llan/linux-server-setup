How to prepare a baseline installation Linux distribution on a virtual machine to host web applications.

### Assumptions

- Your cloud provider has provided you with an rsa key (as is the case with AWS).

### Steps

1. Connect to your virtual machine using SSH.

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
    {username}   ALL=(ALL:ALL) NOPASSWD:ALL
    ``` 

4. Configure rsa key for the user

    Create .ssh directory in user's home folder. One way to do this is by logging in as the user and attempting to SSH into the server. This will fail but the SSH client will create a .ssh directory with the appropriate permissions.

    ```
    login {username}
    ssh localhost

    ```

    The ssh request will fail, but there will now be a .ssh directory in the user's home folder. 

    Logout of the user's account, copy the root authroized_keys to the users .ssh directory, and make the user the owner of the copied file.

    ```
    logout
    cp /root/.ssh/authorized_keys /home/{username}/.ssh/
    chown {username}:{users-default-groupname} /home/{username}/.ssh/authorized_keys
    ```

    Verify owner permission's are limited to read and write for the user


5. Configure SSH 

    Open the sshd configuration file:
    ```
    nano /etc/ssh/sshd_config
    ```

    If you would like, change the SSH port to be something besides the default port 22. This [may reduce](http://security.stackexchange.com/questions/32308/should-i-change-the-default-ssh-port-on-linux-servers) the number of attempted attacks on your server.
    ```
    # What ports, IPs and protocols we listen for
    Port {your-new-port-number in [1025, 65536)}
    ```

    Disable root login by changing PermitRootLogin to no:
    ```
    # Authentication:
    PermitRootLogin no
    ```


    Reload the ssh:
    ```
    reload ssh
    ```


#### Helpful Commands

List all available commands you can run: `compgen -c`


#### References
[Steps 1-4](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-12-04)

