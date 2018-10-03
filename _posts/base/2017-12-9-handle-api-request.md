---
layout: post
title: Retrofit sample
tag : Retrofit Gson
category : Android
---

## handle api request by Retrofit
### dependencies
{% highlight java %}
dependencies {
   compile 'com.squareup.retrofit2:retrofit:2.3.0'
   compile 'com.squareup.retrofit2:converter-gson:2.0.2'
}
{% endhighlight %}
<br>

### Website
**[https://github.com/square/retrofit](https://github.com/square/retrofit).**  
<br>

### New instance
{% highlight java %}
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
{% endhighlight %}
<br>

### Set path
{% highlight java %}
public interface ApiService {
    @GET("weather/{lat}/{lng}")
    Call<WeatherData> getWeather(@Path("lat") String lat, @Path("lng") String lng);
}
{% endhighlight %}
<br>

### Convert json to Object

#### maybe this your sample json 

{% highlight java %}
{
    "temp": 16.3,
    "location": "臺北",
}
{% endhighlight %}
<br>

> Hint : 
> **[paste the json to the website to get code automatic](http://www.jsonschema2pojo.org/).**
> And remember to `implements Serializable`

#### Result
{% highlight java %}
public class WeatherData implements Serializable {
    @SerializedName("location")
    public String location;
    @SerializedName("temp")
    public float temperature;
}
{% endhighlight %}
<br>

### Get data
{% highlight java %}
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
{% endhighlight %}
<br>