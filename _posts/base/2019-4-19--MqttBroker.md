---
layout: post
title: Mqtt broker and client
tag : mqtt
category : Mqtt
---

## Intro
Environment: Ubuntu
Client lanaguage: Python
Server: Mosquitto



## Broker

**install:**
``` bash
sudo apt-get install mosquitto
```
**start service:**
``` bash
mosquitto
```
**Set Up User/Password:** 
1. 建立一個帳號名為 User
``` bash
sudo mosquitto_passwd -c /etc/mosquitto/passwd User
```
2. 執行指令後輸入密碼
``` bash
Password: xxxx
Reenter password: xxxx
```
3. 在 /etc/mosquitto/mosquitto.conf 檔案最後加上
``` bash
allow_anonymous false
password_file /etc/mosquitto/passwd
```

**start service with config:**
``` bash
mosquitto -c /etc/mosquitto/mosquitto.conf
```


## Client (command test)

**install:**
``` bash
sudo apt-get install mosquitto-clients
```
**subscribe:**
``` bash
 mosquitto_sub -t "topicName"
```
**subscribe with password:**
``` bash
 mosquitto_sub -h "localhost" -p "port" -v -d -t "topicName" -u "username" -P "password"
```

**publish:**
``` bash
 mosquitto_pub -m "message from mosquitto_pub client" -t "topicName"
```
**publish  with password:**
``` bash
mosquitto_pub -h "localhost" -p "port"  -t "topicName" -m "test message" -u "username" -P "password"
```


## Client (python)

**install:**
```
pip install paho-mqtt
```
**SubscribeTest.py :**
``` python=
import paho.mqtt.client as mqtt


def on_connect(client, userdata, flags, rc):
    print("Connected with result code "+str(rc))
    client.subscribe("test")

def on_message(client, userdata, msg):
    print(msg.topic+" "+str(msg.payload))

client = mqtt.Client()
client.on_connect = on_connect
client.on_message = on_message
client.username_pw_set(username="px2",password="px2")
client.connect("localhost", 1883, 60)
client.loop_forever()
```

**PublisherTest.py :**
``` python=
# Publisher.py
import paho.mqtt.client as mqtt

_g_cst_ToMQTTTopicServerIP = "localhost"
_g_cst_ToMQTTTopicServerPort = 1883 #port
_g_cst_MQTTTopicName = "test" #TOPIC name

mqttc = mqtt.Client("python_pub")
mqttc.username_pw_set(username="px2",password="px2")
mqttc.connect(_g_cst_ToMQTTTopicServerIP, _g_cst_ToMQTTTopicServerPort)
mqttc.publish(_g_cst_MQTTTopicName, "Hello")
```
