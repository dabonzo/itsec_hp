# Bandit Level 0-1@overthewire.org

### Description
The password for the next level is stored in a file called `readme` located in the `home directory`. Use this password to log into `bandit1` using `SSH`. Whenever you find a password for a level, use `SSH` (on `port 2220`) to log into that level and continue the game.

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit0
Password:                  | bandit0


### Current level login
??? note "Login"

    ``` bash
    sshpass -p bandit0 ssh -p 2220 bandit0@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested.

### Hints And Solution


??? faq "Hint(s)"

    === "Hint 1"

        to list information about files in a directory, use `ls`

    === "Hint 2"

        to print files on the standard output, use `cat`



??? done "Solution"

    ``` bash hl_lines="1 9"
    bandit0@bandit:~$ ls -al # (1) 
    total 24  
    drwxr-xr-x  2 root    root    4096 May  7  2020 .  
    drwxr-xr-x 41 root    root    4096 May  7  2020 ..  
    -rw-r--r--  1 root    root     220 May 15  2017 .bash_logout  
    rw-r--r--   1 root    root    3526 May 15  2017 .bashrc  
    -rw-r--r--  1 root    root     675 May 15  2017 .profile  
    -rw-r-----  1 bandit1 bandit0   33 May  7  2020 readme  
    bandit0@bandit:~$ cat readme  # (2) 
    ```
    
    1.  use `ls` to list the files in the current directory. Use switches like `-a` to get more information about files 
    2.  use `cat` to print the content of the file `readme` to the standard output 
    
    {==
      
      First we check the directory if there is a file called `readme` in the folder. We can use `ls` for that. We find the file and `cat` it out to the standard output with `cat readme`
      
    ==}







### Resources

??? info "Resources"

    [Bandit-level1@overthewire](https://overthewire.org/wargames/bandit/bandit1.html)    
    [cat man page @ linux.die.net](https://linux.die.net/man/1/cat)    
    [ls man page @ linux.die.net](https://linux.die.net/man/1/ls)   


 
