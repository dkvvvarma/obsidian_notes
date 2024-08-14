
Bash is well known Scripting language used to by Unix-based OS to give commands to the system , kernel from terminal (Command line interface)

Since May 2019 , Windows provided a Windows Subsystem for Linux which allowed users to utilize bash in a windows environment.

The main difference between a programming and scripting language is that in scripting we don't need need to compile the code to execute it in scripting language.

As a penetration testers, we must be able to work with any OS and efficiency depends on the proficiency of users on systems they work with especially in priv esc field.

In large Unix based Enterprise networks, employees typically deal with large amounts of data which needs to be sorted  out and filtered out accordingly to determine potential gaps and info as fast as possible.

So, it is essential to learn how to combine several commands and work with individual results which can be done with scripting increasing speed and efficiency. the structure of scripting is divided into:
- `Input` & `Output`
- `Arguments`, `Variables` & `Arrays`
- `Conditional execution`
- `Arithmetic`
- `Loops`
- `Comparison operators`
- `Functions`

Scripting is commonly used to automate some process rather than repeat them all the time. In general, A script doesn't create a process but is executed by the interpreter. To execute a script we have to specify the interpreter and tell it which script it should process. An example for it looks like this

``` Bash
bash script.sh <optional arguments>
 or
sh script.sh  <optional arguments>
 or
./script.sh <optional arguments>
```



An example for executed shell script looks like

![[Pasted image 20240806095234.png]]

The `CIDR.sh` code is as follows

```Bash
#!/bin/bash

# Check for given arguments
if [ $# -eq 0 ]
then
	echo -e "You need to specify the target domain.\n"
	echo -e "Usage:"
	echo -e "\t$0 <domain>"
	exit 1
else
	domain=$1
fi

# Identify Network range for the specified IP address(es)
function network_range {
	for ip in $ipaddr
	do
		netrange=$(whois $ip | grep "NetRange\|CIDR" | tee -a CIDR.txt)
		cidr=$(whois $ip | grep "CIDR" | awk '{print $2}')
		cidr_ips=$(prips $cidr)
		echo -e "\nNetRange for $ip:"
		echo -e "$netrange"
	done
}

# Ping discovered IP address(es)
function ping_host {
	hosts_up=0
	hosts_total=0
	
	echo -e "\nPinging host(s):"
	for host in $cidr_ips
	do
		stat=1
		while [ $stat -eq 1 ]
		do
			ping -c 2 $host > /dev/null 2>&1
			if [ $? -eq 0 ]
			then
				echo "$host is up."
				((stat--))
				((hosts_up++))
				((hosts_total++))
			else
				echo "$host is down."
				((stat--))
				((hosts_total++))
			fi
		done
	done
	
	echo -e "\n$hosts_up out of $hosts_total hosts are up."
}

# Identify IP address of the specified domain
hosts=$(host $domain | grep "has address" | cut -d" " -f4 | tee discovered_hosts.txt)

echo -e "Discovered IP address:\n$hosts\n"
ipaddr=$(host $domain | grep "has address" | cut -d" " -f4 | tr "\n" " ")

# Available options
echo -e "Additional options available:"
echo -e "\t1) Identify the corresponding network range of target domain."
echo -e "\t2) Ping discovered hosts."
echo -e "\t3) All checks."
echo -e "\t*) Exit.\n"

read -p "Select your option: " opt

case $opt in
	"1") network_range ;;
	"2") ping_host ;;
	"3") network_range && ping_host ;;
	"*") exit 0 ;;
esac
```

To better understand the code there are several parts of the script are commented which are split into

- Check for given arguments  
- Identify network range for specified IP address
- Ping discovered Ip address
- Identify Ip address of specified domain
- Available options

 `Check for given arguments`:In this part of script, it consists of if-else statement that checks if  we have specified a domain of target network.

`Identify network range for specified IP `: In this part of script there is a function that makes a "Whois" query for each IP address and displays the line for the reserved network range, and stores it in CIDR.txt

`Ping discovered IP`: This additional function is used to check if the found hosts are reachable with respective IP

`Identify Ip address of specified domain`: As specified  in first step of script, we identify the Ipv4 address of domain and return it to us.

`Available options`: Then we decide which function we want to use to find out more information about the infrastructure.


### Conditional Execution

Conditional execution allow us to control the flow of the script by reaching different conditions. It is one of the essential components. Otherwise, we can only execute one command after another.

When defining various conditions, we must specify which functions or sections of code should be executed for specific value. When specific condition is reached, only the code for that condition is executed and others are skipped. As soon as code section is completed, the following commands will be executed outside conditional execution.

Let's take a look at the first part of script and analyze it

![[Pasted image 20240806113941.png]]

In summary , this code section works based on following components:

- `#!/bin/bash` - shebang
- `if-else-fi` - conditional execution
- `echo` - Print specific output
-  `$# / $0 / $1` - Special variables.
- `domain` - variables
