
Today Let's solve the "[Baby Encryption](https://app.hackthebox.com/challenges/BabyEncryption)" challenge

**Challenge Description**

You are after an organised crime group which is responsible for the illegal weapon market in your country. As a secret agent, you have infiltrated the group enough to be included in meetings with clients. During the last negotiation, you found one of the confidential messages for the customer. It contains crucial information about the delivery. Do you think you can decrypt it?

Download the file and unzip the folder

It has only 2 files in it. 


A wireshark packet capture file and a python program.

```python
import string
from secret import MSG

def encryption(msg):
    ct = []
    for char in msg:
        ct.append((123 * char + 18) % 256)
    return bytes(ct)

ct = encryption(MSG)
f = open('./msg.enc','w')
f.write(ct.hex())
f.close()
```

The above is the content of python file.