---
layout: post
title: Android App bundle實作
tag : build
category : Android
---

### 使用情境

* 手機圖片過多且支援每個解析度的圖，APK時會載到不必要的解析度的圖，因此導致apk檔案過大
* 不同硬體的手機使用不同程式碼時有可以利用此方式減少apk大小
* 有時候手機只需安裝某種特定語言的語言包

### 使用App bundle build aab

使用3.2.1最新的Android studio版本
並選擇Android App Bundle

![](https://lh6.googleusercontent.com/Yz3x9cut92AtOmcidAowZw0VyK7s0KvBzMIpj-d5NNlchwh8A15uN6y5N51zgFLCaVDFBc8nmoFH7Z-RbATZXPUZ7npO4r9WrrlIRRDZ)
![](https://lh5.googleusercontent.com/JTK6NXvo6jQe4VD2QdgZL3IHNfxYk6Ju6rs1udTumVtprLmBOhQfJdtqu9GuIHMuwRtfpoHMV3QUlqZTdhiFyplRyzBFttO-aEsOxmkg)

### 結果

會產生附檔名為aab的檔案

![](https://lh3.googleusercontent.com/UarFtDnXF2r0l4OGP240PXNn2GkjfZ7pKuDCR0QDSn6xyfJIOXLAk4J_V11hZRCUf7Wcl5E1InKpPAfbor7TpPHLIFvJOGCiTiO6TwZ8b5Ylu6ItygTZiwklwvciyQpt2DsZKC6DXaY)

### 修改輸出檔案名稱

在gradle的 defaultConfig中加入

``` java
setArchivesBaseName("apk name")
```

遺憾的是目前並不支援flavors輸出不同名稱

### 將aab運行安裝在手機上

1. 至 https://github.com/google/bundletool 下載最新版本的tool 

2. 與aab放在同個路徑下(或是修改下方指令路徑) 

3. 執行下方指令，會產生apks 

    ``` java
    java -jar bundletool-all-0.6.2.jar build-apks --bundle=app.aab --output=my_app.apks --ks=../../../../chtSmartGreenParkKey.jks --ks-pass=pass:iocapp --ks-key-alias=ioc --key-pass=pass:iocapp
    ```

4. 接上手機後執行下方指令安裝至手機

    ``` java
    java -jar bundletool-all-0.6.2.jar install-apks --apks=my_app.apks --adb=C:\Users\aslanyan\AppData\Local\Android\sdk\platform-tools\adb.exe
    ```

### gradle選擇那些需要被分離

``` java
android {
    // Instead, use the bundle block to control which types of configuration APKs
    // you want your app bundle to support.
    bundle {
        language {
            enableSplit = false
        }
        density {
            // This property is set to true by default.
            enableSplit = true
        }
        abi {
            // This property is set to true by default.
            enableSplit = false
        }
    }
}
```

### 優缺點

* 優點：減少APP大小
* 缺點：難以直接測試

### 可能遇到的問題

如果Android studio目前有連到裝置，可能會造成指令無法成功安裝的問題，可以先拔掉裝置，執行
``` java
adb kill-sever
```
再重新連上裝置執行
``` java
adb start-server
```
來連上

