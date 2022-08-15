# Bandit Level 1-2@overthewire.org

### Description
The password for the next level is stored in a file called `-` located in the home directory

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit1
Password:                  | CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9 ssh -p 2220 bandit2@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested.

### Hints And Solution


??? faq "Hint(s)"

    === "Hint 1"

        Look through the links in the Resources section. 




??? done "Solution"

    ``` bash 
    bandit2@bandit:~$ cat 'spaces in this filename'    # (1)
    
    bandit2@bandit:~$ cat "spaces in this filename"    # (2)
    
    bandit2@bandit:~$ cat spaces\ in\ this\ filename  # (3)
    ```

    1. enclosed in single quotes 
    2. enclosed in double quotes 
    3. escaped blank spaces in filename

    {==
    
    It is necessary to escape blank spaces in filenames with a backslash, `\`, in order to read them. For instance, `blank\ space`. Alternatively, the filename might be enclosed in single or double quotes like `"blank space."` or `'blank space'` 
    
    ==}







### Resources

??? info "Resources"
    [Bandit-level3@overthewire](https://overthewire.org/wargames/bandit/bandit3.html)   
    [How to reference filename with spaces in Linux](https://linuxhint.com/reference-filename-with-spaces-linux/)   
    [Google Search for “spaces in filename”](https://www.google.com/search?q=spaces+in+filename)    

 




 
