---
description: This is today's weather forecast.
icon: sun-cloud
---

# Weather

<figure><img src="../../../../.gitbook/assets/041-weather.png" alt=""><figcaption></figcaption></figure>

The weather kernel addon allows you to check your city's weather forecast using either the Weather.com API (`weather`) or the OpenWeatherMap forecast API (`weather-old`). You can check the information about the current weather condition in your city, such as the state of the forecast, the wind speed, the humidity, and so on.

{% hint style="info" %}
`weather`: You'll need to consult [this page](https://www.ibm.com/products/environmental-intelligence-suite) to learn how to get a 15 Day weather forecast API key from Weather.com.

`weather-old`: You can get the free weather forecast API key by going to [this page](https://home.openweathermap.org/api\_keys). You'll need an OpenWeatherMap account.
{% endhint %}

The weather kernel addon provides the following properties:

* Weather: The state of the weather forecast in your city (Clear, smoke, cloudy, raining, etc.).
* Temperature: The temperature of the outside environment either in Celsius, in Kelvin, or in Fahrenheit.
* Feels like: The temperature that makes you feel like it either in Celsius, in Kelvin, or in Fahrenheit.
* Wind speed: The wind speed in the outside environment in meters per second.
* Wind direction: The direction of the wind in degrees
* Pressure: The pressure in Hectopascals
* Humidity: The weather humidity in percentage

{% hint style="info" %}
We recommend the Weather.com implementation using the weather command. The syntax is a bit different, because it requires that you know the latitude and the longitude of your area. If you want to get this information, you can use the `-list` switch, providing it your city name and your API key.
{% endhint %}

In addition to that, you can consult the weather in an interactive TUI by calling the `weather` command with just the `-tui` switch. Please note that this is not supported for the OpenWeatherMap version of the `weather` command.
