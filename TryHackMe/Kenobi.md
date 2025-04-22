## Task -1
Start the machine and perform intial Nmap scan

```bash
┌──(dkvv㉿OMEN)-[~/Desktop/TryHackMe/Kenobi]
└─$ nmap -sV -sC 10.10.138.199
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 22:38 IST
Nmap scan report for 10.10.138.199
Host is up (0.17s latency).
Not shown: 993 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         ProFTPD 1.3.5
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b3:ad:83:41:49:e9:5d:16:8d:3b:0f:05:7b:e2:c0:ae (RSA)
|   256 f8:27:7d:64:29:97:e6:f8:65:54:65:22:f7:c8:1d:8a (ECDSA)
|_  256 5a:06:ed:eb:b6:56:7e:4c:01:dd:ea:bc:ba:fa:33:79 (ED25519)
80/tcp   open  http        Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/admin.html
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
111/tcp  open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      33667/tcp6  mountd
|   100005  1,2,3      50987/udp6  mountd
|   100005  1,2,3      58119/udp   mountd
|   100005  1,2,3      59463/tcp   mountd
|   100021  1,3,4      35325/tcp6  nlockmgr
|   100021  1,3,4      38309/udp   nlockmgr
|   100021  1,3,4      46011/tcp   nlockmgr
|   100021  1,3,4      50685/udp6  nlockmgr
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
2049/tcp open  nfs         2-4 (RPC #100003)
Service Info: Host: KENOBI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb2-time: 
|   date: 2025-03-24T17:08:35
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_clock-skew: mean: 1h39m59s, deviation: 2h53m12s, median: -1s
|_nbstat: NetBIOS name: KENOBI, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: kenobi
|   NetBIOS computer name: KENOBI\x00
|   Domain name: \x00
|   FQDN: kenobi
|_  System time: 2025-03-24T12:08:35-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 26.50 seconds
```


## Task-2

Samba commonly used by users to access and utilise files, printers, authentication, domain integration and other shared resources in a company's internet and intranet often referred as "Network File System".

Samba basically an open source software suite that allows Linux and Unix system to communicate with windows devices based on a common Client/Server protocol called `Server Message block(SMB) protocol`.

It primarily uses the following port numbers
- TCP 139 - netBIOS-SSn (session service) Used for oldern SMB version
- TCP 445 - Direct SMB over TCP (Used by modern SMB Communication)(The key port in modern Communications)
- UDP 137 - NetBIOS Name Service (For name Resolution)
- UDP 138 - NetBIOS Datagram Service(For Browser elections and messaging.)

### **Key Features of Samba:**

1. **File and Printer Sharing:**
    - Allows Linux/Unix servers to share files and printers with Windows clients.
    - Provides seamless integration with Windows file explorer.
        
2. **Active Directory Integration:**
    - Can act as a **Domain Controller (DC)** to manage user authentication.
    - Supports Kerberos and LDAP for authentication.
        
3. **Interoperability with Windows:**
    - Supports **SMB/CIFS (Common Internet File System)** protocols.
    - Compatible with different Windows versions for network communication.
        
4. **User Authentication & Access Control:**
    - Uses NTLM and Kerberos authentication.
    - Can enforce permissions like Windows ACLs (Access Control Lists).
        
5. **Cross-Platform Compatibility:**
    - Works on Linux, Unix, and macOS, enabling them to interact with Windows.
---
### **How Samba Works?**

1. A Samba server is installed on a Linux/Unix machine.
2. It runs services like `smbd` (for file sharing) and `nmbd` (for name resolution).
3. A Windows machine connects to the Samba share via the network using SMB.
4. Users authenticate using their credentials, and access is granted based on configured permissions.
---

### **Why is Samba Important?**

- Enables Windows-Linux integration in enterprise networks.
- Facilitates easy file sharing without additional software.
- Reduces dependency on Windows servers, cutting costs.


Since SMB is specifically developed for Windows environments without Samba other computer platforms will get isolated from windows machines even if they are part of same network.

##### **Nmap Script to perform SMB shares Enumeration**

![[Pasted image 20250324225722.png]]

```bash
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.138.199
```

1. Using the nmap command above, how many shares have been found?
```Bash
┌──(dkvv㉿OMEN)-[~/Desktop/TryHackMe/Kenobi]
└─$ nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.138.199
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 22:55 IST
Nmap scan report for 10.10.138.199
Host is up (0.17s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares: 
|   account_used: guest
|   \\10.10.138.199\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (kenobi server (Samba, Ubuntu))
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.138.199\anonymous: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\home\kenobi\share
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.138.199\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|_    Current user access: <none>
Nmap done: 1 IP address (1 host up) scanned in 28.95 seconds
```

We can deduce 3 shares from the results `\IPC`, `\anonymous` and `\print`

Let's Connect to one of SMB shares

```Bash
smbclient //10.10.138.199/anonymous
```

Smbclient is a Linux Command line tool used to interact with SMB/CIFS shares on Windows and Samba servers. It functions like an FTP client allowing you to list , upload and download files from SMB shares.

### **Basic Syntax:**
```Bash
smbclient -U <username> //<server>/<share>
```
- `<username>` → The user accessing the share.
- `<server>` -> The hostname or IP address of the SMB server.
- `<share>` → The name of the shared folder.


Smbget - Similar to Wget in shell, Smbget is used to recursively download files from SMB share.

### **Command Breakdown:**

```Bash
smbget -R smb://10.10.138.199/anonymous`
```
- `-R` → **Recursive download** (fetches all files and subdirectories).
- `smb://10.10.138.199/anonymous` → The SMB share located at **10.10.138.199**, named **anonymous**.

---

### **Authentication with SMBGet**
If the share requires authentication, use:

```Bash
smbget -R smb://10.10.138.199/anonymous -U <username>
```

It will prompt for a password. To specify the password in the command:

```Bash
smbget -R smb://10.10.138.199/anonymous -U <username>%<password>
```

For anonymous login (if no password is needed):

```Bash
smbget -R smb://10.10.138.199/anonymous -U anonymous
```


2. What port is FTP running on?

After downloading the log.txt file you can find the answer.

3. our earlier nmap port scan will have shown port 111 running the service rpcbind. This is just a server that converts remote procedure call (RPC) program number into universal addresses. When an RPC service is started, it tells rpcbind the address at which it is listening and the RPC program number its prepared to serve. 

In our case, port 111 is access to a network file system. Lets use nmap to enumerate this.

```Bash
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.138.199
```
   What mount can we see?

```Bash
┌──(dkvv㉿OMEN)-[~/Desktop/TryHackMe/Kenobi]
└─$ nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.138.199
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 23:44 IST
Nmap scan report for 10.10.138.199
Host is up (0.16s latency).

PORT    STATE SERVICE
111/tcp open  rpcbind
| nfs-ls: Volume /var
|   access: Read Lookup NoModify NoExtend NoDelete NoExecute
| PERMISSION  UID  GID  SIZE  TIME                 FILENAME
| rwxr-xr-x   0    0    4096  2019-09-04T08:53:24  .
| rwxr-xr-x   0    0    4096  2019-09-04T12:27:33  ..
| rwxr-xr-x   0    0    4096  2019-09-04T12:09:49  backups
| rwxr-xr-x   0    0    4096  2019-09-04T10:37:44  cache
| rwxrwxrwx   0    0    4096  2019-09-04T08:43:56  crash
| rwxrwsr-x   0    50   4096  2016-04-12T20:14:23  local
| rwxrwxrwx   0    0    9     2019-09-04T08:41:33  lock
| rwxrwxr-x   0    108  4096  2019-09-04T10:37:44  log
| rwxr-xr-x   0    0    4096  2019-01-29T23:27:41  snap
| rwxr-xr-x   0    0    4096  2019-09-04T08:53:24  www
|_
| nfs-statfs: 
|   Filesystem  1K-blocks  Used       Available  Use%  Maxfilesize  Maxlink
|_  /var        9204224.0  1836528.0  6877100.0  22%   16.0T        32000
| nfs-showmount: 
|_  /var *

Nmap done: 1 IP address (1 host up) scanned in 4.15 seconds
```

`/var` is the share

Remote PRoceedure Call(RPC) is protocol that allows a system to execute commands or services on another system remotely. In windows, SMB uses  RPC to manage services, users and other resources.

`RPCClient` is part of Samba suite that allows user to interact with RPC services on Windows and Samba servers. Its useful in enumeration, user listing and administrative tasks on SMB-enabled systems.

### **Basic Usage**

To connect to a remote SMB service:

```Bash
rpcclient -U "" -N 10.10.138.199`
```
- `-U ""` → Connect as an **anonymous user**
- `-N` → No password
- `10.10.138.199` → Target SMB server

Once connected, you can enter commands to enumerate users, shares, policies, etc.

---

## Task-3
#### Proftpd

Proftpd(Professional FTP Daemon) is a popular open-source FTP server used in Linux Environments since it is compatible in both Unix and Windows. It's known for being highly configurable, supporting anonymous access, TLS encryption and virtual hosting.

It is also found to be vulnerable in past software versions.

### **Common Ports Used by ProFTPD**

| **Port** | **Protocol** | **Description**                      |
| -------- | ------------ | ------------------------------------ |
| 21       | TCP          | Default FTP control port             |
| 20       | TCP          | FTP data transfer port (active mode) |
| 990      | TCP          | FTPS (FTP Secure)                    |
| 50000+   | TCP          | Passive mode data transfer           |

4. Lets get the version of ProFtpd. Use netcat to connect to the machine on the FTP port.

   What is the version?

```Bash
┌──(dkvv㉿OMEN)-[~/Desktop/TryHackMe/Kenobi]
└─$ nc 10.10.138.199 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.138.199]
```

5.  We can use searchsploit to find exploits for a particular software version.

  Searchsploit is basically just a command line search tool for exploit-db.com.

  How many exploits are there for the ProFTPd running?
  
![[Pasted image 20250325000009.png]]

5. You should have found an exploit from ProFtpd's [mod_copy module](http://www.proftpd.org/docs/contrib/mod_copy.html). 

The mod_copy module implements **SITE CPFR** and **SITE CPTO** commands, which can be used to copy files/directories from one place to another on the server. Any unauthenticated client can leverage these commands to copy files from any part of the filesystem to a chosen destination.

We know that the FTP service is running as the Kenobi user (from the file on the share) and an ssh key is generated for that user.



ProFTPD versions with the **mod_copy** module enabled (such as **ProFTPD 1.3.5**) can be exploited using the `SITE CPFR` and `SITE CPTO` commands. These allow **unauthenticated users** to copy files **from anywhere** on the system to a directory they can access.

---

### **1️⃣ Understanding the Vulnerability**

- `SITE CPFR` → **Copy From** (source file)
- `SITE CPTO` → **Copy To** (destination file)
- If ProFTPD is misconfigured, **anyone** can use these commands **without authentication** to access sensitive files.


6. What is Kenobi's user flag (/home/kenobi/user.txt)?

![[Pasted image 20250325002502.png]]




## Task- 4  Privilege Escalation with Path variable Manipulation

![[Pasted image 20250325003315.png]]

| **Permission**         | **On files**                                                                        | **On Directories**                                                                                                                                                               |
| ---------------------- | ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **SUID(Set User ID)**  | The file executes **with the owner's permissions**,regardless of who runs it.       | 🚫 (Not applicable)                                                                                                                                                              |
| **SGID(Set Group ID)** | The file executes **with the group owner's permissions**,regardless of who runs it. | **Files created inside inherit the group's ownership**, instead of user's primary group                                                                                          |
| **Sticky bit**         | 🚫 (Not applicable)                                                                 | Users are prevented from deleting files from other users. Basically they can delete their own files, even if they have write permission in the directory(commonly used in /tmp). |

7. SUID bits can be dangerous, some binaries such as passwd need to be run with elevated privileges (as its resetting your password on the system), however other custom files could that have the SUID bit can lead to all sorts of issues.

   To search the a system for these type of files run the following: find / -perm -u=s -type f 2>/dev/null

   What file looks particularly out of the ordinary?

```Bash
kenobi@kenobi:~$ find / -perm -u=s -type f 2>/dev/null
/sbin/mount.nfs
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/bin/chfn
/usr/bin/newgidmap
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/newuidmap
/usr/bin/gpasswd
"/usr/bin/menu"
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/at
/usr/bin/newgrp
/bin/umount
/bin/fusermount
/bin/mount
/bin/ping
/bin/su
/bin/ping6
```

8. Run the binary, how many options appear?

```Bash
kenobi@kenobi:~$ menu
***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :
```



Strings is a command on Linux that looks for human readable strings on a binary.

![](https://i.imgur.com/toHFALv.png)

This shows us the binary is running without a full path (e.g. not using /usr/bin/curl or /usr/bin/uname).

As this file runs as the root users privileges, we can manipulate our path gain a root shell.

![[Pasted image 20250325004222.png]]

We copied the /bin/sh shell, called it curl, gave it the correct permissions and then put its location in our path. This meant that when the /usr/bin/menu binary was run, its using our path variable to find the "curl" binary.. Which is actually a version of /usr/sh, as well as this file being run as root it runs our shell as root!

10. What is the root flag?

![[Pasted image 20250325004412.png]]




<details>
  <summary>Click to reveal</summary>
  User: d0b0f3f53b6caa532a83915e19224899
  Root: 177b3cd8562289f37382721c28381f02

</details>
