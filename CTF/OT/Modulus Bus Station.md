

You are inside an industry network and identify Modbus protocol running, enumerate to find sensitive information!

`nc 0.cloud.chals.io 25066`

The challenge involves identifying and interacting with a Modbus protocol running on a network service to find sensitive information. Modbus is a serial communication protocol used for transmitting information between electronic devices.

**Challenge Details:**

- **Service Address:** `0.cloud.chals.io`
- **Port:** `25066`
- **Modbus Protocol:** A standard used in industrial networks to communicate between devices such as sensors and controllers.


**Modbus Protocol:**

- **Description:** Modbus is a communication protocol used for transmitting information over serial lines between devices. It operates in various modes, such as Modbus RTU (Remote Terminal Unit) and Modbus TCP (Transmission Control Protocol).
- **Usage:** Commonly used in industrial settings to connect and manage devices like PLCs (Programmable Logic Controllers) and sensors.

**Technical approach:**

Install pymodbus 

```shell
pip install pymodbus
```

Also install pymodbusTCP

```bash
pip install pymodbuTCP
```

Write a python script to get the flag

``` python3
ip = "0.cloud.chals.io" 
port = 25066 
start = 0 
count = 100 
client = ModbusClient(ip, port) 
r = client.read_holding_registers(start, count) 
for i in r: 
    if i!=0: 
        print(bytearray.fromhex(hex(i).replace("0x",'')).decode(),end='') 
print()
```

### Explanation of the Code

1. **Importing the Library**:
    
    python
    
    Copy code
    
    `from pymodbus.client.sync import ModbusTcpClient`
    
    This line imports the `ModbusTcpClient` class from the `pymodbus` library, which allows you to create a client to communicate with a Modbus server using TCP.
    
2. **Defining Variables**:
    
    python
    
    Copy code
    
    `ip = "0.cloud.chals.io" port = 25066 start = 0 count = 100`
    
    - `ip`: The IP address of the Modbus server.
    - `port`: The port number on which the Modbus server is running.
    - `start`: The starting register address for reading.
    - `count`: The number of registers to read.
3. **Creating the Modbus Client**:
    
    python
    
    Copy code
    
    `client = ModbusClient(ip, port)`
    
    This line initializes the Modbus client with the server's IP address and port number.
    
4. **Reading Holding Registers**:
    
    python
    
    Copy code
    
    `r = client.read_holding_registers(start, count)`
    
    This function reads `count` number of holding registers starting from the `start` address. The data is returned as a list of register values.
    
5. **Processing and Printing Data**:
    
    python
    
    Copy code
    
    `for i in r:     if i != 0:         print(bytearray.fromhex(hex(i).replace("0x",'')).decode(), end='') print()`
    
    - **Loop through Registers**: The code iterates through the list of register values.
    - **Filter and Decode**: For each non-zero register value, it converts the value from hexadecimal to a bytearray, then decodes it to a string.
    - **Output**: The decoded strings are concatenated and printed.

![[Pasted image 20240810180751.png]]

The flag is `KPMG_CTF{xFN0Vd32BLvk1_oLe1AIpVhB33G9haTIiQF4GRcUoWfpdbv5O-asVrfn8ENpoL8y00RT4jy3A6VRpBsMz9HAZsiRYKODLd4aALAxUwAS}`
