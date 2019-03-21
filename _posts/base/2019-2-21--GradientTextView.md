---
layout: post
title: 實作漸層 TextView 
tag : widget
category : Android
---

### 設置顏色漸層的 TextView

``` kotlin
import android.content.Context;
import android.graphics.LinearGradient;
import android.graphics.Shader;
import android.support.v4.content.ContextCompat;
import android.util.AttributeSet;

public class GradientTextView extends android.support.v7.widget.AppCompatTextView {
    public GradientTextView(Context context) {
        super(context);
    }

    public GradientTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public GradientTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
        super.onLayout(changed, left, top, right, bottom);

        //Setting the gradient if layout is changed
        if (changed) {
            getPaint().setShader(new LinearGradient(getWidth()/2, 0, getWidth()/2, getHeight(),
                    ContextCompat.getColor(getContext(), R.color.textColor),
                    ContextCompat.getColor(getContext(), R.color.textColorEnd),
                    Shader.TileMode.CLAMP));
        }
    }
}
```

### LinearGradient參數
LinearGradient(漸層 **起**點**X**值, 漸層**起**點**Y**值, 漸層**終**點**X**值, 漸層**終**點**Y**值, 
起始顏色, 結束顏色, shader type)
* [shader可以參考](https://developer.android.com/reference/android/graphics/Shader.TileMode.html)


