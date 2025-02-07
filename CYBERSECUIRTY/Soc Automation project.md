**Flow**



![[Pasted image 20241116103123.png]]


1. Windows 10 client with Wazuh Agent sends event data to Wazuh manager through the router.

2. Wazuh manager receives the event logs from windows 10 client.

3. Wazuh manager sends the alerts to Shuffle.

4. On receiving the alerts, Shuffle performs OSINT in order to Enrich the indicators of compromise (IOCS).

5. After enriching the IOCS, Shuffle will send the alerts to The Hive.

6. Shuffle also sends an Email to the SOC Analyst regarding the alerts.

7.  The email that the analyst receives contains the details regarding the IOCS & Prompts the analyst to take action.

8. The SOC Analyst sends a responsive action to be taken to Shuffle, which in turn is forwarded to Wazuh manager.

9.  Wazuh manager performs the responsive action on the client system.


![[Pasted image 20241116103220.png]]

# Setting Up Client System - Win 10 (Sysmon)
  
We spin up a windows 10 virtual machine to install Sysmon on it.

![[Pasted image 20241116103257.png]]


  
Now we download Sysmon and Sysmon config file to install on the machine.
![[Pasted image 20241116103339.png]]

![[Pasted image 20241116103348.png]]

after downloading sysmon and the config file, extract the zip file.

open a powershell window with admin privileges and install sysmon with the following command

```powershell
.\sysmon64.exe
```

Then we add in the config file with the following command

```powershell
.\sysmon64.exe -i .\sysmonconfig/xml
```

now we check if sysmon is installed by checking the services running on the system

![[Pasted image 20241116103614.png]]


# Setting Up Wazuh instance on the cloud (Digital Ocean)

## Creating the cloud instance

We logon to digital ocean and create a droplet and choose the location closest to you in order to have faster connectivity.

![[Pasted image 20241116103639.png]]

In this case we are choosing the basic plan with ubuntu installed on it, Make sure to have at least 8gb of ram to run things smooth.

![[Pasted image 20241116103655.png]]

![[Pasted image 20241116103657.png]]

Now we set our root password for the server, this will enable us to ssh into the system.

![[Pasted image 20241116103716.png]]

![[Pasted image 20241116103718.png]]


## Creating the firewall

The main reason for creating this firewall is that the servers we are creating are public on the internet and anyone can access it we secure them with the firewall so that we can only access it and avoid any unwanted traffic.

![[Pasted image 20241116103809.png]]

![[Pasted image 20241116103812.png]]

![[Pasted image 20241116103814.png]]

![[Pasted image 20241116103816.png]]


![[Pasted image 20241116103857.png]]


![[Pasted image 20241116103902.png]]

![[Pasted image 20241116103907.png]]

![[Pasted image 20241116103912.png]]

![[Pasted image 20241116103918.png]]

Loggin to the server through ssh


![[Pasted image 20241116103941.png]]

![[Pasted image 20241116103946.png]]



update the server

![[Pasted image 20241116104004.png]]


![[Pasted image 20241116104008.png]]


![[Pasted image 20241116104018.png]]

![[Pasted image 20241116104022.png]]


Install Wazuh

```powershell
curl -sO https://packages.wazuh.com.4.9/wazuh-insta;;.sh && bash wazuh-install.sh -a
```

![[Pasted image 20241116104151.png]]


**Note** : Remember the username and password.

It will open a web portal and will ask you the creds

![[Pasted image 20241116104242.png]]

login with the saved creds

![[Pasted image 20241116104315.png]]


# Setting Up TheHive on the Cloud

The steps are same as of creating the wazuh droplet

![[Pasted image 20241116104335.png]]

![[Pasted image 20241116104340.png]]

also add thehive droplet to the firewall

![[Pasted image 20241116104424.png]]

**Tip** : Name the firewall as you wish in my case im going with "Soc firewall"

Now login to thehive using `ssh`

install the dependencies such as `the cassandra` ; `Elastisearch`

![[Pasted image 20241116104751.png]]

Once done edit the cassandra config files

```bash
nano /etc/cassandra/cassandra.yaml
```

![[Pasted image 20241116104839.png]]

we change the cluster name, listen address, rpc address, seed address

![[Pasted image 20241116104853.png]]

![[Pasted image 20241116104856.png]]

![[Pasted image 20241116104859.png]]

![[Pasted image 20241116104903.png]]


Now stop the cassandra service and delete the previous files.

![[Pasted image 20241116105015.png]]

```bash
rm -fr /var/lib/cassandra/*
```

Then restart the cassandra for the changes to take place.

![[Pasted image 20241116105037.png]]

Setting up elastic search

```bash
nano /etc/elastisearch/elastisearch.yml
```

![[Pasted image 20241116105109.png]]

![[Pasted image 20241116105113.png]]

![[Pasted image 20241116105116.png]]

![[Pasted image 20241116105120.png]]


Once done with the changes, start and enable Elastisearch

![[Pasted image 20241116105158.png]]

Now lets configure `Thehive`

![[Pasted image 20241116105217.png]]

![[Pasted image 20241116105222.png]]

![[Pasted image 20241116105225.png]]

```bash
nano /etc/thehive/application.conf
```


![[Pasted image 20241116105317.png]]

![[Pasted image 20241116105321.png]]

![[Pasted image 20241116105325.png]]


now we access the hive through the public ip on port 9000

![[Pasted image 20241116105337.png]]

Default Credentials on port 9000
credentials are '[admin@thehive.local](mailto:admin@thehive.local)' with a password of 'secret'

![[Pasted image 20241116105355.png]]



## Configuring wazuh

First we login to the wazuh portal and create an agent

![[Pasted image 20241116105448.png]]

![[Pasted image 20241116105452.png]]

![[Pasted image 20241116105458.png]]


![[Pasted image 20241116105504.png]]


now copy and run the following command on the win 10 client machine

![[Pasted image 20241116105516.png]]


![[Pasted image 20241116105521.png]]


![[Pasted image 20241116105531.png]]

Upon successful execution check the task manager whether the service is running or not

![[Pasted image 20241116105605.png]]

we can see the agent on the dashboard


![[Pasted image 20241116105622.png]]


![[Pasted image 20241116105626.png]]


The virtual SOC is deployed and configured  and it is up and running.

You can try out the effectiveness of the environment by tackling it with some scnarios.