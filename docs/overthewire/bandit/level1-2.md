# Bandit Level 1-2@overthewire.org

### Description
The password for the next level is stored in a file called `-` located in the home directory

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit1
Password:                  | boJ9jbbUNNfktd78OOpsqOltutMc3MY1


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p boJ9jbbUNNfktd78OOpsqOltutMc3MY1 ssh -p 2220 bandit1@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested.

### Hints And Solution


??? faq "Hint(s)"

    === "Hint 1"

        Look through the links in the Resources section. Look up `linux print filename with dash` in Google. 




??? done "Solution"

    ``` bash 
    bandit1@bandit:~$ cat < -    # (1)
      
    bandit1@bandit:~$ cat ./-    # (2)

    bandit1@bandit:~$ cat /home/bandit1/- # (3)
    ```

    1.  redirect `-` into `cat` 
    2.  add the path to the filename, either absolute or relative. The current directory is indicated by `./`. This is a relative path
    3. `cat` with an absolute path (absolute paths always begin with `/`)

    {==
    
    The argument `-` denotes STDIN/STDOUT, i.e. dev/stdin or dev/stdout. You can read such a file by redirecting the filename into cat, `cat < -` or by using the absolute or relative path to the file. `cat./-` or `cat /home/bandit1/-` are two examples. If the filename contains additional characters after `-`, for example, `-filename`, it no longer refers to STDIN/STDOUT, but the shell treats it as a `cat` option rather than a filename. To make it treat it as a filename, use a double dash. `--` indicates the end of command options, so a filename with a dash in it will no longer be treated as an option. 
    
    ==}







### Resources
[Bandit-level2@overthewire](https://overthewire.org/wargames/bandit/bandit2.html)       
[Google Search for “dashed filename”](https://www.google.com/search?q=dashed+filename)      
[Advanced Bash-scripting Guide - Chapter 3 - Special Characters](http://tldp.org/LDP/abs/html/special-chars.html)       
[What does "--" (double-dash) mean?](https://unix.stackexchange.com/questions/11376/what-does-double-dash-mean)     
[Absolute and Relative Pathnames in UNIX @geeksforgeeks.org](https://www.geeksforgeeks.org/absolute-relative-pathnames-unix/)




 
