
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


## Conditional Execution

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

The conditions of the conditional executions will be defined by variables such as (`$#`, `$0`, `$1`,`domain`), values (`0`), and strings. These values are compared with the `comparisions operators` such as `-eq`

Some of the prominent `comparision operators`
- **Numeric Comparison**:
    - `-eq`: Equal to
    - `-ne`: Not equal to
    - `-lt`: Less than
    - `-le`: Less than or equal to
    - `-gt`: Greater than
    - `-ge`: Greater than or equal to
- **String Comparison**:
    - `=`: Equal to
    - `!=`: Not equal to
    - `-z`: String is empty
    - `-n`: String is not empty

### SHEBANG

The `shebang` line is the header part of bash script and always starts with `#!`.The shebang specifies which interpreter should be used to execute the script. If it's missing, the script will be run by the shell specified by the user's environment (typically `/bin/sh` or another default shell). We can also use shebang to define other interpreter like `python`, `perl` and others.

Example script

```bash
#!/bin/bash
echo "This script runs in Bash."
```

Example in other environment

```python
#!/usr/bin/env python
```

**Note**: Even though shebang is header line not all script use this but it is considered as a good  practice to use them without them the behavior can vary depending environment.

### IF-ELSE-IF

The most fundamental programming tasks is to check different conditions. There are two different forms for checking o conditions in programming and scripting languages,

- `if-else condtion`
- `case statements`

##### Pseudo-code

```bash
if [ the number of given arguments equals 0 ]
then
	Print: "You need to specify the target domain."
	Print: "<empty line>"
	Print: "Usage:"
	Print: "   <name of the script> <domain>"
	Exit the script with an error
else
	The "domain" variable serves as the alias for the given argument 
finish the if-condition
```

In default, an `if-else` condition can contain only a single `if` 

#### If-Only.sh
Code: bash

```bash
#!/bin/bash

value=$1

if [ $value -gt "10" ]
then
        echo "Given argument is greater than 10."
fi
```

#### If-Only.sh - Execution

  Conditional Execution

```shell-session
0xWAYNE@htb[/htb]$ bash if-only.sh 5
```

  Conditional Execution

```shell-session
0xWAYNE@htb[/htb]$ bash if-only.sh 12

Given argument is greater than 10.
```

When adding `elif` or `else` , we add alternatives to treat specific values or statuses. If a particular value doesn't apply on first case , it will be caught by others.


#### If-Elif-Else.sh

```bash
#!/bin/bash

value=$1

if [ $value -gt "10" ]
then
	echo "Given argument is greater than 10."
elif [ $value -lt "10" ]
then
	echo "Given argument is less than 10."
else
	echo "Given argument is not a number."
fi
```

#### If-Elif-Else.sh - Execution

  Conditional Execution

```shell-session
0xWAYNE@htb[/htb]$ bash if-elif-else.sh 5

Given argument is less than 10.
```

  Conditional Execution

```shell-session
0xWAYNE@htb[/htb]$ bash if-elif-else.sh 12

Given argument is greater than 10.
```

  Conditional Execution

```shell-session
0xWAYNE@htb[/htb]$ bash if-elif-else.sh HTB

if-elif-else.sh: line 5: [: HTB: integer expression expected
if-elif-else.sh: line 8: [: HTB: integer expression expected
Given argument is not a number.
```

We can alter the script by extending and specifying several conditions. It will be as follows:

#### Several Conditions - Script.sh

```bash
#!/bin/bash

# Check for given argument
if [ $# -eq 0 ]
then
	echo -e "You need to specify the target domain.\n"
	echo -e "Usage:"
	echo -e "\t$0 <domain>"
	exit 1
elif [ $# -eq 1 ]
then
	domain=$1
else
	echo -e "Too many arguments given."
	exit 1
fi

<SNIP>
```

Here we define another condition such as `(elif [<condition>];then)` prints a line stating `(echo -e "...")` we have given more than one argument and exits the program with an error (`exit1`).


### **Exercise Script**
```bash
#!/bin/bash
# Count number of characters in a variable:
#     echo $variable | wc -c

# Variable to encode
var="nef892na9s1p9asn2aJs71nIsm"

for counter in {1..40}
do
        var=$(echo $var | base64)
done
```

Here the given script performs `base64` encoding to a string variable `var` repeatedly in a loop. The task is to print the number of characters in string after 35th iteration of loop.

- **Initialization**:
    
    - The variable `var` is initialized with the value `"nef892na9s1p9asn2aJs71nIsm"`.
    - This variable will undergo base64 encoding 40 times, one for each iteration of the loop.
- **For Loop**:
    
    - The `for` loop runs 40 times (`for counter in {1..40}`).
    - During each iteration, the variable `var` is re-encoded using base64 (`var=$(echo $var | base64)`), which increases its length each time.
- **Checking the 35th Iteration**:
    
    - The `if [ $counter -eq 35 ]` statement checks whether the current iteration is the 35th one.
    - If it is the 35th iteration, the script prints the number of characters in `var` using `wc -c`.
- **Counting Characters**:
    
    - `echo $var | wc -c` counts the number of characters in the variable `var` at that moment. `wc -c` returns the total number of bytes (including a newline character).
    - The output will be the character count at the 35th iteration.
- **Final Output**:
    
    - When the loop reaches the 35th iteration, the script prints something like:  
        `The number of characters at the 35th iteration: <number>`
    - The `<number>` represents how long the base64-encoded string has become by that point.

The output script is

```bash
#!/bin/bash
# Count number of characters in a variable:
#     echo $variable | wc -c

# Variable to encode
var="nef892na9s1p9asn2aJs71nIsm"

for counter in {1..40}
do
    var=$(echo $var | base64)
    
    if [ $counter -eq 35 ]; then
       echo "The number of characters at the 35th iteration: $(echo $var | wc -c)"
    fi
done
```

Explanation:

- The `if [ $counter -eq 35 ]` condition checks if the loop is at the 35th iteration.
- If it is, the script uses `echo $var | wc -c` to print the number of characters of the variable `var` at that point.
- The result is printed to the terminal.

![[Pasted image 20241010104354.png]]


## Arguments, Variables, and Arrays

### Arguments

An advantage of the bash script is we can pass up to 9 arguments at once without variabales [$0 - $9] or setting up any correspondent requirements for this.  But the argument [$0] is reserved for the script so only 9 arguments

The assignment would look like this in comparison:

```shell-session
0xWAYNE@htb[/htb]$ ./script.sh ARG1 ARG2 ARG3 ... ARG9
       ASSIGNMENTS:       $0      $1   $2   $3 ...   $9
```

This means that we have automatically assigned the corresponding arguments to the predefined variables in this place. These variables are called special variables. These special variables serve as placeholders. If we now look at the code section again, we will see where and which arguments have been used.

## Special Variables

Special variables use the [Internal Field Separator](https://bash.cyberciti.biz/guide/$IFS) (`IFS`) to identify when an argument ends and when the consecutive argument begins Bash provides various special variables that assist while scripting. Some of these variables are:

| **IFS** | **Description**                                                                                                                                                         |
| ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$#`    | This variable holds the number of arguments passed to the script.                                                                                                       |
| `$@`    | This variable can be used to retrieve the list of command-line arguments.                                                                                               |
| `$n`    | Each command-line argument can be selectively retrieved using its position. For example, the first argument is found at `$1`.                                           |
| `$$`    | The process ID of the currently executing process.                                                                                                                      |
| `$?`    | The exit status of the script. This variable is useful to determine a command's success. The value 0 represents successful execution, while 1 is a result of a failure. |

Of the ones shown above, we have 3 such special variables in our `if-else` condition.

|**IFS**|**Description**|
|---|---|
|`$#`|In this case, we need just one variable that needs to be assigned to the `domain` variable. This variable is used to specify the target we want to work with. If we provide just an FQDN as the argument, the `$#` variable will have a value of `1`.|
|`$0`|This special variable is assigned the name of the executed script, which is then shown in the "`Usage:`" example.|
|`$1`|Separated by a space, the first argument is assigned to that special variable.|

## Variables

We also see at the end of the if-else loop that we assign the value of the first argument to the variable called "`domain`". The assignment of variables takes place without the dollar sign (`$`). The dollar sign is only intended to allow this variable's corresponding value to be used in other code sections. When assigning variables, there must be no spaces between the names and values
In contrast to other programming languages, there is no direct differentiation and recognition between the types of variables in Bash like "`strings`," "`integers`," and "`boolean`." All contents of the variables are treated as string characters. Bash enables arithmetic functions depending on whether only numbers are assigned or not. It is important to note when declaring variables that they do `not` contain a `space`. Otherwise, the actual variable name will be interpreted as an internal function or a command.


## Arrays

There is also the possibility of assigning several values to a single variable in Bash. This can be beneficial if we want to scan multiple domains or IP addresses. These variables are called `arrays` that we can use to store and process an ordered sequence of specific type values. `Arrays` identify each stored entry with an `index` starting with `0`. When we want to assign a value to an array component, we do so in the same way as with standard shell variables. All we do is specify the field index enclosed in square brackets. The declaration for `arrays` looks like this in Bash:

```bash
#!/bin/bash

domains=(www.inlanefreight.com ftp.inlanefreight.com vpn.inlanefreight.com www2.inlanefreight.com)

echo ${domains[0]}
```

We can also retrieve them individually using the index using the variable with the corresponding index in curly brackets. Curly brackets are used for variable expansion.

```shell-session
0xWAYNE@htb[/htb]$ ./Arrays.sh

www.inlanefreight.com
```

It is important to note that single quotes (`'` ... `'`) and double quotes (`"` ... `"`) prevent the separation by a space of the individual values in the array. This means that all spaces between the single and double quotes are ignored and handled as a single value assigned to the array.

```bash
#!/bin/bash

domains=("www.inlanefreight.com ftp.inlanefreight.com vpn.inlanefreight.com" www2.inlanefreight.com)
echo ${domains[0]}
```

```shell-session
0xWAYNE@htb[/htb]$ ./Arrays.sh

www.inlanefreight.com ftp.inlanefreight.com vpn.inlanefreight.com
```

#### Questions

Answer the question(s) below to complete this Section and earn cubes!

+ 2  Submit the echo statement that would print "www2.inlanefreight.com" when running the last "Arrays.sh" script.

Ans: echo${domain[1]}


# Comparison Operators

-----

To compare specific values with each other, we need elements that are called [comparison operators](https://www.tldp.org/LDP/abs/html/comparison-ops.html). The `comparison operators` are used to determine how the defined values will be compared. For these operators, we differentiate between:

- `string` operators
- `integer` operators
- `file` operators
- `boolean` operators

---

## String Operators

If we compare strings, then we know what we would like to have in the corresponding value.

| **Operator** | **Description**                             |
| ------------ | ------------------------------------------- |
| `==`         | is equal to                                 |
| `!=`         | is not equal to                             |
| `<`          | is less than in ASCII alphabetical order    |
| `>`          | is greater than in ASCII alphabetical order |
| `-z`         | if the string is empty (null)               |
| `-n`         | if the string is not null                   |

It is important to note here that we put the variable for the given argument (`$1`) in double-quotes (`"$1"`). This tells Bash that the content of the variable should be handled as a string. Otherwise, we would get an error.


```bash
#!/bin/bash

# Check the given argument
if [ "$1" != "HackTheBox" ]
then
	echo -e "You need to give 'HackTheBox' as argument."
	exit 1

elif [ $# -gt 1 ]
then
	echo -e "Too many arguments given."
	exit 1

else
	domain=$1
	echo -e "Success!"
fi
```

String comparison operators "`<` / `>`" works only within the double square brackets `[[ <condition> ]]`. We can find the ASCII table on the Internet or by using the following command in the terminal. We take a look at an example later.

  Comparison Operators

```shell-session
0xWAYNE@htb[/htb]$ man ascii
```

#### ASCII Table

|**Decimal**|**Hexadecial**|**Character**|**Description**|
|---|---|---|---|
|0|00|NUL|End of a string|
|...|...|...|...|
|65|41|A|Capital A|
|66|42|B|Capital B|
|67|43|C|Capital C|
|68|44|D|Capital D|
|...|...|...|...|
|127|7F|DEL|Delete|

`ASCII` stands for `American Standard Code for Information Interchange` and represents a 7-bit character encoding. Since each bit can take two values, there are `128` different bit patterns, which can also be interpreted as the decimal integers `0` - `127` or in hexadecimal values `00` - `7F`. The first 32 ASCII character codes are reserved as so-called [control characters](https://en.wikipedia.org/wiki/Control_character).

---

## Integer Operators

Comparing integer numbers can be very useful for us if we know what values we want to compare. Accordingly, we define the next steps and commands how the script should handle the corresponding value.

|**Operator**|**Description**|
|---|---|
|`-eq`|is equal to|
|`-ne`|is not equal to|
|`-lt`|is less than|
|`-le`|is less than or equal to|
|`-gt`|is greater than|
|`-ge`|is greater than or equal to|

---

```bash
#!/bin/bash

# Check the given argument
if [ $# -lt 1 ]
then
	echo -e "Number of given arguments is less than 1"
	exit 1

elif [ $# -gt 1 ]
then
	echo -e "Number of given arguments is greater than 1"
	exit 1

else
	domain=$1
	echo -e "Number of given arguments equals 1"
fi
```

---

## File Operators

The file operators are useful if we want to find out specific permissions or if they exist.

| **Operator** | **Description**                                        |
| ------------ | ------------------------------------------------------ |
| `-e`         | if the file exist                                      |
| `-f`         | tests if it is a file                                  |
| `-d`         | tests if it is a directory                             |
| `-L`         | tests if it is if a symbolic link                      |
| `-N`         | checks if the file was modified after it was last read |
| `-O`         | if the current user owns the file                      |
| `-G`         | if the file’s group id matches the current user’s      |
| `-s`         | tests if the file has a size greater than 0            |
| `-r`         | tests if the file has read permission                  |
| `-w`         | tests if the file has write permission                 |
| `-x`         | tests if the file has execute permission               |

Code: bash

```bash
#!/bin/bash

# Check if the specified file exists
if [ -e "$1" ]
then
	echo -e "The file exists."
	exit 0

else
	echo -e "The file does not exist."
	exit 2
fi
```

---

## Boolean and Logical Operators

We get a boolean value "`false`" or "`true`" as a result with logical operators. Bash gives us the possibility to compare strings by using double square brackets `[[ <condition> ]]`. To get these boolean values, we can use the string operators. Whether the comparison matches or not, we get the boolean value "`false`" or "`true`".

```bash
#!/bin/bash

# Check the boolean value
if [[ -z $1 ]]
then
	echo -e "Boolean value: True (is null)"
	exit 1

elif [[ $# > 1 ]]
then
	echo -e "Boolean value: True (is greater than)"
	exit 1

else
	domain=$1
	echo -e "Boolean value: False (is equal to)"
fi
```

---

## Logical Operators

With logical operators, we can define several conditions within one. This means that all the conditions we define must match before the corresponding code can be executed.

| **Operator** | **Description**        |
| ------------ | ---------------------- |
| `!`          | logical negotation NOT |
| `&&`         | logical AND            |
| `\|`         | logical OR             |

Code: bash

```bash
#!/bin/bash

# Check if the specified file exists and if we have read permissions
if [[ -e "$1" && -r "$1" ]]
then
	echo -e "We can read the file that has been specified."
	exit 0

elif [[ ! -e "$1" ]]
then
	echo -e "The specified file does not exist."
	exit 2

elif [[ -e "$1" && ! -r "$1" ]]
then
	echo -e "We don't have read permission for this file."
	exit 1

else
	echo -e "Error occured."
	exit 5
fi
```

---

## Exercise Script

```bash
#!/bin/bash

var="8dm7KsjU28B7v621Jls"
value="ERmFRMVZ0U2paTlJYTkxDZz09Cg"

for i in {1..40}
do
        var=$(echo $var | base64)
		
		#<---- If condition here:
done
```

The code

```bash
#!/bin/bash

var="8dm7KsjU28B7v621Jls"
value="ERmFRMVZ0U2paTlJYTkxDZz09Cg"

for i in {1..40}
do
    var=$(echo "$var" | base64)

    # If condition to check if var contains value and has more than 113450 characters
    if [[ "$var" == *"$value"* ]] && [ $(echo -n "$var" | wc -c) -gt 113450 ]; then
        echo "$var" | tail -c 20
        break
    fi
done
```


Answer: `2paTlJYTkxDZz09Cg==`


# Arithmetic

---

In Bash, we have seven different `arithmetic operators` we can work with. These are used to perform different mathematical operations or to modify certain integers.

#### Arithmetic Operators

|**Operator**|**Description**|
|---|---|
|`+`|Addition|
|`-`|Substraction|
|`*`|Multiplication|
|`/`|Division|
|`%`|Modulus|
|`variable++`|Increase the value of the variable by 1|
|`variable--`|Decrease the value of the variable by 1|

We can summarize all these operators in a small script:

#### Arithmetic.sh

```bash
#!/bin/bash

increase=1
decrease=1

echo "Addition: 10 + 10 = $((10 + 10))"
echo "Substraction: 10 - 10 = $((10 - 10))"
echo "Multiplication: 10 * 10 = $((10 * 10))"
echo "Division: 10 / 10 = $((10 / 10))"
echo "Modulus: 10 % 4 = $((10 % 4))"

((increase++))
echo "Increase Variable: $increase"

((decrease--))
echo "Decrease Variable: $decrease"
```

The output of this script looks like this:

#### Arithmetic.sh - Execution

```shell-session
0xWAYNE@htb[/htb]$ ./Arithmetic.sh

Addition: 10 + 10 = 20
Substraction: 10 - 10 = 0
Multiplication: 10 * 10 = 100
Division: 10 / 10 = 1
Modulus: 10 % 4 = 2
Increase Variable: 2
Decrease Variable: 0
```

---

We can also calculate the length of the variable. Using this function `${#variable}`, every character gets counted, and we get the total number of characters in the variable.

#### VarLength.sh

```bash
#!/bin/bash

htb="HackTheBox"

echo ${#htb}
```

#### VarLength.sh

```shell-session
0xWAYNE@htb[/htb]$ ./VarLength.sh

10
```

---

If we look at our `CIDR.sh` script, we will see that we have used the `increase` and `decrease` operators several times. This ensures that the while loop, which we will discuss later, runs and pings the hosts while the variable "`stat`" has a value of `1`. If the ping command ends with code `0` (successful), we get a message that the `host is up` and the "`stat`" variable, as well as the variables "`hosts_up`" and "`hosts_total`" get changed.

#### CIDR.sh

Code: bash

```bash
<SNIP>
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
<SNIP>
```


# Input and Output

---

## Input Control

Basically , this is about controlling the the script. If you consider `cidr.sh`, there are lot of commands that gets executed when ran and we get results from our specific requests and executed commands.  But we can decide manually on how to proceed. In another instance , when performing a scan operation we may be restricted not to perform some specific scans Therefore we need to make sure the script waits for our instructions.


##### Example:


```bash
# Available options
<SNIP>
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

From the script
- The first echo lines serves a display menu for available options
- With `read` command the line `Select your option` is displayed
- The `-p` flag ensures our output is confined to same line, which is stored in `opt` variable.
- Then `case` statement with corresponding fucntions will be executed.
- Depending on the option selected, the `case` statement determines the functions are executed.



---

## Output Control

SImilary like `input control` , the `output control` is about redirections of output in `Linux FUndamentals` module. One of the trouble with output redirection is we never get any output  from respective command, It will be redirected to an appropriate file. The time taken is directly proportional to scripts complications.

So to avoid these scenarios we use `tee` utility. it ensures that we see results we get immediately ASAP and they are stored in corresponding file.

##### Example
Here is an example from `cidr.sh`

```bash
<SNIP>

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

<SNIP>

# Identify IP address of the specified domain
hosts=$(host $domain | grep "has address" | cut -d" " -f4 | tee discovered_hosts.txt)

<SNIP>
```

When using the `tee` function , we transfer the received output and use the pipe ( | ) to forward it to `tee`.

The `-a/ --append`  parameter ensures the specified file is not overwritten but supplemented with new results. At same time showing the results and how they will be found in file.

-------------------------------------------------


# Flow Control - Loops

The control of flow of our scripts is essential as we have learnt it in `if-else` conditions. Since we want our scripts to work quickly and efficiently when we utilize them in our work environments. to achieve this we can use other components to increase efficiency and allow error-free processing. Each control structure will either be a branch or loop. 

Logical expressions of boolean values usually control the execution of control structure which include:
   - Branches:
       - If - Else conditions
       - Case statements
   - Loops: 
       - For loops
       - While loops
       - Until loops

### For Loops

The `for` loop is executed on each pass for precisely one parameter, which the sell takes from a list and calculated from  an increment or takes another data source.

The for loop runs as long as it finds corresponding data. This type of loop can be structured in two ways:
- They are often used when we need to work with many different values from an array. This is used to scan different hosts or ports.
- We can also use it to execute specific commands for known ports and their services to speed up our enumerations process.

Here is a syntax

```bash
for variable in 1 2 3 4
do 
    echo $variable
done
```

 or

```bash
for variable in file1 file2 file3
do
     echo $variable
done
```

or 

```bash
for ip in "10.10.10.170 10.10.10.174 10.10.10.175"
do 
    ping -c 1 $ip
done
```


All this can be written  on single line if you will so. which will look like the following


```bash
for ip in "10.10.10.170 10.10.10.174";do ping -c 1 $ip;done
```

Referring to another section of `cidr.sh` script

```bash
<SNIP>

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

<SNIP>
```

As from previous example, for each Ip address from array `ipaddr` we make a `whois` request which output is filtered for `NetRange` and `CIDR`. This helps to determine which address range our target is located in. We can utilize this info to search for additional hosts during penetration test. `if approved by client`. The results that we receive are displayed accordng are stored in file `CIDR.txt`


#### While Loops
The `While` loop is conceptually simple and follows a principle

` As long as condition is fulfilled the statement is executed`.

We combine loops and merge their execution with different values. It is important to notice excessive combination of several loops in each other can make the code very unclear and lead to errors that can be hard to find and follow.

Such an example will look like the following

```bash
<SNIP>
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
<SNIP>
```

The `While` loop also works with conditions like `if-else`. A while loop needs some sort counter to orientate itself when it needs to stop executing the commands it contains. otherwise it will become an endless loop.

Such counters can be a variable that declared with specific value or boolean value. `While` loop run while boolean value is `True`. Besides counter , we can also use command `break`, which interrupts the loop when reaching this command like in following example.

```bash
#!/bin/bash

counter=0

while [ $counter -lt 10 ]
do
  # Increase $counter by 1
  ((counter++))
  echo "Counter: $counter"

  if [ $counter == 2 ]
  then
    continue
  elif [ $counter == 4 ]
  then
    break
  fi
done
```

From the code you can see it used a break function to exit code when the variable value become 4.

### Until Loops

Then there is `until` loops which are relatively rare. Nevertheless, The `until` loops works precisely like `while` loops but with a difference

- The code inside a `until` loops is executed as long as particular conditions is `false`.

The other way is to let loop run until the desired value is reached. The `until` loops are very well suited for this. This type of loop works similarly to `while` but until the boolean value is `false`

```bash
#!/bin/bash

counter=0

until [ $counter -eq 10 ]
do
  # Increase $counter by 1
  ((counter++))
  echo "Counter: $counter"
done
```

#### Exercise script

```bash
#!/bin/bash

# Decrypt function
function decrypt {
	MzSaas7k=$(echo $hash | sed 's/988sn1/83unasa/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/4d298d/9999/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/3i8dqos82/873h4d/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/4n9Ls/20X/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/912oijs01/i7gg/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/k32jx0aa/n391s/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/nI72n/YzF1/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/82ns71n/2d49/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/JGcms1a/zIm12/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/MS9/4SIs/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/Ymxj00Ims/Uso18/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/sSi8Lm/Mit/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/9su2n/43n92ka/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/ggf3iunds/dn3i8/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/uBz/TT0K/g')

	flag=$(echo $MzSaas7k | base64 -d | openssl enc -aes-128-cbc -a -d -salt -pass pass:$salt)
}

# Variables
var="9M"
salt=""
hash="VTJGc2RHVmtYMTl2ZnYyNTdUeERVRnBtQWVGNmFWWVUySG1wTXNmRi9rQT0K"

# Base64 Encoding Example:
#        $ echo "Some Text" | base64

# <- For-Loop here
# Check if $salt is empty
if [[ ! -z "$salt" ]]
then
	decrypt
	echo $flag
else
	exit 1
fi
```

Q) Create a "For" loop that encodes the variable "var" 28 times in "base64". The number of characters in the 28th hash is the value that must be assigned to the "salt" variable.

append this in code at below for loop comment

```bash
for i in {1..28}; do
    var=$(echo -n "$var" | base64)
done

salt=var${#var}
```

The updated script

```bash
#!/bin/bash

# Decrypt function
function decrypt {
    MzSaas7k=$(echo $hash | sed 's/988sn1/83unasa/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/4d298d/9999/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/3i8dqos82/873h4d/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/4n9Ls/20X/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/912oijs01/i7gg/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/k32jx0aa/n391s/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/nI72n/YzF1/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/82ns71n/2d49/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/JGcms1a/zIm12/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/MS9/4SIs/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/Ymxj00Ims/Uso18/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/sSi8Lm/Mit/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/9su2n/43n92ka/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/ggf3iunds/dn3i8/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/uBz/TT0K/g')

    flag=$(echo $MzSaas7k | base64 -d | openssl enc -aes-128-cbc -a -d -salt -pass pass:$salt)
}

# Variables
var="9M"
salt=""
hash="VTJGc2RHVmtYMTl2ZnYyNTdUeERVRnBtQWVGNmFWWVUySG1wTXNmRi9rQT0K"

# Base64 Encoding Loop
for i in {1..28}
do
    var=$(echo $var | base64)
done

salt=$(echo $var | wc -c)

# Check if $salt is empty
if [[ ! -z "$salt" ]]
then
    decrypt
    echo $flag
else
    exit 1
fi
```

Answer: HTBL00p5r0x

# Flow Control - Branches
---
## Case Statements

`Case` statements are also known as `switch-case` statements in other languages, such as C/C++ and C#. The main difference between `if-else` and `switch-case` is that `if-else` constructs allow us to check any boolean expression, while `switch-case` always compares only the variable with the exact value. Therefore, the same conditions as for `if-else`, such as "greater-than," are not allowed for `switch-case`. The syntax for the switch-case statements looks like this:

#### Syntax - Switch-Case

Code: bash

```bash
case <expression> in
	pattern_1 ) statements ;;
	pattern_2 ) statements ;;
	pattern_3 ) statements ;;
esac
```

The definition of switch-case starts with `case`, followed by the variable or value as an expression, which is then compared in the pattern. If the variable or value matches the expression, then the statements are executed after the parenthesis and ended with a double semicolon (`;;`).

In our `CIDR.sh` script, we have used such a `case` statement. Here we defined four different options that we assigned to our script, how it should proceed after our decision.

#### CIDR.sh

Code: bash

```bash
<SNIP>
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
<SNIP>
```

With the first two options, this script executes different functions that we had defined before. With the third option, both functions are executed, and with any other option, the script will be terminated.

# Functions

---

The bigger our scripts get, the more chaotic they become. If we use the same routines several times in the script, the script's size will increase accordingly. In such cases, `functions` are the solution that improves both the size and the clarity of the script many times. We combine several commands in a block between curly brackets ( `{` ... `}` ) and call them with a function name defined by us with `functions`. Once a function has been defined, it can be called and used again during the script.

`Functions` are an essential part of scripts and programs, as they are used to execute recurring commands for different values and phases of the script or program. Therefore, we do not have to repeat the whole section of code repeatedly but can create a single function that executes the specific commands. The definition of such functions makes the code easier to read and helps to keep the code as short as possible. It is important to note that functions must always be defined logically `before` the first call since a script is also processed from top to bottom. Therefore the definition of a function is always `at the beginning` of the script. There are two methods to define the functions:

#### Method 1 - Functions

Code: bash

```bash
function name {
	<commands>
}
```

#### Method 2 - Functions

Code: bash

```bash
name() {
	<commands>
}
```

We can choose the method to define a function that is most comfortable for us. In our `CIDR.sh` script, we used the first method because it is easier to read with the keyword "`function`."

#### CIDR.sh

Code: bash

```bash
<SNIP>
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
<SNIP>
```

The function is called only by calling the specified name of the function, as we have seen in the case statement.

#### Function Execution - CIDR.sh

Code: bash

```bash
<SNIP>
case $opt in
	"1") network_range ;;
	"2") ping_host ;;
	"3") network_range && ping_host ;;
	"*") exit 0 ;;
esac
```

---

## Parameter Passing

Such functions should be designed so that they can be used with a fixed structure of the values or at least only with a fixed format. Like we have already seen in our `CIDR.sh` script, we used the format of an IP address for the function "`network_range`". The parameters are optional, and therefore we can call the function without parameters. In principle, the same applies to the passed parameters as to parameters passed to a shell script. These are `$1` - `$9` (`${n}`), or `$variable` as we have already seen. Each function has its own set of parameters. So they do not collide with those of other functions or the parameters of the shell script.

An important difference between bash scripts and other programming languages is that all defined variables are always processed `globally` unless otherwise declared by "[local](https://www.tldp.org/LDP/abs/html/localvar.html)." This means that the first time we have defined a variable in a function, we will call it in our main script (outside the function). Passing the parameters to the functions is done the same way as we passed the arguments to our script and looks like this:

#### PrintPars.sh

Code: bash

```bash
#!/bin/bash

function print_pars {
	echo $1 $2 $3
}

one="First parameter"
two="Second parameter"
three="Third parameter"

print_pars "$one" "$two" "$three"
```

  Functions

```shell-session
0xWAYNE@htb[/htb]$ ./PrintPars.sh

First parameter Second parameter Third parameter
```

---

## Return Values

When we start a new process, each `child process` (for example, a `function` in the executed script) returns a `return code` to the `parent process` (`bash shell` through which we executed the script) at its termination, informing it of the status of the execution. This information is used to determine whether the process ran successfully or whether specific errors occurred. Based on this information, the `parent process` can decide on further program flow.

|**Return Code**|**Description**|
|---|---|
|`1`|General errors|
|`2`|Misuse of shell builtins|
|`126`|Command invoked cannot execute|
|`127`|Command not found|
|`128`|Invalid argument to exit|
|`128+n`|Fatal error signal "`n`"|
|`130`|Script terminated by Control-C|
|`255\*`|Exit status out of range|

---

To get the value of a function back, we can use several methods like `return`, `echo`, or a `variable`. In the next example, we will see how to use "`$?`" to read the "`return code`," how to pass the arguments to the function and how to assign the result to a variable.

#### Return.sh

Code: bash

```bash
#!/bin/bash

function given_args {

        if [ $# -lt 1 ]
        then
                echo -e "Number of arguments: $#"
                return 1
        else
                echo -e "Number of arguments: $#"
                return 0
        fi
}

# No arguments given
given_args
echo -e "Function status code: $?\n"

# One argument given
given_args "argument"
echo -e "Function status code: $?\n"

# Pass the results of the funtion into a variable
content=$(given_args "argument")

echo -e "Content of the variable: \n\t$content"
```

#### Return.sh - Execution

  Functions

```shell-session
0xWAYNE@htb[/htb]$ ./Return.sh

Number of arguments: 0
Function status code: 1

Number of arguments: 1
Function status code: 0

Content of the variable:
    Number of arguments: 1
```


# Debugging

---

Bash gives us an excellent opportunity to find, track, and fix errors in our code. The term `debugging` can have many different meanings. Nevertheless, [Bash debugging](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_02_03.html) is the process of removing errors (bugs) from our code. Debugging can be performed in many different ways. For example, we can use our code for debugging to check for typos, or we can use it for code analysis to track them and determine why specific errors occur.

This process is also used to find vulnerabilities in programs. For example, we can try to cause errors using different input types and track their handling in the CPU through the assembler, which may provide a way to manipulate the handling of these errors to insert our own code and force the system to execute it. This topic will be covered and discussed in detail in other modules. Bash allows us to debug our code by using the "`-x`" (`xtrace`) and "`-v`" options. Now let us see an example with our `CIDR.sh` script.

#### CIDR.sh - Debugging

  Debugging

```shell-session
0xWAYNE@htb[/htb]$ bash -x CIDR.sh

+ '[' 0 -eq 0 ']'
+ echo -e 'You need to specify the target domain.\n'
You need to specify the target domain.

+ echo -e Usage:
Usage:
+ echo -e '\tCIDR.sh <domain>'
	CIDR.sh <domain>
+ exit 1
```

Here Bash shows us precisely which function or command was executed with which values. This is indicated by the plus sign (`+`) at the beginning of the line. If we want to see all the code for a particular function, we can set the "`-v`" option that displays the output in more detail.

#### CIDR.sh - Verbose Debugging

  Debugging

```shell-session
0xWAYNE@htb[/htb]$ bash -x -v CIDR.sh

#!/bin/bash

# Check for given argument
if [ $# -eq 0 ]
then
	echo -e "You need to specify the target domain.\n"
	echo -e "Usage:"
	echo -e "\t$0 <domain>"
	exit 1
else
	domain=$1
fi
+ '[' 0 -eq 0 ']'
+ echo -e 'You need to specify the target domain.\n'
You need to specify the target domain.

+ echo -e Usage:
Usage:
+ echo -e '\tCIDR.sh <domain>'
	CIDR.sh <domain>
+ exit 1
```

In comparison to normal debugging, we see the entire code section that has been processed so far and then the individual steps that have been taken.