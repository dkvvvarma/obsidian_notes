

Similar to previous challenge

We have the username: `jdoe`
password: `KPMG_IS_TOO_COOL@FIRE_EMOJI`

We got these from part-1

Now we need forward the ssh internal port to local host

`ssh -L 8888:localhost:5000 jdoe@0.cloud.chals.io -p 16977`

![[Pasted image 20240811021755.png]]

Now headed over to the local host `localhost:8888`

![[Pasted image 20240811060122.png]]

Enter the same password as you did for `jdoe` in ssh

![[Pasted image 20240811021855.png]]

Then going to the next page

![[Pasted image 20240811021919.png]]

The command is `cat /root/root.txt`

![[Pasted image 20240811022149.png]]


The flag is
```
KPMG_CTF{RpT8Ua0u1uU70JjaXfrOPOBE6Vi4M2mCN64vGxQF7pK9qCzXq6FQ0iNZ_BGJHq3VVtVi4zQ_frMRqFu1LST70p-o3yeHcw8l4xK6JJwK} 
```


