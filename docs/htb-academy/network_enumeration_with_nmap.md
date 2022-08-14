# Network Enumeration with Nmap 

[Nmap](https://en.wikipedia.org/wiki/Nmap) is used to discover hosts and services on a computer network  by sending packets and analyzing the responses. Nmap provides a number of features for probing computer networks,  including host discovery and service and operating system detection. Besides other features, Nmap also offers scanning capabilities that can  determine if packet filters, firewalls, or intrusion detection systems  (IDS) are configured as needed.

## Host Enumeration with Nmap

### Host Discovery

The first step should be getting an overwiew over all the system that are online and that we can work with.
This step may not be needed if we are given an IP to check it out. A typical host discovery scan looks like:

```
bonzo@srv001:/opt/share> nmap 192.168.0.0/24 -sn -oA tnet | grep for | cut -d" " -f5
192.168.0.1
192.168.0.2
192.168.0.4
192.168.0.5
192.168.0.6
192.168.0.7
192.168.0.8
192.168.0.9
```

The nmap options used are:

| Option         | What it does                                                 |
| -------------- | ------------------------------------------------------------ |
| 192.168.0.0/24 | The network in [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) format |
| -sn            | -sn: Ping Scan - disables port scan                          |
| -oA            | -oA \<filename\>: Output in the three major formats at once. Nmap has 5 output formats. The 3 major output formats are `normal`, `XML` and `grepable` |
| tnet           | belongs to -oA and is the name of the files, without extension, -oA outputs (`tnet.nmap` for the normal output, `tnet.gnmap` for the grepable output and `tnet.xml` for the XML output) |

Sometimes we get a list with predefined hosts to check. This is a simple list with an IP on each row. 

For example, if we have a list with 3 IP's:

```
192.168.0.1
192.168.0.2
192.168.0.3
```

We can use this list to scan only the hosts with that IP.

```shell-session
Bonzo@htb[/htb]$ sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
192.168.0.1
192.168.0.2
192.168.0.3
```

We can define more IP's on the command line.

```
Bonzo@htb[/htb]$ sudo nmap -sn -oA tnet 192.168.0.1 192.168.0.2 192.168.03 | grep for | cut -d" " -f5
192.168.0.1
192.168.0.2
192.168.0.3
```

We can also define ranges of IP's

```
Bonzo@htb[/htb]$ sudo nmap -sn -oA tnet 192.168.0.1-3 | grep for | cut -d" " -f5
192.168.0.1
192.168.0.2
192.168.0.3
```

### Single Host Scan

If we look at a single host scan like:

```
srv001:/opt/share # nmap 192.168.0.100 -sn -oA host
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-25 00:06 CEST
Nmap scan report for kalirp (192.168.0.100)
Host is up (0.00013s latency).
MAC Address: DC:A6:32:38:B1:22 (Raspberry Pi Trading)
Nmap done: 1 IP address (1 host up) scanned in 0.09 seconds
```

We assume the discovery is with a `PING SCAN`. But if we add the `--packet-trace` option we can see that it was actually an `ARP PING` scan.

```
srv001:/opt/share # nmap 192.168.0.100 -sn -oA host -PE --packet-trace
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-25 00:08 CEST
SENT (0.0478s) ARP who-has 192.168.0.100 tell 192.168.0.10
RCVD (0.0479s) ARP reply 192.168.0.100 is-at DC:A6:32:38:B1:22
Nmap scan report for kalirp (192.168.0.100)
Host is up (0.00012s latency).
MAC Address: DC:A6:32:38:B1:22 (Raspberry Pi Trading)
Nmap done: 1 IP address (1 host up) scanned in 0.10 seconds
```

If we want to know why the host was classiefied as up we can use the `--reason` option

```
srv001:/opt/share # nmap 192.168.0.100 -sn -oA host -PE --reason
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-25 00:10 CEST
Nmap scan report for kalirp (192.168.0.100)
Host is up, received arp-response (0.000097s latency).
MAC Address: DC:A6:32:38:B1:22 (Raspberry Pi Trading)
Nmap done: 1 IP address (1 host up) scanned in 0.10 seconds

```

If we want to disable the `ARP PING` and specifically use `ICMP PING`, we can do thath with the option `--disable-arp-ping `

```
srv001:/opt/share # nmap 192.168.0.100 -sn -oA host -PE --packet-trace --disable-arp-ping
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-25 00:12 CEST
SENT (0.0675s) ICMP [192.168.0.10 > 192.168.0.100 Echo request (type=8/code=0) id=62396 seq=0] IP [ttl=42 id=59438 iplen=28 ]
RCVD (0.0676s) ICMP [192.168.0.100 > 192.168.0.10 Echo reply (type=0/code=0) id=62396 seq=0] IP [ttl=64 id=58669 iplen=28 ]
Nmap scan report for kalirp (192.168.0.100)
Host is up (0.00014s latency).
MAC Address: DC:A6:32:38:B1:22 (Raspberry Pi Trading)
Nmap done: 1 IP address (1 host up) scanned in 0.13 seconds
```

Network scanning strategies can b e found here: [https://nmap.org/book/host-discovery-strategies.html](https://nmap.org/book/host-discovery-strategies.html)

### Default TTL for different hosts

At last, we can try to figure out what OS (roughly) is running on the last scanned host only by TTL. We can se that the TTL is 64 and searching the internet for "Default TTL OS" we can see that it has to be a Linux system.

| TTL  | OS                |
| ---- | ----------------- |
| 64   | *nix (Linux/Unix) |
| 128  | Windows           |
| 256  | Solaris/AIX       |

More TTL values can be found here: [https://subinsb.com/default-device-ttl-values/](https://subinsb.com/default-device-ttl-values/)



## Host and Port Scanning

After the initial host discovery, we want to find out the services that run on the host. Therefore we scan for ports, service versions on discovered ports, maybe some information those services provide and te OS version if possible.

### Port States

| **State**         | **Description**                                              |
| ----------------- | ------------------------------------------------------------ |
| `open`            | This indicates that the connection to the scanned port has been established. These connections can be **TCP connections**, **UDP datagrams** as well as **SCTP associations**. |
| `closed`          | When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an `RST` flag. This scanning method can also be used to determine if our target is alive or not. |
| `filtered`        | Nmap cannot correctly identify whether the scanned port is open or  closed because either no response is returned from the target for the  port or we get an error code from the target. |
| `unfiltered`      | This state of a port only occurs during the **TCP-ACK** scan and means that the port is accessible, but it cannot be determined whether it is open or closed. |
| `open|filtered`   | If we do not get a response for a specific port, `Nmap` will set it to that state. This indicates that a firewall or packet filter may protect the port. |
| `closed|filtered` | This state only occurs in the **IP ID idle** scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall. |

### TCP Port Discovery

By default, `Nmap` scans the top 1000 TCP ports with the `SYN scan` (-sS). The `SYN` scan is set to default if we run nmap as a privileged uses (`root`). Otherwise a `TCP Scan` (-sT) is performed as default. 

There are different options to define the ports to scan



| Ports          | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| -p-            | Scan all ports (65535 ports, a 16 bit unsigned number)       |
| -p 53          | Scan port 53                                                 |
| -p 53,80,443   | Scan ports 53, 80 ,443                                       |
| -p 53-100      | Scan port range from port 53 up to port 100                  |
| --top-ports=10 | Scan the top 10 ports. Top ports is nmaps own list of ports that are usually open. The maker of nmap scanned he scanned most of the Internet and determined which ports are *usually* open, and he built lists of the top ports for use within `nmap`. [https://danielmiessler.com/blog/nmap-use-the-top-ports-option-for-both-tcp-and-udp-simultaneously/](https://danielmiessler.com/blog/nmap-use-the-top-ports-option-for-both-tcp-and-udp-simultaneously/). --top-ports usually scans only `TCP` ports. To scan `UDP` as well, use `nmap -sTU --top-ports`. An basic scan would be `nmap -vv -O -P0 -sTUV –top-ports 1000 -oA target $target` |
| -F             | Fast port scan (top 100 ports)                               |

##### Port States Deep-Dive

```
srv001:/opt/share # nmap 10.129.222.195 --top-ports=10 
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-25 11:23 CEST
Nmap scan report for 10.129.222.195
Host is up (0.038s latency).

PORT     STATE    SERVICE
21/tcp   closed   ftp
22/tcp   open     ssh
23/tcp   closed   telnet
25/tcp   open     smtp
80/tcp   open     http
110/tcp  open     pop3
139/tcp  filtered netbios-ssn
443/tcp  closed   https
445/tcp  filtered microsoft-ds
3389/tcp closed   ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 0.22 seconds
```

Let's take a look why some states are closes or filtered. Let's take port 21/ftp. Also, let's clear some things for a better view.
We are going to use `--packet-trace`, disable `TCP ping` and `ARP ping` (`-Pn` and `--disable-arp-ping`) and disable DNS resolution `-n`

```
srv001:/opt/share # nmap 10.129.37.146 -p 21 --packet-trace -Pn -n --disable-arp-ping --reason
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-25 12:24 CEST
SENT (0.0759s) TCP 10.10.15.152:47521 > 10.129.37.146:21 S ttl=40 id=32078 iplen=44  seq=1160986713 win=1024 <mss 1460>
RCVD (0.1159s) TCP 10.129.37.146:21 > 10.10.15.152:47521 RA ttl=63 id=59050 iplen=40  seq=0 win=0 
Nmap scan report for 10.129.37.146
Host is up, received user-set (0.040s latency).

PORT   STATE  SERVICE REASON
21/tcp closed ftp     reset ttl 63

Nmap done: 1 IP address (1 host up) scanned in 0.18 seconds
```

As we can see, a `SYN` request is sent and the server answers with `ACK, RST`. That means, for some reason, the service does not want to accept the packet. It acknowledges the packet but it drops it with a reset. 

Let's check the port 139, a filtered port

```
srv001:/opt/share # nmap 10.129.37.146 -p 139 --packet-trace -Pn -n --disable-arp-ping --reason
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-25 12:36 CEST
SENT (0.0620s) TCP 10.10.15.152:36362 > 10.129.37.146:139 S ttl=41 id=57032 iplen=44  seq=2129173419 win=1024 <mss 1460>
RCVD (1.1010s) TCP 10.129.37.146:139 > 10.10.15.152:36362 S ttl=63 id=0 iplen=44  seq=229234553 win=29200 <mss 1357>
Nmap scan report for 10.129.37.146
Host is up, received user-set (0.039s latency).

PORT    STATE SERVICE     REASON
139/tcp filtered  netbios-ssn syn ttl 63

Nmap done: 1 IP address (1 host up) scanned in 0.14 seconds
```

We can see that 2 packets were sent. The first one took only 0.06 s but the second one took a whole second. That speaks for a dropped packet. If the time were similar and the server returns a failed ICMP response (type 3, code 3), the firewall is rejecting the packet. If we know for sure the host is up, that means this port is somehow accesible and we need to investigate it further. 

### UDP Port Discovery

Since `UDP` is a stateless protocol, we don't get any acknowledgment. Therefore `UDP` scans take a lot longer than `TCP` scans. A typical `UDP` scan looks like this (we use `-F` for the top 100 ports): 

```
srv001:/opt/share # nmap 10.129.37.146 -sU -F
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-25 13:00 CEST
Nmap scan report for 10.129.37.146
Host is up (0.039s latency).
Not shown: 97 closed udp ports (port-unreach)
PORT    STATE         SERVICE
68/udp  open|filtered dhcpc
137/udp open          netbios-ns
138/udp open|filtered netbios-dgm

Nmap done: 1 IP address (1 host up) scanned in 112.46 seconds
```



### XML Output to HTML

XML output can be easily converted to HTML wit the tool `xsltproc`

```
Bonzo@htb[/htb]$ xsltproc target.xml -o target.html
```

More about output formats: [https://nmap.org/book/output.html](https://nmap.org/book/output.html)



## Service Enumeration

After we discover all ports, it is important to find out as much as possible about this ports. It is essential to determine the application and version. With an exact version we can search for publicly available exploits or weaknesses.

### Service Version Detection

We add the `-sV` option to scan for applications and their versions.

```
srv001:/opt/share # nmap 10.129.239.85 -p- -sV
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-25 17:25 CEST
Nmap scan report for 10.129.239.85
Host is up (0.037s latency).
Not shown: 65528 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http        Apache httpd 2.4.18 ((Ubuntu))
110/tcp   open  pop3        Dovecot pop3d
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp   open  imap        Dovecot imapd
445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
31337/tcp open  Elite?
Service Info: Host: NIX-NMAP-DEFAULT; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 174.06 seconds
```

Scans can take a long time. We can press ++space++ to let nmap show us the current status or we can use the parameter `--stats-every=5s`. The time can be set as you wish, in seconds `s` or minutes `s`.  We can use the `-v` or `-vv` option to get the scan be more verbose and to show us detected ports as they are discovered. Sometimes `nmap` misses the version and we can try to get it manually via `nc`

```
srv001:/opt/share # nc -nv 10.129.239.85 22
Connection to 10.129.239.85 22 port [tcp/*] succeeded!
SSH-2.0-OpenSSH_7.2p2 Ubuntu-4ubuntu2.10
```

!!! note
    Give `nc` some time to finish the screen grabbing, it can tak a while to get a result

## Nmap Scripting Engine (NSE)

`NSE` allows users to write (and share) simple scripts (using the [Lua programming language](http://lua.org)) to automate a wide variety of networking tasks.

There are 14 categories in wich the scripts are divided.

| **Category** | **Description**                                              |
| ------------ | ------------------------------------------------------------ |
| `auth`       | Determination of authentication credentials.                 |
| `broadcast`  | Scripts, which are used for host discovery by broadcasting and the  discovered hosts, can be automatically added to the remaining scans. |
| `brute`      | Executes scripts that try to log in to the respective service by brute-forcing with credentials. |
| `default`    | Default scripts executed by using the `-sC` option.          |
| `discovery`  | Evaluation of accessible services.                           |
| `dos`        | These scripts are used to check services for denial of service vulnerabilities and are used less as it harms the services. |
| `exploit`    | This category of scripts tries to exploit known vulnerabilities for the scanned port. |
| `external`   | Scripts that use external services for further processing.   |
| `fuzzer`     | This uses scripts to identify vulnerabilities and unexpected packet  handling by sending different fields, which can take much time. |
| `intrusive`  | Intrusive scripts that could negatively affect the target system. |
| `malware`    | Checks if some malware infects the target system.            |
| `safe`       | Defensive scripts that do not perform intrusive and destructive access. |
| `version`    | Extension for service detection.                             |
| `vuln`       | Identification of specific vulnerabilities.                  |

The `default` category is executed bey default using the `-sC` option. 

You can specify script categories or specific scripts with `--script <category>` or `--script <scrip>,<script>,...`
You can scan for vulnerabilities with the category `vuln` .

```shell-session
Bonzo@htb[/htb]$ sudo nmap 10.129.2.28 -p 80 -sV --script vuln 

Nmap scan report for 10.129.2.28
Host is up (0.036s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-enum:
|   /wp-login.php: Possible admin folder
|   /readme.html: Wordpress version: 2
|   /: WordPress version: 5.3.4
|   /wp-includes/images/rss.png: Wordpress version 2.2 found.
|   /wp-includes/js/jquery/suggest.js: Wordpress version 2.5 found.
|   /wp-includes/images/blank.gif: Wordpress version 2.6 found.
|   /wp-includes/js/comment-reply.js: Wordpress version 2.7 found.
|   /wp-login.php: Wordpress login page.
|   /wp-admin/upgrade.php: Wordpress login page.
|_  /readme.html: Interesting, a readme.
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-wordpress-users:
| Username found: admin
|_Search stopped at ID #25. Increase the upper limit if necessary with 'http-wordpress-users.limit'
| vulners:
|   cpe:/a:apache:http_server:2.4.29:
|     	CVE-2019-0211	7.2	https://vulners.com/cve/CVE-2019-0211
|     	CVE-2018-1312	6.8	https://vulners.com/cve/CVE-2018-1312
|     	CVE-2017-15715	6.8	https://vulners.com/cve/CVE-2017-15715
<SNIP>
```

 

## Performance

Scanning performance plays a significant role when we need to scan an  extensive network or are dealing with low network bandwidth. We can use  various options to tell `Nmap` how fast (`-T <1-5>`), with which frequency (`--min-parallelism <number>`), which timeouts (`--max-rtt-timeout <time>`) the test packets should have, how many packets should be sent simultaneously (`--min-rate <number>`), and with the number of retries (`--max-retries <number>`) for the scanned ports the targets should be scanned.

To make things easier, we can use templates with predifined options. We use the templated with `-T <number>`.
Here are the options used for each template. By default, all nmap scans run on `–T3` timing template. 

|                                                   | T0                                        | T1     | T2     | T3      | T4         | T5      |
| ------------------------------------------------- | ----------------------------------------- | ------ | ------ | ------- | ---------- | ------- |
| Name                                              | Paranoid                                  | Sneaky | Polite | Normal  | Aggressive | Insane  |
| `min-rtt-timeout`                                 | 100                                       | 100    | 100    | 100     | 100        | 50      |
| `max-rtt-timeout`                                 | 300,000                                   | 15,000 | 10,000 | 10,000  | 1,250      | 300     |
| `initial-rtt-timeout`                             | 300,000                                   | 15,000 | 1,000  | 1,000   | 500        | 250     |
| `max-retries`                                     | 10                                        | 10     | 10     | 10      | 6          | 2       |
| Initial (and minimum) scan delay (`--scan-delay`) | 300,000                                   | 15,000 | 400    | 0       | 0          | 0       |
| Maximum TCP scan delay                            | 300,000                                   | 15,000 | 1,000  | 1,000   | 10         | 5       |
| Maximum UDP scan delay                            | 300,000                                   | 15,000 | 1,000  | 1,000   | 1,000      | 1,000   |
| `host-timeout`                                    | 0                                         | 0      | 0      | 0       | 0          | 900,000 |
| `min-parallelism`                                 | Dynamic, not affected by timing templates |        |        |         |            |         |
| `max-parallelism`                                 | 1                                         | 1      | 1      | Dynamic | Dynamic    | Dynamic |
| `min-hostgroup`                                   | Dynamic, not affected by timing templates |        |        |         |            |         |
| `max-hostgroup`                                   | Dynamic, not affected by timing templates |        |        |         |            |         |
| `min-rate`                                        | No minimum rate limit                     |        |        |         |            |         |
| `max-rate`                                        | No maximum rate limit                     |        |        |         |            |         |
| `defeat-rst-ratelimit`                            | Not enabled by default                    |        |        |         |            |         |


## Firewall and IDS/IPS Evasion

Nmap uses fragmentation of packets, the use of decoys, and others to evade IDS/IPS.

### Firewalls

A firewall is a security measure against unauthorized connection  attempts from external networks. Every firewall security system is based on a software component that monitors network traffic between the  firewall and incoming data connections and decides how to handle the  connection based on the rules that have been set. It checks whether  individual network packets are being passed, ignored, or blocked. This  mechanism is designed to prevent unwanted connections that could be  potentially dangerous.

------

### IDS/IPS

Like the firewall, the intrusion detection system (`IDS`) and intrusion prevention system (`IPS`) are also software-based components. `IDS` scans the network for potential attacks, analyzes them, and reports any detected attacks. `IPS` complements `IDS` by taking specific defensive measures if a potential attack should have been detected. The analysis of such attacks is based on pattern  matching and signatures. If specific patterns are detected, such as a  service detection scan, `IPS` may prevent the pending connection attempts.

### Firewall evasion

If a firewall blocks our scans, we can try `ACK` or `Connected` scan (`-sA` and `-sT`). It's much harder for a firewall or IDS/IPS.

#### Scan by Using Decoys

There are cases in which administrators block specific subnets from  different regions in principle. This prevents any access to the target  network. Another example is when IPS should block us. For this reason,  the Decoy scanning method (`-D`) is the right choice. With  this method, Nmap generates various random IP addresses inserted into  the IP header to disguise the origin of the packet sent. With this method, we can generate random (`RND`) a specific number (for example: `5`) of IP addresses separated by a colon (`:`). 

```shell-session
Bonzo@htb[/htb]$ sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-21 16:14 CEST
SENT (0.0378s) TCP 102.52.161.59:59289 > 10.129.2.28:80 S ttl=42 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460>
SENT (0.0378s) TCP 10.10.14.2:59289 > 10.129.2.28:80 S ttl=59 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460>
SENT (0.0379s) TCP 210.120.38.29:59289 > 10.129.2.28:80 S ttl=37 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460>
SENT (0.0379s) TCP 191.6.64.171:59289 > 10.129.2.28:80 S ttl=38 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460>
SENT (0.0379s) TCP 184.178.194.209:59289 > 10.129.2.28:80 S ttl=39 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460>
SENT (0.0379s) TCP 43.21.121.33:59289 > 10.129.2.28:80 S ttl=55 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460>
RCVD (0.1370s) TCP 10.129.2.28:80 > 10.10.14.2:59289 SA ttl=64 id=0 iplen=44  seq=4056111701 win=64240 <mss 1460>
Nmap scan report for 10.129.2.28
Host is up (0.099s latency).

PORT   STATE SERVICE
80/tcp open  http
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 0.15 seconds
```

### Scan by Using Different Source IP

We add the source IP with the option `-S`

```
Bonzo@htb[/htb]$ sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-22 01:16 CEST
Nmap scan report for 10.129.2.28
Host is up (0.010s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Linux 2.6.32 - 2.6.35 (94%), Linux 2.6.32 - 3.5 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 4.11 seconds

```

### DNS Proxying

We can use the DNS server port (53) to scan from this port. The reason to do that is that this port is often not blocked and we can do a scan on the internal network if the source port is port 53

```
Bonzo@htb[/htb]$ sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53

SENT (0.0482s) TCP 10.10.14.2:53 > 10.129.2.28:50000 S ttl=58 id=27470 iplen=44  seq=4003923435 win=1024 <mss 1460>
RCVD (0.0608s) TCP 10.129.2.28:50000 > 10.10.14.2:53 SA ttl=64 id=0 iplen=44  seq=540635485 win=64240 <mss 1460>
Nmap scan report for 10.129.2.28
Host is up (0.013s latency).

PORT      STATE SERVICE
50000/tcp open  ibm-db2
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 0.08 seconds
```

To figure out more about a service that is open if we scan from port 53, we have to connect to it. We can do that with `nc` and adding the `-p` option to define the source port

```
Bonzo@htb[/htb]$ ncat -nv -p 53 10.129.2.28 50000

Ncat: Version 7.80 ( https://nmap.org/ncat )
Ncat: Connected to 10.129.2.28:50000.
220 ProFTPd
```

