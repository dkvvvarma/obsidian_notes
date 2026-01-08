
## Introduction to Metasploit

The metasploit project is Ruby-Based, modular penetration testing platform that enables you to write,test and execute the exploit code. This exploit code can be custom made by user or taken from a database containing the latest already discovered and modularized exploits. The metasploit framework includes a suite of tools that you can use to test security vulnerabilities, enumerate networks, execute attacks and evade detection. At its core, the Metasploit Project is a collection of commonly  used tools that provide a complete environment for penetration testing and exploit development.

![[Pasted image 20250913221942.png]]

The modules mentioned are actual exploit proof-of-concepts that have already developed and tested in wild and integrated within framework to provide pentesters with ease of access to different attack vectors for different attack vectors for different platforms and services. Metasploit is not a jack of all trades but a swiss army knife with just enough tools to get us through most common unpatched vulnerabilities.

### Metasploit Pro

`Metasploit` as a product is split into two versions. The `Metasploit Pro` version is different from the `Metasploit Framework` one with some additional features:

- Task Chains
- Social Engineering
- Vulnerability Validations
- GUI
- Quick Start Wizards
- Nexpose Integration

If you're more of a command-line user and prefer the extra features, the Pro version also contains its own console, much like `msfconsole`.

To have a general idea of what Metasploit Pro's newest features can achieve, check out the list below:

| **Infiltrate**           | **Collect Data**         | **Remediate**             |
| ------------------------ | ------------------------ | ------------------------- |
| Manual Exploitation      | Import and Scan Data     | Bruteforce                |
| Anti-virus Evasion       | Discovery Scans          | Task Chains               |
| IPS/IDS Evasion          | Meta-Modules             | Exploitation Workflow     |
| Proxy Pivot              | Nexpose Scan Integration | Session Rerun             |
| Post-Exploitation        |                          | Task Replay               |
| Session Clean-up         |                          | Project Sonar Integration |
| Credentials Reuse        |                          | Session Management        |
| Social Engineering       |                          | Credential Management     |
| Payload Generator        |                          | Team Collaboration        |
| Quick Pen-testing        |                          | Web Interface             |
| VPN Pivoting             |                          | Backup and Restore        |
| Vulnerability Validation |                          | Data Export               |
| Phishing Wizard          |                          | Evidence Collection       |
| Web App Testing          |                          | Reporting                 |
| Persistent Sessions      |                          | Tagging Data              |
### Metasploit Framework Console

The `msfconsole` is probably the most popular interface to the `Metasploit Framework` `(MSF)`. It provides an "all-in-one" centralized console and allows you efficient access to virtually all options available in the `MSF`. `Msfconsole` may seem intimidating at first, but once you learn the syntax of the commands, you will learn to appreciate the power of utilizing this interface.

The features that `msfconsole` generally brings are the following:

- It is the only supported way to access most of the features within `Metasploit`
- Provides a console-based interface to the `Framework`
- Contains the most features and is the most stable `MSF` interface
- Full readline support, tabbing, and command completion
- Execution of external commands in `msfconsole`
  
The key term here is usability—user experience. The ease with which we can control the console can improve our learning experience. Therefore, let us delve into the specifics.

---

### Understanding the Architecture
To fully operate whatever tool we are using, we must first look under its hood. It is good practice, and it can offer us better insight into what will be going on during our security assessments when that tool comes into play. It is essential not to have [any wildcards that might leave you or your client exposed to data breaches](https://www.cobaltstrike.com/blog/cobalt-strike-rce-active-exploitation-reported).

By default, all the base files related to Metasploit Framework can be found under `/usr/share/metasploit-framework` in our `ParrotOS Security` distro.

#### Data, Documentation, Lib
These are the base files for the Framework. The Data and Lib are the functioning parts of the msfconsole interface, while the Documentation folder contains all the technical details about the project.

#### Modules
The Modules detailed above are split into separate categories in this folder. We will go into detail about these in the next sections. They are contained in the following folders:
```shell-session
0xWAYNE@htb[/htb]$ ls /usr/share/metasploit-framework/modules

auxiliary  encoders  evasion  exploits  nops  payloads  post
```

#### Plugins
Plugins offer the pentester more flexibility when using the `msfconsole` since they can easily be manually or automatically loaded as needed to provide extra functionality and automation during our assessment.

```shell-session
0xWAYNE@htb[/htb]$ ls /usr/share/metasploit-framework/plugins/

aggregator.rb      ips_filter.rb  openvas.rb           sounds.rb
alias.rb           komand.rb      pcap_log.rb          sqlmap.rb
auto_add_route.rb  lab.rb         request.rb           thread.rb
beholder.rb        libnotify.rb   rssfeed.rb           token_adduser.rb
db_credcollect.rb  msfd.rb        sample.rb            token_hunter.rb
db_tracker.rb      msgrpc.rb      session_notifier.rb  wiki.rb
event_tester.rb    nessus.rb      session_tagger.rb    wmap.rb
ffautoregen.rb     nexpose.rb     socket_logger.rb
```

#### Scripts

Meterpreter functionality and other useful scripts.

```shell-session
0xWAYNE@htb[/htb]$ ls /usr/share/metasploit-framework/scripts/

meterpreter  ps  resource  shell
```

#### Tools

Command-line utilities that can be called directly from the `msfconsole` menu.

```shell-session
0xWAYNE@htb[/htb]$ ls /usr/share/metasploit-framework/tools/

context  docs     hardware  modules   payloads
dev      exploit  memdump   password  recon
```

### Introduction to MSFconsole

To start interacting with the Metasploit Framework, we need to type `msfconsole` in the terminal of our choice. Many security-oriented distributions such as Parrot Security and Kali Linux come with `msfconsole` preinstalled. We can use several other options when launching the script as with any other command-line tool. These vary from graphical display switches/options to procedural ones.

### Preparation
Upon launching the `msfconsole`, we are met with their coined splash art and the command line prompt, waiting for our first command.

#### Launching MSFconsole
Alternatively, we can use the `-q` option, which does not display the banner.

```shell-session
0xWAYNE@htb[/htb]$ msfconsole -q

msf6 > 
```

To better look at all the available commands, we can type the `help` command. First things first, our tools need to be sharp. One of the first things we need to do is make sure the modules that compose the framework are up to date, and any new ones available to the public can be imported.

### Modules

Metasploit modules are prepared scripts with specific purpose and corresponding functions that have already been developed and tested in wild. The exploit category consists of so-called  proof-of-concepts(POC's) that can be used to exploit existing  vulnerabilities in largely automated manner. Many people often think failure of exploit disproves the existence of suspected vulnerability. However, it is proof Metasploit exploit does not work and not that the vulnerability does not exist. This is because many exploits require customisation according to the target hosts to make exploit work. Therefore, automated tools such as metasploit framework should only be considered a support tool and not a substitute for our manual skills


Once we are in the `msfconsole`, we can select from an extensive list containing all the available Metasploit modules. Each of them is structured into folders, which will look like this:

#### Syntax
```shell-session
<No.> <type>/<os>/<service>/<name>
```

#### Example
```shell-session
794   exploit/windows/ftp/scriptftp_list
```

#### Index No.
The `No.` tag will be displayed to select the exploit we want afterward during our searches. We will see how helpful the `No.` tag can be to select specific Metasploit modules later.

#### Type
`Type` tag is used as segregation for metasploit modules.It helps to identify what the module will accomplish. Some types are directly exploitable , for ex they are set to be introduce the structure alongside the interactable ones for better modularization.To explain better, here are the possible types that could appear in this field:

|**Type**|**Description**|
|---|---|
|`Auxiliary`|Scanning, fuzzing, sniffing, and admin capabilities. Offer extra assistance and functionality.|
|`Encoders`|Ensure that payloads are intact to their destination.|
|`Exploits`|Defined as modules that exploit a vulnerability that will allow for the payload delivery.|
|`NOPs`|(No Operation code) Keep the payload sizes consistent across exploit attempts.|
|`Payloads`|Code runs remotely and calls back to the attacker machine to establish a connection (or shell).|
|`Plugins`|Additional scripts can be integrated within an assessment with `msfconsole` and coexist.|
|`Post`|Wide array of modules to gather information, pivot deeper, etc.|

Note that when selecting a module to use for payload delivery, the `use <no.>` command can only be used with the following modules that can be used as `initiators` (or interactable modules):

|**Type**|**Description**|
|---|---|
|`Auxiliary`|Scanning, fuzzing, sniffing, and admin capabilities. Offer extra assistance and functionality.|
|`Exploits`|Defined as modules that exploit a vulnerability that will allow for the payload delivery.|
|`Post`|Wide array of modules to gather information, pivot deeper, etc.|
#### OS
The `OS` tag specifies which operating system and architecture the module was created for. Naturally, different operating systems require different code to be run to get the desired results.

#### Service
The `Service` tag refers to the vulnerable service that is running on the target machine. For some modules, such as the `auxiliary` or `post` ones, this tag can refer to a more general activity such as `gather`, referring to the gathering of credentials, for example.

#### Name
Finally, the `Name` tag explains the actual action that can be performed using this module created for a specific purpose.

---

## Targets

Targets are unique OS identifiers extracted from versions of those specific OS which adapt the selected exploit module to run on that particular version of OS. The show targets command issued within an exploit module view will display all available vulnerable targets for that specific exploit, while issuing same command in root menu, outside of any selected exploit module will let us know that we need to select an exploit module first.

### Selecting a Target
We can see that there is only one general type of target set for this type of exploit. What if we change the exploit module to something that needs more specific target ranges? The following exploit is aimed at:
- `MS12-063 Microsoft Internet Explorer execCommand Use-After-Free Vulnerability`
  
if want to learn more about the specific module and what the vulnerability behind it does we can use `info` command. This command helps out whenever we are unsure about origins or functionality of different exploits or auxiliary modules. 

Pro tip: The `info` command should be one of the first steps we take when using a new module.

For example if an exploit has options for both different versions of internet explorer and various Windows versions. Leaving the selection on Automatic will let msfconsole know that it needs to perform service detection on given target before launching a successful attack.

IF we however know what versions are running on our target we can use `set target <index no>` command to pick a target from list.

### Target Types

There is a large variety of target types. Every target can vary from another by service pack, OS version and even language version. It all depends on return address and other parameters in target or within exploit module.

The return address can vary because a particular language pack changes addresses, a different software version is available or addresses are shifted due to hooks. It is all determined by type of return of address required to identify the target. The address can be jmp esp a jump to a specfic register that identifies the target or a pop/pop/ret. 

To identify a target correctly we will need to:
 - Obtain a copy of target binaries
 - Use msfpescan to locate a suitable return address.
   
## Payloads

A `Payload` in metasploit refers to a module that aids the exploit module in  returning a shell to attacker. the payloads are sent together with exploit itself to bypass standard functioning procedures of vulnerable service(exploits job) and then run on target OS to typically return a reverse connection to attacker and establish a foothold(payload's job)

There are 3 different types of payloads modules in Metasploit Framework: Singles, Stagers and Stages. Using three typologies of payload interaction will prove beneficial to pentester. It can offer the flexibility we need to perform certain types of tasks. Whether or not a payload is staged is represented by `/` in payload name.

For ex, `Windows/shell_bind_tcp` is a single payload with no stage, whereas windows/shell/bind_tcp consists of a stager (bind_tcp) and a stage (shell).

### Singles

A single payload contains the exploit and entire shellcode for selected task. Inline payloads are by design more stable than their counterparts because they contain everything all-in-one. However some exploits will not support the resulting size of these payloads as they can get quite large. Singles are self-contained payloads. They are sole object sent and executed on target system, getting us a result immediately after running. A single payload can be as simple as adding a user to target system or booting up a process.

### Stagers
`Stagers` payloads work with Stage payloads to perform a specific task. A stager is waiting on attacker machine ready to establish a connection to victim host once stage completes its run on remote host. Stagers are typically used to set up a network connection between attacker and victim and are designed to be small and reliable. Metasploit will use best one and fall back to less-preferred one when necessary.

Windows NX vs NO-NX Stagers
 - Reliability isse for NX CPU's and DEP
 - NX stagers are bigger(VirtualAlloc memory)
 - Default is now NX + Win7 Compatible
   

### Stages
`Stages` are payload components that are downloaded by stager's modules. The various payload stages provide advance features with no size limits such as meterpreter, VNC injection and others. Payload stages automatically use middle stagers:
 - A single recv() fails with large payloads
 - The stager receives the middle stager
 - The middle stager the performs a full download
 - Also better fir RWX
   
### Staged Payloads
A staged payload can simply be put as an exploitation process that is modularized and functionally separated to help segregate the different functions it accomplishes into different code blocks each completing its objective individually but working on chaining the attack together. This will ultimately grant an attacker remote access to target machine if all stages work correctly.

The scope of this payload as with any other besides granting shell access to target system is to be as compact and inconspicuous(not really noticeable) as possible to aid with Antivirus(AV)/Intrusion Prevention System(IPS) evasion as much as possible.

`Stage0` of a staged payload represents the initial shellcode sent over network to target machine's vulnerable service, which has sole purpose of initializing  a connection back to attacker machine. This is what is known as reverse connection. As a Metasploit user, we will meet under the common names reverse_tcp, reverse_https and bind_tcp. For example, under the show payloads command you can look for payloads that look like following:

#### MSF - Staged Payloads
```shell-session
msf6 > show payloads

<SNIP>

535  windows/x64/meterpreter/bind_ipv6_tcp                                normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager
536  windows/x64/meterpreter/bind_ipv6_tcp_uuid                           normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager with UUID Support
537  windows/x64/meterpreter/bind_named_pipe                              normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind Named Pipe Stager
538  windows/x64/meterpreter/bind_tcp                                     normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind TCP Stager
539  windows/x64/meterpreter/bind_tcp_rc4                                 normal  No     Windows Meterpreter (Reflective Injection x64), Bind TCP Stager (RC4 Stage Encryption, Metasm)
540  windows/x64/meterpreter/bind_tcp_uuid                                normal  No     Windows Meterpreter (Reflective Injection x64), Bind TCP Stager with UUID Support (Windows x64)
541  windows/x64/meterpreter/reverse_http                                 normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (wininet)
542  windows/x64/meterpreter/reverse_https                                normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (wininet)
543  windows/x64/meterpreter/reverse_named_pipe                           normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse Named Pipe (SMB) Stager
544  windows/x64/meterpreter/reverse_tcp                                  normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse TCP Stager
545  windows/x64/meterpreter/reverse_tcp_rc4                              normal  No     Windows Me
```

Reverse connections are less likely to trigger prevention systems like the one initializing the connection is the victim host, which most of the time resides in what is known as a `security trust zone`. However, of course, this trust policy is not blindly followed by the security devices and personnel of a network, so the attacker must tread carefully even with this step.

Stage0 code also aims to read a larger, subsequent payload into memory once it arrives. After the stable communication channel is established between the attacker and the victim, the attacker machine will most likely send an even bigger payload stage which should grant them shell access. This larger payload would be the `Stage1` payload.

#### Meterpreter Payload
The `Meterpreter` payload is a specific type of multi-faceted payload that uses `DLL injection` to ensure the connection to the victim host is stable, hard to detect by simple checks, and persistent across reboots or system changes. Meterpreter resides completely in the memory of the remote host and leaves no traces on the hard drive, making it very difficult to detect with conventional forensic techniques. In addition, scripts and plugins can be `loaded and unloaded` dynamically as required.

Once the Meterpreter payload is executed, a new session is created, which spawns up the Meterpreter interface. It is very similar to the msfconsole interface, but all available commands are aimed at the target system, which the payload has "infected." It offers us a plethora of useful commands, varying from keystroke capture, password hash collection, microphone tapping, and screenshotting to impersonating process security tokens. We will delve into more detail.

Using Meterpreter, we can also `load` in different Plugins to assist us with our assessment. We will talk more about these in the Plugins section of this module.

### Searching for Payloads
To select our first payload, we need to know what we want to do on the target machine. For example, if we are going for access persistence, we will probably want to select a Meterpreter payload.

Meterpreter payloads offer us a significant amount of flexibility. Their base functionality is already vast and influential. We can automate and quickly deliver combined with plugins such as [GentilKiwi's Mimikatz Plugin](https://github.com/gentilkiwi/mimikatz) parts of the pentest while keeping an organized, time-effective assessment. To see all of the available payloads, use the `show payloads` command in `msfconsole`.

#### MSF - List Payloads
```shell-session
msf6 > show payloads

Payloads
========

   #    Name                                                Disclosure Date  Rank    Check  Description
-    ----                                                ---------------  ----    -----  -----------
   0    aix/ppc/shell_bind_tcp                                               manual  No     AIX Command Shell, Bind TCP Inline
   1    aix/ppc/shell_find_port                                              manual  No     AIX Command Shell, Find Port Inline
   2    aix/ppc/shell_interact                                               manual  No     AIX execve Shell for inetd
   3    aix/ppc/shell_reverse_tcp                                            manual  No     AIX Co
```

As seen above, there are a lot of available payloads to choose from. Not only that, but we can create our payloads using `msfvenom`, but we will dive into that a little bit later. We will use the same target as before, and instead of using the default payload, which is a simple `reverse_tcp_shell`, we will be using a `Meterpreter Payload for Windows 7(x64)`.

Scrolling through the list above, we find the section containing `Meterpreter Payloads for Windows(x64)`.

  Payloads

```shell-session
   515  windows/x64/meterpreter/bind_ipv6_tcp                                manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager
   516  windows/x64/meterpreter/bind_ipv6_tcp_uuid                           manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager with UUID Support
   517  windows/x64/meterpreter/bind_named_pipe                              manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind Named Pipe Stager
   518  windows/x64/meterpreter/bind_tcp                                     manual  No     Window
```

As we can see, it can be pretty time-consuming to find the desired payload with such an extensive list. We can also use `grep` in `msfconsole` to filter out specific terms. This would speed up the search and, therefore, our selection.

We have to enter the `grep` command with the corresponding parameter at the beginning and then the command in which the filtering should happen. For example, let us assume that we want to have a `TCP` based `reverse shell` handled by `Meterpreter` for our exploit. Accordingly, we can first search for all results that contain the word `Meterpreter` in the payloads.

#### MSF - Searching for Specific Payload
```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > grep meterpreter show payloads

   6   payload/windows/x64/meterpreter/bind_ipv6_tcp                        normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager
   7   payload/windows/x64/meterpreter/bind_ipv6_tcp_uuid                   normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager with UUID Support
   8   payload/windows/x64/meterpreter/bind_named_pipe                      normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind Named Pipe Stager
   9   payload/windows/x64/meterpreter/bind_tcp                             normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind TCP Stager
   10  payload/windows/x64/meterpreter/bind_tcp_rc4                         normal  No     Windows
```

This gives us a total of `14` results. Now we can add another `grep` command after the first one and search for `reverse_tcp`.
```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > grep meterpreter grep reverse_tcp show payloads

   15  payload/windows/x64/meterpreter/reverse_tcp                          normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse TCP Stager
   16  payload/windows/x64/meterpreter/reverse_tcp_rc4                      normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   17  payload/windows/x64/meterpreter/reverse_tcp_uuid                     normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager with UUID Support (Windows x64)
   
   
msf6 exploit(windows/smb/ms17_010_eternalblue) > grep -c meterpreter grep reverse_tcp show payloads

[*] 3
```

With the help of `grep`, we reduced the list of payloads we wanted down to fewer. Of course, the `grep` command can be used for all other commands. All we need to know is what we are looking for.

#### Selecting Payloads
Same as with the module, we need the index number of the entry we would like to use. To set the payload for the currently selected module, we use `set payload <no.>` only after selecting an Exploit module to begin with.

After selecting a payload, we will have more options available to us.
As we can see, by running the `show payloads` command within the Exploit module itself, msfconsole has detected that the target is a Windows machine, and such only displayed the payloads aimed at Windows operating systems.

We can also see that a new option field has appeared, directly related to what the payload parameters will contain. We will be focusing on `LHOST` and `LPORT` (our attacker IP and the desired port for reverse connection initialization). Of course, if the attack fails, we can always use a different port and relaunch the attack.

#### Using Payloads

Time to set our parameters for both the Exploit module and the payload module. For the Exploit part, we will need to set the following:

|**Parameter**|**Description**|
|---|---|
|`RHOSTS`|The IP address of the remote host, the target machine.|
|`RPORT`|Does not require a change, just a check that we are on port 445, where SMB is running.|

For the payload part, we will need to set the following:

|**Parameter**|**Description**|
|---|---|
|`LHOST`|The host's IP address, the attacker's machine.|
|`LPORT`|Does not require a change, just a check that the port is not already in use.|

If we want to check our LHOST IP address quickly, we can always call the `ifconfig` command directly from the msfconsole menu.

#### MSF - Exploit and Payload Configuration
The prompt is not a Windows command-line one but a `Meterpreter` prompt. The `whoami` command, typically used for Windows, does not work here. Instead, we can use the Linux equivalent of `getuid`. Exploring the `help` menu gives us further insight into what Meterpreter payloads are capable of.

#### MSF - Meterpreter Navigation
```shell-session
meterpreter > cd Users
meterpreter > ls

Listing: C:\Users
=================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
40777/rwxrwxrwx   8192  dir   2017-07-21 06:56:23 +0000  Administrator
40777/rwxrwxrwx   0     dir   2009-07-14 05:08:56 +0000  All Users
40555/r-xr-xr-x   8192  dir   2009-07-14 03:20:08 +0000  Default
40777/rwxrwxrwx   0     dir   2009-07-14 05:08:56 +0000  Default User
40555/r-xr-xr-x   4096  dir   2009-07-14 03:20:08 +0000  Public
100666/rw-rw-rw-  174   fil   2009-07-14 04:54:24 +0000  desktop.ini
40777/rwxrwxrwx   8192  dir   2017-07-14 13:45:33 +0000  haris


meterpreter > shell

Process 2664 created.
Channel 1 created.

Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation. All rights reserved.

C:\Users>
```

`Channel 1` has been created, and we are automatically placed into the CLI for this machine. The channel here represents the connection between our device and the target host, which has been established in a reverse TCP connection (from the target host to us) using a Meterpreter Stager and Stage. The stager was activated on our machine to await a connection request initialized by the Stage payload on the target machine.

Moving into a standard shell on the target is helpful in some cases, but Meterpreter can also navigate and perform actions on the victim machine. So we see that the commands have changed, but we have the same privilege level within the system.

#### MSF - Windows CMD
```shell-session
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation. All rights reserved.

C:\Users>dir

dir
 Volume in drive C has no label.
 Volume Serial Number is A0EF-1911

 Directory of C:\Users

21/07/2017  07:56    <DIR>          .
21/07/2017  07:56    <DIR>          ..
21/07/2017  07:56    <DIR>          Administrator
14/07/2017  14:45    <DIR>          haris
12/04/2011  08:51    <DIR>          Public
               0 File(s)              0 bytes
               5 Dir(s)  15,738,978,304 bytes free

C:\Users>whoami

whoami
nt authority\system
```

Let's see what other types of payloads we can use. We will be looking at the most common ones related to Windows operating systems.

#### Payload Types

The table below contains the most common payloads used for Windows machines and their respective descriptions.

|**Payload**|**Description**|
|---|---|
|`generic/custom`|Generic listener, multi-use|
|`generic/shell_bind_tcp`|Generic listener, multi-use, normal shell, TCP connection binding|
|`generic/shell_reverse_tcp`|Generic listener, multi-use, normal shell, reverse TCP connection|
|`windows/x64/exec`|Executes an arbitrary command (Windows x64)|
|`windows/x64/loadlibrary`|Loads an arbitrary x64 library path|
|`windows/x64/messagebox`|Spawns a dialog via MessageBox using a customizable title, text & icon|
|`windows/x64/shell_reverse_tcp`|Normal shell, single payload, reverse TCP connection|
|`windows/x64/shell/reverse_tcp`|Normal shell, stager + stage, reverse TCP connection|
|`windows/x64/shell/bind_ipv6_tcp`|Normal shell, stager + stage, IPv6 Bind TCP stager|
|`windows/x64/meterpreter/$`|Meterpreter payload + varieties above|
|`windows/x64/powershell/$`|Interactive PowerShell sessions + varieties above|
|`windows/x64/vncinject/$`|VNC Server (Reflective Injection) + varieties above|

Other critical payloads that are heavily used by penetration testers during security assessments are Empire and Cobalt Strike payloads. These are not in the scope of this course, but feel free to research them in our free time as they can provide a significant amount of insight into how professional penetration testers perform their assessments on high-value targets.

Besides these, of course, there are a plethora of other payloads out there. Some are for specific device vendors, such as Cisco, Apple, or PLCs. Some we can generate ourselves using `msfvenom`. However, next up, we will look at `Encoders` and how they can be used to influence the attack outcome.

## Encoders

Encoders assisted with making payloads compatible  with different processor architectures while helping antivirus evasion.`Encoders` come into play with the role of changing the payload to run on different operating systems and architectures. These architectures include:

|`x64`|`x86`|`sparc`|`ppc`|`mips`|
|---|---|---|---|---|
They also assist in removing hexadecimal opcodes known as `Bad Characters` from the payload. Use of encoders for AV evasion has been diminished over time as IPS/IDS improved.

Shikata Ga Nai(SGN) was one of most utilized encoding schemes back in day because it was hard to detect payloads encoded through its mechanism, Modern technology caught up with its encoding mechanism.The name (`仕方がない`) means `It cannot be helped` or `Nothing can be done about it`, and rightfully so if we were reading this a few years ago. However, there are other methodologies we will explore to evade protection systems. [This article from FireEye](https://www.fireeye.com/blog/threat-research/2019/10/shikata-ga-nai-encoder-still-going-strong.html) details the why and the how of Shikata Ga Nai's previous rule over the other encoders.

### Selecting an Encoder

Until 2015, MSF has different submodules for payloads and encoders called `msfpayload` and `msfencoder`.

First we have to create a custom payload then encode it according to target OS architecture using `msfencode`. A pipe would take output from one command feed it into next which would generate an encoded payload read to sent and run on target machine.

After 2015, updates to these scripts have combined them within the `msfvenom` tool, which takes care of payload generation and Encoding.

#### Shikata Ga Nai Encoding

![GIF showcasing the Shikata Ga Nai encoding with various XOR keys.](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/39/shikata_ga_nai.gif) Source: https://hatching.io/blog/metasploit-payloads2/

If we want to look at the functioning of the `shikata_ga_nai` encoder, we can look at an excellent post [here](https://hatching.io/blog/metasploit-payloads2/).

Suppose we want to select an Encoder for an `existing payload`. Then, we can use the `show encoders` command within the `msfconsole` to see which encoders are available for our current `Exploit module + Payload` combination.

  Encoders

```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload 15

payload => windows/x64/meterpreter/reverse_tcp


msf6 exploit(windows/smb/ms17_010_eternalblue) > show encoders

Compatible Encoders
===================

   #  Name              Disclosure Date  Rank    Check  Description
   -  ----              ---------------  ----    -----  -----------
   0  generic/eicar                      manual  No     The EICAR Encoder
   1  generic/none                       manual  No     The "none" Encoder
   2  x64/xor                            manual  No     XOR Encoder
   3  x64/xor_dynamic                    manual  No     Dynamic key XOR Encoder
   4  x64/zutto_dekiru                   manual  No     Zutto Dekiru
```

## DataBases

Databses in msfconsole are used to keep track of results. Msfconsole has built in PostgreSQL database system.Database entries can also be used to configure exploit module parameters with already existing findings directly.

###  Setting up the Database
First, we must ensure that the PostgreSQL server is up and running on our host machine. To do so, input the following 

### Metasploit Database Startup – Step-by-Step (Production-Grade)

### 1. Verify PostgreSQL is installed

`dpkg -l | grep postgresql`

If nothing shows → PostgreSQL is not installed (install before proceeding).


### 2. Start PostgreSQL service

`sudo systemctl start postgresql`

Optional (older systems):

`sudo service postgresql start`


### 3. Enable PostgreSQL at boot (one-time setup)

`sudo systemctl enable postgresql`

This ensures the DB is available after every reboot.


### 4. Verify PostgreSQL status

`systemctl status postgresql`

You want:

- **Active: active (running)**
    

### 5. Initialize Metasploit database (first time only)

`sudo msfdb init`

What this does:

- Creates PostgreSQL user
    
- Creates Metasploit database
    
- Writes `database.yml`
    
- Builds schema
    

⚠️ Run **once**, not every boot.

### 6. Start Metasploit Framework

`msfconsole`

(or `msfconsole -q` for quiet mode)

### 7. Confirm database connection inside Metasploit

`db_status`

Expected output:

`[*] Connected to msf. Connection type: postgresql.`

If you see this → DB is live and usable.

### Daily Workflow (After Reboot)

You **do NOT** re-init anything.

Only do:

`sudo systemctl start postgresql msfconsole -q db_status`

### Using the Database

With the help of the database, we can manage many different categories and hosts that we have analyzed. Alternatively, the information about them that we have interacted with using Metasploit. These databases can be exported and imported. This is especially useful when we have extensive lists of hosts, loot, notes, and stored vulnerabilities for these hosts. After confirming that the database is successfully connected, we can organize our `Workspaces`.

### Workspaces 
These work same way as folders, We can segregate different scan results,hosts and extracted information by IP, Subnet, Network or Domain.
Notice that the default Workspace is named `default` and is currently in use according to the `*` symbol. Type the `workspace [name]` command to switch the presently used workspace. Looking back at our example, let us create a workspace for this assessment and select it.

### Importing Scan Results
Next, let us assume we want to import a `Nmap scan` of a host into our Database's Workspace to understand the target better. We can use the `db_import` command for this. After the import is complete, we can check the presence of the host's information in our database by using the `hosts` and `services` commands. Note that the `.xml` file type is preferred for `db_import`.

```shell-session
msf6 > db_nmap -sV -sS 10.10.10.8
```

Command to perfrom nmpa scan isnide msfdb

#### MSF - DB Export
```shell-session
msf6 > db_export -h

Usage:
    db_export -f <format> [filename]
    Format can be one of: xml, pwdump
[-] No output file was specified
```

This data can be imported back to msfconsole later when needed. Other commands related to data retention are the extended use of `hosts`, `services`, and the `creds` and `loot` commands.

#### Hosts

The hosts command displays database table automatically populated with host addresses, host names and other information we find about these during scans and interactions.hosts -h

For example, suppose `msfconsole` is linked with scanner plugins that can perform service and OS detection. In that case, this information should automatically appear in the table once the scans are completed through msfconsole. Again, tools like Nessus, NexPose, or Nmap will help us in these cases.

Hosts can also be manually added as separate entries in this table. After adding our custom hosts, we can also organize the format and structure of the table, add comments, change existing information, and more

#### Services 

The `services` command functions the same way as the previous one. It contains a table with descriptions and information on services discovered during scans or interactions. In the same way as the command above, the entries here are highly customizable.

#### Creds
The `creds` command allows you to visualize the credentials gathered during your interactions with the target host. We can also add credentials manually, match existing credentials with port specifications, add descriptions, etc.

#### Loot

The `loot` command works in conjunction with command above to offer you an at-glance list of owned services and users. The loot in this scenario refers to hash dumps from diffferent system types, namely hashes, passwd, shadow and more

## Plugins
Plugins are readily available software released by third parties and given approval to creators of Metasploit to integrate their own software inside framework. 

#### Using plugins

To start a plugin ensure its correctly installed into the the `plugin` sub-folder of metasploit framework which is the default directory.

Many people write many different plugins for the Metasploit framework. They all have a specific purpose and can be an excellent help to save time after familiarizing ourselves with them. Check out the list of popular plugins below:

|[nMap (pre-installed)](https://nmap.org/)|[NexPose (pre-installed)](https://sectools.org/tool/nexpose/)|[Nessus (pre-installed)](https://www.tenable.com/products/nessus)|
|[Mimikatz (pre-installed V.1)](http://blog.gentilkiwi.com/mimikatz)|[Stdapi (pre-installed)](https://www.rubydoc.info/github/rapid7/metasploit-framework/Rex/Post/Meterpreter/Extensions/Stdapi/Stdapi)|[Railgun](https://github.com/rapid7/metasploit-framework/wiki/How-to-use-Railgun-for-Windows-post-exploitation)|
|[Priv](https://github.com/rapid7/metasploit-framework/blob/master/lib/rex/post/meterpreter/extensions/priv/priv.rb)|[Incognito (pre-installed)](https://www.offensive-security.com/metasploit-unleashed/fun-incognito/)|[Darkoperator's](https://github.com/darkoperator/Metasploit-Plugins)|

#### Mixins

The Metasploit Framework is written in Ruby, an object-oriented programming language. This plays a big part in what makes `msfconsole` excellent to use. Mixins are one of those features that, when implemented, offer a large amount of flexibility to both the creator of the script and the user.

Mixins are classes that act as methods for use by other classes without having to be the parent class of those other classes. Thus, it would be deemed inappropriate to call it inheritance but rather inclusion. They are mainly used when we:

1. Want to provide a lot of optional features for a class.
2. Want to use one particular feature for a multitude of classes.

Most of the Ruby programming language revolves around Mixins as Modules. The concept of Mixins is implemented using the word `include`, to which we pass the name of the module as a `parameter`. We can read more about mixins [here](https://en.wikibooks.org/wiki/Metasploit/UsingMixins).

## Sessions

MSFconsole can manage multiple modules at the same time. This is one of the many reasons it provides the user with so much flexibility. This is done with the use of `Sessions`, which creates dedicated control interfaces for all of your deployed modules.

## Using Sessions

While running any available exploits or auxiliary modules in msfconsole, we can background the session as long as they form a channel of communication with the target host. This can be done either by pressing the `[CTRL] + [Z]` key combination or by typing the `background` command in the case of Meterpreter stages. This will prompt us with a confirmation message. After accepting the prompt, we will be taken back to the msfconsole prompt (`msf6 >`) and will immediately be able to launch a different module.

## Jobs

If, for example, we are running an active exploit under a specific port and need this port for a different module, we cannot simply terminate the session using `[CTRL] + [C]`. If we did that, we would see that the port would still be in use, affecting our use of the new module. So instead, we would need to use the `jobs` command to look at the currently active tasks running in the background and terminate the old ones to free up the port.

### Meterpreter

Meterpreter payload is  specific type of multi-faceted, extensible payload that uses DLL injection to ensure connection to victim host is stable and difficult to detect using simple checks can be configured to be persistent.
Furthermore, Meterpreter resides entirely in the memory of the remote host and leaves no traces on the hard drive, making it difficult to detect with conventional forensic techniques.

For some interesting reading, check out this [post](https://www.rapid7.com/blog/post/2015/03/25/stageless-meterpreter-payloads/) on Meterpreter stageless payloads and this [post](https://www.blackhillsinfosec.com/modifying-metasploit-x64-template-for-av-evasion) on modifying Metasploit templates for evasion. These topics are outside the scope of this module, but we should be aware of these possibilities.

### Running Meterpreter

To run Meterpreter, we only need to select any version of it from the `show payloads` output, taking into consideration the type of connection and OS we are attacking.

When the exploit is completed, the following events occur:

- The target executes the initial stager. This is usually a bind, reverse, findtag, passivex, etc.

- The stager loads the DLL prefixed with Reflective. The Reflective stub handles the loading/injection of the DLL.

- The Meterpreter core initializes, establishes an AES-encrypted link over the socket, and sends a GET. Metasploit receives this GET and configures the client.

- Lastly, Meterpreter loads extensions. It will always load `stdapi` and load `priv` if the module gives administrative rights. All of these extensions are loaded over AES encryption.
  
The main idea we need to get about Meterpreter is that it is just as good as getting a direct shell on the target OS but with more functionality. The developers of Meterpreter set clear design goals for the project to skyrocket in usability in the future. Meterpreter needs to be:

- Stealthy
- Powerful
- Extensible

#### Stealthy

Meterpreter, when launched and after arriving on the target, resides entirely in memory and writes nothing to the disk. No new processes are created either as Meterpreter injects itself into a compromised process. Moreover, it can perform process migrations from one running process to another.

#### Powerful

Meterpreter's use of a channelized communication system between the target host and the attacker proves very useful. We can notice this first-hand when we immediately spawn a host-OS shell inside of our Meterpreter stage by opening a dedicated channel for it. This also allows for the use of AES-encrypted traffic.

#### Extensible

Meterpreter's features can constantly be augmented at runtime and loaded over the network. Its modular structure also allows new functionality to be added without rebuilding it.

## Generation Features

- End to end encryption across Meterpreter sessions for all five implementations (Windows, Python, Java, Mettle, and PHP)
    
- SMBv3 client support to further enable modern exploitation workflows
    
- New polymorphic payload generation routine for Windows shellcode that improves evasive capabilities against common antivirus and intrusion detection system (IDS) products
    

## Expanded Encryption

- Increased complexity for creation of signature-based detections for certain network operations and Metasploit’s main payload binaries
    
- All Meterpreter payloads will use AES encryption during communication between the attacker and the target system
    
- SMBv3 encryption integration will increase complexity for signature-based detections used to identify key operations performed over SMB
    

## Cleaner Payload Artifacts

- DLLs used by the Windows Meterpreter now resolve necessary functions by ordinal instead of name
    
- The standard export ReflectiveLoader used by reflectively loadable DLLs is no longer present in the payload binaries as text data
    
- Commands that Meterpreter exposes to the Framework are now encoded as integers instead of strings