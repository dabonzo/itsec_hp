# Bandit Level 0@overthewire.org

### Description
The goal of this level is for you to log into the game using `SSH`. The host to which you need to connect is `bandit.labs.overthewire.org`, on `port 2220`. The username is `bandit0` and the password is `bandit0`. Once logged in, go to the Level 1 page to find out how to beat Level 1.

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit0
Password:                  | bandit0


### Current level login
??? note "Log in"
    ``` bash
    sshpass -p bandit0 ssh -p 2220 bandit0@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested.


### Hints And Solution

??? faq "Hint(s)"
    to connect to a non-standard port on SSH, use the `#!bash -p` option



??? done "Solution"
    ``` bash
      ssh -p 2220 bandit0@bandit.labs.overthewire.org
    ``` 



### Resources

??? info "Resources"
    [Bandit-level0@overthewire](https://overthewire.org/wargames/bandit/bandit0.html)    
    [Secure Shell (SSH) on Wikipedia](https://en.wikipedia.org/wiki/Secure_Shell)     
    [How to use SSH on wikiHow](https://www.wikihow.com/Use-SSH)    
    [ssh man page @linux.die.net](https://linux.die.net/man/1/ssh)
