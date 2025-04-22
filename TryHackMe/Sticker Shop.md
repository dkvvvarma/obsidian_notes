


![[Pasted image 20241216143802.png]]


Start up the virtual machine 

Then perform an Ip scan with nmap to check for open ports.

```bash
┌──(dkvv㉿kali)-[~/Desktop/tryhackme/stickershop]
└─$ nmap -sC -A 10.10.55.10
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-12-16 14:34 IST
Nmap scan report for 10.10.55.10
Host is up (0.20s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE    SERVICE    VERSION
22/tcp   open     ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 b2:54:8c:e2:d7:67:ab:8f:90:b3:6f:52:c2:73:37:69 (RSA)
|   256 14:29:ec:36:95:e5:64:49:39:3f:b4:ec:ca:5f:ee:78 (ECDSA)
|_  256 19:eb:1f:c9:67:92:01:61:0c:14:fe:71:4b:0d:50:40 (ED25519)
666/tcp  filtered doom
8080/tcp open     http-proxy Werkzeug/3.0.1 Python/3.8.10

```


There are multiple open ports let's check out the webpage.

