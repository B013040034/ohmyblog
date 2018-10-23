---
layout: post
title: RelativeLayout sample
tag : layout
category : Android
---

``` java

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp"
    tools:context="com.chunghwa.androidtraining.MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        android:gravity="center"
        android:text="內建儲存空間"
        android:textSize="18dp" />

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignBottom="@+id/textView"
        android:layout_alignParentEnd="true"
        android:layout_alignParentRight="true"
        android:gravity="center"
        android:text="詳細資料 >"
        android:textSize="16dp" />

    <TextView
        android:id="@+id/textView3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_alignTop="@+id/imageView"
        android:gravity="center"
        android:text="總共："
        android:textSize="16dp" />

    <TextView
        android:id="@+id/textView4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/textView3"
        android:gravity="center"
        android:text="64.00 GB"
        android:textSize="16dp" />

    <TextView
        android:id="@+id/textView5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/textView4"
        android:layout_marginTop="8dp"
        android:gravity="center"
        android:text="剩餘："
        android:textSize="16dp" />

    <TextView
        android:id="@+id/textView6"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/textView5"
        android:gravity="center"
        android:text="29.87 GB"
        android:textSize="16dp" />

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentEnd="true"
        android:layout_alignParentRight="true"
        android:layout_below="@+id/textView2"
        android:layout_marginEnd="32dp"
        android:layout_marginRight="32dp"
        android:layout_marginTop="31dp"
        android:src="@drawable/ic_launcher_background" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/imageView"
        android:layout_marginTop="16dp"
        android:orientation="horizontal"
        android:id="@+id/linear">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="16dp"
            android:layout_weight="1"
            android:orientation="vertical">

            <TextView
                android:id="@+id/textView7"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="影像"/>
            <TextView
                android:id="@+id/textView8"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="16.8 GB"/>
        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="16dp"
            android:layout_weight="1"
            android:orientation="vertical">

            <TextView
                android:id="@+id/textView9"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="音樂"/>
            <TextView
                android:id="@+id/textView10"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="2.68 GB"/>
        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="16dp"
            android:layout_weight="1"
            android:orientation="vertical">

            <TextView
                android:id="@+id/textView11"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="影片"/>
            <TextView
                android:id="@+id/textView12"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="680 MB"/>
        </LinearLayout>
    </LinearLayout>

    <!--

    <TextView
        android:id="@+id/textPass"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/textAccount"
        android:layout_weight="1"
        android:textSize="12dp"
        android:text="密碼" />

    <EditText
        android:id="@+id/editPass"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@+id/textPass"
        android:layout_weight="3"
        android:textSize="12dp" />

    <TextView
        android:id="@+id/textPassRepeat"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/textPass"
        android:layout_weight="1"
        android:text="再次確認密碼"
        android:textSize="12dp" />

    <EditText
        android:id="@+id/editPassRepeat"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="3"
        android:textSize="12dp" />

    <TextView
        android:id="@+id/textFirstName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:textSize="12dp"
        android:text="姓" />

    <EditText
        android:id="@+id/editFirstName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="3"
        android:textSize="12dp" />

    <TextView
        android:id="@+id/textLastName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:textSize="12dp"
        android:text="名" />

    <EditText
        android:id="@+id/editLastName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="3"
        android:textSize="12dp" />

    <Button
        android:id="@+id/buttonNext"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignEnd="@+id/textPassRepeat"
        android:layout_alignParentBottom="true"
        android:layout_alignRight="@+id/textPassRepeat"
        android:layout_gravity="center"
        android:layout_marginBottom="150dp"
        android:padding="8dp"
        android:text="下一步" /> -->



</RelativeLayout>


```
	