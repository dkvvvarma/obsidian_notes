
Today Let's try and crack the "[The Needle](https://app.hackthebox.com/challenges/The%2520Needle)" challenge in HackTheBox.

CHALLENGE DESCRIPTION

As a part of our SDLC process, we've got our firmware ready for security testing. Can you help us by performing a security assessment?


Let's download the necessary files


![[The Needle.zip]]

### Identification
Initially when I performed an Nmap scan on the machine
```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/challenges/TheNeedle]
└─$ nmap -Pn 94.237.59.24
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-10-16 11:30 IST
Nmap scan report for 94-237-59-24.uk-lon1.upcloud.host (94.237.59.24)
Host is up (0.23s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT    STATE SERVICE
22/tcp  open  ssh
111/tcp open  rpcbind
Nmap done: 1 IP address (1 host up) scanned in 36.11 seconds

```

There are 2 open ports. We don't know any credentials to log in ssh. So, let's checkout `rpcbind` 

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/challenges/TheNeedle]
└─$ nc 94.237.59.24 40209
��������
ng-2008231-hwtheneedle-qnrbk-5fff844c4b-xgvvj login:
```
Welp! It's asking for credentials. Let's check if we can find them in downloaded content.

After downloading the zip , extracting the contents I found `firmware.bin` folder.
Then I checked out the type of is that `firmware.bin`

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/challenges]
└─$ file firmware.bin 
firmware.bin: Linux kernel ARM boot executable zImage (big-endian)
```

### Analysis

Now, onto the tricky analysis part.

For this part, I'm using the tool `binwalk`. I tried to extract the contents of the `firmware.bin` folder using the command

```bash
binwalk -e firmaware.bin
```

It extracted with minor errors which can be neglected.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/challenges]
└─$ binwalk -e firmware.bin 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             Linux kernel ARM boot executable zImage (big-endian)
14419         0x3853          xz compressed data
14640         0x3930          xz compressed data

WARNING: Extractor.execute failed to run external extractor 'sasquatch -p 1 -le -d 'squashfs-root-0' '%e'': [Errno 2] No such file or directory: 'sasquatch', 'sasquatch -p 1 -le -d 'squashfs-root-0' '%e'' might not be installed correctly

WARNING: Extractor.execute failed to run external extractor 'sasquatch -p 1 -be -d 'squashfs-root-0' '%e'': [Errno 2] No such file or directory: 'sasquatch', 'sasquatch -p 1 -be -d 'squashfs-root-0' '%e'' might not be installed correctly

WARNING: Symlink points outside of the extraction directory: /home/dkvv/Desktop/htb/challenges/_firmware.bin.extracted/squashfs-root/var -> /tmp; changing link target to /dev/null for security purposes.

WARNING: Symlink points outside of the extraction directory: /home/dkvv/Desktop/htb/challenges/_firmware.bin.extracted/squashfs-root/etc/mtab -> /proc/8602/mounts; changing link target to /dev/null for security purposes.

WARNING: Symlink points outside of the extraction directory: /home/dkvv/Desktop/htb/challenges/_firmware.bin.extracted/squashfs-root/etc/localtime -> /tmp/localtime; changing link target to /dev/null for security purposes.

WARNING: Symlink points outside of the extraction directory: /home/dkvv/Desktop/htb/challenges/_firmware.bin.extracted/squashfs-root/etc/TZ -> /tmp/TZ; changing link target to /dev/null for security purposes.

WARNING: Symlink points outside of the extraction directory: /home/dkvv/Desktop/htb/challenges/_firmware.bin.extracted/squashfs-root/etc/resolv.conf -> /tmp/resolv.conf; changing link target to /dev/null for security purposes.

WARNING: Symlink points outside of the extraction directory: /home/dkvv/Desktop/htb/challenges/_firmware.bin.extracted/squashfs-root/etc/ppp/resolv.conf -> /tmp/resolv.conf.ppp; changing link target to /dev/null for security purposes.
538952        0x83948         Squashfs filesystem, little endian, version 4.0, compression:xz, size: 2068458 bytes, 995 inodes, blocksize: 262144 bytes, created: 2021-03-11 03:18:10

```

Now, let's check out the extracted contents.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/challenges/TheNeedle/_firmware.bin.extracted]
└─$ ls -la
total 36172
drwxrwxr-x  4 dkvv dkvv     4096 Oct 16 11:20 .
drwxrwxr-x  3 dkvv dkvv     4096 Oct 16 11:26 ..
-rw-rw-r--  1 dkvv dkvv 16762797 Oct 16 11:20 3853.xz
-rw-rw-r--  1 dkvv dkvv  1423244 Oct 16 11:20 3930
-rw-rw-r--  1 dkvv dkvv 16762576 Oct 16 11:20 3930.xz
-rw-rw-r--  1 dkvv dkvv  2068458 Oct 16 11:20 83948.squashfs
drwxr-xr-x 16 dkvv dkvv     4096 Oct 16 11:20 squashfs-root
drwxrwxr-x  2 dkvv dkvv     4096 Oct 16 11:20 squashfs-root-0
```

There are few files in the extracted contents. let's check out for those credentials in the folder using `grep` command.


```bash
grep -rn "./" -e login
```

The command `grep -rn "./" -e login` is used to search for the word "login" in the files within a given directory and its subdirectories. Here's a breakdown of the command:

1. **`grep`**:
   - Stands for **Global Regular Expression Print**. It is a command-line utility used to search text or patterns within files.

2. **`-r`**:
   - Stands for **recursive**. It tells `grep` to search through all files and subdirectories within the specified directory (`./` in this case) recursively.

3. **`-n`**:
   - Stands for **line numbers**. It tells `grep` to display the line number where the search term "login" is found in each file.

4. **`"./"`**:
   - Refers to the **current directory**. This is the path where the `grep` command will start searching. You can replace `./` with any specific directory path if you want to search elsewhere.

5. **`-e`**:
   - Stands for **expression**. It indicates the search pattern. In this case, it is looking for the term **"login"** in the files. `-e` is used when you want to specify the search pattern explicitly, but in many cases, you can omit it and just type the search term directly after `grep`.

6. **`login`**:
   - This is the **search term** or **pattern** you are looking for in the files. Here, it's the word "login".

The result of the command as follows

![[Pasted image 20241016114924.png]]

There isn't much useful data but it did find a username `Device_Admin`

Now, Let's look for a password. The password will be related to some `$sign`

Let's sue `find` command this time.

```bash
find ./ -name sign
```

The result of the command is as follows

![[Pasted image 20241016115331.png]]

The password is `qS6-X/n]u>fVfAt!`
Yup! we got the credentials. Now let's try out our luck with these by connecting to `rpcbind`
Let's get our flag now

![[Pasted image 20241016121441.png]]


<details>
  <summary>Click to reveal</summary>
 Flag : HTB{4_hug3_blund3r_d289a1_!!}
</details>


![[Pasted image 20241016121622.png]]