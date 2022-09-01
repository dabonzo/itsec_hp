# Bandit Level 16-17@overthewire.org

### Description
The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | 16
Password:                  | cluFn7wTiGryunymYOu4RcffSxQluehd


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p cluFn7wTiGryunymYOu4RcffSxQluehd ssh -p 2220 bandit16@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        Use `nmap` to scan the ports. 

    === "Hint 2"

        Use the `-sV` flag with `nmap` to identify a `SSL` port. 

    === "Hint 3"

        The command `openssl s client -connect` connects to an SSL server. What are the arguments required to connect to a server? Is the hostname or IP address sufficient?  

    === "Hint 4"
        You can copy paste the `private key` and echo it out into a file. The use the `-n` flag with echo and redirection `>`.

    === "Hint 5"
        Use `chmod` to change file permissions 

    === "Hint 5"
        If you want to execute only one command on the remote host, you can do it with `ssh` in one go. 

??? done "Solution"


    ``` bash hl_lines="3 11 15"
    bandit16@bandit:~$ nmap -p31000-32000 -sV localhost    # (1) 
    
    Starting Nmap 7.40 ( https://nmap.org ) at 2022-07-21 22:07 CEST  
    Nmap scan report for localhost (127.0.0.1)  
    Host is up (0.00030s latency).  
    Not shown: 996 closed ports  
    PORT      STATE    SERVICE     VERSION  
    31046/tcp open     echo  
    31518/tcp filtered unknown  
    31691/tcp open     echo  
    31790/tcp open     ssl/unknown   # (2) 
    31960/tcp open     echo  
    1 service unrecognized despite returning data. If you know the service/version, please submit the follow  
    ing fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :  
    SF-Port31790-TCP:V=7.40%T=SSL%I=7%D=7/21%Time=62D9B21A%P=x86_64-pc-linux-g   # (3) 
    SF:nu%r(GenericLines,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20cu  
    SF:rrent\x20password\n")%r(GetRequest,31,"Wrong!\x20Please\x20enter\x20the  
    SF:\x20correct\x20current\x20password\n")%r(HTTPOptions,31,"Wrong!\x20Plea  
    ... <SNIP>
    ... <SNIP>
    ... <SNIP>
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
    Nmap done: 1 IP address (1 host up) scanned in 90.28 seconds
    ```

    1. Using nmap for port scan. `-p` for specifying the ports and `-sV` for service discovery
    2. A port running SSL
    3. Some output from the service asking for a password, mentions the port too
    
    {==
    
    To begin, we run a `nmap` port scan. We use the flag `-p` and then specify the ports to scan ports `31000-32000`. We also perform service discovery on those ports using the `-sV` flag. We notice that the only port that uses `SSL` is **31790**. In addition, you can see in the output that it requests a password. This output also mentions port `3179`. We are using `openssl s_client -connect` after identifying the listening port as an `SSL` port, as we did in the previous level. We get an `ssh-key` by entering the current level password. 

    ==}


    ``` bash hl_lines="1 15 41"
    bandit16@bandit:~$ openssl s_client -connect localhost:31790  # (1) 
    CONNECTED(00000003)  

    ... <SNIP>
    ... <SNIP>
    ... <SNIP>
    
       Start Time: 1658434202  
       Timeout   : 7200 (sec)  
       Verify return code: 18 (self signed certificate)  
       Extended master secret: yes  
    ---  
    cluFn7wTiGryunymYOu4RcffSxQluehd  
    Correct!  
    -----BEGIN RSA PRIVATE KEY-----  # (2) 
    MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ  
    imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ  
    Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu  
    DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW  
    JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX  
    x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD  
    KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl  
    J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd  
    d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC  
    YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A  
    vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama  
    +TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT  
    8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx  
    SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd  
    HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt  
    SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A  
    R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi  
    Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg  
    R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu  
    L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni  
    blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU  
    YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM  
    77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b  
    dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3  
    vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=  
    -----END RSA PRIVATE KEY-----  # (3) 
    
    closed
    ```

    1. connect to service over SSL
    2. private key: start copying from here ...
    3. ... to the end of here


    {==
    
    After we obtain the `ssh-key`, we must save it so that we can use it with `ssh`. We put it in the writable directory `/tmp`. Copy the relevant output (from `——-BEGIN RSA PRIVATE KEY——` to `——-END RSA PRIVATE KEY——`) to a new file, `echo` it and redirect it to the new file. We can use that `private key` after changing the permission to `400`. 

    ==}


    ``` bash hl_lines="1-25 30"
    bandit16@bandit:~$ echo -n "-----BEGIN RSA PRIVATE KEY-----                 
    MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ     
    imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ     
    Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu     
    DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW     
    JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX     
    x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD     
    KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl     
    J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd     
    d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC     
    YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A     
    vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama     
    +TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT     
    8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx     
    SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd     
    HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt     
    SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A     
    R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi     
    Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg     
    R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu     
    L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni     
    blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU     
    YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM     
    77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b     
    -----END RSA PRIVATE KEY-----  " > /tmp/bonzo.private; chmod 400 /tmp/bonzo.private  
    # (1) 
    bandit16@bandit:~$ ls -al /tmp/bonzo.private  
    -r-------- 1 bandit16 root 1728 Jul 21 22:39 /tmp/bonzo.private

    bandit16@bandit:~$ ssh -i /tmp/bonzo.private bandit17@localhost 'cat /etc/bandit_pass/bandit17'  
    Could not create directory '/home/bandit16/.ssh'.  
    The authenticity of host 'localhost (127.0.0.1)' can't be established.  
    ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.  
    Are you sure you want to continue connecting (yes/no)? yes  
    Failed to add the host to the list of known hosts (/home/bandit16/.ssh/known_hosts).  
    This is a OverTheWire game server. More information on http://www.overthewire.org/wargames  
    
    xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn
    ```

    1. echo out the private key and redirect it to a file
    2. connect to the server with ssh and execute the `cat` command


    {==
    
    From previous levels, we know that all of the passwords are in the folder `/etc/bandit pass/`'. So we connect as `bandit17` via ssh and `cat` the password from `/etc/bandit pass/bandit17` in one go. 

    ==}




### Resources

??? info "Resources"

    [Bandit-level16@overthewire](https://overthewire.org/wargames/bandit/bandit16.html)         
    [Port scanner on Wikipedia](https://en.wikipedia.org/wiki/Port_scanner)         
    [nmap @linux.die.net](https://linux.die.net/man/1/nmap)         
    [echo @linux.die.net](https://linux.die.net/man/1/echo)         
    [How to run SSH command and exit @linuxhint.com](https://linuxhint.com/run-exit-ssh-command/)           
    





  









    




 
