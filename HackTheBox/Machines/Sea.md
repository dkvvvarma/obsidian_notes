
Linux   Easy [Machine](https://app.hackthebox.com/machines/Sea)





FootHolding

Intial Nmap scans reveal multiple ports are open on this linux system

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/sea]
└─$ nmap -sV -sC -Pn 10.10.11.28
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-10-20 15:01 IST
Nmap scan report for 10.10.11.28
Host is up (0.26s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 e3:54:e0:72:20:3c:01:42:93:d1:66:9d:90:0c:ab:e8 (RSA)
|   256 f3:24:4b:08:aa:51:9d:56:15:3d:67:56:74:7c:20:38 (ECDSA)
|_  256 30:b1:05:c6:41:50:ff:22:a3:7f:41:06:0e:67:fd:50 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Sea - Home
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.52 seconds

```


So . add the `ip addr` and domain `sea.htb` to your local dns file. Before moving forward

## Web App 
Let's check the running service on port `80`

![[Pasted image 20241020152548.png]]

Seems like its hosting some website for night biking event name `velik71`, reading through the info, it says we can register for event at `/contacts`

