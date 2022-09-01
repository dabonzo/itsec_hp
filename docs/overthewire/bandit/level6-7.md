# Bandit Level 6-7@overthewire.org

### Description
The password for the next level is stored **somewhere on the server** and has all of the following properties:

-   owned by user bandit7
-   owned by group bandit6
-   33 bytes in size

### Current level credentials
|          Key | Value                            |
| -----------: | -------------------------------- |
| Server-name: | bandit.labs.overthewire.org      |
|        Port: | 2220                             |
|        User: | bandit6                          |
|    Password: | P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU |


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU ssh -p 2220 bandit6@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        In the manpage for `find`, look up what options you need to use to find a file with the specified attributes. 

    === "Hint 2"

        If you encounter `permission denied` errors, redirect them to `/dev/null` to disregard them. Learn about `file descriptors` and the `standard output`. 




??? done "Solution"

    ``` bash hl_lines="1 4"
    bandit6@bandit:~$ find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null # (1)                 
    /var/lib/dpkg/info/bandit7.password  

    bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password   # (2)
    ```
    
    1. use `find` with necessary options, start the search from `/`. Redirect errors to `/dev/null` 
    2. display the password using `cat` on the found file
    
    {==
    
    Using `man find`, we search for the necessary options.

        - 33 bytes - `-size 33c`
        - owned by user bandit7 - `-user bandit7`
        - owned by group bandit6 - `-group bandit6`


    There are no subdirectories and the file is not located in the home directory. We start our search at the root directory `/` since we need to search the entire filesystem for the file. We use the parameters `-size`, `-user`, and `-group` to define the options necessary to find the file. As we search from the root directory up, we will come across many files that we do not have permission to examine. As a result, we direct any error messages we receive as a result of this to the device `/dev/null`. We disregard them.
    In bash and sh, the default file descriptor for errors is `2`. We utilize the redirection `2>/dev/null` to disregard error messages. We use `cat` to display the password after discovering the whole path to the file. 

    ==}




### Resources

??? info "Resources"
    [Bandit-level7@overthewire](https://overthewire.org/wargames/bandit/bandit7.html)   
    [man page for find command @man7.org](https://man7.org/linux/man-pages/man1/find.1.html)    
    [BASH Shell Redirect Output and Errors To /dev/null @cyberciti.biz](https://www.cyberciti.biz/faq/how-to-redirect-output-and-errors-to-devnull/)    








    




 
