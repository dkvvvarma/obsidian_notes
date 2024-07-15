


##### **Room link: [GoldenEye](https://tryhackme.com/r/room/goldeneye)**




#### **TASK-1: Intro**

Deployed the machine and connect to ovpn in my attacker machine 

1.**First things first, connect to our network and deploy the machine.**
   
   Launched an NMAP scan with `nmap -sV -p- 10.10.74.253 -Pn
    
    ![[Pasted image 20240712140501.png]]
2. **Use nmap to scan the network for all ports. How many ports are open?**
  
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

If you're on Linux edit your "/etc/hosts" file and add:

<machines ip> severnaya-station.com

**If you're on Windows do the same but in the "c:\Windows\System32\Drivers\etc\hosts" file**

**Once you have done that, in your browser navigate to: http://severnaya-station.com/gnocertdir.**


**Try using the credentials you found earlier. Which user can you login as?**

 Ans: `Xenia`

1.png

Have a poke around the site. What other user can you find?

2.png

Ans:  `Doak`


