

For today's adventure let's deep dive and pwn the BoardLight machine from HackTheBox.The machine link is [here](https://app.hackthebox.com/machines/BoardLight)

Let' start pwning and check whether is up or not by pinging. If up move forward with `nmap` port scan.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/boardlight]
└─$ nmap -sCV -p- --min-rate 5000 -T3 10.10.11.11
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-23 11:32 IST
Nmap scan report for 10.10.11.11
Host is up (0.50s latency).
Not shown: 64864 filtered tcp ports (no-response), 669 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 06:2d:3b:85:10:59:ff:73:66:27:7f:0e:ae:03:ea:f4 (RSA)
|   256 59:03:dc:52:87:3a:35:99:34:44:74:33:78:31:35:fb (ECDSA)
|_  256 ab:13:38:e4:3e:e0:24:b4:69:38:a9:63:82:38:dd:f4 (ED25519)
80/tcp open  http    Apache httpd 2.4.41
Service Info: Host: board.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 172.61 seconds
```

There two up and running ports `22` and `80`, before trying to access port `80`  add the domain `board.htb` to your local dns file `/etc/hosts` since this is a vHost