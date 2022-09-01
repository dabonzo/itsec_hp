# Bandit Level 12-13@overthewire.org

### Description
The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!).

### Current level credentials
Key                        | Value
-------------------------: |----------------------------------------
Server-name:               | bandit.labs.overthewire.org
Port:                      | 2220
User:                      | bandit12
Password:                  | JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv


### Current level login
??? note "Log in"

    ``` bash
    sshpass -p JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv ssh -p 2220 bandit12@bandit.labs.overthewire.org
    ```
    Note: You might need to install `sshpass` before using it. The `ssh` command can also be used on its own. If so, copy-paste the password when requested. 

### Hints And Solution


??? hint "Hint(s)"

    === "Hint 1"

        Research how to unzip `gzip` and `bzip2` compressed files. 

    === "Hint 2"

        To unzip `gzip` compressed files, the file must have an extension that allows `gzip` to recognize that it is compressed.   

    === "Hint 3"

        Research how to eXtract `tar` archives. 



??? done "Solution"

    {==

    This challenge resembles a Matryoshka (Russian doll). The file has been numerous times compressed and tar'ed, and we must figure out how to `unzip` and `untar` the file in order to obtain the password for the subsequent level (peel off the layers). Every time we create a new file, we must use the `file` command to check what type of file it is in order to "peel off another layer of the puzzle". 
    
    ==}

    ``` bash hl_lines="13"
    bandit12@bandit:~$ ls -Al  
    total 24  
    -rw-r--r--  1 root     root      220 May 15  2017 .bash_logout  
    -rw-r--r--  1 root     root     3526 May 15  2017 .bashrc  
    -rw-r-----  1 bandit13 bandit12 2582 May  7  2020 data.txt  
    -rw-r--r--  1 root     root      675 May 15  2017 .profile  
    
    bandit12@bandit:~$ mkdir /tmp/bonzo_otw    
    bandit12@bandit:~$ cp data.txt /tmp/bonzo_otw    
    bandit12@bandit:~$ cd /tmp/bonzo_otw  
    bandit12@bandit:/tmp/bonzo_otw$ ls -Al  
    total 1996  
    -rw-r----- 1 bandit12 root    2582 Aug 15 14:11 data.txt

    bandit12@bandit:/tmp/bonzo_otw$ xxd -r data.txt > data.bin  
    bandit12@bandit:/tmp/bonzo_otw$ ls -Al  
    total 8  
    -rw-r--r-- 1 bandit12 root  606 Aug 15 14:32 data.bin  
    -rw-r----- 1 bandit12 root 2582 Aug 15 14:11 data.txt
    ```
    
    {==
    
    We'll create new files by modifying the existing one. To achieve that, we must be in a location where we have write permission. The `/tmp` directory must have a folder created in it, and we must utilize that directory as our working directory. We copy the initial file to the working directory.
    Because the initial file is described as a `hexdump`, we use `xxd` to convert it back to a binary file. 

    ==}

    ``` bash hl_lines="13"
    bandit12@bandit:/tmp/bonzo_otw$ file data.bin    
    data.bin: gzip compressed data, was "data2.bin", last modified: Thu May  7 18:14:30 2020, max compressio  
    n, from Unix  
    bandit12@bandit:/tmp/bonzo_otw$ gunzip data.bin  
    gzip: data.bin: unknown suffix -- ignored  
    bandit12@bandit:/tmp/bonzo_otw$ mv data.bin data.gz  
    bandit12@bandit:/tmp/bonzo_otw$ gunzip data.gz  
    bandit12@bandit:/tmp/bonzo_otw$ ls -Al  
    total 8  
    -rw-r--r-- 1 bandit12 root  573 Aug 15 14:32 data  
    -rw-r----- 1 bandit12 root 2582 Aug 15 14:11 data.txt
    ```
    
    {==
    
    The `file` command is used to determine the file type of the new binary file, and the output indicates that it is a `gzip` compressed file. We try to unzip the file with `'gunzip`, but it requires a file extension. To unzip the file, we could also use `gzip-d`. So we rename the `data.bin` file to `data.gz`, and now we can unzip it. Also, for future reference, keep in mind that `unzip` requires an extension to function.  

    ==}

    ``` bash
    bandit12@bandit:/tmp/bonzo_otw$ file data  
    data: bzip2 compressed data, block size = 900k  
    
    bandit12@bandit:/tmp/bonzo_otw$ bunzip2 data  
    bunzip2: Can't guess original name for data -- using data.out  

    bandit12@bandit:/tmp/bonzo_otw$ ls -Al  
    total 8  
    -rw-r--r-- 1 bandit12 root  431 Aug 15 14:32 data.out  
    -rw-r----- 1 bandit12 root 2582 Aug 15 14:11 data.txt
    ```
    
    {==
    
    To determine the file type, we use the `file` command. It's compressed with `bzip2`. We look up how to decompress `gzip2` compressed data and use that information to unzip the file. Unlike with `gunzip`, we do not need to rename the file to make it work with `bunzip`.

    ==}

    ``` bash
    bandit12@bandit:/tmp/bonzo_otw$ file data.out    
    data.out: gzip compressed data, was "data4.bin", last modified: Thu May  7 18:14:30 2020, max compressio  
    n, from Unix  
    bandit12@bandit:/tmp/bonzo_otw$ mv data.out data.gz  
    bandit12@bandit:/tmp/bonzo_otw$ gunzip data.gz    
    bandit12@bandit:/tmp/bonzo_otw$ ls -Al                
    total 24  
    -rw-r--r-- 1 bandit12 root 20480 Aug 15 14:32 data  
    -rw-r----- 1 bandit12 root  2582 Aug 15 14:11 data.txt
    ```
    
    {==
    
    The steps are the same. Determine the file format. If it's `gzip`, rename the file; if it's `bzip2`, you can leave it alone. `ls` will show you the new file. 

    ==}

    ``` bash
    bandit12@bandit:/tmp/bonzo_otw$ file data  
    data: POSIX tar archive (GNU)  
    bandit12@bandit:/tmp/bonzo_otw$ tar -xf data  
    bandit12@bandit:/tmp/bonzo_otw$ ls -Al         
    total 36  
    -rw-r--r-- 1 bandit12 root 20480 Aug 15 14:32 data  
    -rw-r--r-- 1 bandit12 root 10240 May  7  2020 data5.bin  
    -rw-r----- 1 bandit12 root  2582 Aug 15 14:11 data.txt
    ```
    
    {==
    
    We have a `tar` archive this time. The command changes, but the process stays the same. To `untar` the file, we use `tar -xf`. Unlike the zip-commands, a new file is created, but the original file is kept. 

    ==}

    ``` bash
    bandit12@bandit:/tmp/bonzo_otw$ file data5.bin    
    data5.bin: POSIX tar archive (GNU)  
    bandit12@bandit:/tmp/bonzo_otw$ tar -xf data5.bin  
    bandit12@bandit:/tmp/bonzo_otw$ ls -Al  
    total 40  
    -rw-r--r-- 1 bandit12 root 20480 Aug 15 14:32 data  
    -rw-r--r-- 1 bandit12 root 10240 May  7  2020 data5.bin  
    -rw-r--r-- 1 bandit12 root   222 May  7  2020 data6.bin  
    -rw-r----- 1 bandit12 root  2582 Aug 15 14:11 data.txt

    bandit12@bandit:/tmp/bonzo_otw$ rm data data5.bin data.txt    
    bandit12@bandit:/tmp/bonzo_otw$ ls -Al  
    total 4  
    -rw-r--r-- 1 bandit12 root 222 May  7  2020 data6.bin
    ```
    
    {==

    Since it's a `tar` archive again, we simply repeat the steps from before. Since there are now files that we don't use anymore, let's do some housekeeping and delete those unneeded files.  Because this is another tar archive, we simply repeat the previous steps. Since there are now files that we no longer use, let's do some housekeeping and delete those files. 

    ==}

    ``` bash
    bandit12@bandit:/tmp/bonzo_otw$ file data6.bin    
    data6.bin: bzip2 compressed data, block size = 900k  
    bandit12@bandit:/tmp/bonzo_otw$ bunzip2 data6.bin  
    bunzip2: Can't guess original name for data6.bin -- using data6.bin.out  
    bandit12@bandit:/tmp/bonzo_otw$ ls -Al  
    total 12  
    -rw-r--r-- 1 bandit12 root 10240 May  7  2020 data6.bin.out
    ```
    
    {==
    
    It's a compressed `bzip2` file. We don't need to change the extension, so we just use the command `gunzip` to unzip it. 

    ==}

    ``` bash
    bandit12@bandit:/tmp/bonzo_otw$ file data6.bin.out    
    data6.bin.out: POSIX tar archive (GNU)  
    bandit12@bandit:/tmp/bonzo_otw$ tar -xf data6.bin.out  
    bandit12@bandit:/tmp/bonzo_otw$ ls -Al  
    total 16  
    -rw-r--r-- 1 bandit12 root 10240 May  7  2020 data6.bin.out  
    -rw-r--r-- 1 bandit12 root    79 May  7  2020 data8.bin
    ```
    
    {==
    
    It's a `tar` archive. Repeat the steps to untar a `tar` archive.

    ==}


    ``` bash
    bandit12@bandit:/tmp/bonzo_otw$ file data8.bin    
    data8.bin: gzip compressed data, was "data9.bin", last modified: Thu May  7 18:14:30 2020, max compressi  
    on, from Unix  
    bandit12@bandit:/tmp/bonzo_otw$ mv data8.bin data8.gz  
    bandit12@bandit:/tmp/bonzo_otw$ gunzip data8.gz    
    bandit12@bandit:/tmp/bonzo_otw$ ls -Al             
    total 16  
    -rw-r--r-- 1 bandit12 root 10240 May  7  2020 data6.bin.out  
    -rw-r--r-- 1 bandit12 root    49 May  7  2020 data8
    ```
    
    {==
    
    It's a `gzip` compressed file. Repeat the steps to unzip a `gzip`.

    ==}

    ``` bash
    bandit12@bandit:/tmp/bonzo_otw$ file data8    
    data8: ASCII text  
    bandit12@bandit:/tmp/bonzo_otw$ cat data8    
    The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
    ```
    
    {==
    
    This time, we received a file with `ASCII text` content, allowing us to output the file's contents.The output (with `cat`) reveals the next level's password. 

    ==}




### Resources

??? info "Resources"

    [Bandit-level13@overthewire](https://overthewire.org/wargames/bandit/bandit13.html)     
    [xxd manpage @linux.die.net](https://linux.die.net/man/1/xxd)       
    [bzip2 manpage @linux.die.net](https://linux.die.net/man/1/bzip2)       
    [gzip manpage @linux.die.net](https://linux.die.net/man/1/gzip)     
    [tar manpage @linux.die.net](https://linux.die.net/man/1/tar)       
    [Hex dump on Wikipedia](https://en.wikipedia.org/wiki/Hex_dump)         




  









    




 
