
| **Command** | **Description**                                                                                                                    |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `whoami`    | Displays current username.                                                                                                         |
| `id`        | Returns users identity                                                                                                             |
| `hostname`  | Sets or prints the name of current host system.                                                                                    |
| `uname`     | Prints basic information about the operating system name and system hardware.                                                      |
| `pwd`       | Returns working directory name.                                                                                                    |
| `ifconfig`  | The ifconfig utility is used to assign or to view an address to a network interface and/or configure network interface parameters. |
| `ip`        | Ip is a utility to show or manipulate routing, network devices, interfaces and tunnels.                                            |
| `netstat`   | Shows network status.                                                                                                              |
| `ss`        | Another utility to investigate sockets.                                                                                            |
| `ps`        | Shows process status.                                                                                                              |
| `who`       | Displays who is logged in.                                                                                                         |
| `env`       | Prints environment or sets and executes command.                                                                                   |
| `lsblk`     | Lists block devices.                                                                                                               |
| `lsusb`     | Lists USB devices                                                                                                                  |
| `lsof`      | Lists opened files.                                                                                                                |
| `lspci`     | Lists PCI devices.                                                                                                                 |

What is the path to the htb-student's mail?

we can search through environment  with mail `$ env | grep MAIL`

Which shell is specified for the htb-student user?

To find the assigned shell we can find it through two ways

``
### Working with Files and Directories

The terminal's efficiency stems from its ability to access files with just a few commands, and it allows you to modify files selectively using regular expressions (`regex`). Additionally, you can run multiple commands at once, redirecting output to files and automating batch editing tasks, which is a major time-saver when working with numerous files simultaneously. 


#### Nano Editor
Nano’s straightforward interface (also called "`pager`") makes it a great choice for quickly editing text files, especially when you’re just getting started.

#### VIM
`Vim` is an open-source editor for all kinds of ASCII text, just like Nano. It is an improved clone of the previous Vi. It is an extremely powerful editor that focuses on the essentials, namely editing text. For tasks that go beyond that, Vim provides an interface to external programs, such as `grep`, `awk`, `sed`, etc., which can handle their specific tasks much better than a corresponding function directly implemented in an editor usually can. This makes the editor small and compact, fast, powerful, flexible, and less error-prone.

Vim follows the Unix principle here: many small specialized programs that are well tested and proven, when combined and communicating with each other, resulting in a flexible and powerful system.


In contrast to Nano, `Vim` is a modal editor that can distinguish between text and command input. Vim offers a total of six fundamental modes that make our work easier and make this editor so powerful:

|**Mode**|**Description**|
|---|---|
|`Normal`|In normal mode, all inputs are considered as editor commands. So there is no insertion of the entered characters into the editor buffer, as is the case with most other editors. After starting the editor, we are usually in the normal mode.|
|`Insert`|With a few exceptions, all entered characters are inserted into the buffer.|
|`Visual`|The visual mode is used to mark a contiguous part of the text, which will be visually highlighted. By positioning the cursor, we change the selected area. The highlighted area can then be edited in various ways, such as deleting, copying, or replacing it.|
|`Command`|It allows us to enter single-line commands at the bottom of the editor. This can be used for sorting, replacing text sections, or deleting them, for example.|
|`Replace`|In replace mode, the newly entered text will overwrite existing text characters unless there are no more old characters at the current cursor position. Then the newly entered text will be added.|
|`Ex`|Emulates the behavior of the text editor [Ex](https://man7.org/linux/man-pages/man1/ex.1p.html), one of the predecessors of `Vim`. Provides a mode where we can execute multiple commands sequentially without returning to Normal mode after each command.|

## Find Files and Directories

#### Which

One of the common tools is `which`. This tool returns the path to the file or link that should be executed. This allows us to determine if specific programs, like cURL,netcat,wget,python,gcc are available on OS.

#### Find
Another handy tool is `find`. Besides the function to find files and folders, this tool also contains the function to filter the results. We can use filter parameters like the size of the file or the date. We can also specify if we only search for files or folders.
```Bash
$ find / -type f -name *.conf -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null

-rw-r--r-- 1 root root 136392 Apr 25 20:29 /usr/src/linux-headers-5.5.0-1parrot1-amd64/include/config/auto.conf
-rw-r--r-- 1 root root 82290 Apr 25 20:29 /usr/src/linux-headers-5.5.0-1parrot1-amd64/include/config/tristate.conf
-rw-r--r-- 1 root root 95813 May  7 14:33 /usr/share/metasploit-framework/data/jtr/repeats32.conf
-rw-r--r-- 1 root root 60346 May  7 14:33 /usr/share/metasploit-framework/data/jtr/dynamic.conf
```

|**Option**|**Description**|
|---|---|
|`-type f`|Hereby, we define the type of the searched object. In this case, '`f`' stands for '`file`'.|
|`-name *.conf`|With '`-name`', we indicate the name of the file we are looking for. The asterisk (`*`) stands for 'all' files with the '`.conf`' extension.|
|`-user root`|This option filters all files whose owner is the root user.|
|`-size +20k`|We can then filter all the located files and specify that we only want to see the files that are larger than 20 KiB.|
|`-newermt 2020-03-03`|With this option, we set the date. Only files newer than the specified date will be presented.|
|`-exec ls -al {} \;`|This option executes the specified command, using the curly brackets as placeholders for each result. The backslash escapes the next character from being interpreted by the shell because otherwise, the semicolon would terminate the command and not reach the redirection.|
|`2>/dev/null`|This is a `STDERR` redirection to the '`null device`', which we will come back to in the next section. This redirection ensures that no errors are displayed in the terminal. This redirection must `not` be an option of the 'find' command.|

#### Locate
The command `locate` offers us a quicker way to search through the system. In contrast to the `find` command, `locate` works with a local database that contains all information about existing files and folders. We can update this database with the following command. `sudo updatedb`

If we now search for all files with the "`.conf`" extension,It is typically faster than find

```shell-session
0xWAYNE@htb[/htb]$ locate *.conf
/etc/GeoIP.conf
/etc/NetworkManager/NetworkManager.conf
/etc/UPower/UPower.conf
/etc/adduser.conf
<SNIP>
```

However, this tool does not have as many filter options that we can use. So choose find and locate  depending upon your use.

## File Descriptors and Redirections

A file descriptor (`FD`) in Unix/Linux operating systems is a reference, maintained by the kernel, that allows the system to manage Input/Output (`I/O`) operations. It acts as a unique identifier for an open file, socket, or any other I/O resource. In Windows-based operating systems, this is known as a file handle. Essentially, the file descriptor is the system's way of keeping track of active `I/O` connections, such as reading from or writing to a file.

By default, the first three file descriptors in Linux are:
1. Data Stream for Input
    - `STDIN – 0`
2. Data Stream for Output
    - `STDOUT – 1`
3. Data Stream for Output that relates to an error occurring.
    - `STDERR – 2`

#### STDIN and STDOUT

Let us see an example with `cat`. When running `cat`, we give the running program our standard input (`STDIN - FD 0`), marked `green`, wherein this case "SOME INPUT" is. As soon as we have confirmed our input with `[ENTER]`, it is returned to the terminal as standard output (`STDOUT - FD 1`), marked **red**.
![[Pasted image 20250510224430.png]]

#### STDOUT and STDERR

In the next example, by using the `find` command, we will see the standard output (`STDOUT - FD 1`) marked in `green` and standard error (`STDERR - FD 2`) marked in red.

In this case, the error is marked and displayed with "`Permission denied`". We can check this by redirecting the file descriptor for the errors (`FD 2 - STDERR`) to "`/dev/null`." This way, we redirect the resulting errors to the "null device," which discards all data. `2>/dev/null`

#### Redirect STDOUT to a File
Now we can see that all errors (`STDERR`) previously presented with "`Permission denied`" are no longer displayed. The only result we see now is the standard output (`STDOUT`), which we can also redirect to a file with the name `results.txt` that will only contain standard output without the standard errors.

![Terminal window with user 'htb-student@nixfund' executing 'find /etc/ -name shadow 2>/dev/null > results.txt' and 'cat results.txt'. Output shows '/etc/shadow'.](https://academy.hackthebox.com/storage/modules/18/find3.png)

#### Redirect STDOUT and STDERR to Separate Files

We should have noticed that we did not use a number before the greater-than sign (`>`) in the last example. That is because we redirected all the standard errors to the "`null device`" before, and the only output we get is the standard output (`FD 1 - STDOUT`). To make this more precise, we will redirect standard error (`FD 2 - STDERR`) and standard output (`FD 1 - STDOUT`) to different files.

![Terminal window with user 'htb-student@nixfund' executing 'find /etc/ -name shadow 2> stderr.txt 1> stdout.txt'. Output shows '/etc/shadow' in stdout.txt and 'Permission denied' messages in stderr.txt.](https://academy.hackthebox.com/storage/modules/18/find4.png)

#### Redirect STDIN


In Linux/macOS terminals, you can use the < symbol to tell a command to take input from a file instead of the keyboard. Think of it like an arrow pointing to the command, saying "Hey, take input from this file!"

Example with cat command

cat < stdout.txt

Here, cat will read the contents of stdout.txt and print them to the screen. Normally, cat would expect you to type something, but with <, you're telling it to take input from the file instead.

In short
- > redirects output to a file
- < redirects input from a file

Key difference

When you use cat stdout.txt, the cat command itself knows how to open and read the file. You're passing the file name as an argument to the command.

When you use cat < stdout.txt, the shell (not the cat command) opens the file and redirects its contents to cat as standard input. cat doesn't even know the file name; it just reads from its input stream.

When to use redirection

Redirection (<) is useful when a command doesn't accept a file name as an argument, but can read from standard input. For example, some commands might only work with piped input or redirected files.

In the case of cat, both methods work, but using cat stdout.txt is more common and straightforward.

![[Pasted image 20250510225630.png]]

#### Redirect STDOUT and Append to a File

When we use the greater-than sign (`>`) to redirect our `STDOUT`, a new file is automatically created if it does not already exist. If this file exists, it will be overwritten without asking for confirmation. If we want to append `STDOUT` to our existing file, we can use the double greater-than sign (`>>`).

![Terminal window with user 'htb-student@nixfund' executing 'find /etc/ -name passwd >> stdout.txt 2>/dev/null' and 'cat stdout.txt'. Output shows '/etc/pam.d/passwd', '/etc/cron.daily/passwd', and '/etc/passwd'.](https://academy.hackthebox.com/storage/modules/18/find9.png)

#### Redirect STDIN Stream to a File
You can use << (double less-than signs) to create a stream of input that goes until you specify an End-Of-File (EOF) marker. This is handy for adding content to a file without having to open an editor.

Example with cat command

cat << EOF > stream.txt

- cat reads the input stream
- << EOF tells the shell to keep reading input until it encounters the string "EOF" (End-Of-File marker)
- > stream.txt redirects the output to a file named "stream.txt"

How it works
1. You type the command and press Enter.
2. You can start typing your content, line by line.
3. When you're done, type "EOF" on a new line and press Enter.
4. The content will be saved to "stream.txt".

Example usage:
![Terminal window with user 'htb-student@nixfund' executing 'cat << EOF > stream.txt' with input 'Hack The Box' and 'EOF'. Then 'cat stream.txt' displays 'Hack The Box'.](https://academy.hackthebox.com/storage/modules/18/find6.png)


This will create a file named "stream.txt" with the specified content.

You can use any string as the EOF marker, but "EOF" is a common convention. Just make sure to use the same string at the beginning and end of your input stream.

#### Pipes
Another way to redirect `STDOUT` is to use pipes (`|`). These are useful when we want to use the `STDOUT` from one program to be processed by another. One of the most commonly used tools is `grep`, which we will use in the next example. Grep is used to filter `STDOUT` according to the pattern we define. In the next example, we use the `find` command to search for all files in the "`/etc/`" directory with a "`.conf`" extension. Any errors are redirected to the "`null device`" (`/dev/null`). Using `grep`, we filter out the results and specify that only the lines containing the pattern "`systemd`" should be displayed.

```Shell-session
htb-student@nixfund:~$ find /etc/ -name *.conf 2>/dev/null | grep systemd
/etc/systemd/system.conf
/etc/systemd/timesyncd.conf
/etc/systemd/journald.conf
/etc/systemd/user.conf
/etc/systemd/logind.conf
/etc/systemd/resolved.conf
```

The redirections work, not only once. We can use the obtained results to redirect them to another program. For the next example, we will use the tool called `wc`, which should count the total number of obtained results.

```Bash
htb-student@nixfund:~$ find /etc/ -name *.conf 2>/dev/null | grep systemd | wc -l
6
```

How many files exist on the system that have the ".log" file extension?

```Bash
htb-student@nixfund:~$ find / -type f -name "*.log" 2>/dev/null | wc -l
32

```

 How many total packages are installed on the target system?

```Bash
htb-student@nixfund:~$ dpkg -l | grep '^ii' | wc -l
737
```

### Filter Contents – CLI Tools for Text Processing

This section covers various Linux command-line utilities for filtering and viewing the contents of files.



### Viewing File Content

### `more`

- Displays file content one page at a time.
- Usage:

```Bash
cat /etc/passwd | more
```

- Output remains in the terminal window.

### `less`

- Similar to `more`, but supports scrolling and searching.
- Usage:
```Bash
less /etc/passwd
```

- Output is not retained in the terminal after exit.

### Viewing Start or End of Files

### `head`

- Shows the first 10 lines of a file by default.
- Usage:
  
```Bash
head /etc/passwd
```    

### `tail`

- Shows the last 10 lines of a file by default.
- Usage:    
```Bash
tail /etc/passwd
```


### Sorting and Searching

### `sort`

- Sorts lines in ascending order (alphabetically or numerically).
- Usage:    
```Bash
cat /etc/passwd | sort
```


### `grep`

- Searches for lines matching a pattern.  
- Example (search for `/bin/bash` shell users):
```Bash
cat /etc/passwd | grep "/bin/bash"
```
   

#### `grep -v`

- Excludes lines matching a pattern.
- Example (exclude `/bin/false` and `nologin`):

```Bash
cat /etc/passwd | grep -v "false\|nologin"`
```   

---

### Text Extraction and Formatting

### `cut`

- Extracts specific fields from a line.
- Example (extract usernames):
```Bash
cat /etc/passwd | grep -v "false\|nologin" | cut -d":" -f1`
```    

### `tr`

- Translates or replaces characters.
- Example (replace `:` with a space):

```Bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " "`
```    

### `column`

- Formats output into aligned columns.
- Usage:

```Bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | column -t
```

---

### Text Processing

### `awk`

- Processes and prints specific fields.
- Example (print first and last fields):

```Bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}'
```


---

### Stream Editing

### `sed`

- Performs text substitution.
- Example (replace `bin` with `HTB`):

```Bash
cat /etc/passwd | grep -v "false\|nologin" | sed 's/bin/HTB/g'`
```

---

# Regular Expressions

RegEx allow you to find,replace and manipulate data with incredible precision. it is sequence of characters and symbols that together form a search pattern.The pattern often involve special symbols called metacharacters, which define the structure of search rather than representing literal text.

Ex: Metacharcters allow you to specify whether you're searching for digits ,lettters or any characters that fit certain pattern.

### Grouping Operators
| **  <br>Operators** | **Description**                                                                                                                                                             |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `(a)`               | The round brackets are used to group parts of a regex. Within the brackets, you can define further patterns which should be processed together.                             |
| `[a-z]`             | The square brackets are used to define character classes. Inside the brackets, you can specify a list of characters to search for.                                          |
| `{1,10}`            | The curly brackets are used to define quantifiers. Inside the brackets, you can specify a number or a range that indicates how often a previous pattern should be repeated. |
| `\|`                | Also called the OR operator and shows results when one of the two expressions matches                                                                                       |
| `.*`                | Operates similarly to an AND operator by displaying results only when both expressions are present and match in the specified order                                         |

the `OR` operator. The regex searches for one of the given search parameters.

```Bash
htb-student@nixfund:~$ grep -E "(my|false)" /etc/passwd
lxd:x:105:65534::/var/lib/lxd/:/bin/false
pollinate:x:109:1::/var/cache/pollinate:/bin/false
mysql:x:116:120:MySQL Server,,,:/nonexistent:/bin/false
```


 if we use the `AND` operator, we will get a different result for the same search parameters.

```Bash
htb-student@nixfund:~$ grep -E "(my.*false)" /etc/passwd
mysql:x:116:120:MySQL Server,,,:/nonexistent:/bin/false
```

Basically, what we are saying with this command is that we are looking for a line where we want to see both `my` and `false`. A simplified example would also be to use `grep` twice and look like this:

```Bash
htb-student@nixfund:~$ grep -E "my" /etc/passwd | grep -E "false"
mysql:x:116:120:MySQL Server,,,:/nonexistent:/bin/false
```

---

## Permission Management

In Linux Permissions are like keys that control access to file and directories.these permisions are assigned to both users and groups. Each user can belong to multiple groups and being in group grants him additional access rights allowing users to perform specific actions on files and directories.

Every file and directory has an owner and is associated with a group. The permission for these files are defined for both the owner and group determining what actions - like reading writing executing are allowed.

In concise Linux permissions act like a set of rules or keys that dictate who can access and modify certain resource , ensuring security and proper collab across system.

The whole permission system on Linux systems is based on the octal number system, and basically, there are three different types of permissions a file or directory can be assigned:

- (`r`) - Read
- (`w`) - Write
- (`x`) - Execute

It is important to note that `execute` permissions are necessary to traverse a directory, no matter the user's level of access. Also, `execute` permissions on a directory do not allow a user to execute or modify any files or contents within the directory, only to traverse and access the content of the directory.

To execute files within the directory, a user needs `execute` permissions on the corresponding file. To modify the contents of a directory (create, delete, or rename files and subdirectories), the user needs `write` permissions on the directory.


### SUID & SGID Permissions in Linux

#### **Definition**

- **SUID (Set User ID)** and **SGID (Set Group ID)** are special file permissions in Linux.
- These allow users to execute files with the **permissions of the file owner (SUID)** or **group (SGID)** rather than the user who runs them.


#### **How It Appears**

- When viewing permissions using `ls -l`, the presence of these bits replaces the executable flag (`x`) with:
    - `s` for **SUID** in the user permission section.
    - `s` for **SGID** in the group permission section.
    - Example: `-rwsr-xr-x` (SUID set), `-rwxr-sr-x` (SGID set). 

#### **Functionality**

- Programs with SUID or SGID bits execute with **elevated privileges**, typically needed for system utilities.
- Useful for tasks requiring higher privileges (e.g., `passwd` command uses SUID to allow password changes).

#### **Risks**

- **Security vulnerabilities** can occur if these bits are set on unsafe programs.
- Example: If `journalctl` has the SUID bit and includes shell launch functionality, a user could spawn a **root shell**, gaining full system access.

#### **Best Practices**

- Limit SUID/SGID usage to **only trusted and necessary programs**.
- Regularly **audit files** with these permissions using:

```Bash
find / -perm -4000 2>/dev/null   # SUID 
find / -perm -2000 2>/dev/null   # SGID
```

- Refer to **GTFObins** for known binaries that can be exploited when SUID/SGID is set:  
    https://gtfobins.github.io

### Sticky Bit in Linux

#### Overview

The **sticky bit** is a special permission used in Linux systems to control file deletion within shared directories. When set on a directory, it ensures that only:

- The **file's owner**
- The **directory owner**
- Or the **root user**

can delete or rename files, **regardless of the directory's write permissions** for other users.

#### Use Case Analogy
A shared directory without a sticky bit is like a public bulletin board—anyone can post and remove notes. With a sticky bit, only the person who posted a note (or an admin) can remove it.

#### Technical Details
- The sticky bit is represented by a **`t`** or **`T`** in the **execute position** for **others** in a directory's permission string.
- It appears in the **10th character** (last) of the permission string in `ls -l` output.

#### Examples
```bash
cry0l1t3@htb:/htb$ ls -l 
drw-rw-r-t 3 cry0l1t3 cry0l1t3 4096 Jan 12 12:30 scripts 
drw-rw-r-T 3 cry0l1t3 cry0l1t3 4096 Jan 12 12:32 reports
```
- `scripts` directory: `t` — sticky bit is set, and **execute permission is enabled** for others.
- `reports` directory: `T` — sticky bit is set, but **execute permission is NOT set** for others. This means other users cannot access the contents or execute from the directory.

#### Set the Sticky Bit

To set the sticky bit on a directory:

```bash

chmod +t /path/to/directory
```

To remove the sticky bit:

```Bash
chmod -t /path/to/directory
```

---

## User Management

Effective user management is fundamental aspect for linux administration.Admins frequently need to create a new user accounts or assign existing users to specific groups to enforce appropriate access controls. Additionally, executing commands as other user is often necessary for tasks that require different privileges.

|**Command**|**Description**|
|---|---|
|`sudo`|Execute command as a different user.|
|`su`|The `su` utility requests appropriate user credentials via PAM and switches to that user ID (the default user is the superuser). A shell is then executed.|
|`useradd`|Creates a new user or update default new user information.|
|`userdel`|Deletes a user account and related files.|
|`usermod`|Modifies a user account.|
|`addgroup`|Adds a group to the system.|
|`delgroup`|Removes a group from the system.|
|`passwd`|Changes user password.|

---

## Package Management

The features that most package management systems provide are:
- Package downloading
- Dependency resolution
- A standard binary package format
- Common installation and configuration locations
- Additional system-related configuration and functionality
- Quality control

| **Command** | **Description**                                                                                                                                                                                                                                                                                                                                         |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dpkg`      | The `dpkg` is a tool to install, build, remove, and manage Debian packages. The primary and more user-friendly front-end for `dpkg` is aptitude.                                                                                                                                                                                                        |
| `apt`       | Apt provides a high-level command-line interface for the package management system.                                                                                                                                                                                                                                                                     |
| `aptitude`  | Aptitude is an alternative to apt and is a high-level interface to the package manager.                                                                                                                                                                                                                                                                 |
| `snap`      | Install, configure, refresh, and remove snap packages. Snaps enable the secure distribution of the latest apps and utilities for the cloud, servers, desktops, and the internet of things.                                                                                                                                                              |
| `gem`       | Gem is the front-end to RubyGems, the standard package manager for Ruby.                                                                                                                                                                                                                                                                                |
| `pip`       | Pip is a Python package installer recommended for installing Python packages that are not available in the Debian archive. It can work with version control repositories (currently only Git, Mercurial, and Bazaar repositories), logs output extensively, and prevents partial installs by downloading all requirements before starting installation. |
| `git`       | Git is a fast, scalable, distributed revision control system with an unusually rich command set that provides both high-level operations and full access to internals.                                                                                                                                                                                  |



#### Advanced Package Manager (APT)

Debian-based Linux distributions use the `APT` package manager. A package is an archive file containing multiple ".deb" files. The `dpkg` utility is used to install programs from the associated ".deb" file. `APT` makes updating and installing programs easier because many programs have dependencies. When installing a program from a standalone ".deb" file, we may run into dependency issues and need to download and install one or multiple additional packages. `APT` makes this easier and more efficient by packaging together all of the dependencies needed to install a program.

---

### Service and Process Management

Services, also known as daemons, are fundamental components of a Linux system that run silently in the background "without direct user interaction". They perform crucial tasks that keep the system operational and provide additional functionalities. Generally, services can be categorized into two types:


#### System Services
These are internal services required during system startup. They perform essential hardware-related tasks and initialize system components necessary for the operating system to function properly. These are like the engine and transmission systems

#### User-Installed Services
These services are added by users and typically include server applications and other background processes that provide specific features or capabilities. These types of services are like the car's air conditioning or GPS navigation system.

Daemons are often identified by the letter `d` at the end of their program names, such as `sshd` (SSH daemon) or `systemd`.  Linux system utilizes both system and user-installed services to function efficiently and meet user needs.

In general, there are just a few goals that we have when we deal with a service or a process:

1. Start/Restart a service/process
2. Stop a service/process
3. See what is/was happening with a service/process
4. Enable/Disable a service/process on boot
5. Find a service/process

Most modern Linux distributions have adopted `systemd` as their initialization system (init system). It is the first process that starts during the boot process and is assigned the Process ID (`PID`). All processes in a Linux system are assigned a `PID` and can be viewed under the `/proc/` directory, which contains information about each process. Processes may also have a Parent Process ID (`PPID`), indicating that they were started by another process (the parent), making them child processes.


#### Systemctl

We can also use `systemctl` to list all services.It is quite possible that the services do not start due to an error. To see the problem, we can use the tool `journalctl` to view the logs.

#### Kill a Process

A process can be in the following states:
- Running
- Waiting (waiting for an event or system resource)
- Stopped
- Zombie (stopped but still has an entry in the process table).

Processes can be controlled using `kill`, `pkill`, `pgrep`, and `killall`. To interact with a process, we must send a signal to it. 

The most commonly used signals are:

|**Signal**|**Description**|
|---|---|
|`1`|`SIGHUP` - This is sent to a process when the terminal that controls it is closed.|
|`2`|`SIGINT` - Sent when a user presses `[Ctrl] + C` in the controlling terminal to interrupt a process.|
|`3`|`SIGQUIT` - Sent when a user presses `[Ctrl] + D` to quit.|
|`9`|`SIGKILL` - Immediately kill a process with no clean-up operations.|
|`15`|`SIGTERM` - Program termination.|
|`19`|`SIGSTOP` - Stop the program. It cannot be handled anymore.|
|`20`|`SIGTSTP` - Sent when a user presses `[Ctrl] + Z` to request for a service to suspend. The user can handle it afterward.|
#### Background a Process
Sometimes it will be necessary to put the scan or process we just started in the background to continue using the current session to interact with the system or start other processes. As we have already seen, we can do this with the shortcut `[Ctrl + Z]`. As mentioned above, we send the `SIGTSTP` signal to the kernel, which suspends the process.


#### Foreground a Process

After that, we can use the `jobs` command to list all background processes. Backgrounded processes do not require user interaction, and we can use the same shell session without waiting until the process finishes first. Once the scan or process finishes its work, we will get notified by the terminal that the process is finished.

There are three possibilities to run several commands, one after the other. These are separated by:

- Semicolon (`;`)
- Double `ampersand` characters (`&&`)
- Pipes (`|`)

The difference between them lies in the previous processes' treatment and depends on whether the previous process was completed successfully or with errors. The semicolon (`;`) is a command separator and executes the commands by ignoring previous commands' results and errors.

For example, if we execute the same command but replace it in second place, the command `ls` with a file that does not exist, we get an error, and the third command will be executed nevertheless.

```shell-session
0xWAYNE@htb[/htb]$ echo '1'; ls MISSING_FILE; echo '3'

1
ls: cannot access 'MISSING_FILE': No such file or directory
3
```

However, it looks different if we use the double AND characters (`&&`) to run the commands one after the other. If there is an error in one of the commands, the following ones will not be executed anymore, and the whole process will be stopped.

```shell-session
0xWAYNE@htb[/htb]$ echo '1' && ls MISSING_FILE && echo '3'

1
ls: cannot access 'MISSING_FILE': No such file or directory
```

Pipes (`|`) depend not only on the correct and error-free operation of the previous processes but also on the previous processes' results.

---

#### Task Scheduling

Task scheduling is a critical feature in Linux systems that allows users and administrators to automate tasks by running them at specific times or regular intervals, eliminating the need for manual initiation. Available in distributions like Ubuntu, Red Hat Linux, and Solaris, this functionality manages a wide array of tasks such as automatic software updates, script execution, database maintenance, and backup automation. By scheduling regular and repetitive tasks, it ensures they are performed consistently and reliably. Additionally, alerts can be configured to notify administrators or users when certain events occur.


#### Systemd

Systemd is a service used in Linux systems such as Ubuntu, Redhat Linux, and Solaris to start processes and scripts at a specific time. With it, we can set up processes and scripts to run at a specific time or time interval and can also specify specific events and triggers that will trigger a specific task. To do this, we need to take some steps and precautions before our scripts or processes are automatically executed by the system.

1. Create a timer (schedules when your `mytimer.service` should run)
2. Create a service (executes the commands or script)
3. Activate the timer

#### Create a Timer

To create a timer for systemd, we need to create a directory where the timer script will be stored.

```shell-session
0xWAYNE@htb[/htb]$ sudo mkdir /etc/systemd/system/mytimer.timer.d
0xWAYNE@htb[/htb]$ sudo vim /etc/systemd/system/mytimer.timer
```

Next, we need to create a script that configures the timer. The script must contain the following options: "Unit", "Timer" and "Install". The "Unit" option specifies a description for the timer. The "Timer" option specifies when to start the timer and when to activate it. Finally, the "Install" option specifies where to install the timer.

#### Mytimer.timer


```txt
[Unit]
Description=My Timer

[Timer]
OnBootSec=3min
OnUnitActiveSec=1hour

[Install]
WantedBy=timers.target
```

Here it depends on how we want to use our script. For example, if we want to run our script only once after the system boot, we should use `OnBootSec` setting in `Timer`. However, if we want our script to run regularly, then we should use the `OnUnitActiveSec` to have the system run the script at regular intervals. Next, we need to create our `service`.

#### Create a Service


```shell-session
0xWAYNE@htb[/htb]$ sudo vim /etc/systemd/system/mytimer.service
```

Here we set a description and specify the full path to the script we want to run. The "multi-user.target" is the unit system that is activated when starting a normal multi-user mode. It defines the services that should be started on a normal system startup.


```txt
[Unit]
Description=My Service

[Service]
ExecStart=/full/path/to/my/script.sh

[Install]
WantedBy=multi-user.target
```

After that, we have to let `systemd` read the folders again to include the changes.

#### Reload Systemd

```shell-session
0xWAYNE@htb[/htb]$ sudo systemctl daemon-reload
```

After that, we can use `systemctl` to `start` the service manually and `enable` the autostart.

#### Start the Timer & Service

```shell-session
0xWAYNE@htb[/htb]$ sudo systemctl start mytimer.timer
0xWAYNE@htb[/htb]$ sudo systemctl enable mytimer.timer
```

This way, `mytimer.service` will be launched automatically according to the intervals (or delays) you set in `mytimer.timer`.

### Cron

Cron is another tool that can be used in Linux systems to schedule and automate processes. It allows users and administrators to execute tasks at a specific time or within specific intervals. For the above examples, we can also use Cron to automate the same tasks. We just need to create a script and then tell the cron daemon to call it at a specific time.

With Cron, we can automate the same tasks, but the process for setting up the Cron daemon is a little different than Systemd. To set up the cron daemon, we need to store the tasks in a file called `crontab` and then tell the daemon when to run the tasks. Then we can schedule and automate the tasks by configuring the cron daemon accordingly. The structure of Cron consists of the following components:

|**Time Frame**|**Description**|
|---|---|
|Minutes (0-59)|This specifies in which minute the task should be executed.|
|Hours (0-23)|This specifies in which hour the task should be executed.|
|Days of month (1-31)|This specifies on which day of the month the task should be executed.|
|Months (1-12)|This specifies in which month the task should be executed.|
|Days of the week (0-7)|This specifies on which day of the week the task should be executed.|

For example, such a crontab could look like this:

```txt
# System Update
0 */6 * * * /path/to/update_software.sh

# Execute scripts
0 0 1 * * /path/to/scripts/run_scripts.sh

# Cleanup DB
0 0 * * 0 /path/to/scripts/clean_database.sh

# Backups
0 0 * * 7 /path/to/scripts/backup.sh
```

---

# Network Services

When working with Linux, managing various network services is essential. Proficiency in handling these services is crucial for several reasons. Network services are designed to perform specific tasks, many of which enable remote operations.


#### SSH
Secure Shell (`SSH`) is a network protocol that allows the secure transmission of data and commands over a network. It is widely used to securely manage remote systems and securely access remote systems to execute commands or transfer files. In order to connect to our or a remote Linux host via SSH, a corresponding SSH server must be available and running.

#### NFS
Network File System (`NFS`) is a network protocol that allows us to store and manage files on remote systems as if they were stored on the local system. It enables easy and efficient management of files across networks. For example, administrators use NFS to store and manage files centrally (for Linux and Windows systems) to enable easy collaboration and management of data. For Linux, there are several NFS servers, including NFS-UTILS (`Ubuntu`), NFS-Ganesha (`Solaris`), and OpenNFS (`Redhat Linux`).

We can configure NFS via the configuration file `/etc/exports`. This file specifies which directories should be shared and the access rights for users and systems. It is also possible to configure settings such as the transfer speed and the use of encryption. NFS access rights determine which users and systems can access the shared directories and what actions they can perform. Here are some important access rights that can be configured in NFS:

|**Permissions**|**Description**|
|---|---|
|`rw`|Gives users and systems read and write permissions to the shared directory.|
|`ro`|Gives users and systems read-only access to the shared directory.|
|`no_root_squash`|Prevents the root user on the client from being restricted to the rights of a normal user.|
|`root_squash`|Restricts the rights of the root user on the client to the rights of a normal user.|
|`sync`|Synchronizes the transfer of data to ensure that changes are only transferred after they have been saved on the file system.|
|`async`|Transfers data asynchronously, which makes the transfer faster, but may cause inconsistencies in the file system if changes have not been fully committed.|

#### Web Server
Understanding the operation of web servers is essential for penetration testers, as these servers are integral to web applications and frequently serve as primary targets during security assessments. A web server is software that delivers data, documents, applications, and various functions over the Internet. It utilizes the Hypertext Transfer Protocol (`HTTP`) to transmit data to clients such as web browsers and to receive requests from these clients. The received data is then rendered as Hypertext Markup Language (`HTML`) within the client's browser, facilitating the creation of dynamic web pages that respond interactively to user requests

#### VPN

A Virtual Private Network (`VPN`) functions like a secure, invisible tunnel that connects us to another network, allowing seamless and protected access as if we were physically present within it. This is achieved by establishing an encrypted tunnel between the client and the server, ensuring that all data transmitted through this connection remains confidential and safeguarded from unauthorized access.

Organizations primarily utilize VPNs to grant their employees secure access to the internal network without requiring them to be on-site. This flexibility enables employees to reach internal resources and applications from any location, enhancing productivity and mobility. Additionally, VPNs serve to anonymize internet traffic and block external intrusions, further bolstering security.

For penetration testers, OpenVPN offers invaluable capabilities. It allows testers to securely connect to internal networks, especially when direct access is not feasible due to geographical constraints. By utilizing OpenVPN, penetration testers can perform comprehensive security assessments of internal systems, identifying and addressing potential vulnerabilities. The versatility of OpenVPN, with features such as encryption, tunneling, traffic shaping, network routing, and adaptability to dynamic network environments, makes it an essential tool in the arsenal of both network administrators and security professionals. We can install the server and client with the following command:


### Working with Web Services

Another crucial element in web development is the communication between browsers and web servers. Setting up a web server on a Linux operating system can be done in several ways, with popular options including Nginx, IIS, and Apache. Among these, Apache is one of the most widely used web servers. Think of Apache as the engine that powers your website, ensuring smooth communication between your website and visitors.


### Backup and Restore
Linux systems provide a range of powerful tools for backing up and restoring data, designed to be both efficient and secure. These tools help ensure that our data is not only protected from loss or corruption, but also easily accessible when we need it.

When backing up data on an Ubuntu system, we have several options, including:

- Rsync
- Deja Dup
- Duplicity

Rsync is an open-source tool that allows for fast and secure backups, whether locally or to a remote location. One of its key advantages is that it only transfers the portions of files that have changed, making it highly efficient when dealing with large amounts of data. Rsync is particularly useful for network transfers, such as syncing files between servers or creating incremental backups over the internet.

Duplicity is another powerful tool that builds on Rsync, but adds encryption features to protect the backups. It allows you to encrypt your backup copies, ensuring that sensitive data remains secure even if stored on remote servers, FTP sites, or cloud services like Amazon S3. Duplicity provides an extra layer of security while maintaining Rsync's efficient data transfer capabilities.

Deja Dup is a simple, accessible safe that anyone can operate, while still offering the same level of protection. Encrypting your backups adds an additional lock on your safe, ensuring that even if someone finds it, they can't get inside.

For users who prefer a simpler, more user-friendly option, Deja Dup offers a graphical interface that makes the backup process straightforward. Behind the scenes, it also uses Rsync, and like Duplicity, it supports encrypted backups. Deja Dup is ideal for users who want quick, easy access to backup and restore options without needing to dive into the command line.


---

# File System Management
Managing file systems on Linux is a crucial task that involves organizing, storing, and maintaining data on a disk or other storage device. Linux is a versatile operating system that supports many different file systems, including ext2, ext3, ext4, XFS, Btrfs, and NTFS, among others. Each of these file systems has unique features and is suited to specific use cases. The best file system choice depends on the specific requirements of the application or user such as:

- `ext2` is an older file system with no journaling capabilities, which makes it less suited for modern systems but still useful in certain low-overhead scenarios (like USB drives).
- `ext3` and `ext4` are more advanced, with journaling (which helps in recovering from crashes), and ext4 is the default choice for most modern Linux systems because it offers a balance of performance, reliability, and large file support.
- `Btrfs` is known for advanced features like snapshotting and built-in data integrity checks, making it ideal for complex storage setups.
- `XFS` excels at handling large files and has high performance. It is best suited for environments with high I/O demands
- `NTFS`, originally developed for Windows, is useful for compatibility when dealing with dual-boot systems or external drives that need to work on both Linux and Windows systems.


When selecting a file system, it’s essential to analyze the needs of the application or user factors such as performance, data integrity, compatibility, and storage requirements will influence the decision.

Linux file system architecture is based on Unix model organised hierarchial structure. This structure consists of several components. the most critical ones `inodes`. Inodes are data structures that  store metadata about each file and directory,including permissions,ownership,size and timestamps. Inodes do not store the file's actual data or name they contain pointers to blocks where file's data is stored on disk.

inode table is a collection of these inodes, A database that Linux kernel uses to track every file and directory on system..This type of structure allows Operating systems to efficiently access and manage files. Understanding and managing inodes is crucial aspect in scenarios where a disk is running out of inode space before running out of actual storage capacity.

Think of analogy where Linux file system like a library. The `inodes` are like index cards in the library’s catalog system (`inode table`). Each card contains detailed information about a book (file) its title, author, location, and other details but not the actual book. The `inode` table is the entire catalog that helps the library (operating system) quickly find and manage the books (files).

In Linux, files can be stored in one of several key types:

- Regular files
- Directories
- Symbolic links

#### Regular Files
Regular files are the most common type and typically consist of text data (such as ASCII) and/or binary data (such as images, audio, or executables). They reside in various directories throughout the file system, not just in the root directory. The root directory (/) is simply the top of the hierarchical directory tree, and files can exist in any directory within that structure.

#### Directories
Directories are special types of files that act as containers for other files (both regular files and other directories). When a file is stored in a directory, that directory is referred to as the file’s parent directory. Directories help organize files within the Linux file system, allowing for an efficient way to manage collections of files.

#### Symbolic Links
Linux also supports symbolic links (`symlinks`), which act as shortcuts or references to other files or directories. Symbolic links allow quick access to files located in different parts of the file system without duplicating the file itself. Symlinks can be used to streamline access or organize complex directory structures by pointing to important files across various locations.

### Disks & Drives

Disk mgmt involves managing physical storage devices such as hard drives, SSD and removable storage devices. Main tool for linux disk mgmt is `fdisk` which allows create, delete and manage partitions on a drive. Partitioning a drive on linux involves dividing the physical storage space into separate logical sections. Each partition can be formatted with specific file system such as ext4, NTFS, or FAT32 and can be mounted as seperate file systems.

Most common partitioning tool on linux is `fdisk`, `gpart`, and `GParted`.

### Mounting

Each logical partition or storage drive must be assigned to specific directory in file system. This process is known as mounting. Mounting involves linking a drive or partition to a directory making the contents accessible within the overall file system hierarchy. Once a drive is mounted to a directory(also known as mount point) it can be accessed and used like any other directory on system.

The mount command is commonly used to  manually mount file systems on linux. If you want file systems or partitions to be automatically mounted during system boot, you need to define them in `/etc/fstab` file. This file lists the file systems and their associated mount points, along with options like read/write permissions and file system types ensuring the specific drive or partitions are available upon starting without needing manual intervention.

#### Mount a USB drive
```shell-session
0xWAYNE@htb[/htb]$ sudo mount /dev/sdb1 /mnt/usb
```

#### Unmount
```shell-session
0xWAYNE@htb[/htb]$ sudo umount /mnt/usb
```

It is important to note that we must have sufficient permissions to unmount a file system. We also cannot unmount a file system that is in use by a running process. To ensure that there are no running processes that are using the file system, we can use the `lsof` command to list the open files on the file system.

If we find any processes that are using the file system, we need to stop them before we can unmount the file system. Additionally, we can also unmount a file system automatically when the system is shut down by adding an entry to the `/etc/fstab` file. The `/etc/fstab` file contains information about all the file systems that are mounted on the system, including the options for automatic mounting at boot time and other mount options. To unmount a file system automatically at shutdown, we need to add the `noauto` option to the entry in the `/etc/fstab` file for that file system. This would look like, for example, the following:

#### Fstab File
```txt
/dev/sda1 / ext4 defaults 0 0
/dev/sda2 /home ext4 defaults 0 0
/dev/sdb1 /mnt/usb ext4 rw,noauto,user 0 0
192.168.1.100:/nfs /mnt/nfs nfs defaults 0 0
```


### SWAP

Swap space is essential part of memory mgmt in linux and plays a crucial role in ensuring smooth system performance,especially when the available physical memory(RAM) is fully utilised. When system runs out of RAM, the kernel moves inactive pages of memory(data not being actively used) to swap space, freeing up RAM for active processes.This is known a swapping.

#### Creating Swap Space

Swap space can be set up either during the installation of the operating system or added later using the mkswap and swapon commands.

- `mkswap` is used to prepare a device or file to be used as swap space by creating a Linux swap area
- `swapon` activates the swap space, allowing the system to use it

#### Sizing and Managing Swap Space
The size of the `swap space` is not fixed and depends on your system's physical memory and intended usage. For example, a system with less RAM or running memory-intensive applications might need more swap space. However, modern systems with large amounts of RAM may require less or even no swap space, depending on specific use cases.

When setting up swap space, it’s important to allocate it on a dedicated partition or file, separate from the rest of the file system. This prevents fragmentation and ensures efficient use of the swap area when needed. Additionally, because sensitive data can be temporarily stored in swap space, it's recommended to encrypt the swap space to safeguard against potential data exposure.

#### Swap Space for Hibernation
Besides extending physical memory, swap space is also used for `hibernation`. Hibernation is a power-saving feature that saves the system’s state (including open applications and processes) to the swap space and powers off the system. When the system is powered back on, it restores its previous state from the swap space, resuming exactly where it left off.

## Containerization

- Containerization is the process of packaging and running applications in isolated environments called **containers**.
- It ensures consistent application behaviour across different systems by encapsulating all dependencies, configurations, and libraries.

##### Key Technologies
- **Docker**
- **Docker Compose**
- **Linux Containers (LXC)**

These tools enable easy creation, deployment, and management of containerised applications, primarily on Linux-based systems.


### Difference from Virtual Machines (VMs)

|Feature|Containers|Virtual Machines|
|---|---|---|
|Kernel Usage|Share host OS kernel|Include separate OS kernel|
|Resource Efficiency|Lightweight, minimal overhead|Heavier, due to full OS per VM|
|Startup Time|Fast|Slower|
|Isolation Level|Process-level|Hardware-level|

#### Benefits
- **Lightweight**: Minimal resource usage allows running multiple containers simultaneously.
- **Consistent Environments**: Containers behave the same across dev, test, and production.
- **Portability**: Run anywhere with a compatible container runtime.
- **Scalability**: Ideal for microservices and distributed architectures.
- **Fast Deployment**: Quick to spin up and destroy containers as needed.

##### Analogy
- Like portable "stage pods" for bands that contain all required equipment and can be deployed on a shared main stage, containers provide isolated environments that run on a shared host without interference.

#### Security Considerations
- **Isolation**: Containers isolate applications from each other and from the host.
- **Reduced Attack Surface**: Minimises the risk of one container affecting another or the host.
- **Not Full Isolation**: Weaker than VMs; kernel vulnerabilities can lead to:
    - **Privilege Escalation**
    - **Container Escape**
- **Hardening Required**: Proper configuration and monitoring are critical.

##### Limitations
- Not immune to:
    - Misconfigurations
    - Privilege escalation attacks
    - Breakouts (e.g., CVE exploits enabling access to host system)
- Requires secure base images, regular updates, and runtime hardening.

##### Use Cases
- Microservice architectures
- CI/CD pipelines
- Development/testing consistency
- Scalable web applications


### Docker

Docker is an open source platform for automating deployment of applications as self contained units called containers.It uses a layered file systems and resource isolation features to provide flexibility and portability. Additionally it provides a robust set of tools for creating, deploying and managing applications which help streamline the containerisation process.

Imagine Docker containers as a sealed lunchbox. You can eat the food (run applications) inside, but once you close the box (stop the container), everything resets. To make a new lunchbox (new container) with updated contents (modified configurations), you create a new recipe (Dockerfile) based on the original. When serving multiple lunchboxes in a restaurant (production), you'd use a kitchen system (Kubernetes/Docker Compose) to manage all the orders smoothly.

### Docker Engine & Docker Hub

- **Docker Engine**: Core component that runs containers using defined Docker images.
- **Docker Hub**: Official cloud-based registry for Docker images.
    - **Public Area**:
        - Contains community-contributed images.
        - Hosts official images from Docker and well-known open-source projects.

    - **Private Area**:
        - For internal use by teams or organizations.
        - Not publicly accessible.

### Creating a Docker Image
### Dockerfile Overview
A **Dockerfile** is a text file containing a list of instructions to create a Docker image. The Docker engine reads this file and builds a container environment accordingly.

#### Use Case

To set up a **file hosting server** using:

- Ubuntu 22.04    
- Apache HTTP server
- SSH server


```dockerfile
# Use the latest Ubuntu 22.04 LTS as the base image
FROM ubuntu:22.04

# Install Apache and OpenSSH server
RUN apt-get update && \
    apt-get install -y \
        apache2 \
        openssh-server \
        && \
    rm -rf /var/lib/apt/lists/*

# Create a new user and set password
RUN useradd -m docker-user && \
    echo "docker-user:password" | chpasswd

# Assign permissions to the docker-user
RUN chown -R docker-user:docker-user /var/www/html && \
    chown -R docker-user:docker-user /var/run/apache2 && \
    chown -R docker-user:docker-user /var/log/apache2 && \
    chown -R docker-user:docker-user /var/lock/apache2 && \
    usermod -aG sudo docker-user && \
    echo "docker-user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Expose SSH and HTTP ports
EXPOSE 22 80

# Start SSH and Apache services
CMD service ssh start && /usr/sbin/apache2ctl -D FOREGROUND
```


### Building the Docker Image

```build cmd
docker build -t custom-ubuntu-apache-ssh 
```

- `-t`: Tags the image with a name (`custom-ubuntu-apache-ssh`).
- `.`: Specifies the build context (current directory).

> If any instruction in the Dockerfile fails, the build process is aborted.

Once the Docker image has been created, it can be executed through the Docker engine, making it a very efficient and easy way to run a container. It is similar to the virtual machine concept, based on images. Still, these images are read-only templates and provide the file system necessary for runtime and all parameters. A container can be considered a running process of an image. When a container is to be started on a system, a package with the respective image is first loaded if unavailable locally. We can start the container by the following command [docker run](https://docs.docker.com/engine/reference/commandline/run/):

#### Docker Run - Syntax

```shell-session
0xWAYNE@htb[/htb]$ docker run -p <host port>:<docker port> -d <docker container name>
```

#### Docker Run

```shell-session
0xWAYNE@htb[/htb]$ docker run -p 8022:22 -p 8080:80 -d FS_docker
```

In this case, we start a new container from the image `FS_docker` and map the host ports 8022 and 8080 to container ports 22 and 80, respectively. The container runs in the background, allowing us to access the SSH and HTTP services inside the container using the specified host ports.


#### Docker Management
When managing Docker containers, Docker provides a comprehensive suite of tools that enable us to easily create, deploy, and manage containers. With these powerful tools, we can list, start and stop containers and effectively manage them, ensuring seamless execution of applications. Some of the most commonly used Docker management commands are:

|**Command**|**Description**|
|---|---|
|`docker ps`|List all running containers|
|`docker stop`|Stop a running container.|
|`docker start`|Start a stopped container.|
|`docker restart`|Restart a running container.|
|`docker rm`|Remove a container.|
|`docker rmi`|Remove a Docker image.|
|`docker logs`|View the logs of a container.|

### Docker Image vs. Container

- **Docker Image**: Read-only template with application code, libraries, and dependencies.
- **Docker Container**: A **runtime instance** of an image (an executable, isolated environment).
- When starting a container, the Docker engine checks for the image locally; if not found, it pulls it from a registry (e.g., Docker Hub).

### Advanced Usage & Best Practices

#### 1. Persistence and State
- Containers are stateless by default.
- Any change made inside the container is **lost on stop or delete**.
- To persist data:
    - Use **Docker volumes**:
```Bash
docker run -v /host/path:/container/path ..
```        

#### 2. Customizing Containers

- To preserve modifications:    
    - Write a new **Dockerfile**.
    - Use `FROM` to specify the base image.
    - Add steps for your changes.
    - Build the new image:

```Bash
docker build -t updated-image .
```        

---

## **Scaling & Orchestration**

- Managing many containers requires orchestration tools:
    - **Docker Compose**: Define and manage multi-container applications using `docker-compose.yml`.
    - **Kubernetes**: Enterprise-grade container orchestration for deployment, scaling, and management.

### Use Case for Security and File Transfers
- **Apache**: Host files accessible via `curl`, `wget`, or browser.
- **SSH Server**: Use `scp` to securely copy files into the container.
- **Useful in CTFs, pentesting labs**, or controlled environments for file staging.

### Linux Containers

LXC is lightweight virtualisation technology that allow multiple isolated linux systems called container to run on single host. LXC uses key resource isolation features such as control groups and namespaces to ensure that each container operates independently. Unlike VM which require full OS for each instance containers share host's kernel making LXC more efficient in terms of resource usage.

### LXC vs Docker – Comparison of Containerization Approaches
- Both **LXC (Linux Containers)** and **Docker** are containerization technologies that isolate processes and environments on a Linux system.
- However, they differ in design philosophy, use cases, and user experience.

#### Comparison Table

|**Category**|**LXC**|**Docker**|
|---|---|---|
|**Approach**|System-level containerization — acts like a lightweight VM.|Application-level containerization — packages a single app with dependencies.|
|**Image Building**|Manual setup of root filesystem and configurations.|Uses `Dockerfile` to automate image builds including app, dependencies, and environment.|
|**Portability**|Less portable — closely tied to the host OS.|Highly portable — standardized image format easily shareable via Docker Hub or other registries.|
|**Ease of Use**|More complex — requires strong knowledge of Linux internals.|User-friendly — simple CLI, large community support, and extensive documentation.|
|**Security**|Requires additional configuration for tight isolation.|Strong default isolation — integrates with **AppArmor**, **SELinux**, and uses read-only FS.|

#### Technical Notes
- **LXC** creates full **Linux environments**, ideal for users who need system-level functionality (e.g., simulating full OS behavior).
- **Docker** simplifies deployment of applications by abstracting system internals and focusing on **microservice architecture**.
- Both tools use Linux kernel features like **cgroups** and **namespaces** for resource control and isolation.

### Security Considerations
- Docker includes:
    - Mandatory Access Controls (AppArmor/SELinux)
    - Seccomp profiles
    - Read-only image layers

- LXC can be hardened but may expose greater attack surfaces if misconfigured.
- **Both** can become vectors for **local privilege escalation** if not securely configured — a key focus area in Linux exploitation and defense.

#### Summary
- **Use LXC** when you need:
    - System-level virtualization
    - Full OS-like containers
    - Deep control over Linux internals

- **Use Docker** when you need:
    - Fast, repeatable app deployment
    - Lightweight containers with high portability
    - Simple tooling and DevOps integration

To install LXC on a Linux distribution, we can use the distribution's package manager. For example, on Ubuntu, we can use the `apt` package manager to install LXC with the following command:

#### Install LXC
```shell-session
0xWAYNE@htb[/htb]$ sudo apt-get install lxc lxc-utils -y
```

Once LXC is installed, we can start creating and managing containers on the Linux host. It is worth noting that LXC requires the Linux kernel to support the necessary features for containerization. Most modern Linux kernels have built-in support for containerization, but some older kernels may require additional configuration or patching to enable support for LXC.

#### Creating an LXC Container

To create a new LXC container, we can use the `lxc-create` command followed by the container's name and the template to use. For example, to create a new Ubuntu container named `linuxcontainer`, we can use the following command:

  Containerization

```shell-session
0xWAYNE@htb[/htb]$ sudo lxc-create -n linuxcontainer -t ubuntu
```

#### Managing LXC Containers

When working with LXC containers, several tasks are involved in managing them. These tasks include creating new containers, configuring their settings, starting and stopping them as necessary, and monitoring their performance. Fortunately, there are many command-line tools and configuration files available that can assist with these tasks. These tools enable us to quickly and easily manage our containers, ensuring they are optimized for our specific needs and requirements. By leveraging these tools effectively, we can ensure that our LXC containers run efficiently and effectively, allowing us to maximize our system's performance and capabilities.

|Command|Description|
|---|---|
|`lxc-ls`|List all existing containers|
|`lxc-stop -n <container>`|Stop a running container.|
|`lxc-start -n <container>`|Start a stopped container.|
|`lxc-restart -n <container>`|Restart a running container.|
|`lxc-config -n <container name> -s storage`|Manage container storage|
|`lxc-config -n <container name> -s network`|Manage container network settings|
|`lxc-config -n <container name> -s security`|Manage container security settings|
|`lxc-attach -n <container>`|Connect to a container.|
|`lxc-attach -n <container> -f /path/to/share`|Connect to a container and share a specific directory or file.|

### Linux Containers in Penetration Testing

#### Why Use Containers in Penetration Testing?

- **Containers** package software with all its dependencies (code, libraries, configs) into a lightweight, standalone unit.
- They provide **consistent environments** across any Linux host regardless of the host’s configuration.   
- Useful for testing complex software stacks or applications with intricate dependencies without manual setup.
- Enable creation of **isolated environments** simulating vulnerable systems/networks for safe exploit or malware testing.
- Containers reduce risk to the tester’s local machine or production network by sandboxing potentially harmful code.

### Key Benefits of Containers for Pen Testers

- Quickly create tailored environments for testing specific software versions or configurations.
- Easily reset environments to a clean state after tests.
- Facilitate automation of exploit testing and vulnerability assessments.
- Help maintain reproducibility and consistency across test systems.


### LXC Container Security Best Practices

#### 1. Restrict Access to Containers
- Limit entry points (e.g., disable SSH if unnecessary).
- Use strong authentication and secure protocols.
- Restrict SSH access to trusted IPs or remove openssh-server inside container if not needed.

#### 2. Limit Container Resources

- Prevent containers from consuming excessive CPU, memory, or disk resources.
- Use **cgroups** (control groups) to enforce resource quotas.

##### Example: CPU and Memory Limits for LXC Container

Create or edit the container config file:

```Bash
sudo vim /usr/share/lxc/config/linuxcontainer.conf`
```

Add resource limits:

```ini
lxc.cgroup.cpu.shares = 512 lxc.cgroup.memory.limit_in_bytes = 512M
```

- `lxc.cgroup.cpu.shares` defines relative CPU time (default 1024). Setting it to 512 means the container gets half the CPU time compared to default.
- `lxc.cgroup.memory.limit_in_bytes` caps the container’s memory usage (512 megabytes in this example).

```C#
[Esc] :wq
```

Apply changes by restarting LXC service:

```Bash
sudo systemctl restart lxc.service
```


#### 3. Isolate Containers Using Linux Namespaces

- **Namespaces** isolate:
    - Process IDs (pid)
    - Network interfaces (net)
    - Mount points/filesystems (mnt)
    - User IDs, IPC, and more


This means each container:
- Has its own process ID space, preventing interference with host or other containers.
- Operates with isolated networking (interfaces, routing, firewall rules).
- Uses a separate root filesystem from the host, preventing direct file system interference.

#### 4. Additional Security Measures

- Keep containers up to date with security patches.
- Enforce **Mandatory Access Controls** (MAC) like AppArmor or SELinux if supported.
- Disable unnecessary services and network exposure.
- Monitor container activities and logs.

### Important Security Note

- Namespaces provide **isolation**, but **not full security**.
- Containers share the same host kernel, so kernel vulnerabilities can potentially be exploited for container breakout.
- Harden container configurations and host system to minimize attack surfaces.


### Network Config

As a pentester, managing and configuring network in Linux is essential. Mastering this skill allow to setup testing environments, manipulate network traffic and identify or exploit vulns efficiently. A good understanding of linux network configs give us ability to tailor our testing approach to suit specific needs help optimise both our testing procedures and results.

One of the primary tasks in network configuration is managing network interfaces. This involves assigning IP addresses, configuring network devices such as routers and switches, and setting up various network protocols. A deep understanding of network protocols, including TCP/IP (the core protocol suite for Internet communications), DNS (domain name resolution), DHCP (for dynamic IP address allocation), and FTP (file transfer), is critical. We must also be familiar with different types of network interfaces—whether wired or wireless—and be able to troubleshoot connectivity issues.

Network Access Control (NAC) governs how devices and users gain access to network resources, enforcing policies to enhance security and prevent unauthorized access.

Key NAC Models

|Type|Description|
|---|---|
|**Discretionary Access Control (DAC)**|Resource owners set permissions for access; flexible but less secure.|
|**Mandatory Access Control (MAC)**|OS enforces access policies, independent of resource owner; more secure, less flexible.|
|**Role-Based Access Control (RBAC)**|Permissions assigned based on organizational roles; simplifies privilege management.|
#### Configuring NAC on Linux
- **SELinux (Security-Enhanced Linux):** Implements MAC policies to enforce fine-grained security controls.
- **AppArmor:** Provides application-level security profiles to restrict programs' capabilities.
- **TCP Wrappers:** Controls access to network services by filtering connections based on IP addresses or hostnames.


These tools allow defining and enforcing security policies that control network device behavior and access permissions.

#### Monitoring & Analysis Tools for Network Traffic

- **syslog / rsyslog:** Centralized logging systems that capture and store system and network events.
- **ss:** Displays detailed socket statistics, useful for checking active network connections.
- **lsof:** Lists open files and associated network sockets, helping identify network resource usage.
- **ELK Stack (Elasticsearch, Logstash, Kibana):** Powerful platform for collecting, analyzing, and visualizing network logs and traffic patterns.

These tools assist in detecting anomalies, information leaks, breaches, and other critical network issues.

#### Analogy for Understanding NAC

- Configuring network interfaces = wiring the building infrastructure.
- NAC = managing building security:
    - DAC = rooms open to anyone the owner permits.
    - MAC = rooms secured by strict building rules.
    - RBAC = access based on employees’ roles.

- Monitoring traffic = surveillance cameras and alarms.
- Troubleshooting = toolkit for fixing broken connections, locks, or vulnerabilities.


### Configuring Network Interfaces in Linux

In Linux systems, network interfaces can be configured using the `ifconfig` or `ip` commands. These tools allow administrators and users to view, modify, and troubleshoot networking settings such as IP addresses, interface states, and broadcast information.

Understanding how to work with network interfaces is a fundamental skill in modern networking, penetration testing, and system administration.

#### `ifconfig` vs `ip`

|Feature|`ifconfig` (legacy)|`ip` (modern)|
|---|---|---|
|Status|Deprecated in many modern distributions|Actively supported and feature-rich|
|Usability|Simple and familiar|More versatile with detailed options|
|Scope|Interfaces only|Interfaces, IPs, routes, tunnels, etc.|

##### Sample Output: `ifconfig`
### eth0
- **IPv4**: 178.62.32.126
- **Netmask**: 255.255.192.0
- **Broadcast**: 178.62.63.255
- **MAC**: 8a:d9:fa:cf:79:7a

### eth1
- **IPv4**: 10.106.0.66
- **Netmask**: 255.255.240.0
- **Broadcast**: 10.106.15.255
- **MAC**: ba:ab:52:32:1f:33

### lo (Loopback)
- **IPv4**: 127.0.0.1
- **Netmask**: 255.0.0.0
- **IPv6**: ::1


#### Sample Output: `ip addr`
`$ ip addr`

### lo
- **IPv4**: 127.0.0.1/8
- **IPv6**: ::1/128
- **Type**: Loopback

### eth0
- **IPv4**: 178.62.32.126/18
- **Broadcast**: 178.62.63.255
- **MAC**: 8a:d9:fa:cf:79:7a
- **Alt names**: enp0s3, ens3

### eth1
- **IPv4**: 10.106.0.66/20
- **Broadcast**: 10.106.15.255
- **MAC**: ba:ab:52:32:1f:33    
- **Alt names**: enp0s4, ens4



#### Activating and Configuring Interfaces

##### Using `ifconfig`

```Bash
sudo ifconfig eth0 up            # Bring interface up
sudo ifconfig eth0 down          # Bring interface down
sudo ifconfig eth0 192.168.1.10 netmask 255.255.255.0
```
##### Using `ip`
```Bash
sudo ip link set eth0 up         # Activate eth0
sudo ip link set eth0 down       # Deactivate eth0
sudo ip addr add 192.168.1.10/24 dev eth0
sudo ip addr del 192.168.1.10/24 dev eth0
```

#### Activate Network Interface
```shell-session
0xWAYNE@htb[/htb]$ sudo ifconfig eth0 up     # OR
0xWAYNE@htb[/htb]$ sudo ip link set eth0 up
```

One way to allocate an IP address to a network interface is by utilizing the `ifconfig` command. We must specify the interface's name and IP address as arguments to do this. This is a crucial step in setting up a network connection. The IP address serves as a unique identifier for the interface and enables the communication between devices on the network.

#### Assign IP Address to an Interface
```shell-session
0xWAYNE@htb[/htb]$ sudo ifconfig eth0 192.168.1.2
```

To set the netmask for a network interface, we can run the following command with the name of the interface and the netmask:

#### Assign a Netmask to an Interface

```shell-session
0xWAYNE@htb[/htb]$ sudo ifconfig eth0 netmask 255.255.255.0
```

When we want to set the default gateway for a network interface, we can use the `route` command with the `add` option. This allows us to specify the gateway's IP address and the network interface to which it should be applied. By setting the default gateway, we are designating the IP address of the router that will be used to send traffic to destinations outside the local network. Ensuring that the default gateway is set correctly is important, as incorrect configuration can lead to connectivity issues.

#### Assign the Route to an Interface

```shell-session
0xWAYNE@htb[/htb]$ sudo route add default gw 192.168.1.1 eth0
```

When configuring a network interface in Linux, it is often necessary to set Domain Name System (`DNS`) servers to ensure proper network functionality. DNS servers are responsible for translating domain names (like example.com) into IP addresses, which allows devices to locate and connect to one another on the internet. Proper DNS configuration is crucial for enabling devices to access websites, online services, and other networked resources. Without correctly configured DNS servers, devices may experience issues such as the inability to resolve domain names, leading to network connectivity problems.

On Linux systems, this can be achieved by updating the `/etc/resolv.conf` file, which is a simple text file containing the system’s DNS information. By adding the appropriate DNS server addresses (Google's public DNS - `8.8.8.8` or `8.8.4.4`), the system can correctly resolve domain names to IP addresses, ensuring smooth communication over the network.

#### Editing DNS Settings

```shell-session
0xWAYNE@htb[/htb]$ sudo vim /etc/resolv.conf
```

#### /etc/resolv.conf

```txt
nameserver 8.8.8.8
nameserver 8.8.4.4
```

After completing the necessary modifications to the network configuration, it is essential to ensure that these changes are saved to persist across reboots. This can be achieved by editing the `/etc/network/interfaces` file, which defines network interfaces for Linux-based operating systems. Thus, it is vital to save any changes made to this file to avoid any potential issues with network connectivity.

It’s important to note that changes made directly to the `/etc/resolv.conf` file are not persistent across reboots or network configuration changes. This is because the file may be automatically overwritten by network management services like `NetworkManager` or `systemd-resolved`. To make DNS changes permanent, you should configure DNS settings through the appropriate network management tool, such as editing network configuration files or using network management utilities that store persistent settings.

#### Editing Interfaces

```shell-session
0xWAYNE@htb[/htb]$ sudo vim /etc/network/interfaces
```

This will open the `interfaces` file in the vim editor. We can add the network configuration settings to the file like this:

#### /etc/network/interfaces
```txt
auto eth0
iface eth0 inet static
  address 192.168.1.2
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 8.8.8.8 8.8.4.4
```

By setting the `eth0` network interface to use a static IP address of `192.168.1.2`, with a netmask of `255.255.255.0` and a default gateway of `192.168.1.1`, we can ensure that your network connection remains stable and reliable. Additionally, by specifying DNS servers of `8.8.8.8` and `8.8.4.4`, we can ensure that our computer can easily access the internet and resolve domain names. Once we have made these changes to the configuration file, saving the file and exiting the editor is important. After that, we must restart the networking service to apply the changes.

#### Restart Networking Service

  Network Configuration

```shell-session
0xWAYNE@htb[/htb]$ sudo systemctl restart networking
```

---

## Network Access Control

Network access control (NAC) is a crucial component of network security, especially in today's era of increasing cyber threats. As a penetration tester, it is vital to understand the significance of NAC in protecting the network and the various NAC technologies that can be utilized to enhance security measures. NAC is a security system that ensures that only authorized and compliant devices are granted access to the network, preventing unauthorized access, data breaches, and other security threats. By implementing NAC, organizations can be confident in their ability to protect their assets and data from cybercriminals who always seek to exploit system vulnerabilities. The following are the different NAC technologies that can be used to enhance security measures:

- Discretionary access control (DAC)
- Mandatory access control (MAC)
- Role-based access control (RBAC)

These technologies are designed to provide different levels of access control and security. Each technology has its unique characteristics and is suitable for different use cases. As a penetration tester, it is essential to understand these technologies and their specific use cases to test and evaluate the network's security effectively.

#### Discretionary Access Control

DAC is a crucial component of modern security systems as it helps organizations provide access to their resources while managing the associated risks of unauthorized access. It is a widely used access control system that enables users to manage access to their resources by granting resource owners the responsibility of controlling access permissions to their resources. This means that users and groups who own a specific resource can decide who has access to their resources and what actions they are authorized to perform. These permissions can be set for reading, writing, executing, or deleting the resource.

#### Mandatory Access Control

MAC is used in infrastructure that provides more fine-grained control over resource access than DAC systems. Those systems define rules that determine resource access based on the resource's security level and the user's security level or process requesting access. Each resource is assigned a security label that identifies its security level, and each user or process is assigned a security clearance that identifies its security level. Access to a resource is only granted if the user's or process's security level is equal to or greater than the security level of the resource. MAC is often used in operating systems and applications that require a high level of security, such as military or government systems, financial systems, and healthcare systems. MAC systems are designed to prevent unauthorized access to resources and minimize the impact of security breaches.

#### Role-based Access Control

RBAC assigns permissions to users based on their roles within an organization. Users are assigned roles based on their job responsibilities or other criteria, and each role is granted a set of permissions that determine the actions they can perform. RBAC simplifies the management of access permissions, reduces the risk of errors, and ensures that users can access only the resources necessary to perform their job functions. It can restrict access to sensitive resources and data, limit the impact of security breaches, and ensure compliance with regulatory requirements. Compared to Discretionary Access Control (DAC) systems, RBAC provides a more flexible and scalable approach to managing resource access. In an RBAC system, each user is assigned one or more roles, and each role is assigned a set of permissions that define the user's actions. Resource access is granted based on the user's assigned role rather than their identity or ownership of the resource. RBAC systems are typically used in environments with many users and resources, such as large organizations, government agencies, and financial institutions.

---

## Monitoring

Network monitoring involves capturing, analyzing, and interpreting network traffic to identify security threats, performance issues, and suspicious behavior. The primary goal of analyzing and monitoring network traffic is identifying security threats and vulnerabilities. For example, as penetration testers, we can capture credentials when someone uses an unencrypted connection and tries to log in to an FTP server. As a result, we will obtain this user’s credentials that might help us to infiltrate the network even further or escalate our privileges to a higher level. In short, by analyzing network traffic, we can gain insights into network behavior and identify patterns that may indicate security threats. Such analysis includes detecting suspicious network activity, identifying malicious traffic, and identifying potential security risks. However, we cover this vast topic in the [Intro to Network Traffic Analysis](https://academy.hackthebox.com/module/details/81) module, where we use several tools for network monitoring on Linux systems like Ubuntu and Windows systems, like Wireshark, tshark, and Tcpdump.

---

## Troubleshooting

Network troubleshooting is an essential process that involves diagnosing and resolving network issues that can adversely affect the performance and reliability of the network. This process is critical for ensuring the network operates optimally and avoiding disruptions that could impact business operations during our penetration tests. It also involves identifying, analyzing, and implementing solutions to resolve problems. Such problems include connectivity problems, slow network speeds, and network errors. Various tools can help us identify and resolve issues regarding network troubleshooting on Linux systems. Some of the most commonly used tools include:

1. Ping
2. Traceroute
3. Netstat
4. Tcpdump
5. Wireshark
6. Nmap

By using these tools and others like them, we can better understand how the network functions and quickly diagnose any issues that may arise. For example, `ping` is a command-line tool used to test connectivity between two devices. It sends packets to a remote host and measures the time to return them. To use `ping`, we can enter the following command:

#### Ping

  Network Configuration

```shell-session
0xWAYNE@htb[/htb]$ ping <remote_host>
```

For example, pinging the Google DNS server will send ICMP packets to the Google DNS server and display the response times.

  Network Configuration

```shell-session
0xWAYNE@htb[/htb]$ ping 8.8.8.8

PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=119 time=1.61 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=119 time=1.06 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=119 time=0.636 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=119 time=0.685 ms
^C
--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3017ms
rtt min/avg/max/mdev = 0.636/0.996/1.607/0.388 ms
```

Another tool is the `traceroute`, which traces the route packets take to reach a remote host. It sends packets with increasing Time-to-Live (TTL) values to a remote host and displays the IP addresses of the devices that the packets pass through. For example, to trace the route to the Google DNS server, we would enter the following command:

#### Traceroute

  Network Configuration

```shell-session
0xWAYNE@htb[/htb]$ traceroute www.inlanefreight.com

traceroute to www.inlanefreight.com (134.209.24.248), 30 hops max, 60 byte packets
 1  * * *
 2  10.80.71.5 (10.80.71.5)  2.716 ms  2.700 ms  2.730 ms
 3  * * *
 4  10.80.68.175 (10.80.68.175)  7.147 ms  7.132 ms 10.80.68.161 (10.80.68.161)  7.393 ms
```

This will display the IP addresses of the devices that the packets pass through to reach the Google DNS server. The output of a traceroute command shows how it is used to trace the path of packets to the website [www.inlanefreight.com](http://www.inlanefreight.com/), which has an IP address of 134.209.24.248. Each line of the output contains valuable information.

When setting up a network connection, it's important to specify the destination host and IP address. In this example, the destination host is 134.209.24.248, and the maximum number of hops allowed is 30. This ensures that the connection is established efficiently and reliably. By providing this information, the system can route traffic to the correct destination and limit the number of intermediate stops the data needs to make.

The second line shows the first hop in the traceroute, which is the local network gateway with the IP address 10.80.71.5, followed by the next three columns show the time it took for each of the three packets sent to reach the gateway in milliseconds (2.716 ms, 2.700 ms, and 2.730 ms).

Next, we see the second hop in the traceroute. However, there was no response from the device at that hop, indicated by the three asterisks instead of the IP address. This could mean the device is down, blocking ICMP traffic, or a network issue caused the packets to drop.

In the fourth line, we can see the third hop in the traceroute, consisting of two devices with IP addresses 10.80.68.175 and 10.80.68.161, and again the next three columns show the time it took for each of the three packets to reach the first device (7.147 ms, 7.132 ms, and 7.393 ms).

#### Netstat

`Netstat` is used to display active network connections and their associated ports. It can be used to identify network traffic and troubleshoot connectivity issues. To use `netstat`, we can enter the following command:

  Network Configuration

```shell-session
0xWAYNE@htb[/htb]$ netstat -a

Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 localhost:5901          0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:sunrpc          0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:http            0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:ssh             0.0.0.0:*               LISTEN
...SNIP...
```

We can expect to receive detailed information about each connection when using this tool. This includes the protocol used, the number of bytes received and sent, IP addresses, port numbers of both local and remote devices, and the current connection state. The output provides valuable insights into the network activity on the system, highlighting four specific connections currently active and listening on specific ports. These connections include the VNC remote desktop software, the Sun Remote Procedure Call service, the HTTP protocol for web traffic, and the SSH protocol for secure remote shell access. By knowing which ports are used by which services, users can quickly identify any network issues and troubleshoot accordingly. The most common network issues we will encounter during our penetration tests are as follows:

- Network connectivity issues
- DNS resolution issues (it's always about DNS)
- Loss of data packets
- Network performance issues

The most common causes for them are:

- Incorrectly configured firewalls or routers,
- damaged network cables or connections,
- incorrect network settings,
- hardware failures,
- incorrect DNS server settings or DNS server failures
- incorrectly configured DNS entries,
- network congestion,
- outdated network hardware or incorrectly configured network settings,
- unpatched software or firmware and missing security controls.

Understanding these common network issues and their causes is important for effectively identifying and exploiting vulnerabilities in network systems during our testing.

---

## Hardening

Several mechanisms are highly effective in securing Linux systems in keeping our and other companies' data safe. Three such mechanisms are SELinux, AppArmor, and TCP wrappers. These tools are designed to safeguard Linux systems against various security threats, from unauthorized access to malicious attacks. This is critical not only during penetration tests, where systems are intentionally stressed to uncover vulnerabilities, but also in real-world scenarios where an actual compromise could have serious consequences (few situations are as severe as a real-life breach.) By implementing these security measures and ensuring that we set up corresponding protection against potential attackers, we can significantly reduce the risk of data leaks and ensure our systems remain secure. While these tools share some similarities, they also have important differences.

#### Security-Enhanced Linux

Security-Enhanced Linux (`SELinux`) is a mandatory access control (`MAC`) system integrated into the Linux kernel. It provides fine-grained control over access to system resources and applications by enforcing security policies. These policies define the permissions for each process and file on the system, significantly limiting the damage that a compromised process or service can do. SELinux operates at a low level, and though it offers strong security, it can be complex to configure and manage due to its granular controls.

#### AppArmor

Like SELinux, `AppArmor` is a MAC system that controls access to system resources and applications, but it operates in a simpler, more user-friendly manner. AppArmor is implemented as a Linux Security Module (`LSM`) and uses application profiles to define what resources an application can access. While it may not provide the same level of fine-grained control as SELinux, AppArmor is often easier to configure and is generally considered more straightforward for day-to-day use.

#### TCP Wrappers

`TCP wrappers` are a host-based network access control tool that restricts access to network services based on the IP address of incoming connections. When a network request is made, TCP wrappers intercept it, checking the request against a list of allowed or denied IP addresses. This is a simple yet effective way to control access to services, especially for blocking unauthorized systems from accessing networked resources. While it does not offer the fine-grained control of SELinux or AppArmor, TCP wrappers are an excellent tool for basic network-level protection.

Regarding similarities, the three security mechanisms share the common goal of ensuring the safety and security of Linux systems. In addition to providing extra protection, they can restrict access to resources and services, thus reducing the risk of unauthorized access and data breaches. It's also worth noting that these mechanisms are readily available as part of most Linux distributions, making them accessible to us to enhance their systems' security. Furthermore, these mechanisms can be easily customized and configured using standard tools and utilities, making them a convenient choice for Linux users.

Although both `SELinux` and `AppArmor` are MAC systems that provide fine-grained control, they work in different ways. SELinux is deeply integrated into the kernel and offers more detailed security controls, but it can be more complex to configure and maintain. In contrast, AppArmor operates as a kernel module and uses profile-based security, making it easier to manage, though it may not offer the same level of granularity as SELinux.

On the other hand, `TCP wrappers` focus on controlling access to network services based on client IP addresses, which makes it simpler but limited to network-level access control. It doesn't offer the broader system resource protections that SELinux and AppArmor provide, but it’s useful for restricting access to services from unauthorized systems.

---

## Setting Up

As we navigate the world of Linux, we inevitably encounter a wide range of technologies, applications, and services that we need to become familiar with. This is a crucial skill, particularly if we work in cybersecurity and strive to improve our expertise continuously. For this reason, we highly recommend dedicating time to learning about configuring important security measures such as `SELinux`, `AppArmor`, and `TCP wrappers` on your own. By taking on this (optional but highly efficient) challenge, you'll deepen your understanding of these technologies, build up your problem-solving skills, and gain valuable experience that will serve you well in the future. We highly recommend to use a personal VM and make snapshots before making changes.

When it comes to implementing cybersecurity measures, there is no one-size-fits-all approach. It is important to consider the specific information you want to protect and the tools you will use to do so. However, you can practice and implement several optional tasks with others in the Discord channel to increase your knowledge and skills in this area. By taking advantage of the helpfulness of others and sharing your own expertise, you can deepen your understanding of cybersecurity and help others do the same. Remember, explaining concepts to others is essential to teaching and learning.

#### SELinux
1.	Install SELinux on your VM.
2.	Configure SELinux to prevent a user from accessing a specific file.
3.	Configure SELinux to allow a single user to access a specific network service but deny access to all others.
4.	Configure SELinux to deny access to a specific user or group for a specific network service.

##### AppArmor
5.	Configure AppArmor to prevent a user from accessing a specific file.
6.	Configure AppArmor to allow a single user to access a specific network service but deny access to all others.
7.	Configure AppArmor to deny access to a specific user or group for a specific network service.

##### TCP Wrappers
8.	Configure TCP wrappers to allow access to a specific network service from a specific IP address.
9.	Configure TCP wrappers to deny access to a specific network service from a specific IP address.
10.	Configure TCP wrappers to allow access to a specific network service from a range of IP addresses.



## Remote Desktop Protocols in Linux – Key Points for Beginners

#### 1. Purpose of Remote Desktop Protocols

- Allow graphical access to remote systems.
- Used for system management, software installation, and troubleshooting.
- Enable interaction with remote desktops as if you are physically present.

#### 2. Common Protocols
#### Remote Desktop Protocol (RDP)
- Developed by Microsoft.
- Mostly used in **Windows environments**.
- Allows full desktop access over the network.

#### Virtual Network Computing (VNC)

- Cross-platform, popular in **Linux**.
- Based on the RFB (Remote Frame Buffer) protocol.    
- Offers desktop sharing and control.

#### 3. X Server (X11 / X Window System)
- Handles GUI rendering in Unix/Linux.
- Provides **network transparency** (apps run on one machine, displayed on another).
- Uses **ports 6000–6009**.
- **Not secure** by default (no encryption).
- Can be secured using **SSH tunneling (X11 forwarding)**.

**Example:**
```Bash
ssh -X user@target_ip /usr/bin/firefox
```

To enable X11 forwarding, set in `/etc/ssh/sshd_config`:
```nginix
X11Forwarding yes
```


#### 4. X11 Security Concerns

- Sends data **unencrypted** (can leak sensitive info).
- Use tools like `xwd`, `xgrabsc` to capture screen data.
- Known vulnerabilities (e.g., **CVE-2017-2624** etc.).

#### 5. XDMCP (X Display Manager Control Protocol)

- Communicates via **UDP port 177**.    
- Redirects full GUIs like GNOME or KDE.
- **Insecure** – vulnerable to **Man-in-the-Middle attacks**.
- Use with caution, avoid in high-security environments.


#### 6. VNC in Detail
 Features
- Allows remote GUI sessions.
- Cross-platform tools available (e.g., RealVNC, UltraVNC).
- Default port: **TCP 5900** (`:0`), then 5901, 5902 for more displays.

##### VNC Types
- **Shared Desktop**: Real-time screen control.
- **Virtual Session**: Separate GUI session per user.

 Installation (TigerVNC + XFCE)
```Bash
sudo apt install xfce4 xfce4-goodies tigervnc-standalone-server -y vncpasswd
```

#### Configuration

Create files:

```bash
touch ~/.vnc/xstartup ~/.vnc/config 
```

`~/.vnc/xstartup`:

```Bash
#!/bin/bash
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
/usr/bin/startxfce4
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
x-window-manager &
```

Make it executable:
```Bash
chmod +x ~/.vnc/xstartup
```

`~/.vnc/config`:

```ini
geometry=1920x1080 dpi=96
```


### 7. Start and Connect to VNC

#### Start VNC server
```Bash
vncserver
```

#### List Sessions
```Bash
vncserver -list
```

#### Secure with SSH Tunnel
```bash
ssh -L 5901:127.0.0.1:5901 -N -f -l user 10.129.x.x
```

### **Connect via VNC Viewer**

```bash
xtightvncviewer localhost:5901
```

### Summary

- Use **RDP for Windows**, **VNC/X11 for Linux**.    
- **X11** is fast but insecure unless tunneled over SSH.
- **VNC** is more user-friendly and can be encrypted.
- Avoid **XDMCP** unless in trusted environments.

## Linux Security

All computer systems have an inherent risk of intrusion. Some present more of a risk than others, such as an internet-facing web server hosting multiple complex web applications. Linux systems are also less prone to viruses that affect Windows operating systems and do not present as large an attack surface as Active Directory domain-joined hosts. Regardless, it is essential to have certain fundamentals in place to secure any Linux system.

One of the Linux operating systems' most important security measures is keeping the OS and installed packages up to date. This can be achieved with a command such as:

```shell-session
0xWAYNE@htb[/htb]$ apt update && apt dist-upgrade
```

If firewall rules are not appropriately set at the network level, we can use the Linux firewall and/or `iptables` to restrict traffic into/out of the host.

If SSH is open on the server, the configuration should be set up to disallow password login and disallow the root user from logging in via SSH. It is also important to avoid logging into and administering the system as the root user whenever possible and adequately managing access control. Users' access should be determined based on the principle of least privilege. For example, if a user needs to run a command as root, then that command should be specified in the `sudoers` configuration instead of giving them full sudo rights. Another common protection mechanism that can be used is `fail2ban`. This tool counts the number of failed login attempts, and if a user has reached the maximum number, the host that tried to connect will be handled as configured.

It is also important to periodically audit the system to ensure that issues do not exist that could facilitate privilege escalation, such as an out-of-date kernel, user permission issues, world-writable files, and misconfigured cron jobs, or misconfigured services. Many administrators forget about the possibility that some kernel versions have to be updated manually.

An option for further locking down Linux systems is `Security-Enhanced Linux` (`SELinux`) or `AppArmor`. This is a kernel security module that can be used for security access control policies. In SELinux, every process, file, directory, and system object is given a label. Policy rules are created to control access between these labeled processes and objects and are enforced by the kernel. This means that access can be set up to control which users and applications can access which resources. SELinux provides very granular access controls, such as specifying who can append to a file or move it.

This list is incomplete, as safety is not a product but a process. This means that specific steps must always be taken to protect the systems better, and it depends on the administrators how well they know their operating systems. The better the administrators are familiar with the system, and the more they are trained, the better and more secure their security precautions and security measures will be.

This list is incomplete, as safety is not a product but a process. This means that specific steps must always be taken to protect the systems better, and it depends on the administrators how well they know their operating systems. The better the administrators are familiar with the system, and the more they are trained, the better and more secure their security precautions and security measures will be.

### TCP Wrappers
TCP wrapper is a security mechanism used in Linux systems that allows the system administrator to control which services are allowed access to the system. It works by restricting access to certain services based on the hostname or IP address of the user requesting access. When a client attempts to connect to a service the system will first consult the rules defined in the TCP wrappers configuration files to determine the IP address of the client. If the IP address matches the criteria specified in the configuration files, the system will then grant the client access to the service. However, if the criteria are not met, the connection will be denied, providing an additional layer of security for the service. TCP wrappers use the following configuration files:

- `/etc/hosts.allow`
- `/etc/hosts.deny`


In short, the `/etc/hosts.allow` file specifies which services and hosts are allowed access to the system, whereas the `/etc/hosts.deny` file specifies which services and hosts are not allowed access. These files can be configured by adding specific rules to the files.

#### /etc/hosts.allow
```shell-session
0xWAYNE@htb[/htb]$ cat /etc/hosts.allow

# Allow access to SSH from the local network
sshd : 10.129.14.0/24

# Allow access to FTP from a specific host
ftpd : 10.129.14.10

# Allow access to Telnet from any host in the inlanefreight.local domain
telnetd : .inlanefreight.local
```

#### /etc/hosts.deny
```shell-session
0xWAYNE@htb[/htb]$ cat /etc/hosts.deny

# Deny access to all services from any host in the inlanefreight.com domain
ALL : .inlanefreight.com

# Deny access to SSH from a specific host
sshd : 10.129.22.22

# Deny access to FTP from hosts with IP addresses in the range of 10.129.22.0 to 10.129.22.255
ftpd : 10.129.22.0/24
```

It is important to remember that the order of the rules in the files is important. The first rule that matches the requested service and host is the one that will be applied. It is also important to note that TCP wrappers are not a replacement for a firewall, as they are limited by the fact that they can only control access to services and not to ports.


## Firewall Setup

### Linux Firewalls and Netfilter: Overview and History
1. Purpose of Firewalls

- **Primary Function**: Control and monitor **network traffic** between different segments (e.g., internal ↔ external, DMZ ↔ LAN).
    
- **Security Objectives**:
    - Prevent **unauthorized access**
    - Block **malicious traffic**
    - Ensure **confidentiality**, **integrity**, and **availability** of networked systems


2. Linux and Firewall Capabilities

- Linux includes **built-in firewall functionality** via:
    - **Netfilter** (kernel-level packet filtering framework)
    - **iptables** (user-space utility for rule management)

- Firewall capabilities include:
    
    - Packet filtering by **IP, port, protocol**
    - Stateful inspection and NAT (Network Address Translation)
    - Rule chains for **INPUT, OUTPUT, FORWARD**, etc.

3. Historical Evolution
Predecessors to iptables

|Tool|Description|
|---|---|
|`ipfwadm`|Used in Linux 2.0 kernel|
|`ipchains`|Replaced ipfwadm (Linux 2.2 kernel)|
|`iptables`|Introduced in Linux 2.4 (2000)|

iptables Overview:

- Became **standard firewall tool** in Linux systems.
- Uses command-line interface for flexible, granular rule creation.
- Supported **stateless** and **stateful** packet filtering.

4. Netfilter Framework

- **Kernel subsystem** for packet filtering, NAT, and connection tracking.
- Exposes **hooks** within the network stack:
    - Allows modules like `iptables`, `nftables` to intercept packets.

- Operates at various points of the packet lifecycle:
    - **PREROUTING**, **INPUT**, **FORWARD**, **OUTPUT**, **POSTROUTING**

5. Typical Use Cases for iptables

- **Block or allow traffic** based on:
    - IP address (source/destination)
    - Protocol (TCP, UDP, ICMP)
    - Port number

- **Defend against threats** such as:
    - Denial-of-Service (DoS) attacks
    - Port scans
    - Unauthorized remote access

Example: Basic iptables Rule

```Bash
# Block all incoming traffic on port 22 (SSH) iptables -A INPUT -p tcp --dport 22 -j DROP
```

## **Modern Alternatives**

- **nftables** (introduced in Linux 3.13) is the successor to `iptables`.
    - Simplifies rule syntax
    - Uses unified framework for IPv4, IPv6, NAT, and more


## Iptables

The iptables utility provides a flexible set of rules for filtering network traffic based on various criteria such as source and destination IP addresses, port numbers, protocols, and more. There also exist other solutions like nftables, ufw, and firewalld. `Nftables` provides a more modern syntax and improved performance over iptables. However, the syntax of nftables rules is not compatible with iptables, so migration to nftables requires some effort. `UFW` stands for “Uncomplicated Firewall” and provides a simple and user-friendly interface for configuring firewall rules. UFW is built on top of the iptables framework like nftables and provides an easier way to manage firewall rules. Finally, FirewallD provides a dynamic and flexible firewall solution that can be used to manage complex firewall configurations, and it supports a rich set of rules for filtering network traffic and can be used to create custom firewall zones and services. It consists of several components that work together to provide a flexible and powerful firewall solution. The main components of iptables are:

|**Component**|**Description**|
|---|---|
|`Tables`|Tables are used to organize and categorize firewall rules.|
|`Chains`|Chains are used to group a set of firewall rules applied to a specific type of network traffic.|
|`Rules`|Rules define the criteria for filtering network traffic and the actions to take for packets that match the criteria.|
|`Matches`|Matches are used to match specific criteria for filtering network traffic, such as source or destination IP addresses, ports, protocols, and more.|
|`Targets`|Targets specify the action for packets that match a specific rule. For example, targets can be used to accept, drop, or reject packets or modify the packets in another way.|
#### Tables

When working with firewalls on Linux systems, it is important to understand how tables work in iptables. Tables in iptables are used to categorize and organize firewall rules based on the type of traffic that they are designed to handle. These tables are used to organize and categorize firewall rules. Each table is responsible for performing a specific set of tasks.

|**Table Name**|**Description**|**Built-in Chains**|
|---|---|---|
|`filter`|Used to filter network traffic based on IP addresses, ports, and protocols.|INPUT, OUTPUT, FORWARD|
|`nat`|Used to modify the source or destination IP addresses of network packets.|PREROUTING, POSTROUTING|
|`mangle`|Used to modify the header fields of network packets.|PREROUTING, OUTPUT, INPUT, FORWARD, POSTROUTING|

In addition to the built-in tables, iptables provides a fourth table called the raw table, which is used to configure special packet processing options. The raw table contains two built-in chains: PREROUTING and OUTPUT.

#### Chains
In iptables, chains organize rules that define how network traffic should be filtered or modified. There are two types of chains in iptables:

- Built-in chains
- User-defined chains

The built-in chains are pre-defined and automatically created when a table is created. Each table has a different set of built-in chains. For example, the filter table has three built-in chains:

- INPUT
- OUTPUT
- FORWARD

These chains are used to filter incoming and outgoing network traffic, as well as traffic that is being forwarded between different network interfaces. The nat table has two built-in chains:

- PREROUTING
- POSTROUTING

The PREROUTING chain is used to modify the destination IP address of incoming packets before the routing table processes them. The POSTROUTING chain is used to modify the source IP address of outgoing packets after the routing table has processed them. The mangle table has five built-in chains:

- PREROUTING
- OUTPUT
- INPUT
- FORWARD
- POSTROUTING

These chains are used to modify the header fields of incoming and outgoing packets and packets being processed by the corresponding chains.

`User-defined chains` can simplify rule management by grouping firewall rules based on specific criteria, such as source IP address, destination port, or protocol. They can be added to any of the three main tables. For example, if an organization has multiple web servers that all require similar firewall rules, the rules for each server could be grouped in a user-defined chain. Another example is when a user-defined chain could filter traffic destined for a specific port, such as port 80 (HTTP). The user could then add rules to this chain that specifically filter traffic destined for port 80.

#### Rules and Targets

Iptables rules are used to define the criteria for filtering network traffic and the actions to take for packets that match the criteria. Rules are added to chains using the `-A` option followed by the chain name, and they can be modified or deleted using various other options.

Each rule consists of a set of criteria or matches and a target specifying the action for packets that match the criteria. The criteria or matches match specific fields in the IP header, such as the source or destination IP address, protocol, source, destination port number, and more. The target specifies the action for packets that match the criteria. They specify the action to take for packets that match a specific rule. For example, targets can accept, drop, reject, or modify the packets. Some of the common targets used in iptables rules include the following:

|**Target Name**|**Description**|
|---|---|
|`ACCEPT`|Allows the packet to pass through the firewall and continue to its destination|
|`DROP`|Drops the packet, effectively blocking it from passing through the firewall|
|`REJECT`|Drops the packet and sends an error message back to the source address, notifying them that the packet was blocked|
|`LOG`|Logs the packet information to the system log|
|`SNAT`|Modifies the source IP address of the packet, typically used for Network Address Translation (NAT) to translate private IP addresses to public IP addresses|
|`DNAT`|Modifies the destination IP address of the packet, typically used for NAT to forward traffic from one IP address to another|
|`MASQUERADE`|Similar to SNAT but used when the source IP address is not fixed, such as in a dynamic IP address scenario|
|`REDIRECT`|Redirects packets to another port or IP address|
|`MARK`|Adds or modifies the Netfilter mark value of the packet, which can be used for advanced routing or other purposes|

Let us illustrate a rule and consider that we want to add a new entry to the INPUT chain that allows incoming TCP traffic on port 22 (SSH) to be accepted. The command for that would look like the following:

  Firewall Setup

```shell-session
0xWAYNE@htb[/htb]$ sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

#### Matches

`Matches` are used to specify the criteria that determine whether a firewall rule should be applied to a particular packet or connection. Matches are used to match specific characteristics of network traffic, such as the source or destination IP address, protocol, port number, and more.

|**Match Name**|**Description**|
|---|---|
|`-p` or `--protocol`|Specifies the protocol to match (e.g. tcp, udp, icmp)|
|`--dport`|Specifies the destination port to match|
|`--sport`|Specifies the source port to match|
|`-s` or `--source`|Specifies the source IP address to match|
|`-d` or `--destination`|Specifies the destination IP address to match|
|`-m state`|Matches the state of a connection (e.g. NEW, ESTABLISHED, RELATED)|
|`-m multiport`|Matches multiple ports or port ranges|
|`-m tcp`|Matches TCP packets and includes additional TCP-specific options|
|`-m udp`|Matches UDP packets and includes additional UDP-specific options|
|`-m string`|Matches packets that contain a specific string|
|`-m limit`|Matches packets at a specified rate limit|
|`-m conntrack`|Matches packets based on their connection tracking information|
|`-m mark`|Matches packets based on their Netfilter mark value|
|`-m mac`|Matches packets based on their MAC address|
|`-m iprange`|Matches packets based on a range of IP addresses|

In general, matches are specified using the '-m' option in iptables. For example, the following command adds a rule to the 'INPUT' chain in the 'filter' table that matches incoming TCP traffic on port 80:

```shell-session
0xWAYNE@htb[/htb]$ sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
```

This example rule matches incoming TCP traffic (`-p tcp`) on port 80 (`--dport 80`) and jumps to the accept target (`-j ACCEPT`) if the match is successful.

### System Logs

System logs on Linux are a set of files that contain information about the system and the activities taking place on it. These logs are important for monitoring and troubleshooting the system, as they can provide insights into system behavior, application activity, and security events. These system logs can be a valuable source of information for identifying potential security weaknesses and vulnerabilities within a Linux system as well. By analyzing the logs on our target systems, we can gain insights into the system's behavior, network activity, and user activity and can use this information to identify any abnormal activity, such as unauthorized logins, attempted attacks, clear text credentials, or unusual file access, which could indicate a potential security breach.

We, as penetration testers, can also use system logs to monitor the effectiveness of our security testing activities. By reviewing the logs after performing security testing, we can determine if our activities triggered any security events, such as intrusion detection alerts or system warnings. This information can help us refine our testing strategies and improve the overall security of the system.

In order to ensure the security of a Linux system, it is important to configure system logs properly. This includes setting the appropriate log levels, configuring log rotation to prevent log files from becoming too large, and ensuring that the logs are stored securely and protected from unauthorized access. In addition, it is important to regularly review and analyze the logs to identify potential security risks and respond to any security events in a timely manner. There are several different types of system logs on Linux, including:

- Kernel Logs
- System Logs
- Authentication Logs
- Application Logs
- Security Logs

#### Kernel logs

These logs contain information about the system's kernel, including hardware drivers, system calls, and kernel events. They are stored in the `/var/log/kern.log` file. For example, kernel logs can reveal the presence of vulnerable or outdated drivers that could be targeted by attackers to gain access to the system. They can also provide insights into system crashes, resource limitations, and other events that could lead to a denial of service or other security issues. In addition, kernel logs can help us identify suspicious system calls or other activities that could indicate the presence of malware or other malicious software on the system. By monitoring the `/var/log/kern.log` file, we can detect any unusual behavior and take appropriate action to prevent further damage to the system.

#### System logs

These logs contain information about system-level events, such as service starts and stops, login attempts, and system reboots. They are stored in the `/var/log/syslog` file. By analyzing login attempts, service starts and stops, and other system-level events, we can detect any possible access or activities on the system. This can help us identify any vulnerabilities that could be exploited and help us recommend security measures to mitigate these risks. In addition, we can use the `syslog` to identify potential issues that could impact the availability or performance of the system, such as failed service starts or system reboots. Here is an example of how such `syslog` file could look like:

#### Syslog

  System Logs

```shell-session
Feb 28 2023 15:00:01 server CRON[2715]: (root) CMD (/usr/local/bin/backup.sh)
Feb 28 2023 15:04:22 server sshd[3010]: Failed password for htb-student from 10.14.15.2 port 50223 ssh2
Feb 28 2023 15:05:02 server kernel: [  138.303596] ata3.00: exception Emask 0x0 SAct 0x0 SErr 0x0 action 0x6 frozen
Feb 28 2023 15:06:43 server apache2[2904]: 127.0.0.1 - - [28/Feb/2023:15:06:43 +0000] "GET /index.html HTTP/1.1" 200 13484 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36"
Feb 28 2023 15:07:19 server sshd[3010]: Accepted password for htb-student from 10.14.15.2 port 50223 ssh2
Feb 28 2023 15:09:54 server kernel: [  367.543975] EXT4-fs (sda1): re-mounted. Opts: errors=remount-ro
Feb 28 2023 15:12:07 server systemd[1]: Started Clean PHP session files.
```

#### Authentication logs

These logs contain information about user authentication attempts, including successful and failed attempts. They are stored in the `/var/log/auth.log` file. It is important to note that while the `/var/log/syslog` file may contain similar login information, the `/var/log/auth.log` file specifically focuses on user authentication attempts, making it a more valuable resource for identifying potential security threats. Therefore, it is essential for penetration testers to review the logs stored in the `/var/log/auth.log` file to ensure that the system is secure and has not been compromised.

#### Auth.log

  System Logs

```shell-session
Feb 28 2023 18:15:01 sshd[5678]: Accepted publickey for admin from 10.14.15.2 port 43210 ssh2: RSA SHA256:+KjEzN2cVhIW/5uJpVX9n5OB5zVJ92FtCZxVzzcKjw
Feb 28 2023 18:15:03 sudo:   admin : TTY=pts/1 ; PWD=/home/admin ; USER=root ; COMMAND=/bin/bash
Feb 28 2023 18:15:05 sudo:   admin : TTY=pts/1 ; PWD=/home/admin ; USER=root ; COMMAND=/usr/bin/apt-get install netcat-traditional
Feb 28 2023 18:15:08 sshd[5678]: Disconnected from 10.14.15.2 port 43210 [preauth]
Feb 28 2023 18:15:12 kernel: [  778.941871] firewall: unexpected traffic allowed on port 22
Feb 28 2023 18:15:15 auditd[9876]: Audit daemon started successfully
Feb 28 2023 18:15:18 systemd-logind[1234]: New session 4321 of user admin.
Feb 28 2023 18:15:21 CRON[2345]: pam_unix(cron:session): session opened for user root by (uid=0)
Feb 28 2023 18:15:24 CRON[2345]: pam_unix(cron:session): session closed for user root
```

In this example, we can see in the first line that a successful public key has been used for authentication for the user `admin`. Additionally, we can see that this user is in the `sudoers` group because he can execute commands using `sudo`. The kernel message indicates that unexpected traffic was allowed on port 22, which could indicate a potential security breach. After that, we see that a new session was created for user "admin" by `systemd-logind` and that a `cron` session opened and closed for the user `root`.

#### Application logs

These logs contain information about the activities of specific applications running on the system. They are often stored in their own files, such as `/var/log/apache2/error.log` for the Apache web server or `/var/log/mysql/error.log` for the MySQL database server. These logs are particularly important when we are targeting specific applications, such as web servers or databases, as they can provide insights into how these applications are processing and handling data. By examining these logs, we can identify potential vulnerabilities or misconfigurations. For example, access logs can be used to track requests made to a web server, while audit logs can be used to track changes made to the system or to specific files. These logs can be used to identify unauthorized access attempts, data exfiltration, or other suspicious activity.

Besides, access and audit logs are critical logs that record information about the actions of users and processes on the system. They are crucial for security and compliance purposes, and we can use them to identify potential security issues and attack vectors.

For example, `access logs` keep a record of user and process activity on the system, including login attempts, file accesses, and network connections. `Audit logs` record information about security-relevant events on the system, such as modifications to system configuration files or attempts to modify system files or settings. These logs help track potential attacks and activities or identify security breaches or other issues. An example entry in an access log file can look like the following:

#### Access Log Entry

  System Logs

```shell-session
2023-03-07T10:15:23+00:00 servername privileged.sh: htb-student accessed /root/hidden/api-keys.txt
```

In this log entry, we can see that the user `htb-student` used the `privileged.sh` script to access the `api-keys.txt` file in the `/root/hidden/` directory. On Linux systems, most common services have default locations for access logs:

|**Service**|**Description**|
|---|---|
|`Apache`|Access logs are stored in the /var/log/apache2/access.log file (or similar, depending on the distribution).|
|`Nginx`|Access logs are stored in the /var/log/nginx/access.log file (or similar).|
|`OpenSSH`|Access logs are stored in the /var/log/auth.log file on Ubuntu and in /var/log/secure on CentOS/RHEL.|
|`MySQL`|Access logs are stored in the /var/log/mysql/mysql.log file.|
|`PostgreSQL`|Access logs are stored in the /var/log/postgresql/postgresql-version-main.log file.|
|`Systemd`|Access logs are stored in the /var/log/journal/ directory.|

#### Security logs

These security logs and their events are often recorded in a variety of log files, depending on the specific security application or tool in use. For example, the Fail2ban application records failed login attempts in the `/var/log/fail2ban.log` file, while the UFW firewall records activity in the `/var/log/ufw.log` file. Other security-related events, such as changes to system files or settings, may be recorded in more general system logs such as `/var/log/syslog` or `/var/log/auth.log`. As penetration testers, we can use log analysis tools and techniques to search for specific events or patterns of activity that may indicate a security issue and use that information to further test the system for vulnerabilities or potential attack vectors.

It is important to be familiar with the default locations for access logs and other log files on Linux systems, as this information can be useful when performing a security assessment or penetration test. By understanding how security-related events are recorded and stored, we can more effectively analyze log data and identify potential security issues.

All these logs can be accessed and analyzed using a variety of tools, including the log file viewers built into most Linux desktop environments, as well as command-line tools such as the `tail`, `grep`, and `sed` commands. Proper analysis of system logs can help identify and troubleshoot system issues, as well as detect security breaches and other events of interest.


----
# Solaris

---

Solaris is a Unix-based operating system developed by Sun Microsystems (later acquired by Oracle Corporation) in the 1990s. It is known for its robustness, scalability, and support for high-end hardware and software systems. Solaris is widely used in enterprise environments for mission-critical applications, such as database management, cloud computing, and virtualization. For example, it includes a built-in hypervisor called `Oracle VM Server for SPARC`, which allows multiple virtual machines to run on a single physical server. Overall, it is designed to handle large amounts of data and provide reliable and secure services to users and is often used in enterprise environments where security, performance, and stability are key requirements.

The goal of Solaris is to provide a highly stable, secure, and scalable platform for enterprise computing. It has built-in features for high availability, fault tolerance, and system management, making it ideal for mission-critical applications. It is widely used in the banking, finance, and government sectors, where security, reliability, and performance are paramount. It is also used in large-scale data centers, cloud computing environments, and virtualization platforms. Companies such as Amazon, IBM, and Dell use Solaris in their products and services, highlighting its importance in the industry.

---

## Linux Distributions vs Solaris

Solaris and Linux distributions are two types of operating systems that differ significantly. Firstly, Solaris is a proprietary operating system owned and developed by Oracle Corporation, and its source code is not available to the general public. In contrast, most Linux distributions are open-source, meaning that their source code is available for anyone to modify and use. Additionally, Linux distributions commonly use the Zettabyte File System (`ZFS`), which is a highly advanced file system that offers features such as data compression, snapshots, and high scalability. On the other hand, Solaris uses a Service Management Facility (`SMF`), which is a highly advanced service management framework that provides better reliability and availability for system services.

|**Directory**|**Description**|
|---|---|
|`/`|The root directory contains all other directories and files in the file system.|
|`/bin`|It contains essential system binaries that are required for booting and basic system operations.|
|`/boot`|The boot directory contains boot-related files such as boot loader and kernel images.|
|`/dev`|The dev directory contains device files that represent physical and logical devices attached to the system.|
|`/etc`|The etc directory contains system configuration files, such as system startup scripts and user authentication data.|
|`/home`|Users’ home directories.|
|`/kernel`|This directory contains kernel modules and other kernel-related files.|
|`/lib`|Directory for libraries required by the binaries in /bin and /sbin directories.|
|`/lost+found`|This directory is used by the file system consistency check and repair tool to store recovered files.|
|`/mnt`|Directory for mounting file systems temporarily.|
|`/opt`|This directory contains optional software packages that are installed on the system.|
|`/proc`|The proc directory provides a view into the system's process and kernel status as files.|
|`/sbin`|This directory contains system binaries required for system administration tasks.|
|`/tmp`|Temporary files created by the system and applications are stored in this directory.|
|`/usr`|The usr directory contains system-wide read-only data and programs, such as documentation, libraries, and executables.|
|`/var`|This directory contains variable data files, such as system logs, mail spools, and printer spools.|

Solaris has a number of unique features that set it apart from other operating systems. One of its key strengths is its support for high-end hardware and software systems. It is designed to work with large-scale data centers and complex network infrastructures, and it can handle large amounts of data without any performance issues.

In terms of package management, Solaris uses the Image Packaging System (`IPS`) package manager, which provides a powerful and flexible way to manage packages and updates. Solaris also provides advanced security features, such as Role-Based Access Control (`RBAC`) and mandatory access controls, which are not available in all Linux distributions.

---

## Differences

Let's dive deeper into the differences between Solaris and Linux distributions. One of the most important differences is that the source code is not open source and is only known in closed circles. This means that unlike Ubuntu or many other distributions, the source code cannot be viewed and analyzed by the public. In summary, the main differences can be grouped into the following categories:

- Filesystem
- Process management
- Package management
- Kernel and Hardware support
- System monitoring
- Security

To better understand the differences, let's take a look at a few examples and commands.

#### System Information

On Ubuntu, we use the `uname` command to display information about the system, such as the kernel name, hostname, and operating system. This might look like this:

  Solaris

```shell-session
0xWAYNE@htb[/htb]$ uname -a

Linux ubuntu 5.4.0-1045 #48-Ubuntu SMP Fri Jan 15 10:47:29 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
```

On the other hand, in Solaris, the `showrev` command can be used to display system information, including the version of Solaris, hardware type, and patch level. Here is an example output:

  Solaris

```shell-session
$ showrev -a

Hostname: solaris
Kernel architecture: sun4u
OS version: Solaris 10 8/07 s10s_u4wos_12b SPARC
Application architecture: sparc
Hardware provider: Sun_Microsystems
Domain: sun.com
Kernel version: SunOS 5.10 Generic_139555-08
```

The main difference between the two commands is that `showrev` provides more detailed information about the Solaris system, such as the patch level and hardware provider, while `uname` only provides basic information about the Linux system.

#### Installing Packages

On Ubuntu, the `apt-get` command is used to install packages. This could look like the following:

  Solaris

```shell-session
0xWAYNE@htb[/htb]$ sudo apt-get install apache2
```

However, in Solaris, we need to use `pkgadd` to install packages like `SUNWapchr`.

  Solaris

```shell-session
$ pkgadd -d SUNWapchr
```

The main difference between the two commands is the syntax, and the package manager used. Ubuntu uses the Advanced Packaging Tool (APT) to manage packages, while Solaris uses the Solaris Package Manager (SPM). Also, note that we do not use `sudo` in this case. This is because Solaris used the `RBAC` privilege management tool, which allowed the assignment of granular permissions to users. However, `sudo` has been supported since Solaris 11.

#### Permission Management

On Linux systems like Ubuntu but also on Solaris, the `chmod` command is used to change the permissions of files and directories. Here is an example command to give read, write, and execute permissions to the owner of the file:

  Solaris

```shell-session
0xWAYNE@htb[/htb]$ chmod 700 filename
```

To find files with specific permissions in Ubuntu, we use the `find` command. Let us take a look at an example of a file with the SUID bit set:

  Solaris

```shell-session
0xWAYNE@htb[/htb]$ find / -perm 4000
```

To find files with specific permissions, like with the SUID bit set on Solaris, we can use the find command, too, but with a small adjustment.

  Solaris

```shell-session
$ find / -perm -4000
```

The main difference between these two commands is the use of the `-` before the permission value in the Solaris command. This is because Solaris uses a different permission system than Linux.

#### NFS in Solaris

Solaris has its own implementation of NFS, which is slightly different from Linux distributions like Ubuntu. In Solaris, the NFS server can be configured using the `share` command, which is used to share a directory over the network, and it also allows us to specify various options such as read/write permissions, access restrictions, and more. To share a directory over NFS in Solaris, we can use the following command:

  Solaris

```shell-session
$ share -F nfs -o rw /export/home
```

This command shares the `/export/home` directory with read and writes permissions over NFS. An NFS client can mount the NFS file system using the `mount` command, the same way as with Ubuntu. To mount an NFS file system in Solaris, we need to specify the server name and the path to the shared directory. For example, to mount an NFS share from a server with the IP address `10.129.15.122` and the shared directory `/nfs_share`, we use the following command:

  Solaris

```shell-session
0xWAYNE@htb[/htb]$ mount -F nfs 10.129.15.122:/nfs_share /mnt/local
```

In Solaris, the configuration for NFS is stored in the `/etc/dfs/dfstab` file. This file contains entries for each shared directory, along with the various options for NFS sharing.

  Solaris

```shell-session
# cat /etc/dfs/dfstab

share -F nfs -o rw /export/home
```

#### Process Mapping

Process mapping is an essential aspect of system administration and troubleshooting. The `lsof` command is a powerful utility that lists all the files opened by a process, including network sockets and other file descriptors that we can use in Debian distributions like Ubuntu. We can use `lsof` to list all the files opened by a process. For example, to list all the files opened by the Apache web server process, we can use the following command:

  Solaris

```shell-session
0xWAYNE@htb[/htb]$ sudo lsof -c apache2
```

In Solaris, the `pfiles` command can be used to list all the files opened by a process. For example, to list all the files opened by the Apache web server process, we can use the following command:

  Solaris

```shell-session
$ pfiles `pgrep httpd`
```

This command lists all the files opened by the Apache web server process. The output of the `pfiles` command is similar to the output of the `lsof` command and provides information about the type of file descriptor, the file descriptor number, and the file name.

#### Executable Access

In Solaris, `truss` is used, which is a highly useful utility for developers and system administrators who need to debug complex software issues on the Solaris operating system. By tracing the system calls made by a process, `truss` can help identify the source of errors, performance issues, and other problems but can also reveal some sensitive information that may arise during application development or system maintenance. The utility can also provide detailed information about system calls, including the arguments passed to them and their return values, allowing users to better understand the behavior of their applications and the underlying operating system.

`Strace` is an alternative to `truss` but for Ubuntu, and it is an essential tool for system administrators and developers alike, helping them diagnose and troubleshoot issues in real-time. It enables users to analyze the interactions between the operating system and applications running on it, which is especially useful in highly complex and mission-critical environments. With `truss`, users can quickly identify and isolate issues related to application performance, network connectivity, and system resource utilization, among others.

For example, to trace the system calls made by the Apache web server process, we can use the following command:

  Solaris

```shell-session
0xWAYNE@htb[/htb]$ sudo strace -p `pgrep apache2`
```

Here's an example of how to use `truss` to trace the system calls made by the `ls` command in Solaris:

  Solaris

```shell-session
$ truss ls

execve("/usr/bin/ls", 0xFFBFFDC4, 0xFFBFFDC8)  argc = 1
...SNIP...
```

The output is similar to `strace`, but the format is slightly different. One difference between `strace` and `truss` is that `truss` can also trace the signals sent to a process, while `strace` cannot. Another difference is that `truss` has the ability to trace the system calls made by child processes, while `strace` can only trace the system calls made by the process specified on the command line.


# Shortcuts

---

There are many shortcuts that we can use to make working with Linux easier and faster. After we have familiarized ourselves with the most important of them and have made them a habit, we will save ourselves much typing. Some of them will even help us to avoid using our mouse in the terminal.

---

#### Auto-Complete

`[TAB]` - Initiates auto-complete. This will suggest to us different options based on the `STDIN` we provide. These can be specific suggestions like directories in our current working environment, commands starting with the same number of characters we already typed, or options.

---

#### Cursor Movement

`[CTRL] + A` - Move the cursor to the `beginning` of the current line.

`[CTRL] + E` - Move the cursor to the `end` of the current line.

`[CTRL] + [←]` / `[→]` - Jump at the beginning of the current/previous word.

`[ALT] + B` / `F` - Jump backward/forward one word.

---

#### Erase The Current Line

`[CTRL] + U` - Erase everything from the current position of the cursor to the `beginning` of the line.

`[Ctrl] + K` - Erase everything from the current position of the cursor to the `end` of the line.

`[Ctrl] + W` - Erase the word preceding the cursor position.

---

#### Paste Erased Contents

`[Ctrl] + Y` - Pastes the erased text or word.

---

#### Ends Task

`[CTRL] + C` - Ends the current task/process by sending the `SIGINT` signal. For example, this can be a scan that is running by a tool. If we are watching the scan, we can stop it / kill this process by using this shortcut. While not configured and developed by the tool we are using. The process will be killed without asking us for confirmation.

---

#### End-of-File (EOF)

`[CTRL] + D` - Close `STDIN` pipe that is also known as End-of-File (EOF) or End-of-Transmission.

---

#### Clear Terminal

`[CTRL] + L` - Clears the terminal. An alternative to this shortcut is the `clear` command you can type to clear our terminal.

---

#### Background a Process

`[CTRL] + Z` - Suspend the current process by sending the `SIGTSTP` signal.

---

#### Search Through Command History

`[CTRL] + R` - Search through command history for commands we typed previously that match our search patterns.

`[↑]` / `[↓]` - Go to the previous/next command in the command history.

---

#### Switch Between Applications

`[ALT] + [TAB]` - Switch between opened applications.

---

#### Zoom

`[CTRL] + [+]` - Zoom in.

`[CTRL] + [-]` - Zoom out.