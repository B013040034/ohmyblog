---
layout: post
title: 建立自己的函式庫-Maven Server
tag : Hackmd
category : Android
---

### From Hackmd : <https://hackmd.io/oHrZ-IOTTl6E6cALrk4VhQ>
---
## What's Maven Server

在區網內架設一個Maven Server來提供開發人員能簡易使用libraries

---

## 安裝注意事項

* 使用內網請 *關閉防火牆* 或是 *開啟8081 port*
* Maven預設帳號 = 'admin'
* Maven預設密碼 = 'admin123'

---

## upload your .aar to Server

三步驟簡易上手
1.  put the maven.gradle in the folder outside the project 

	<https://github.com/B013040034/ohmyblog/blob/master/data/maven.gradle>

2.  in sdk's build.gradle bottom add 

	``` gradle
	dependencies {

	}
	apply from: '/../../maven.gradle'
	```

3. click uploadArchives command
	PS:可以到以下網址確認是否成功上傳
	http://192.168.X.XX:8081/nexus/content/repositories/your_library_server_name/

---

## 注意事項
1. maven.gradle會調整輸出的aar名稱 (gradle不吃dash)
    由 XXXX-release.aar -> XXXX.aar
2. 發布版號來源是sdk的gradle中的 versionName

---
    
## import your .aar from Server
兩步驟導入aar
1. 在要使用的專案的 build.gradle中加入

	``` gradle
	allprojects {
	    repositories {
	        maven{ url 'http://192.168.X.XX:8081/nexus/content/repositories/your_library_server_name/'}
	    }
	}
	```

2. 在app層級的 build.gradle -> 

	``` gradle
	dependencies {
	    implementation 'com.your_library_server_name:sdkName:versionName'
	}
	```
	For Example

	``` gradle
	dependencies {
	implementation 'com.kingwaytek:networkInfoCollectionSdk:1.0.0'
	}
	```
