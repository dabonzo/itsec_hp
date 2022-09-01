# Bandit Level 3-4@overthewire.org

### Description
The password for the next level is stored in a file called **spaces in this filename** located in the home directory

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit3
Password:                  | aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG ssh -p 2220 bandit3@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? faq "Hint(s)"

    === "Hint 1"

        Find out how to switch directories in Linux. Consult the Resources section. 

    === "Hint 2"

        Learn about displaying hidden files in Linux. How to differentiate between conventional (non-hidden) files and hidden ones   




??? done "Solution"

    ``` bash hl_lines="1 9 10"
    bandit3@bandit:~$ ls -al
    total 24  
    drwxr-xr-x  3 root root 4096 May  7  2020 .  
    drwxr-xr-x 41 root root 4096 May  7  2020 ..  
    -rw-r--r--  1 root root  220 May 15  2017 .bash_logout  
    -rw-r--r--  1 root root 3526 May 15  2017 .bashrc  
    drwxr-xr-x  2 root root 4096 May  7  2020 inhere  
    -rw-r--r--  1 root root  675 May 15  2017 .profile  
    bandit3@bandit:~$ cd inhere/  
    bandit3@bandit:~/inhere$ ls -al  # (1)
    total 12  
    drwxr-xr-x 2 root    root    4096 May  7  2020 .  
    drwxr-xr-x 3 root    root    4096 May  7  2020 ..  
    -rw-r----- 1 bandit4 bandit3   33 May  7  2020 .hidden  
    bandit3@bandit:~/inhere$ cat .hidden  
    ```

    1. use the options `-a` or `-A` with `ls`. Learn what the option `-l` implies


    {==
    
    In Linux, hidden files begin with the dot character `.` To display them with `ls`, use the flag `-a` or `-A`. An excerpt from the `ls` man page is provided below. If you are unsure of what options to use with the command, use `man`. `man` may be used online or on the terminal, like `man ls`. For instance, [https://linux.die.net/man](https://linux.die.net/man) includes substantial Linux documentation as well as the man pages for all of the terminal commands. 

    ```
    -a, --all
          do not ignore entries starting with .

    -A, --almost-all
          do not list implied . and ..
    ```
    
    ==}







### Resources

??? info "Resources"

    [Bandit-level4@overthewire](https://overthewire.org/wargames/bandit/bandit4.html)   
    [How to reference filename with spaces in Linux](https://linuxhint.com/reference-filename-with-spaces-linux/)   
    [Google Search for “spaces in filename”](https://www.google.com/search?q=spaces+in+filename)    








 
