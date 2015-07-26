### Goals

- Install and set up Fail2ban.

# Steps


1.  Install Fail2ban.
 
    After updating the package index, install Git.
    ```
    sudo apt-get update
    sudo apt-get install fail2ban
    ```

2. Configure the jail.

    Make a 'local' copy of the jail.conf file.
    
    ```
    cd /etc/fail2ban
    sudo cp jail.conf jail.local 
    ```

    To avoid locking yourself out, add [your ip address](https://www.whatismyip.com/) to the set you want fail2ban to ignore.  You can also customize the bantime (in seconds) and the maximum number of user attempts before banning.
    ```
    ignoreip = [your ip address]
    bantime = 1200
    maxretry = 3
    ```

3. Restart fail2ban to put the new settings into effect.

    ```
    sudo /etc/init.d/fail2ban restart
    ```

#### References & Credits

[Ubuntu](https://help.ubuntu.com/community/Fail2ban)


