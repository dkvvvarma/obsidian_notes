
I entered into portal and got greeted with a login page
![[Pasted image 20240809211329.png]]

Inspected the page and found login credentials as `guest:guest`.
![[Pasted image 20240809211416.png]]



Then found a jwt token in application > cookies.
`eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0Iiwicm9sZSI6Im5vcm1hbCJ9.Ng_08eaaqJBropLy8D-OubWsBMdk6Vu88UYdICswteg`

The decoded the jwt token in jwt.io then performed brute force to find secret key.

`hashcat -a 0 -m 16500 token.txt /usr/share/wordlists/rockyou.txt`

![[Pasted image 20240809211845.png]]

The secret key is `@thebesthacker_intheworld`

append this in jwt.io secret field and copy the new token and paste it in jwt token field 

Then the you will be given a new directory `0d5d1sad1510as0dsa2d0205d0asdas.txt`

Append this to URL remove `dashboard.php`
![[Pasted image 20240809212247.png]]

The flag is `KPMG_CTF{Pd8V1Ptg3_YGrVpxkbUf5wNah7igau4vFvWN9PJ9IgOhiqvmpksoZBZDoE7ApGCj9ZT19NksrkiZK5nW4JvIDz2gPtQau1NvB17HcUNE}`

