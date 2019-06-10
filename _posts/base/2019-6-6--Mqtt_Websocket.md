---
layout: post
title: Mqtt Websocket
tag : mqtt
category : Mqtt
---

## Intro
前篇提到如何簡單建置mqtt server and client  
但若是網頁前端想屆接mqtt server  
光開放tcp port是不夠的  
必須再開放websocket port  
若web使用http,mosquitto只需設置帳號密碼  
若web使用https,mosquitto則要建置tls認證機制  
此篇先已http為主題實作  

## Dependencies
```
sudo apt-get update
sudo apt-get install build-essential python quilt python-setuptools python3
sudo apt-get install openssl
sudo apt-get install libssl-dev
sudo apt-get install cmake
sudo apt-get install g++
sudo apt-get install libc-ares-dev
sudo apt-get install uuid-dev
sudo apt-get install daemon
sudo apt-get install libwebsockets-dev
```

## Broker
與前篇不同，為了開啟websocket開關必須自行下載mosquitto原始碼進行編譯執行  

**install:**
``` bash
wget http://mosquitto.org/files/source/mosquitto-1.6.1.tar.gz
tar -zxvf mosquitto-1.6.1.tar.gz
cd mosquitto-1.6.1/
```

**edit config:**
``` bash
sudo vim config.mk
```
開啟webwocket開關
```
WITH_WEBSOCKETS:=yes
```

**build:**
```
make
sudo make install
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

**edit mosquitto.conf:**
``` bash
port 1883
allow_anonymous false
password_file /etc/mosquitto/passwd
listener 9001
protocol websockets
```

**start service with config:**
``` bash
mosquitto -c /etc/mosquitto/mosquitto.conf
// -d background
```