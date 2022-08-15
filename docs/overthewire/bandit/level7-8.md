# Bandit Level 7-8@overthewire.org

### Description
The password for the next level is stored in the file **data.txt** next to the word **millionth**

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit7
Password:                  | HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs ssh -p 2220 bandit7@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        Find out how to use grep to search for a string in a file. 

    === "Hint 2"

        you can cut out only the password. Use the command `cut` with the field separator option. 




??? done "Solution"

    ``` bash hl_lines="1 10 13"
    bandit7@bandit:~$ ls -al  
    total 4108  
    drwxr-xr-x  2 root    root       4096 May  7  2020 .  
    drwxr-xr-x 41 root    root       4096 May  7  2020 ..  
    -rw-r--r--  1 root    root        220 May 15  2017 .bash_logout  
    -rw-r--r--  1 root    root       3526 May 15  2017 .bashrc  
    -rw-r-----  1 bandit8 bandit7 4184396 May  7  2020 data.txt  
    -rw-r--r--  1 root    root        675 May 15  2017 .profile  

    bandit7@bandit:~$ grep millionth data.txt    # (1)
    millionth       xxxxxxxxxxxxxxxxxxxxxx  

    bandit7@bandit:~$ grep millionth data.txt | cut -f2  # (2)
    xxxxxxxxxxxxxxxxxx
    ```
    
    1. use `grep` with the search string `millionth` on the file `data.txt`
    2. `cut` out only the password. It's in field 2 of the line.
    
    {==
    
    `ls -al`shows that the file `data.txt` is in the home directory. We can just use `grep` this time because we know where the file is located. Using the command `grep "millionth" data.txt`, we look for the term `millionth` within the file `data.txt`. The output is a line with two fields, the second of which is the password. As a result, we pipe the `grep` output to the `cut` command, wich only displays the second field.  

    ==}




### Resources

??? info "Resources"
    [Bandit-level8@overthewire](https://overthewire.org/wargames/bandit/bandit8.html)       
    [How To Use grep Command In Linux / UNIX With Practical Examples @cyberciti.biz](https://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/)       
    [cut command in Linux with examples @geeksforgeeks.org](https://www.geeksforgeeks.org/cut-command-linux-examples/)      









    




 
