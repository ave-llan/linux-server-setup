How to disable root login via SSH and prepare a user account with sudo privileges.

### Goals

- Disable root login. 
    
    See: [Why is root login via SSH so bad that everyone advises to disable it?](http://unix.stackexchange.com/questions/82626/why-is-root-login-via-ssh-so-bad-that-everyone-advises-to-disable-it)

- Create a user account with permission to sudo since you won't be able to SSH in as root.

- Allow your user account to login with the same RSA key that root uses.

- Do not require a password in addition to the RSA key for this user account.


### Assumptions

- Your cloud provider has provided you with an RSA key (as is the case with AWS) for initial access.

# Steps

1. Connect to your virtual machine using SSH.

    ```
    ssh root@{your-server-ip-address} -i {rsa identity file}
    ```


2. Create a new user.

    ```
    adduser {username}
    ```
    Set a password when prompted.


3. Give `{username}` permission to sudo.
    Edit the sudoers file by calling:
    ```
    visudo
    ```
    Then find the 'User privilege specification' section and add the following line:
    ```
    {username}   ALL=(ALL:ALL) NOPASSWD:ALL
    ``` 
    This will give the user sudo privileges without asking for their password. If you would like to require a password, instead set this line to:
    ```
    {username}   ALL=(ALL:ALL) ALL
    ```


4. Configure the RSA key to work for the new user account.

    Create a .ssh directory in {username}'s home folder. One way to do this is by logging in as {username} and attempting to SSH into the server. This will fail but the SSH client will create a .ssh directory with the appropriate permissions.

    ```
    login {username}
    ssh localhost
    ```

    The ssh request will fail, but there will now be a .ssh directory in the user's home folder. 

    Logout of the user's account, copy the root authroized_keys to the users .ssh directory, and set the user as the owner of the copied file.

    ```
    logout
    cp /root/.ssh/authorized_keys /home/{username}/.ssh/
    chown {username}:{users-default-groupname} /home/{username}/.ssh/authorized_keys
    ```

    Verify owner permission's are limited to read and write for the user.


5. Check that logging in with the user account via SSH and RSA key works.
    
    In a new shell, try to log in with the user account.
    ```
    ssh {username}@{your-server-ip-address} -i {rsa identity file}
    ```

    Once inside, check to make sure the user can sudo. 
    ```
    sudo ls
    ```
    If this works, you are ready to disable root login.


6. Disable root login via SSH. 

    While logged in as the newly created user, open the sshd configuration file:
    ```
    sudo nano /etc/ssh/sshd_config
    ```

    Disable root login by changing PermitRootLogin to no:
    ```
    # Authentication:
    PermitRootLogin no
    ```

    Reload the SSH:
    ```
    reload ssh
    ```


#### References & Credits

Steps 1-3, 6: [Digital Ocean](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-12-04)

Step 4: [Carlos Camargo](https://www.linkedin.com/in/vcarlos)
