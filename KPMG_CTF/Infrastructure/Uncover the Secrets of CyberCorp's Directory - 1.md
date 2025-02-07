
 Hi John Doe, CyberCorp's proprietary systems have been under attack by malicious hackers attempting to infiltrate their sensitive directories. Your task is to investigate and secure CyberCorp's LDAP directory, where crucial user information and flags are stored. Base DN: dc=cybercorp,dc=com Admin Name: cn=admin,dc=cybercorp,dc=com 


nc 0.cloud.chals.io 25915

install LDAP utils

then connect to the service as following

` ldapsearch -H ldap://0.cloud.chals.io:25915 -x -D "cn=admin,dc=cybercorp,dc=com" -W -b "dc=cybercorp,dc=com"`

### Breakdown:

1. **`ldapsearch`**:
    
    - This is the tool used to search and query LDAP directories. It retrieves information from an LDAP server.
2. **`-H ldap://0.cloud.chals.io:25915`**:
    
    - `-H` specifies the URI of the LDAP server to connect to. In this case, it's `ldap://0.cloud.chals.io`, with port `25915`.
    - This indicates that the LDAP server is running on a remote host (`0.cloud.chals.io`) and is listening on port `25915`.
3. **`-x`**:
    
    - This specifies the use of simple authentication (rather than SASL). Simple authentication is commonly used when binding with a username and password.
4. **`-D "cn=admin,dc=cybercorp,dc=com"`**:
    
    - `-D` specifies the distinguished name (DN) to bind as, which is the LDAP equivalent of a username. In this case, the DN is `cn=admin,dc=cybercorp,dc=com`.
    - `cn=admin` indicates the user or "common name" is `admin`.
    - `dc=cybercorp,dc=com` represents the domain components (DC), essentially indicating that this LDAP server is for the domain `cybercorp.com`.
5. **`-W`**:
    
    - This flag tells the command to prompt for the password interactively. When you run the command, it will ask for the password for `cn=admin`.
6. **`-b "dc=cybercorp,dc=com"`**:
    
    - `-b` specifies the base DN (Distinguished Name) for the search. This is where the search begins in the LDAP directory tree. In this case, the search starts at `dc=cybercorp,dc=com`.

You will be prompted for password brute force it

![[Pasted image 20240810101921.png]]

The flag is `KPMG_CTF{OVIwb94kbl1SVaWb5hc_34BTB0YoSprPYM1RWgAnwwUHGDu3dy1QSh1lz7Z7Nih-S3ZVkjm5iC5Eoy9oqKCgq34XiGZ_EAUCoGPswk9n} `





