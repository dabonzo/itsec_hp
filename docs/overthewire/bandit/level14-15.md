# Bandit Level 14-15@overthewire.org

### Description
The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | 14
Password:                  | 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e ssh -p 2220 bandit14@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        Learn how to connect to a service from the command line. 

    === "Hint 2"

        Try `nc`.

    === "Hint 3"

        If you don't want to copy and paste, brush up on your knowledge of piping.  



??? done "Solution"


    ``` bash hl_lines="8"
    bandit14@bandit:~$ nc localhost 30000  # (1)
    haadas  
    Wrong! Please enter the correct current password

    bandit14@bandit:~$ telnet localhost 30000

    Trying 127.0.0.1...  
    Connected to localhost.  
    Escape character is '^]'.  
    4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e   # (2)
    Correct!  
    BfMYroe26WYalil77FoDi9qh59eK5xNr  
    Connection closed by foreign host.

    bandit14@bandit:~$ cat /etc/bandit_pass/bandit14 | nc localhost 30000  # (3)
    Correct!  
    BfMYroe26WYalil77FoDi9qh59eK5xNr
    ```

    1. Checking out the service
    2. Using `telnet` and copy/paste the password
    3. The output of `cat` is sent to `nc`, which is connecting to the service. We don't need to copy/paste anything. 
    
    {==
    
    The description states that a service should be available on port 30000. We can access the service via `telnet` or `nc`. We get the password for the new level by entering the current level password after we establish a connection. If you copy/paste the password, `telnet` is fine but I prefer `nc` because I can pipe the output of `cat` to `nc` using pipes. I was unable to pipe using `telnet`. `nc` can also function as a service/listener, but more on that in later lessons. 

    ==}

??? info "One-liner"

    > Bash one-liners can reduce workload, automate something quickly and put the power of ultimate system control in your hands. 
   
    [https://linuxconfig.org/linux-complex-bash-one-liner-examples]()

    ``` bash
    bandit14@bandit:~$ cat /etc/bandit_pass/bandit14 | nc localhost 30000 | tail -2 | sed '/^$/d'  
    BfMYroe26WYalil77FoDi9qh59eK5xNr

    or

    bandit14@bandit:~$ echo 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e | nc localhost 30000 | tail -n 2 | head -n 1
    BfMYroe26WYalil77FoDi9qh59eK5xNr
    ```

    Here are some one-liner examples, with `cat` and `echo` and `sed`, `head` and `tail`. Try out and replace `echo` with `cat`, `sed` with `head` and `tail`. There are numerous possible combinations. 

    




### Resources

??? done "Resources"

    [Bandit-level15@overthewire](https://overthewire.org/wargames/bandit/bandit15.html)     
    [How to remove empty/blank lines from a file in Unix (including spaces) @serverfault.com?](https://serverfault.com/questions/252921/        how-to-remove-empty-blank-lines-from-a-file-in-unix-including-spaces)           
    [How do I use Head and Tail to print specific lines of a file @stackoverflow.com](https://stackoverflow.com/questions/15747912/how-do-i-use-head-and-tail-to-print-specific-lines-of-a-file)                         
    [Localhost on Wikipedia](https://en.wikipedia.org/wiki/Localhost)       
    [Ports @howstuffworks.com](http://computer.howstuffworks.com/web-server8.htm)       
    [Port (computer networking) on Wikipedia](https://en.wikipedia.org/wiki/Port_(computer_networking))     
    [How the Internet works in 5 minutes (YouTube)](https://www.youtube.com/watch?v=7_LPdttKXPc) (Not completely accurate, but good enough for beginners)       
    [IP Addresses @howstuffworks.com](http://computer.howstuffworks.com/web-server5.htm)        
    [IP Address on Wikipedia](https://en.wikipedia.org/wiki/IP_address)     





  









    




 
