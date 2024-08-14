
 Hi John Doe, CyberCorp's proprietary systems have been under attack by malicious hackers attempting to infiltrate their sensitive directories. Your task is to investigate and secure CyberCorp's LDAP directory, where crucial user information and flags are stored. Base DN: dc=cybercorp,dc=com Admin Name: cn=admin,dc=cybercorp,dc=com 


nc 0.cloud.chals.io 25915

install LDAP utils

then connect to the service as following

`ldapsearch -H ldap://0.cloud.chals.io:25915 -x -D "cn=admin,dc=cybercorp,dc=com" -W -b "dc=cybercorp,dc=com"`

You will be prompted for password brute force it

![[Pasted image 20240810101921.png]]

The flag is `KPMG_CTF{OVIwb94kbl1SVaWb5hc_34BTB0YoSprPYM1RWgAnwwUHGDu3dy1QSh1lz7Z7Nih-S3ZVkjm5iC5Eoy9oqKCgq34XiGZ_EAUCoGPswk9n} `





