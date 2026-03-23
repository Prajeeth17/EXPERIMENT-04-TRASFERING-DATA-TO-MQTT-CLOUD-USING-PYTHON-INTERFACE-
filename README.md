



## EXPERIMENT-04-TRASFERING-DATA-TO-MQTT-CLOUD-USING-PYTHON-INTERFACE-

### NAME:Prajeeth K T
### ROLL NO :212222110034
### DEPARTMENT :CSE(IOT)
### DATE:27/02/26


## AIM:
To transfer data from a Python script to an MQTT cloud server (HiveMQ Cloud) using the MQTT protocol. 
## APPARATUS/SOFTWARE REQUIRED: 
1.HiveMQ Cloud account
Python installed on your system
3. Required Python library: paho-mqtt
4. Internet connection

## THEORY 
### Message Queuing Telemetry Transport (MQTT)
MQTT is a lightweight messaging protocol used for efficient communication between devices over the Internet. It follows a publish-subscribe model, making it ideal for IoT applications.


 Message Queuing Telemetry Transport (MQTT)

MQTT is a lightweight and efficient messaging protocol designed for small, low-power devices that need to communicate over unreliable networks. It is widely used in Internet of Things (IoT) applications due to its low bandwidth requirements and minimal overhead.

MQTT Architecture and Working Principle

MQTT follows a publish-subscribe model, which is different from the traditional client-server architecture. Instead of direct communication between devices, MQTT introduces an intermediary called the broker that manages message distribution.

Broker: The central server that routes messages between clients (e.g., HiveMQ Cloud). The broker ensures that messages are delivered to the correct subscribers.

Client: Any device or application that sends (publishes) or receives (subscribes to) messages. Each client can act as a publisher, a subscriber, or both.

Topics: Channels through which messages are communicated (e.g., test/topic). Clients publish messages to a topic, and subscribers receive them based on their topic subscriptions.

QoS (Quality of Service) Levels: Define message delivery reliability:

QoS 0 (At most once): The message is sent without acknowledgment. It may be lost if network issues occur.

QoS 1 (At least once): The message is guaranteed to be delivered but may be duplicated.

QoS 2 (Exactly once): Ensures that the message is delivered only once, providing the highest level of reliability.

Why Use MQTT?

Lightweight Protocol: Ideal for IoT applications with limited resources.

Low Bandwidth Usage: Efficient data transmission over constrained networks.

Reliable Message Delivery: Supports different QoS levels.

Scalability: Suitable for large-scale IoT networks.

![image](https://github.com/user-attachments/assets/d21bcc04-9617-43dc-8cce-1dda54ea4562)

FIGURE -01 BLOCK DIAGRAMATIC REPRESENTATION OF MQTT PROTOCOL


### PROCEDURE 

Step 1: Set Up HiveMQ Cloud Account

Go to HiveMQ Cloud Console.

Sign up or log in.

Create a new cluster (if not already created).

Navigate to "Access Management" > "Credentials" and note the:

Host URL (e.g., your-cluster-url.hivemq.cloud)

Port (default: 8883 for secure connections)

Username & Password for authentication.

Step 2: Install Required Python Library

Run the following command in your terminal or command prompt to install the required MQTT library:

pip install paho-mqtt

Step 3: Write a Python Script to Publish Data

Create a Python script and paste the following code:

import paho.mqtt.client as mqtt

# HiveMQ Cloud credentials
BROKER = "your-cluster-url.hivemq.cloud"  # Replace with your HiveMQ Cloud broker URL
PORT = 8883  # Secure MQTT port
USERNAME = "your-username"  # Replace with your HiveMQ username
PASSWORD = "your-password"  # Replace with your HiveMQ password
TOPIC = "test/topic"  # Define your MQTT topic

#### Callback function when the client connects to the broker
def on_connect(client, userdata, flags, rc):
    if rc == 0:
        print("Connected to HiveMQ Cloud!")
    else:
        print(f"Connection failed with code {rc}")

#### Create MQTT client
client = mqtt.Client()

#### Set username and password for HiveMQ Cloud authentication
client.username_pw_set(USERNAME, PASSWORD)

#### Enable TLS for secure connection
client.tls_set()

#### Assign callback function
client.on_connect = on_connect

#### Connect to HiveMQ Cloud broker
client.connect(BROKER, PORT, 60)

#### Publish a message
message = "Hello from Python to HiveMQ!"
client.publish(TOPIC, message)
print(f"Message published: {message}")

#### Disconnect
client.disconnect()

Step 4: Verify Data Reception in HiveMQ Cloud

Open HiveMQ Cloud Console.

Go to "Live Data" > "MQTT Web Client".

Subscribe to the topic (test/topic).

Run the Python script.

Check if the message appears in the HiveMQ Web Client.
## PROGRAM
~~~
!pip install paho-mqtt
import time
import paho.mqtt.client as mqtt
broker = "97825dc4ffeb4ef69020a34b200834d7.s1.eu.hivemq.cloud"
port = 8883
topic = "iot/demo/sensor"
username = "hivemq.webclient.1772167962865"
password = "C:8ieP7F1U%b2Ltr&Xw!"
client = mqtt.Client(client_id="python-publisher-001",
callback_api_version=mqtt.CallbackAPIVersion.VERSION2)
client.username_pw_set(username, password)
client.tls_set()
def on_connect(client, userdata, flags, reasonCode, properties):
  print("Connected to broker, reasonCode:", reasonCode)
def on_publish(client, userdata, mid):
  print("on_publish called, mid:", mid)
def on_disconnect(client, userdata, reasonCode, properties):
  print("Disconnected, reasonCode:", reasonCode)
client.on_connect = on_connect
client.on_publish = on_publish
client.on_disconnect = on_disconnect
client.connect(broker, port, keepalive=60)
client.loop_start()
message = "AJITH.A"
info = client.publish(topic, payload=message, qos=1, retain=True)
info.wait_for_publish()
time.sleep(0.2)
client.loop_stop()
client.disconnect()
print(f"Message '{message}' published to topic '{topic}' (qos=1 retain=True)")
~~~
~~~
!pip install paho-mqtt
import paho.mqtt.client as mqtt
import time
import random
import ssl
broker = "97825dc4ffeb4ef69020a34b200834d7.s1.eu.hivemq.cloud"
port = 8883
topic = "iot/demo/sensor"
username = "hivemq.webclient.1772167962865"
password = "C:8ieP7F1U%b2Ltr&Xw!"
client = mqtt.Client(client_id="publisher")
client.username_pw_set(username, password)
client.tls_set(tls_version=ssl.PROTOCOL_TLS)
client.connect(broker, port)
while True:
    temprature = round(random.uniform(20.0, 30.0), 2)
    humidity = round(random.uniform(30.0, 70.0), 2)
    payload = f"Temprature: {temprature:.2f} C, Humidity: {humidity:.2f}%"
    client.publish(topic, payload)
    print(f" Published: {payload} + {topic}")
    time.sleep(5)
~~~

### OUTPUT SCREENSHOTS

![041413ed-5e29-4817-ae85-f3037b11053e](https://github.com/user-attachments/assets/2eebb8bc-a7fd-49b2-a416-9bb814fd1ca9)

<img width="1918" height="878" alt="555786796-0fc8f28c-5690-4c20-9b5b-56879b508edb" src="https://github.com/user-attachments/assets/bf5f8547-c64a-4b2b-a569-95d0c9dffa84" />

## Results

Data was successfully transmitted from Python to HiveMQ Cloud and verified using the MQTT Web Client.



 
  
