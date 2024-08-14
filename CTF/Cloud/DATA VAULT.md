
Description:

In a world where data is the new currency, only the best can claim the title of a master infiltrator. Today, you are tasked with a critical mission: to infiltrate a highly secure S3 bucket and retrieve a hidden flag.

`kpmg-ctf1`

So Install AWS CLI in your machine.
To install run the following command
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```


after installed look help instructions to better understand

run this cmd `aws s3 ls s3://kpmg-ctf1 --no-sign-request`
![[Pasted image 20240809154436.png]]

Now copy the files using cmd 
` aws s3 cp s3://kpmg-ctf1/rituognriteuonhbiorentgbvhuitrhoirtsnbiuort.txt ./ --no-sign-request`
![[Pasted image 20240809154507.png]]

The flag is `KPMG_CTF{cbf35723276f7cd30486971cd1027a79}`
