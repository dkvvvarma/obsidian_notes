
![[Pasted image 20250407145323.png]]

| Machine Name         | Nibbles                                                                                                  |
| -------------------- | -------------------------------------------------------------------------------------------------------- |
| Creator              | mrb3n                                                                                                    |
| Operating System     | Linux                                                                                                    |
| Difficulty           | Easy                                                                                                     |
| User Path            | Web                                                                                                      |
| Privilege Escalation | World-writable File / Sudoers Misconfiguration                                                           |
| Ippsec Video         | [https://www.youtube.com/watch?v=s_0GcRGv6Ds](https://www.youtube.com/watch?v=s_0GcRGv6Ds)               |
| Walkthrough          | [https://0xdf.gitlab.io/2018/06/30/htb-nibbles.html](https://0xdf.gitlab.io/2018/06/30/htb-nibbles.html) |


 There are three main types, `black-box`, `grey-box`, and `white-box`, and each differs in the goal and approach.

|**Engagement**|**Description**|
|---|---|
|`Black-Box`|Low level to no knowledge of a target. The penetration tester must perform in-depth reconnaissance to learn about the target. This may be an external penetration test where the tester is given only the company name and no further information such as target IP addresses, or an internal penetration test where the tester either has to bypass controls to gain initial access to the network or can connect to the internal network but has no information about internal networks/hosts. This type of penetration test most simulates an actual attack but is not as comprehensive as other assessment types and could leave misconfigurations/vulnerabilities undiscovered.|
|`Grey-Box`|In a grey-box test, the tester is given a certain amount of information in advance. This may be a list of in-scope IP addresses/ranges, low-level credentials to a web application or Active Directory, or some application/network diagrams. This type of penetration test can simulate a malicious insider or see what an attacker can do with a low level of access. In this scenario, the tester will typically spend less time on reconnaissance and more time looking for misconfigurations and attempting exploitation.|
|`White-Box`|In this type of test, the tester is given complete access. In a web application test, they may be provided with administrator-level credentials, access to the source code, build diagrams, etc., to look for logic vulnerabilities and other difficult-to-discover flaws. In a network test, they may be given administrator-level credentials to dig into Active Directory or other systems for misconfigurations that may otherwise be missed. This assessment type is highly comprehensive as the tester will have access to both sides of a target and perform a comprehensive analysis.|

For Nibbles we are going with `Grey Box` Testing.

Intial nmap scan

```Bash
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Modules]
└─$ nmap -sV -sC 10.129.51.239  
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-09 21:41 IST
Nmap scan report for 10.129.51.239
Host is up (0.30s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c4:f8:ad:e8:f8:04:77:de:cf:15:0d:63:0a:18:7e:49 (RSA)
|   256 22:8f:b1:97:bf:0f:17:08:fc:7e:2c:8f:e9:77:3a:48 (ECDSA)
|_  256 e6:ac:27:a3:b5:a9:f1:12:3c:34:a5:5d:5b:eb:3d:e9 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.43 seconds
```



```Bash
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Modules]
└─$ whatweb 10.129.51.239                            
http://10.129.51.239 [200 OK] Apache[2.4.18], Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][Apache/2.4.18 (Ubuntu)], IP[10.129.51.239]
```

![[Pasted image 20250409214726.png]]
