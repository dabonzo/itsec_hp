# Bandit Level 8-9@overthewire.org

### Description
The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit8
Password:                  | cvX2JJa4CFALtqS87jk27qwqGhBM9plV


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p cvX2JJa4CFALtqS87jk27qwqGhBM9plV ssh -p 2220 bandit8@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        In the manpage for `uniq`, look up what options you need display only unique lines. 

    === "Hint 2"

        Does `uniq` need anything in order to function? Does it matter whether duplicate lines are adjacent or not?  




??? done "Solution"

    ``` bash hl_lines="1 7 9"
    bandit8@bandit:~$ ls -al  
    total 56  
    drwxr-xr-x  2 root    root     4096 May  7  2020 .  
    drwxr-xr-x 41 root    root     4096 May  7  2020 ..  
    -rw-r--r--  1 root    root      220 May 15  2017 .bash_logout  
    -rw-r--r--  1 root    root     3526 May 15  2017 .bashrc  
    -rw-r-----  1 bandit9 bandit8 33033 May  7  2020 data.txt  
    -rw-r--r--  1 root    root      675 May 15  2017 .profile  
    bandit8@bandit:~$ cat data.txt | sort | uniq -u  # (1)
    xxxxxxxxxxxxxxxxxxxx
    ```
    
    1. In order to make all duplicate lines adjacent, pipe the output from `cat` to `sort`. Finally, run `uniq -u` on that output to view just unique lines. 

    
    {==
    
    `ls -al`shows that the file `data.txt` is in the home directory. To only display unique lines, we can use the command `uniq` with the option `-u`. If we look at the man page for `uniq`, we can see that it filters adjacent matching lines. Because the lines in the file are not all adjacent, we must `sort`  them before passing them to `uniq`. 

    ==}




### Resources

??? info "Resources"
    [Bandit-level9@overthewire](https://overthewire.org/wargames/bandit/bandit9.html)       
    [uniq @linux.die.net](https://linux.die.net/man/1/uniq)     









    




 
