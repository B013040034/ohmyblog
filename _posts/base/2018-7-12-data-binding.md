---
layout: post
title: DataBinding Intro
tag : Hackmd
category : Android
---
### From Hackmd : <https://hackmd.io/FvbiZ6c9TLCS11_xEUM37w?both>

## outline

* Basic Setting
* Case 1 : Re-use
* Case 2 : Listener
* Case 3 : String concat
* Case 4 : hide widget

---

## Basic Setting

----

## Gradle
``` java
android { 
    ....dataBinding {
        enabled = true
    }  
}
```

----

## xml : activity_info.xml

```xml
<layout> 
    <LinearLayout 

    </LinearLayout>  
</layout>
```

----

## Bind

``` java
ActivityInfoBinding mBinding 
= DataBindingUtil.setContentView(this, R.layout.activity_info);
```

----

## Usage

``` java
mBinding.XXXX
```

---

## Binding Data

----

## Object (Kotlin)

``` kotlin
@SuppressLint("ParcelCreator")  
@Parcelize  
class User(  
    @SerializedName("name")  
    var mName: String?
): Parcelable
```

----

## xml
```xml
<layout> 
    <data>
        <variable name="user" type="com.example.User"/>  
    </data>

    <LinearLayout 
        
    </LinearLayout>  
</layout>
```

----

## bind
``` java
mBinding.setUser(getUser());
```

----

## Usage

``` xml
<TextView   
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="@{user.MName}"/>

```

---

## Case 1 : Re-use

----

## idea
![](https://i.imgur.com/SFf0RwD.png)


----

## Just do it
* LinearLayout(horizontal)
    * LinearLayout(vertical)
        * ImageView
            * android:src="@drawable/......"
        * TextView
            * android:text="@string/...."
    * LinearLayout(vertical)
        * ImageView
            * android:src="@drawable/......"
        * TextView
            * android:text="@string/...."
    * LinearLayout(vertical)
        * ImageView
            * android:src="@drawable/......"
        * TextView
            * android:text="@string/...."
    * LinearLayout(vertical)
        * ImageView
            * android:src="@drawable/......"
        * TextView
            * android:text="@string/...."

----

## 重複性太高

----

## include xml (或是使用merge)
* LinearLayout(vertical)
    * ImageView
    * TextView

----

## original xml
* LinearLayout(horizontal)
    * include
        * android:id="@+id/...."
    * include
        * android:id="@+id/...."
    * include
        * android:id="@+id/...."
    * include
        * android:id="@+id/...."

----

## activity have to handle the change
```java

mBinding.layoutAb.image.setImageDrawable(R.drawable.xxx);  
mBinding.layoutAb.textView.setText(getString(R.string.xxx));
mBinding.layoutCd.image.setImageDrawable(R.drawable.xxx);  
mBinding.layoutCd.textView.setText(getString(R.string.xxx));
mBinding.layoutEf.image.setImageDrawable(R.drawable.xxx);  
mBinding.layoutEf.textView.setText(getString(R.string.xxx));
mBinding.layoutGh.image.setImageDrawable(R.drawable.xxx);  
mBinding.layoutGh.textView.setText(getString(R.string.xxx));

```

----

## another way
### inclucde xml
``` xml
<data>  
    <variable name="text" type="String"/>  
    <variable name="icon" type="android.graphics.drawable.Drawable"/>
</data>
```
TextView :
``` xml
android:text="@{text}"
```
ImageView :
``` xml
android:src="@{icon}"
```

----

## orginal xml

```xml
app:icon="@{@drawable/myDrawable}"
app:text="@{@string/myText}"
```

----

## result
* LinearLayout(horizontal)
    * include
        * android:id="@+id/...."
        * app:icon="@{@drawable/myDrawable}"
        * app:text="@{@string/myText}"

----

## Timing

---

## Case 2 : Listener

----

## xml
* LinearLayout(horizontal)
    * include
        * android:id="@+id/layoutInclude1
    * include
        * android:id="@+id/layoutInclude2
    * include
        * android:id="@+id/layoutInclude3
    * include
        * android:id="@+id/layoutInclude4

----

## setOnClickListener
**<font color="red">X</font>**
``` java
mBinding.layoutInclude1.setOnClickListener
```
**<font color="red">O</font>**
``` java
findViewById(R.id.layoutInclude1).setOnClickListener();
```

----

## another way
### include xml
1.
``` xml
<variable name="handler" type="XXXXXX.MainActivity"/>
```
2.
``` xml
<variable name="handler" type="yourHandler"/>
```
android:onClick="@{handler::onFeaturesClick}"

----

## result in Activity
``` java
mBinding.layoutInclude1.setHandler(this);
```
``` java
public void onFeaturesClick(View view){}
```

----

## advantage
Collect all handler together

---

## Case 3 : String concat

----

## Example

地址：XXXXXX

----

## First way
``` xml
<TextView
    android:text="@string/address"/>
<TextView
    android:text="@{info.address}"/>
```

----

## second way
``` xml
<TextView
    android:id="@+id/textAddress"
    android:text=""/>
```
```java
mBinding.textAddress.setText(
getString(R.string.address) + info.address);
```

----

## third way
``` xml
<TextView
    android:id="@+id/textAddress"
    android:text="@{@string/address + info.address}"/>
```

----

## another Example
output:
2018/2/15 16:48

```java
mBinding.textTime.setText(
info.date + " " + info.time);
```

----

## you can write
``` xml
<TextView
    android:text="@{info.date + ` ` + info.time}"/>
```

----

## little conclusion
1.顯示邏輯到底要寫在UI中還是必須拉到acitvity中
2.都寫在UI中會不會造成後面閱讀的人的困擾

---

## Case 4 : hide widget

----

## solution
``` java
android:visibility="@{isVisible ? View.VISIBLE : View.GONE}"
```

``` xml
app:isVisible="@{true}"
```







