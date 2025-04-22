

|            |         |
| ---------- | ------- |
| OS         | Linux   |
| Difficulty |  Medium |

As always start with intial nmap scan

```bash
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/cypher]
└─$ nmap -sV -sC 10.10.11.57
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-08 21:56 IST
Nmap scan report for 10.10.11.57
Host is up (0.29s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 be:68:db:82:8e:63:32:45:54:46:b7:08:7b:3b:52:b0 (ECDSA)
|_  256 e5:5b:34:f5:54:43:93:f8:7e:b6:69:4c:ac:d6:3d:23 (ED25519)
80/tcp open  http    nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://cypher.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 28.33 seconds
```

Add the domain in hosts file.

A website 

![[Pasted image 20250308220003.png]]

lets perfom directory enumeration to know more

48a40e7a4329ac4505edcfb36f850458


b666d9ac37c720a80e65588ad9364638
