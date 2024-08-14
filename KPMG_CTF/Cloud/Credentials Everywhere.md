

Deployed the machine and went to target website and inspected the website

We can find 2 brainfuck encoded strings in elements

![[Pasted image 20240809213159.png]]

Decoding the first string, I got 

```brainfuck

++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>+++++++++++++..-----------.<++.>+++++++++++++.>+++++++++++++++.--------------.+++++++++++++.----.<++++++++++++.>-.<++++.<++++++++++++++++++++++++++.<++++++++++++++++++++++.>+++++++.++++++++++.--.--------.>---------------.<-------------.>----------------.---.++++++.---..+.<--.++++.>--.<-.-.>++++++.++++.------------. -->
```

I got the SSH username `AKIAV4FCIFFG26E54KOC`

Decoding the other string, We get

```brainfuck

++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>+++++++++++++..-----------.<++.>++++++++.>---.++++++++++++++++++..++++.--------.+++.--------------.<----------------------.<.>+++++++++++++++++++.>+++++++++++++++++++++.<+++++++++.+++++++++++++++.>-------.+++++++.<------------------.++++++++++++++++++.--.>----.---.<++.----------------------------------.>------.+++.++++++.-----------------.<<+++++++++++++++++.+.+.------------------. -->

```

I got SSH password `MyVerySecureCloud123!`

Now use the netcat command to connect to ubuntu shell
```shell
ssh AKIAV4FCIFFG26E54KOC@0.cloud.chals.io -p 29508
```
After connecting to the shell using following credentials



![[Pasted image 20240809220024.png]]


![[Pasted image 20240809220725.png]]

The final flag is `KPMG_CTF{303e5fd2a01c833789decdb5c4db622b}`


The flag in kpm_ctf-1

![[Pasted image 20240809221724.png]]


`KPMG_CTF{cbf35723276f7cd30486971cd1027a79}`

The flag in kpmg-2

