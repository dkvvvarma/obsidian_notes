

Downloaded the zip Opened it with winRaR

The name of the image resembled to previous osint challenge pastebin URL 
Checked It out and greeted with hex Values

![[Pasted image 20240810204534.png]]

``` hex
 49 6e 20 6f 72 64 65 72 20 74 6f 20 71 75 69 63 6b 6c 79 20 64 69 73 74 72 69 62 75 74 65 20 61 20 70 69 65 63 65 20 6f 66 20 63 6f 64 65 20 74 6f 20 73 65 6e 69 6f 72 73 20 66 6f 72 20 64 65 6d 6f 6e 73 74 72 61 74 69 6f 6e 2c 20 68 74 74 70 73 3a 2f 2f 69 6d 67 62 6f 78 2e 63 6f 6d 2f 47 59 42 37 65 43 6d 4b 20 66 6f 72 67 6f 74 20 74 6f 20 6d 61 6b 65 20 69 74 20 70 72 69 76 61 74 65 2e 0a 4d 61 79 62 65 20 66 69 6e 64 69 6e 67 20 68 69 6d 20 74 68 65 72 65 20 77 6f 72 6b 73 2e
 ```

Decoded them and got a message

```
In order to quickly distribute a piece of code to seniors for demonstration, https://imgbox.com/GYB7eCmK forgot to make it private.
Maybe finding him there works.
```

Went to the specified URL
![[Pasted image 20240810210135.png]]

In the pastebin it was mentioned it was headlined as "To Gist"

Searched for a user in Github and the profile pic matched, then searched in `gist.github.com`

Found a profile

![[Pasted image 20240810210627.png]]

I can see a code with a encoded value of base-64
`HR0cHM6Ly9tZWRpYS5naXBoeS5jb20vbWVkaWEvZDNNSlo4R0pTRm9mdEQ4SS9naXBoeS5naWY=`

![[Pasted image 20240810210724.png]]


navigated to the website `https://media.giphy.com/media/d3MJZ8GJSFoftD8I/giphy.gif`

![[Pasted image 20240810210808.png]]

then went back to his Gist profile found a javascript code below the encoded message

![[Pasted image 20240810210857.png]]

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Loud House GIF</title>
    <script>
       
        var encodedURL = "aHR0cHM6Ly9tZWRpYS5naXBoeS5jb20vbWVkaWEvZDNNSlo4R0pTRm9mdEQ4SS9naXBoeS5naWY=";
       
        var gifURL = atob(encodedURL);
       
        window.onload = function() {
            document.getElementById("loudHouseGif").src = gifURL;
        };
    </script>
</head>
<body>
    <img id="loudHouseGif" alt="The Loud House GIF">
    
</body>
</html>

```

```javascript
(function() {
    var a = String;
    var b = a.fromCharCode;
    var c = [
        55, 114, 117, 51,
        95, 119, 52, 114,
        114, 49, 48, 114
    ];
    var d = "";
    
    for (var i = 0; i < c.length; i++) {
        var e = c[i];
        var f = b(e);
        d += f;
    }
    
    var g = console;
    var h = g.log;
    h(d);
})();
```

the variable d is empty appended the base-64 encoded string in the code and compiled it

![[Pasted image 20240810211054.png]]

```base-64

aHR0cHM6Ly9tZWRpYS5naXBoeS5jb20vbWVkaWEvZDNNSlo4R0pTRm9mdEQ4SS9naXBoeS5naWY=7ru3_w4rr10r
```

Found a new appended value to the string `7ru3_w4rr10r`

This is the password for the `.zip` file, proceeded to unzip the file and got the flag.

![[Pasted image 20240810211243.png]]

The flag is 
```
KPMG_CTF{y0u_go7_34gl3_ey3s}
```

![[sWyANXnm_nibetsap.zip]]

