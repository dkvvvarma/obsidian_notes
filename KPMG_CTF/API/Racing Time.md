
Lets see if you can overcome my lightning speed! Lets bet some **amount** should be more than **0**??

[https://kicyber2024-db574ea106a6-api2-1.chals.io](https://kicyber2024-db574ea106a6-api2-1.chals.io/)

![[Pasted image 20240811032310.png]]

![[Pasted image 20240811032325.png]]


![[Pasted image 20240811032406.png]]


![[Pasted image 20240811032419.png]]

Alter balance to deposit

![[Pasted image 20240811032503.png]]

![[Pasted image 20240811032517.png]]

now alter deposit to `withdraw`

![[Pasted image 20240811032601.png]]

![[Pasted image 20240811032614.png]]


now alter withdraw to `flag` endpoint

![[Pasted image 20240811032652.png]]


![[Pasted image 20240811032701.png]]


head back and send withdraw, balance, flag to repeater

![[Pasted image 20240811032832.png]]
I sent the balance request and I got  a value of `1000`

Now send withdraw

![[Pasted image 20240811032921.png]]

In response of allowed option section we can see only post

So alter the get to POST and then

```bash

Content-Type: application/json


{

	"amount": 800

}
```

![[Pasted image 20240811033320.png]]


![[Pasted image 20240811040517.png]]

![[Pasted image 20240811040708.png]]


![[Pasted image 20240811040641.png]]

Resend the flag now and you can get flag

![[Pasted image 20240811040745.png]]

The flag is 
```bash
KPMG_CTF{NxFKJOeKfCB2jq3RqKmwOzG5A_ndA6J-pBZhLrXHIyw0Ufn20I4yYymCbxK0cPnJNxYVmrz_5cusz0FG62ip-sk_6h1Y4Ey4K4DMpU4R}
```
