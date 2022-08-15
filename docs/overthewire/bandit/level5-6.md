# Bandit Level 5-6@overthewire.org

### Description
The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:

-   human-readable
-   1033 bytes in size
-   not executable

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit5
Password:                  | koReBOKuIDDepwhWk7jZC0RTdopnAYKh


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p koReBOKuIDDepwhWk7jZC0RTdopnAYKh ssh -p 2220 bandit5@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        In the manpage for `find`, look up what options you need to use to find a file with the specified attributes.

    === "Hint 2"

        One option needs to be negated, consult the man page to learn how to negate options. 




??? done "Solution"

    ``` bash hl_lines="1 2 27 30 34"
    bandit5@bandit:~$ cd inhere/  
    bandit5@bandit:~/inhere$ ls -al  
    total 88  
    drwxr-x--- 22 root bandit5 4096 May  7  2020 .  
    drwxr-xr-x  3 root root    4096 May  7  2020 ..  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere00  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere01  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere02  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere03  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere04  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere05  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere06  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere07  
    rwxr-x---  2 root bandit5 4096 May  7  2020 maybehere08  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere09  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere10  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere11  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere12  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere13  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere14  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere15  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere16  
    rwxr-x---  2 root bandit5 4096 May  7  2020 maybehere17  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere18  
    drwxr-x---  2 root bandit5 4096 May  7  2020 maybehere19  

    bandit5@bandit:~/inhere$ find . -type f ! -executable -size 1033c -exec file {} + | grep ASCII   
    ./maybehere07/.file2: ASCII text, with very long lines   # (1)

    bandit5@bandit:~/inhere$ cat ./maybehere07/.file2  
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx  
                                                                                                                                                                                                          
    bandit5@bandit:~/inhere$    
    bandit5@bandit:~/inhere$ cat ./maybehere07/.file2 | tr -d " "    # (2)
    ```
    
    1. use find with necessary options and pipe the output to grep 
    2. use `tr` to remove the empty spaces
    
    {==
    
    Like in the previous level, we use the `find` command but with additional options.

    Using `man find`, we search for the necessary options.

    - 1033 bytes - `-size 1033c`        
    - not executable - `! -executable` , using the options `!` or `-not`, we negate the executable flag     
    - human readable - ` find ..... -exec file {} + | grep ASCII`.      

    We run the `file` command on each file we find, then pipe the output to grep. Since `ASCII` is human readable, we display that filename if the output of that piped command contains it. We `cat` that filename. The output has a large number of empty spaces, therefore we use the command `tr -d " "` to remove them all. 

    ==}




### Resources

??? info "Resources"

    [Bandit-level6@overthewire](https://overthewire.org/wargames/bandit/bandit6.html)       
    [man page for find command @man7.org](https://man7.org/linux/man-pages/man1/find.1.html)        






    




 
