# Bandit Level 11-12@overthewire.org

### Description
The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit11
Password:                  | IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR ssh -p 2220 bandit11@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        It's a substitution encryption. Look it up and see if there are any that shift 13 positions. 

    === "Hint 2"

        Search for `ROT13`. Is there a tool you can use online, a command you can use, or an app you can install? 




??? done "Solution"

    ``` bash hl_lines="13"
    bandit11@bandit:~$ ls -al  
    total 24  
    drwxr-xr-x  2 root     root     4096 May  7  2020 .  
    drwxr-xr-x 41 root     root     4096 May  7  2020 ..  
    -rw-r--r--  1 root     root      220 May 15  2017 .bash_logout  
    -rw-r--r--  1 root     root     3526 May 15  2017 .bashrc  
    -rw-r-----  1 bandit12 bandit11   49 May  7  2020 data.txt  
    -rw-r--r--  1 root     root      675 May 15  2017 .profile  

    bandit11@bandit:~$ cat data.txt    
    Gur cnffjbeq vf 5Gr8L4qetPEsPk8htqjhRK8XSP6x2RHh  

    bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'    # (1)
    The password is xxxxxxxxxxxxxxxxxxxxxx
    ```
    
    1. `cat` output is piped into the command `tr`. `tr` rotates the letters by 13 positions. 



    {==
    
    The `ls -al` command reveals that the file `data.txt` is located in the home directory. The contents of the file `data.txt` are displayed as encrypted text. Because blank spaces are clearly visible, a substitution encryption is very likely. At the most basic level, the `Caesar cipher` or `ROT13` is used. In a basic Latin alphabet, such as the English alphabet, this cipher replaces a letter with the 13th letter of the alphabet. Because the alphabet is 26 characters long, `ROT13` can simply inverse an encoded string by repeating the algorithm on the encoded output. You can decode the string using `tr` or an online encoder/decoder. 
    In this example, the output of `cat data.txt` is piped to `tr 'A-Za-z' 'N-ZA-Mn-za-m'`. 

    ==}




### Resources

??? info "Resources"

    [Bandit-level12@overthewire](https://overthewire.org/wargames/bandit/bandit12.html)     
    [Rot13 on Wikipedia](https://en.wikipedia.org/wiki/Rot13)       
    [man page for tr command @man7.org](https://man7.org/linux/man-pages/man1/tr.1.html)        
    [How to decode rot13 @askubuntu.com](https://askubuntu.com/questions/1085069/how-can-i-decode-a-file-where-each-letter-has-been-replaced-with-the-letter-13-l)      
    [rot13 encoder/decoder @rot13.com](https://rot13.com/)      
    [rot13 encoder/decoder @cyberchef](https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,false,13)&input=R3VyIGNuZmZqYmVxIHZm)       


  









    




 
