# Bandit Level 9-10@overthewire.org

### Description
The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several "=" characters.

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit9
Password:                  | UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR ssh -p 2220 bandit9@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        The data looks messed up when displayed with `cat`. Determine the file's data type. There is a command for that. 

    === "Hint 2"

        A command exists to extract human-readable strings from a binary file. 

    === "Hint 3"

        Because the output isn't that large, you can examine it in this case. But what if the output was significantly longer? How would you filter the output to show only the lines you require? According to the description, the password is preceded by several `=` characters. That could be an option for a filter. 
    
    === "Hint 4"

        Use pipes `|`. Use `grep` to filter the output. 



??? done "Solution"

    ``` bash hl_lines="1 4"
    bandit8@bandit:~$ ls -al  
    total 56  
    drwxr-xr-x  2 root    root     4096 May  7  2020 .  
    drwxr-xr-x 41 root    root     4096 May  7  2020 ..  
    -rw-r--r--  1 root    root      220 May 15  2017 .bash_logout  
    -rw-r--r--  1 root    root     3526 May 15  2017 .bashrc  
    -rw-r-----  1 bandit9 bandit8 33033 May  7  2020 data.txt  
    -rw-r--r--  1 root    root      675 May 15  2017 .profile  

    bandit9@bandit:~$ cat data.txt  # (1)   
    �L�lω;��ßOܛ��ǤX��NdT$��x7��@D@�o��+D��B��M֢�Z/,_���w���#�5���  
                                                               Ў�e�&�-��Ϣ�6Q8��J�%fa��  
    �np�6l  
    |c���WW&8��f���

    bandit9@bandit:~$ file data.txt   # (2)  
    data.txt: data

    bandit9@bandit:~$ strings data.txt    # (3) 
    ...
    ...
    GhG$  
    ========== the*2i4  
    DUJmU
    ...
    ...

    bandit9@bandit:~$ strings data.txt | grep ===  # (4)  
    ========== the*2i4  
    ========== password  
    Z)========== is  
    &========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk # (5)
    ```
    
    1. the output from `cat` is unreadable
    2. the `data.txt` file has the data type `data`
    3. we can extract some human-readable strings with the command `strings`
    4. using `grep`, we can further filter the output to find the password.
    5. the password for the next level


    {==
    
    `ls -al` shows that the file `data.txt` is in the home directory. When we use the command `cat` to display the file's content, we get characters we can't read. It is a (binary) data file, as can be seen if we use the command `file` to determine the file type. With the command `strings`,  we may extract human-readable strings from binary files. If we take into consideration the level description ("preceded by several `=` characters") and pipe the output from `strings` to `grep` with the search string, for instance,`===`, we get some readable strings, and the password is in the last line.


    **Additional information: **
    How do you know the password is on the last line? There are two indicators. If you read the readable strings from first to last, it says "the password is xxxxxxxx." The password's length serves as the second indicator. All passwords up until this point have been 32 characters long. This password is also. The command `wc -c` can be used to determine the length of a string. `echo -n abcdefghijklmnoprstuvwxyz1234567 | wc -c`,  for instance. 

    ==}




### Resources

??? info "Resources"

    [Bandit-level9@overthewire](https://overthewire.org/wargames/bandit/bandit9.html)       
    [file @linux.die.net](https://linux.die.net/man/1/file)     
    [strings @linux.die.net](https://linux.die.net/man/1/strings)       
    [wc @linux.die.net](https://linux.die.net/man/1/wc)     
    [grep @linux.die.net](https://linux.die.net/man/1/grep)     









    




 
