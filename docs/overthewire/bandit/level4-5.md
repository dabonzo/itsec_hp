# Bandit Level 4-5@overthewire.org

### Description
The password for the next level is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the `reset` command.

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit4
Password:                  | UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p pIwrPrtPN36QITSp3EQaw936yaFoFgAB ssh -p 2220 bandit4@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        Find out how to switch directories in Linux. Consult the Resources section. 

    === "Hint 2"

        Learn about displaying hidden files in Linux. How to differentiate between conventional (non-hidden) files and hidden ones  




??? done "Solution"

    ``` bash hl_lines="1 9 10 24 35"
    bandit4@bandit:~$ ls -al  
    total 24  
    drwxr-xr-x  3 root root 4096 May  7  2020 .  
    drwxr-xr-x 41 root root 4096 May  7  2020 ..  
    -rw-r--r--  1 root root  220 May 15  2017 .bash_logout  
    -rw-r--r--  1 root root 3526 May 15  2017 .bashrc  
    drwxr-xr-x  2 root root 4096 May  7  2020 inhere  
    -rw-r--r--  1 root root  675 May 15  2017 .profile  
    bandit4@bandit:~$ cd inhere/  
    bandit4@bandit:~/inhere$ ls -al  
    total 48  
    drwxr-xr-x 2 root    root    4096 May  7  2020 .  
    drwxr-xr-x 3 root    root    4096 May  7  2020 ..  
    -rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file00  
    -rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file01  
    -rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file02  
    -rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file03  
    -rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file04  
    -rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file05  
    -rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file06  
    -rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file07  
    -rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file08  
    -rw-r----- 1 bandit5 bandit4   33 May  7  2020 -file09  
    bandit4@bandit:~/inhere$ file ./*    
    ./-file00: data  
    ./-file01: data  
    ./-file02: data  
    ./-file03: data  
    ./-file04: data  
    ./-file05: data  
    ./-file06: data  
    ./-file07: ASCII text  
    ./-file08: data  
    ./-file09: data  
    bandit4@bandit:~/inhere$ cat ./-file07 
    ```


    {==
    
    The command `file` can be used to determine the file-type. There are just 10 files, so we could manually type `file` for each file name and search for a human-readable type. One such type is `ASCII text`. Using `*` to simultaneously query all files with the command `file` is faster. Given that every file begins with a dash, we must provide a complete path. For instance, `file ./*`. We can also use `ls -al -- *`. The file can be output with `cat` once it has been identified.  

    ```
    -a, --all
          do not ignore entries starting with .

    -A, --almost-all
          do not list implied . and ..
    ```
    
    **Other methods**

    - `strings`  - `strings -- *` or `strings ./*`    
    - `find` - `find . -type f -exec file {} + | grep ASCII`    



    ==}


??? info "One-liner"

    > Bash one-liners can reduce workload, automate something quickly and put the power of ultimate system control in your hands. 
   
    [https://linuxconfig.org/linux-complex-bash-one-liner-examples]()

    ``` bash
    cat $(find ~/inhere -type f -exec file {} + | grep ASCII | cut -f1 -d' '|tr ":" " ")
    ```

    This is an example of an one-liner. It's not part of the challenge and can be omitted.
    The command substitution output is handled by `cat`. A single file with the file type `ASCII` is returned by the commands in `$()`, which searches for all files in the current folder and passes them to the `file` and `grep` commands. To leave only the filename, all unnecessary characters are removed using the commands `tr` and `cut`.

    You can build the one-liner piece by piece and observe how the commands minimize the output. Start by typing `find ~/inhere -type f -exec file {} + `, the command in front of the first pipe `|`. Add the command between the pipes first and second pipe `find ~/inhere -type f -exec file {} + | grep ASCII`. Move on to the next command and so forth. For the filename to be passed to the `cat` command, use command substitution. Refer to the Resources section for more information.



### Resources

??? info "Resources"

    [Bandit-level5@overthewire.org](https://overthewire.org/wargames/bandit/bandit5.html)   
    [tr and cut @geeksforgeeks.org](https://www.geeksforgeeks.org/tr-command-in-unix-linux-with-examples/)  
    [find human readable files @stackexchange.com](https://unix.stackexchange.com/questions/313442/find-human-readable-files)   
    [What is command substitution in a shell? @stackexchange](https://unix.stackexchange.com/questions/440088/what-is-command-substitution-in-a-shell)  
    [What are One-liners @linuxconfig.org](https://linuxconfig.org/linux-complex-bash-one-liner-examples)





    




 
