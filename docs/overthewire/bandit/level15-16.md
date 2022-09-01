# Bandit Level 15-16@overthewire.org

### Description
The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL encryption.

**Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…**

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | 15
Password:                  | jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt ssh -p 2220 bandit15@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        The command `openssl` implements a client that can establish a transparent connection to a remote server that supports SSL/TLS. 

    === "Hint 2"

        `openssl s_client` is a generic SSL/TLS client implementation. What flag should you use to connect to a server? 
    
    === "Hint 3"

        The command `openssl s client -connect` connects to an SSL server. What are the arguments required to connect to a server? Is the hostname or IP address sufficient?  



??? done "Solution"


    ``` bash hl_lines="1 18 20"
    bandit15@bandit:~$ openssl s_client -connect localhost:30001  # (1)
    CONNECTED(00000003)  
    depth=0 CN = localhost  
    verify error:num=18:self signed certificate  
    verify return:1  
    depth=0 CN = localhost  
    verify return:1  
    ---  
    Certificate chain  
    0 s:/CN=localhost  
      i:/CN=localhost  
    ...
    ...
    ...

       Extended master secret: yes  
    ---  
    BfMYroe26WYalil77FoDi9qh59eK5xNr  # (2)
    Correct!  
    cluFn7wTiGryunymYOu4RcffSxQluehd  # (3)
    
    closed
    ```

    1. Using SSL to connect to a service
    2. Entering the current password
    3. Obtaining the password for the next level 
    
    {==
    
    Use the `openssl s client` command with the `-connect` flag to connect to the service over SSL and obtain the password for the next level. After connecting, we enter the password for the current level and receive the password for the following level in return. The `openssl` command can receive the current level password through a pipe. We can avoid having to copy and paste the password this way. However, in order to maintain the connection and read the response, the `openssl` command must also include the flag `-ign-eof`. 

    ==}

??? info "One-liner"

    > Bash one-liners can reduce workload, automate something quickly and put the power of ultimate system control in your hands. 
   
    [https://linuxconfig.org/linux-complex-bash-one-liner-examples]()

    ``` bash

    echo BfMYroe26WYalil77FoDi9qh59eK5xNr | openssl s_client -ign_eof -connect localhost:30001
    ...
    ...
    ...
    Correct!  
    cluFn7wTiGryunymYOu4RcffSxQluehd  
    
    closed
    ```


    




### Resources

??? info "Resources"

    [Bandit-level16@overthewire](https://overthewire.org/wargames/bandit/bandit16.html)     
    [Secure Socket Layer/Transport Layer Security on Wikipedia](https://en.wikipedia.org/wiki/Secure_Socket_Layer)      
    [OpenSSL Cookbook - Testing with OpenSSL](https://www.feistyduck.com/library/openssl-cookbook/online/ch-testing-with-openssl.html)      
    [Connect to a service over ssl from the linux command line](https://docs.pingidentity.com/bundle/solution-guides/page/iqs1569423823079.html)        
    [How to send a string to server using s_client @stackoverflow.com](https://stackoverflow.com/questions/23352152/how-to-send-a-string-to-server-using-s-client)      
    [openssl @linux.die.net](https://linux.die.net/man/1/openssl)       
    [openssl s_client @linux.die.net](https://linux.die.net/man/1/s_client)     





  









    




 
