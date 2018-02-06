---
layout: post
title: Annotation Sample
tag : annotation
category : Android
---

# 目標：建立類似Butterkinfe的Annotation

## Create a Annotation class 
{% highlight java %}
@Retention(CLASS) @Target(FIELD)
public @interface Bind {
    @IdRes int value();
}
{% endhighlight %}
<br>

## write a Singleton class to handle TODO
{% highlight java %}
public class AnnotationBind {
    private static AnnotationBind annotationBind;
    public static AnnotationBind instance() {
        synchronized (AnnotationBind.class){
            if(annotationBind == null){
                annotationBind = new AnnotationBind();
            } return annotationBind;
        }
    }

    public void inject(Object o){
        Class aClass = o.getClass();
        Field[] declaredFields = aClass.getDeclaredFields();
        for (Field field:declaredFields) {
            if(field.isAnnotationPresent(Bind.class)) {
                Bind annotation = field.getAnnotation(Bind.class);
                try {
                    field.setAccessible(true);
                    field.set(o, ((Activity)o).findViewById(annotation.value()));
                } catch (IllegalAccessException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
{% endhighlight %}


## inject the class when activity oncreate
{% highlight java %}
    @Bind(R.id.text)
    TextView before;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        AnnotationBind.instance().inject(this);

        before.setText("dddd");
    }
{% endhighlight %}