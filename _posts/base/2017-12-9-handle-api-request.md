---
layout: post
title: Retrofit sample
tag : Hackmd
category : Android
---

### From Hackmd : <https://hackmd.io/jaW43Q3OTkCqnYJEU-NaWg?both>

## Outline
* What is Retrofit?
* Before start
* Http libraries
* Before start
* Http libraries
* OKhttp
* Retrofit
* Q&A

---

## What is Retrofit?

Retrofit turns your HTTP API into a Java interface.

---

## Before start
* RESTful API
* JSON
* Gson
* Annotation

----

## RESTful API
rule or concept
* 每一個URI代表一種資源
* 客戶端通過四個HTTP動詞，對服務器端資源進行操作
```java
@GET("users/list")
@GET("user/{userid}")
```

----

## JSON & Gson
``` java
public static <T> T fromJson(String json, Class<T> klass) {
    return new Gson().fromJson(json, klass);
}
```
``` java
public static <T> List<T> fromJsonArray(String json, Class<T[]> klass) {
    T[] objects = new Gson().fromJson(json, klass);
    return objects == null ? new ArrayList<>() 
        : new ArrayList<T>(Arrays.asList(objects));
}
```
``` java
public static String toJson(Object object) {
    return new GsonBuilder()
            .create()
            .toJson(object);
}
``` 
``` java
public String toJson() {
    return new Gson().toJson(this);
}
```

----

## Annotation
_Usage_
1. Dagger
2. Retrofit
3. Gson
4. DBflow
5. Butterknife

----

## How to build Annotation 

```java
@Bind(R.id.text)  
TextView text;
```
1. build a Annotation class
2. write a Singleton class to handle TODO
3. inject the class when activity oncreate

preference : https://b013040034.github.io/ohmyblog/android/2018/02/06/annotation.html

---

## Http libraries
* HttpClient
* OkHttp
* Volley
* _retrofit_

---

## OKHttp

----

## Find the bug _1_

``` java
protected void onCreate(@Nullable Bundle savedInstanceState) {  
    super.onCreate(savedInstanceState);  
    setContentView(R.layout.activity_main);  
    try {  
        OkHttpClient client = new OkHttpClient();  
        String url = MessageFormat.format(ApiClient.Postal, 408);  
        final Request request = new Request.Builder()  
                .url(ApiClient.BASE_URL + url)  
                .build();  
        client.newCall(request).execute().body().string();  
    } catch (IOException e) {  
        e.printStackTrace();  
    }  
}
```

----

## Error Msg _1_

android.os.NetworkOnMainThreadException

----

## Resolve _1_

1. build a Singleton
2. init okhttp in constuctor
3. write a method to handle request

``` java
public void getData(String url, Callback callback) {  
    final Request request = new Request.Builder()  
            .url(BASE_URL + url)  
            .build();  
   client.newCall(request).enqueue(callback);  
}

```

----

## Callback -> Find the bug _2_

``` java
apiClient.getDataWithCallBack(url, new Callback() {  
    @Override  
    public void onFailure(Call call, final IOException e) { 
        text.setText(e.getMessage());  
   }  
  
    @Override  
    public void onResponse(Call call, Response response) throws IOException {  
        final String resStr = response.body().string();  
        text.setText(resStr);  
   }  
});  
```

----

## Error Msg _2_

Only the original thread that created a view hierarchy can touch its views.

----

## AsyncTask
``` java
new AsyncTask<String, Void, String>() {  
    @Override  
    protected String doInBackground(String... urls) {  
        try {  
            return apiClient.getDataWithAsync(url);  
        } catch (IOException e) {  
            return e.getMessage();  
        }  
    }  
    @Override  
    protected void onPostExecute(String resStr) {  
        text.setText(resStr);  
    }  
};
```

----

## Okhttp Conclusion
1. you have to make request in work thread or use callback
2. you have to change to ui thread after callback
--> **thread safe**
--> Encapsulation 封裝

----

## Encapsulation

### Volley vs Retrofit
1. Gson
Get : call -> get object
Post : make object -> call
2. Restful Api
easy way to set url and data


----

## What I expect

Lazy Lazy Lazy Lazy

----

1. No need to custom request
  -> _No need to encapsulation_
2. Api class looks like _Api Document_
  -> easy to _find & add & fix_ api
3. get object and post object

---

## Retrofit

----

## dependencies
``` java
dependencies {
   compile 'com.squareup.retrofit2:retrofit:2.3.0'
   compile 'com.squareup.retrofit2:converter-gson:2.0.2'
}
```

----

## Website
**[https://github.com/square/retrofit](https://github.com/square/retrofit).**

----

## Singleton
``` java
public class ApiClient {
    public static final String BASE_URL = "https://api/";
    private static Retrofit retrofit = null;

    public static Retrofit getClient() {
        if (retrofit==null) {
            retrofit = new Retrofit.Builder()
                    .baseUrl(BASE_URL)
                    .addConverterFactory(GsonConverterFactory.create())
                    .build();
        }
        return retrofit;
    }
}
```

----

## Api : Get
``` java
public interface ApiService {
    @GET("weather/{lat}/{lng}")
    Call<WeatherData> getWeather(@Path("lat") String lat, @Path("lng") String lng);
}
```

----

## Api : Get (Volley and okHttp)
``` java
public static final String coordinate = "weather/{0}/{1}";
String url = MessageFormat.format(ApiClient.coordinate, lat, lng);
```

----

## Convert json to Object

1. Website
http://www.jsonschema2pojo.org/
2. Plugins

![](https://i.imgur.com/Kxr22hE.png)


----

### Object
``` java
public class WeatherData implements Serializable {
    @SerializedName("location")
    public String location;
    @SerializedName("temp")
    public float temperature;
}
```
You can use *Kotlin + paracle* instead

----

## Get data
``` java
public void getWeatherData() {
        Retrofit retrofit = ApiClient.getClient();
        apiService = retrofit.create(ApiService.class);
        Call<WeatherData> data = apiService.getWeather("24","120");
        data.enqueue(new Callback<WeatherData>() {
            @Override
            public void onResponse(Call<WeatherData> call, Response<WeatherData> response) {
                WeatherData weatherData = response.body();
            }
            @Override
            public void onFailure(Call<WeatherData> call, Throwable t) {
             
            }
        });
    }
```

----

## Api : Post
``` java
public interface ApiService {
    @POST("AccessToken")  
    Call<AccessToken> getTokenDebug(@Body AccessTokenInput tokenInput);

}
```

----

## Post data

``` java
Call<AccessToken> dataDebug = apiService.getTokenDebug(accessToken);
dataDebug.enqueue(new Callback<AccessToken>() {  
    @Override  
    public void onResponse(Call<AccessToken> call, Response<AccessToken> response) {  
        AccessToken accessToken = response.body();  
        caculateResult(accessToken, textResultDebug);   
    }  
    @Override  
    public void onFailure(Call<AccessToken> call, Throwable t) {  
        textResultDebug.setText(t.getMessage());  
    }  
});

```

----

## Api : Post (Volley)

* StringRequest : with param
* JsonObjectRequest : with json

----

## Query String
``` java
@GET("news")
Call<News> getItem(@Query("type") String type);

@GET("news")
Call<News> getItem(@QueryMap Map<String, String> map);
```

----

## Post with field
``` java
@FormUrlEncoded
@POST("news")
Call<User> postItem(@Field("type") String type);

@FormUrlEncoded
@POST("news")
Call<User> postItem(@FieldMap Map<String, String> map);
```

----

## Other features

* 簽名 : Signature
* 標頭 : Headers
* 檔案 : Multipart

----

## signature
<https://github.com/square/okhttp/wiki/Interceptors>

## Multipart

<https://chenkaihua.com/2016/04/02/retrofit2-upload-multipart-files/>

----

## perference

<http://square.github.io/retrofit/>
<https://ihower.tw/blog/archives/1542>
<https://itw01.com/V33WE9D.html>
<https://bng86.gitbooks.io/android-third-party-/content/retrofit.html>

---
