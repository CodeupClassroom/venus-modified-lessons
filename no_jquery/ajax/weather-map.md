## OpenWeatherMap API

In this section we will work with an API from [OpenWeatherMap](http://openweathermap.org). This API offers a variety of information for free, including [current weather data](http://openweathermap.org/current), [5 day forecast information](https://openweathermap.org/forecast5), and can even be used to upload data from your own [weather station](http://openweathermap.org/stations).

The first thing to do is to [sign up](http://openweathermap.org/register) with OpenWeatherMap. Once you register, you will be given an [API Key](https://home.openweathermap.org/api_keys) it might look like `467fce2fbe4d967cacd8c8886374905a`. This ID is how we will identify ourselves to the API when we make our requests, the previous API key is just an example and it might not work anymore.

!!! note ""
    Most APIs will require the use of an App ID or API Key and
    will return an error if it is omitted or an invalid key is sent.
    OpenWeatherMap is unique in that it... does not. Even though OpenWeatherMap
    will respond with correct data without providing an APPID, it is still good
    practice to include it, in case their requirements change or rate limits
    start being enforced.
    
## Protecting API Keys

A good practice as developers to avoid pushing confidential information like API Keys is to create a separate `keys.js` file, add variables and a values to this file that represent the string values of those API keys. Always making sure that this file is added to a `.gitignore` file that way it never get pushed to a public repository. 

```js
const MAPBOX_KEY = 'pk.ASDQWE@#$';
const OPEN_WEATHER_APPID = "2344ZXCERWETA";
const LINKEDIN_KEY = "34234ASDAD45";
```

We use `const` to avoid this variables to be reassigned, read more about constants [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) we will talk about them more in detail later in the course.

## Pulling San Antonio Weather

We can query for San Antonio weather data by sending a fetch `GET` request and including `q: San Antonio, US` as a query parameter. Note: when you start exploring a new API, it is important to learn what data is sent back from each request. We can explore this information by using `console.log()` inside the `.then()` handler.


```js
// prepare the query parameters to identify us to openweather 
// and give it our weather query
let queryParams = new URLSearchParams({
    APPID: OPEN_WEATHER_APPID,
    q: "San Antonio, US"
});
// concat the query parameters onto the URL
fetch("http://api.openweathermap.org/data/2.5/weather?" + queryParams, {
    method: "GET"
    }
).then(async function(response) {
    // use await to wait for the json data and then do something with it
    console.log(await response.json());
});
```

When we log the data, we will see it is a JSON object like the following:

```json
{
    "coord": {
        "lon": -98.49,
        "lat": 29.42
    },
    "sys": {
        "type": 3,
        "id": 60540,
        "message": 0.008,
        "country": "United States of America",
        "sunrise": 1425473734,
        "sunset": 1425515734
    },
    "weather": [{
        "id": 804,
        "main": "Clouds",
        "description": "overcast clouds",
        "icon": "04n"
    }],
    "base": "cmc stations",
    "main": {
        "temp": 287.55,
        "humidity": 95,
        "pressure": 1013.102,
        "temp_min": 287.55,
        "temp_max": 287.55
    },
    "wind": {
        "speed": 2.12,
        "deg": 178.5
    },
    "rain": {
        "3h": 0
    },
    "clouds": {
        "all": 92
    },
    "dt": 1425430669,
    "id": 4726206,
    "name": "San Antonio",
    "cod": 200
}
```
Much of this information seems relatively obvious, such as `lat`, `lon`, `country`, etc. Some of the other pieces are less clear. If any part of an API request is confusing, remember to always go back to the [documentation](http://openweathermap.org/weather-data#current). Notice that the temperature is expressed in [Kelvin](http://en.wikipedia.org/wiki/Kelvin). We can change that by passing yet another [parameter](http://openweathermap.org/current#other) to the request.

```js
// prepare the query parameters to identify us to openweather 
// and give it our weather query
let queryParams = new URLSearchParams({
    APPID: OPEN_WEATHER_APPID,
    q: "San Antonio, US",
    units: "imperial"
});
// concat the query parameters onto the URL
fetch("http://api.openweathermap.org/data/2.5/weather?" + queryParams, {
    method: "GET"
    }
).then(async function(response) {
    // use await to wait for the json data and then do something with it
    console.log(await response.json());
});
```

Our temperature should now be coming back in degrees fahrenheit. Let's look at other ways we can request this information. In particular, from the data we can see that San Antonio has an `id` of "4726206". We can use that to look up weather information as well:

```js
let queryParams = new URLSearchParams({
    APPID: OPEN_WEATHER_APPID,
    id:     4726206,
    units: "imperial"
});
// concat the query parameters onto the URL
fetch("http://api.openweathermap.org/data/2.5/weather?" + queryParams, {
    method: "GET"
    }
).then(async function(response) {
    // use await to wait for the json data and then do something with it
    console.log(await response.json());
});
```

A list of city ID `city.list.json.gz` can be downloaded [here](http://bulk.openweathermap.org/sample)

We will get the same information back as before. We can also look information by its latitude and longitude:

```js
let queryParams = new URLSearchParams({
    APPID: OPEN_WEATHER_APPID,
    lat:    29.423017,
    lon:   -98.48527,
    units: "imperial"
});
// concat the query parameters onto the URL
fetch("http://api.openweathermap.org/data/2.5/weather?" + queryParams, {
    method: "GET"
    }
).then(async function(response) {
    // use await to wait for the json data and then do something with it
    console.log(await response.json());
});
```

Latitude and longitude have the advantage that we can look up weather information for any place on earth, even if we do not know its name or id. Let's look at how we could get [*forecast* information](http://openweathermap.org/forecast) using those same coordinates:

```js
let queryParams = new URLSearchParams({
    APPID: OPEN_WEATHER_APPID,
    lat:    29.423017,
    lon:   -98.48527,
    units: "imperial"
});
// concat the query parameters onto the URL
fetch("http://api.openweathermap.org/data/2.5/forecast?" + queryParams, {
    method: "GET"
    }
).then(async function(response) {
    // use await to wait for the json data and then do something with it
    console.log(await response.json());
});
```

This time we have changed what URL we are sending our request to. The URL you send API requests to is often called its *endpoint*. We have separate endpoints for each type of API request we can send.

Like we did when we first started exploring the current weather conditions, be sure to take a look at the data our request [returns](https://openweathermap.org/weather-data#5days).

OpenWeatherMap provides a third example of an *endpoint* we could send our request to - let's see how we could use this [One Call API](https://openweathermap.org/api/one-call-api) to, as OpenWeather puts it, "get all your essential weather data" for a location:

```js
let queryParams = new URLSearchParams({
    APPID: OPEN_WEATHER_APPID,
    lat:    29.423017,
    lon:   -98.48527,
    units: "imperial"
});
// concat the query parameters onto the URL
fetch("http://api.openweathermap.org/data/2.5/onecall?" + queryParams, {
    method: "GET"
    }
).then(async function(response) {
    // use await to wait for the json data and then do something with it
    const data = await response.json();
    console.log('The entire response:', data);
    console.log('Diving in - here is current information: ', data.current);
    console.log('A step further - information for tomorrow: ', data.daily[1]);
});
```

Pretty useful for our purposes! The [One Call API documentation](https://openweathermap.org/api/one-call-api#parameter) lists all the data that can come back in this response. Again, be sure to familiarize yourself with the data that is returned from your request.

## Exercise

1. Create a new HTML file called `weather_map.html`.
1. As you complete each step, commit your changes and push them to GitHub.
1. Using HTML, CSS, jQuery, AJAX, and the OpenWeatherMap API, start by showing the current conditions for city you live in on your page.
1. Update your layout and AJAX request(s) to display a five day forecast for the city you live in that looks like the screenshot below.
1. If you want to add the icons the URLs for OpenWeatherMap's icons are formatted like: `http://openweathermap.org/img/w/[icon].png` where `[icon]` comes from the API response.
1. Refer to your Mapbox API exercise. Recreate the map below your 5 day forecast. Read through the documentation for the Mapbox API and figure out how to allow the user to drop a pin on any place on the map. Once the user drops a pin, grab its coordinates and feed those into your OpenWeatherMap API. Update the five-day forecast with the information from those coordinates (you should also get rid of your input boxes at this point).
1. Add a Mapbox text input to search by location and have the forecast update when a new location is searched.
1. As a **bonus** make sure you can update the marker's position to the new search result.

See a couple of examples here:
    
   ![5 Day Forecast Map](/img/5day-forecast-map.png)
   ![5 Day Forecast Map Demo](/img/5day-demo-map.gif)