# ffuf 

**ffuf** stands for **Fuzz Faster U Fool**. It's a tool used for web enumeration, fuzzing, and directory brute forcing.

## Installation

`ffuf` is included with many security oriented distributions.  In most cases `apt install ffuf` is enough to install it.

Since ffuf is a `go` application you simply install it with go (after you've installed golang) like `go get -u github.com/ffuf/ffuf`

## Basics

Minimal required options are `-u` for the URL and `-w` for the wordlist. `FFUZ` is the default keyword. It tell `ffuf` where to the entries from the wordlist will be injected. 

`fuff -u http://<IP_OR_URL>/FFUZ -w wordlist.txt`

This command "fuzzes" or brute-forces all the entries from a wordlist and checks the returned HTTP Codes from the URL.

The default keyword can be changed by adding `:<KEYWORD>`at the end of the wordlist.

`fuff -u http://<IP_OR_URL>/FUZZTHIS -w wordlist.txt:FUZZTHIS`

The option to change the keyword can be used to to use more than one wordlist

`fuff -u http://<IP_OR_URL>/FUZZTHIS/FUZZTHAT -w wordlist.txt:FUZZTHIS -w another_wordlist:FUZZTHAT`

Some wordlists have comments like copyright notices, comments or notes included. To ignore those comments use `-ic`. 

### Finding Pages 

#### Extension Fuzzing

If we don't know the technology used and we assume most of the pages have a index page `index.ext` we can do a quick check with a small extensions wordlist before we use a big, generic wordlist.

```
ffuf -u http://10.10.167.75/indexFUZZ -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt
```
```
└─# ffuf -u http://10.10.167.75/indexFUZZ -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.3.1 Kali Exclusive <3
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.167.75/indexFUZZ
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/web-extensions.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
________________________________________________

.php                    [Status: 302, Size: 0, Words: 1, Lines: 1]
.phps                   [Status: 403, Size: 289, Words: 21, Lines: 11]
:: Progress: [39/39] :: Job [1/1] :: 11 req/sec :: Duration: [0:00:05] :: Errors: 0 ::
```

We can see the technology uses is `php`.

#### Page fuzzing

After we get the extensions/technology used, we can try to find pages with those extensions. We can find pages by using the option `-e` and adding the extension we want to search for (including the `.`). There are wordlists that include generic filenames including with all the possible extensions (e.g. `raft-medium-files-lowercase.txt`). If we know the technology used, we can simply add the option `-e` with the extensions we want to search for and use a simple wordlist without extensions (e.g. `raft-medium-words-lowercase.txt`). We can also search for some generic extensions/ For example for a php-powered website we could use the extensions `.php, .txt, .cfg, .conf`. 

`ffuf -u http://10.10.167.75/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -e .php,.txt,.cfg, .conf`
```
└─# ffuf -u http://10.10.167.75/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -e .php,.txt,.cfg,.conf -fc 403

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.3.1 Kali Exclusive <3
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.167.75/FUZZ
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt
 :: Extensions       : .php .txt .cfg .conf 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
 :: Filter           : Response status: 403
________________________________________________

login.php               [Status: 200, Size: 1523, Words: 89, Lines: 77]
index.php               [Status: 302, Size: 0, Words: 1, Lines: 1]
logout.php              [Status: 302, Size: 0, Words: 1, Lines: 1]
config                  [Status: 301, Size: 312, Words: 20, Lines: 10]
docs                    [Status: 301, Size: 310, Words: 20, Lines: 10]
about.php               [Status: 200, Size: 4840, Words: 331, Lines: 109]
.                       [Status: 302, Size: 0, Words: 1, Lines: 1]
external                [Status: 301, Size: 314, Words: 20, Lines: 10]
setup.php               [Status: 200, Size: 4066, Words: 308, Lines: 123]
robots.txt              [Status: 200, Size: 26, Words: 3, Lines: 2]
security.php            [Status: 302, Size: 0, Words: 1, Lines: 1]
phpinfo.php             [Status: 302, Size: 0, Words: 1, Lines: 1]
instructions.php        [Status: 200, Size: 14014, Words: 1484, Lines: 263]
```

### Finding Directories

Directories are not technology-dependent and you can use a simple directory list

```
ffuf -u http://10.10.167.75/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt
```

### Recursive fuzzing

A directory can contain other/more directories (`login/user/`) and, manually, we would have to fuzz again by starting a new instance of `ffuf` with a new url. We can automate this with recursive scanning. The flag for recursive fuzzing is `-recursiion`. We also have to add the depth with the flag `-recursion-depth`. For example, if we define `-recursion-depth 1` it will only fuzz the main directory and direc subdirectories. We should always use the `-v` flag so we can see the full `URL`. Otherwise, if we fuzz for files, it could be hard to determine what file belongs to what directory. 

```
└─# ffuf -u http://188.166.173.208:30769/FUZZ -recursion -recursion-depth 1 -c -w /usr/share/seclists/Discovery/Web-Content/raft-small-directories-lowercase.txt -e .php -fc 403 -v

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.3.1 Kali Exclusive <3
________________________________________________

 :: Method           : GET
 :: URL              : http://188.166.173.208:30769/FUZZ
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/raft-small-directories-lowercase.txt
 :: Extensions       : .php 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
 :: Filter           : Response status: 403
________________________________________________

[Status: 301, Size: 327, Words: 20, Lines: 10]                                                                                                                                                                                                                                                                               
| URL | http://188.166.173.208:30769/forum
| --> | http://188.166.173.208:30769/forum/
    * FUZZ: forum

[INFO] Adding a new job to the queue: http://188.166.173.208:30769/forum/FUZZ

[Status: 301, Size: 326, Words: 20, Lines: 10]                                                                                                                                                                                                                                                                               
| URL | http://188.166.173.208:30769/blog
| --> | http://188.166.173.208:30769/blog/
    * FUZZ: blog

[INFO] Adding a new job to the queue: http://188.166.173.208:30769/blog/FUZZ

[Status: 200, Size: 986, Words: 423, Lines: 56]                                                                                                                                                                                                                                                                              
| URL | http://188.166.173.208:30769/index.php
    * FUZZ: index.php

[Status: 200, Size: 986, Words: 423, Lines: 56]                                                                                                                                                                                                                                                                              
| URL | http://188.166.173.208:30769/
    * FUZZ: 

[INFO] Starting queued job on target: http://188.166.173.208:30769/forum/FUZZ

[Status: 200, Size: 0, Words: 1, Lines: 1]                                                                                                                                                                                                                                                                                   
| URL | http://188.166.173.208:30769/forum/index.php
    * FUZZ: index.php

[Status: 200, Size: 21, Words: 1, Lines: 1]                                                                                                                                                                                                                                                                                  
| URL | http://188.166.173.208:30769/forum/flag.php
    * FUZZ: flag.php

[Status: 200, Size: 0, Words: 1, Lines: 1]                                                                                                                                                                                                                                                                                   
| URL | http://188.166.173.208:30769/forum/
    * FUZZ: 

[INFO] Starting queued job on target: http://188.166.173.208:30769/blog/FUZZ

[Status: 200, Size: 1046, Words: 438, Lines: 58]                                                                                                                                                                                                                                                                             
| URL | http://188.166.173.208:30769/blog/home.php
    * FUZZ: home.php
...
```



## Filters

You can filter with `matches` and `filters`. `matches` are inclusive. That means they display only values you define. On the other hand `filters` are exclusive. That means everything except the values you add are displayed. 

```
# Everything but the status code 403 responses are displayed
ffuf -u http://10.10.105.129/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fc 403
```

```
# Only status code 200 responses are displayed
ffuf -u http://10.10.105.129/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -mc 200
```

You can add more values to the filter and matches, for example `-mc 200,301,302` displays only  responses with 200, 301 or 302 HTTP status codes.

Sometimes we get a lot of 403 responses for files that don't exist, especially for files with a `.` in front of them like `.htdocs, .php`. Instead of excluding 403 codes entirely, and probably miss some important files, we can filter those files with the regex-filter. 

```
ffuf -u http://10.10.105.129/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fr '/\..*'
```

### Filter and matcher options

```
MATCHER OPTIONS:
  -mc                 Match HTTP status codes, or "all" for everything. (default: 200,204,301,302,307,401,403,405)
  -ml                 Match amount of lines in response
  -mr                 Match regexp
  -ms                 Match HTTP response size
  -mw                 Match amount of words in response

FILTER OPTIONS:
  -fc                 Filter HTTP status codes from response. Comma separated list of codes and ranges
  -fl                 Filter by amount of lines in response. Comma separated list of line counts and ranges
  -fr                 Filter regexp
  -fs                 Filter HTTP response size. Comma separated list of sizes and ranges
  -fw                 Filter by amount of words in response. Comma separated list of word counts and ranges
```

## Parameter fuzzing

Sometimes we don't know what parameters are accepted at an API endpoint or URL. We can `fuzz` for them.

```
└─# ffuf -u 'http://10.10.18.208/sqli-labs/Less-1/?FUZZ=1' -c -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt       

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.3.1 Kali Exclusive <3
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.18.208/sqli-labs/Less-1/?FUZZ=1
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
________________________________________________

title                   [Status: 200, Size: 691, Words: 39, Lines: 29]
file                    [Status: 200, Size: 691, Words: 39, Lines: 29]
type                    [Status: 200, Size: 691, Words: 39, Lines: 29]
password                [Status: 200, Size: 691, Words: 39, Lines: 29]
content                 [Status: 200, Size: 691, Words: 39, Lines: 29]
action                  [Status: 200, Size: 691, Words: 39, Lines: 29]
c                       [Status: 200, Size: 691, Words: 39, Lines: 29]
```

We can see that every response is `200`. It's not possible every single parameter from the wordlist is a valid parameter. A valid parameter has to have more or less than 39 words or a different size than 691. So we use the filter `-fw 39` to filter out responses what are 39 words long.

```
└─# ffuf -u 'http://10.10.18.208/sqli-labs/Less-1/?FUZZ=1' -c -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -fw 39

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.3.1 Kali Exclusive <3
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.18.208/sqli-labs/Less-1/?FUZZ=1
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
 :: Filter           : Response words: 39
________________________________________________

id                      [Status: 200, Size: 721, Words: 37, Lines: 29]
:: Progress: [2588/2588] :: Job [1/1] :: 688 req/sec :: Duration: [0:00:06] :: Errors: 0 ::
```

Filtering out responses with 39 words we get the correct parameter `id`

We can use seclists word-wordlist if we didn't find anything. It's a bigger list than burp's list (`Discovery/Web-Content/raft-medium-words-lowercase.txt `).

### Piping into ffuz

After we found the parameter we can `fuzz` the values. Take a look at the URL and we can see that the ID has to be a integer value. We can use or create a list and save it to a file or, since the values are simple integers, we can create a list in the console and pipe it to ffuf with the paramater `-w -`.

Below are 5 different examples how to create a list `[0-255]` and pipe it to ffuf.

```
$ ruby -e '(0..255).each{|i| puts i}' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
$ ruby -e 'puts (0..255).to_a' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
$ for i in {0..255}; do echo $i; done | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
$ seq 0 255 | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
$ cook '[0-255]' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
```

Again, we have to look at the response and filter out false positives with `-fw`.

## Brute-forcing passwords

We can also brute-force passwords. We can look at burp how the header looks like and use a password list like below. We yest again get a lot of false positives so we filter them out by size `-fs`

```
└─# ffuf -u http://10.10.18.208/sqli-labs/Less-11/ -c -w /usr/share/seclists/Passwords/Leaked-Databases/hak5.txt -X POST -d 'uname=Dummy&passwd=FUZZ&submit=Submit' -fs 1435 -H 'Content-Type: application/x-www-form-urlencoded' 

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.3.1 Kali Exclusive <3
________________________________________________

 :: Method           : POST
 :: URL              : http://10.10.18.208/sqli-labs/Less-11/
 :: Wordlist         : FUZZ: /usr/share/seclists/Passwords/Leaked-Databases/hak5.txt
 :: Header           : Content-Type: application/x-www-form-urlencoded
 :: Data             : uname=Dummy&passwd=FUZZ&submit=Submit
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
 :: Filter           : Response size: 1435
________________________________________________

p@ssword                [Status: 200, Size: 1526, Words: 100, Lines: 50]
:: Progress: [2351/2351] :: Job [1/1] :: 663 req/sec :: Duration: [0:00:06] :: Errors: 0 ::
```

## VHOSTS and subdomains

There are better tools to enumerate vhost and subdomains but we can try it with `ffuf` as well.

```
# subdomain fuzzing
ffuf -u http://FUZZ.mydomain.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -fs 0
# vhost fuzzing, FFUZ in the header
ffuf -u http://mydomain.com -c -w  /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H  'Host: FUZZ.mydomain.com' -fs 0
```

For subdomains we place the `FFUZ` keyword in front of the domain. If we look for vhosts, we need to send the keyword as a header.

## Proxying

Whether it' for [network pivoting](https://blog.raw.pm/en/state-of-the-art-of-network-pivoting-in-2019/) or for using BurpSuite plugins you can send all the ffuf traffic through a web proxy (HTTP or SOCKS5).

```
$ ffuf -u http://10.10.18.208/ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -x http://127.0.0.1:8080
```

It's also possible to send only matches to your proxy for replaying:

```
$ ffuf -u http://10.10.18.208/ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -replay-proxy http://127.0.0.1:8080
```

This may be useful if you don't need all the traffic to traverse an upstream proxy and want to minimize resource usage or to avoid polluting your  proxy history.

## Resources

**github**: https://github.com/ffuf/ffuf

**HTBA**: https://academy.hackthebox.com/module/details/54

**THM**: https://tryhackme.com/room/ffuf

**YT (hackersploit)**: https://www.youtube.com/watch?v=9Hik0xy9qd0

**Articles**: [https://www.hackingarticles.in/comprehensive-guide-on-ffuf/](https://www.hackingarticles.in/comprehensive-guide-on-ffuf/)

