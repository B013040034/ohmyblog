---
layout: post
title: 圖片拖曳
tag : image
category : Kotlin
---

### 設定觸控時監聽

``` java
imageView.setOnTouchListener(imgListener);
```

### 計算出 actionBar長度以供設定圖片位置

> Hint : 可將 `actionBarHeight` 存在全域

``` java
public void getActionBarHeight() {
    TypedValue tv = new TypedValue();
    if (getTheme().resolveAttribute(android.R.attr.actionBarSize, tv, true))
        actionBarHeight = TypedValue.complexToDimensionPixelSize(tv.data,getResources().getDisplayMetrics());
}
```

### 監聽拖曳動作並重設位置

``` java
private View.OnTouchListener imgListener = new View.OnTouchListener() {
    private float lengthXFromLeftToTouchEvent, lengthYFromTopToTouchEvent;
    @Override
    public boolean onTouch(View v, MotionEvent event) {
        switch (event.getAction()) {  
            case MotionEvent.ACTION_DOWN:
                //計算點下去的位置到圖片左上角位置的距離
                lengthXFromLeftToTouchEvent = event.getRawX() - imageView.getX();
                lengthYFromTopToTouchEvent = event.getRawY() - imageView.getY() - actionBarHeight;
                break;
                
            case MotionEvent.ACTION_MOVE:
                mx = (int) (event.getRawX() - lengthXFromLeftToTouchEvent);
                my = (int) (event.getRawY() - lengthYFromTopToTouchEvent - actionBarHeight);
                v.layout(mx, my, mx + v.getWidth(), my + v.getHeight());
                break;
                
            case MotionEvent.ACTION_UP:
                imageView.layout(mx, my, mx + imageView.getWidth(), my + imageView.getHeight());
                break;
        }
        return true;
    }
};
```

### 離開app時儲存資訊

``` java
@Override
protected void onPause() {
    super.onPause();
    sharedpreferencesEditor.saveInt("lengthXFromLeftToTouchEvent", mx);
    sharedpreferencesEditor.saveInt("lengthYFromTopToTouchEvent", my);
}
```

### 進入app時重設位置
``` java
public void initView() {
    imageView = findViewById(R.id.imageView);
    mx = sharedpreferencesEditor.readInt("lengthXFromLeftToTouchEvent");
    my = sharedpreferencesEditor.readInt("lengthYFromTopToTouchEvent");
    imageView.layout(mx, my, mx + imageView.getWidth(), my + imageView.getHeight());
}
```

### 補充

這裡提供兩種設定圖片位置的方法，但是不能混用
``` java
v.layout(mx, my, mx + v.getWidth(), my + v.getHeight());
```

``` java
v.setX(mx);
v.setY(mx);
```
