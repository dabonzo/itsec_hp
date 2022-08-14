# Bandit Level 10-11@overthewire.org

### Description
The password for the next level is stored in the file **data.txt**, which contains base64 encoded data.

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit10
Password:                  | truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk ssh -p 2220 bandit10@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        The command `base64` has a decode flag to decode encoded data. See the man page for details on how to use it. 

    === "Hint 2"

        There are numerous online encoders and decoders available. Look them up. With Google, for example.




??? done "Solution"

    ``` bash hl_lines="13"
    bandit10@bandit:~$ ls -al  
    total 24  
    drwxr-xr-x  2 root     root     4096 May  7  2020 .  
    drwxr-xr-x 41 root     root     4096 May  7  2020 ..  
    -rw-r--r--  1 root     root      220 May 15  2017 .bash_logout  
    -rw-r--r--  1 root     root     3526 May 15  2017 .bashrc  
    -rw-r-----  1 bandit11 bandit10   69 May  7  2020 data.txt  
    -rw-r--r--  1 root     root      675 May 15  2017 .profile  

    bandit10@bandit:~$ cat data.txt    
    VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg==  

    bandit10@bandit:~$ cat data.txt | base64 -d  # (1)
    The password is xxxxxxxxxxxxxxxxxxxxxxxxxxx 

    ```
    
    1. the output of `cat` is piped into the command `base64` using the `-d` flag to decode the data



    {==
    
    The command `ls -al` reveals that the file `data.txt` is in the home directory. The description implies that the data is `base64` encoded, but if it didn't, you could still recognize such an encoding using the following method.  The contents of `data.txt` are displayed as a string with uppercase and lowercase characters and two `=` at the end. It indicates that it is a `base64` encoded string. The character set for `base64` encoded output is `[A-Z, a-z, 0-9, and + /]`. The output of a `base64` encoded string must be a multiple of four. If it is not a multiple of 4, the output is padded with `=` characters until it is a multiple of 4. The string ends with two `=` characters, and the character set matches, indicating that it is almost certainly a `base64-encoded` output.
    Use the `base64` command with the `-d` flag to decode a `base64` encoded string. 

    ==}




### Resources

[Bandit-level11@overthewire](https://overthewire.org/wargames/bandit/bandit11.html)     
[How to check whether a string is Base64 encoded or not](https://stackoverflow.com/questions/8571501/how-to-check-whether-a-string-is-base64-encoded-or-not)               
[man page for base64 command @man7.org](https://man7.org/linux/man-pages/man1/base64.1.html)        
[base64 @wikipedia.org](https://en.wikipedia.org/wiki/Base64)       
[online base64 decoding with cyberchef ](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=VkdobElIQmhjM04zYjNKa0lHbHpJRWxHZFd0M1MwZHpSbGM0VFU5eE0wbFNSbkZ5ZUVVeGFIaFVUa1ZpVlZCU0NnPT0K)

  









    




 
