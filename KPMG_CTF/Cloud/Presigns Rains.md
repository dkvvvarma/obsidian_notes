


It rains Presigns. Try to use some buckets to catch it!

[http://kicyber2024-264a42c58f60-presign-1.chals.io](http://kicyber2024-264a42c58f60-presign-1.chals.io/)

![[Pasted image 20240810200928.png]]

Tried highlighting all / or you try viewing source code

![[Pasted image 20240810201019.png]]

Bucket name: `ctf2k24-best`
AWS access key: `AKIA33VJAWOZJLLBCU2A`


Then checked robots.txt after some time
![[Pasted image 20240810201131.png]]

Region: `us-east-1`
Date: `20240808T094405Z`
Expiry time: `604800`
Signature: `5625d8f847a29410e05b91df5628d6d2fa8146eed792c0ae048279798853d1b9`


greeted with error for all except `/robot`

![[Pasted image 20240810201206.png]]

`https://<bucket_name>.s3.us-east-1.amazonaws.com/flag.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=<aws_access_key>%2F20240808%2F<region>%2Fs3%2Faws4_request&X-Amz-Date=<date>&X-Amz-Expires=<expiry_time>&X-Amz-SignedHeaders=host&X-Amz-Signature=<signature>`

Appending the above parameters into the URL we get a presigned URL as follwoing


```URL

https://ctf2k24-best.s3.us-east-1.amazonaws.com/flag.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIA33VJAWOZJLLBCU2A%2F20240808%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240808T094405Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=5625d8f847a29410e05b91df5628d6d2fa8146eed792c0ae048279798853d1b9
```

![[Pasted image 20240810202104.png]]

Navigating there I found another directory  `/1tr41n5pr3_s1gn3d_UrL5_he73_4nd_T4kE3_y0uR_F1a9`

Appending this to main URL I got the Flag

![[Pasted image 20240810202228.png]]

```
KPMG_CTF{k6lMqSe8LKfz1Sb9Wioh6Mz50-QluoCk2YUC0E4rPqnmpPxM0rb5bOI_QtBKV72jviJJivqU8OiCm-PSQV9aJ3xrVly-KGSdmw7JCodb}
```

