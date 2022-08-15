# Bandit Level 13-14@overthewire.org

### Description
The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. **Note:** **localhost** is a hostname that refers to the machine you are working on.

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit13
Password:                  | 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL ssh -p 2220 bandit13@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        Learn how to use a `private key` with `ssh`. To connect to the host, you will need to use a private ssh key. 




??? done "Solution"


    ``` bash hl_lines="8"
    bandit13@bandit:~$ ls -Al  
    total 16  
    -rw-r--r-- 1 root     root      220 May 15  2017 .bash_logout  
    -rw-r--r-- 1 root     root     3526 May 15  2017 .bashrc  
    -rw-r--r-- 1 root     root      675 May 15  2017 .profile  
    -rw-r----- 1 bandit14 bandit13 1679 May  7  2020 sshkey.private

    bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost # (1)
    ...
    SNIP
    ...
    bandit14@bandit:~$ cat /etc/bandit_pass/bandit14  # (2)
    4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
    ```

    1. Use the `-i` flag to specify the private key
    2. `cat` out the password. All passwords are located in the `/etc/bandit_pass` directory
    
    {==
    
    `ls -al` reveals the file `sshkey.private`.  The file indicates that it is a `private ssh key`. If we have a private ssh key, we can log in anywhere that has a corresponding public key in the `authorized_keys` file. To select the `private key` and connect to the server, add the flag `-i` to the `ssh` command. According to the description, the password for the user `bandit14` is stored in `/etc/bandit pass/bandit14`. 
    `cat` returns the contents of the file, which is the password. 

    ==}

    




### Resources

??? info "Resources"

    [Bandit-level14@overthewire](https://overthewire.org/wargames/bandit/bandit14.html)           
    [SSH/OpenSSH/Keys](https://help.ubuntu.com/community/SSH/OpenSSH/Keys) 
    [ssh @linux.die.net](https://linux.die.net/man/1/ssh)     
    [About creating and using identity keys for key based ssh login @iu.edu](https://kb.iu.edu/d/aews)






  









    




 
