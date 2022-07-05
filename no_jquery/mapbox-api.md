# Mapbox Maps API

Adding maps to our applications can give users a lot of information in a visual format. Maps can display locations, directions, search results, and a wide variety of data in a way that is easily consumed by our users.

For this lesson, we're going to be working with [Mapbox](https://www.mapbox.com/). Thanks to Mapbox, we can use their *application program interface* (API) to dynamically create and manipulate real world maps. Mapbox provides many APIs that offer tools for everything from navigation to autonomous vehicles and augmented reality.

### Mapbox GL JS API

Mapbox GL JS (not to be confused with the deprecated Mapbox JS) is an API to create interactive maps on a web page using WebGL. Many features are available through this API including user interaction controls, animations, custom layers, street views, overlays and much more! To better explore these features, we can refer to both the [API documentation](https://docs.mapbox.com/mapbox-gl-js/api/) and the [many examples](https://docs.mapbox.com/mapbox-gl-js/examples/) provided by Mapbox.

We will be covering several areas of Mapbox APIs in this lesson. Many other web services offer APIs with similar ease of use. Integrating existing technologies into our applications can provide users with a very rich experience.

### Generating an API Key

To interact with Mapbox APIs, we will need to [sign up](https://account.mapbox.com/auth/signup/) for an account, log in, then generate an [access token](https://account.mapbox.com/) for our application to use. A new token should be generated for each application using Mapbox. The documentation provides more information on [access tokens](https://docs.mapbox.com/help/how-mapbox-works/access-tokens/). A token will allow us to make up to 50,000 free requests per month. For more information, consult the Mapbox [pricing rates](https://www.mapbox.com/pricing/). If an application is launched to the public using the API, we might need to purchase a paid plan that allows more requests.

### Generating a Map

To quickly get a Mapbox map working, include the CDN links and boilerplate code from the [quickstart](https://docs.mapbox.com/mapbox-gl-js/guides/install/#quickstart) in your HTML file.

### Customizing the Map (setting the Style, Center, Zoom)

When creating a new map, a map style must be specified. Several [predefined map styles](https://docs.mapbox.com/api/maps/styles/#mapbox-styles) are available. The map center and zoom level have a default value but can be specified. A map center can be set by passing in the latitude and longitude coordinates of a location as an array ```[LONGITUDE_VALUE, LATITUDE_VALUE]```. Zoom levels range from 0 up to 24, with 0 being a global view and 24 being the most detailed at street level (the max zoom level depends on the location). The following example will draw a map of San Antonio:

```

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>First Mapbox Map</title>
    <!-- Mapbox CSS -->
<link href='https://api.mapbox.com/mapbox-gl-js/v2.8.2/mapbox-gl.css' rel='stylesheet' />    <!-- Custom CSS -->
    <style>
        #map {
            /* the width and height may be set to any size */
            width: 100%;
            height: 700px;
        }
    </style>
</head>
<body>

<h1>My First Mapbox Map!</h1>

<!-- The HTML element that serves as the Mapbox container -->
<div id='map'></div>

<!-- Mapbox JS -->
<script src='https://api.mapbox.com/mapbox-gl-js/v2.8.2/mapbox-gl.js'></script>

<!-- Custom JS -->
<script>
    mapboxgl.accessToken = YOUR_API_TOKEN_HERE;
    var map = new mapboxgl.Map({
        container: 'map',
        style: 'mapbox://styles/mapbox/streets-v9',
        zoom: 10,
        center: [-98.4916, 29.4252]
    });
</script>
</body>
</html>


```

After the map variable holding the Mapbox map object has been initialized, the map view may be changed using the various methods the Mapbox GL JS API provides. For example, to change the zoom level, including ```map.setZoom(4)``` after the map variable is created would change the zoom level to show most of the US. You can find a comprehensive list of these methods and properties starting [here](https://docs.mapbox.com/mapbox-gl-js/api/#map#scrollzoom). Try changing the map center, zoom, and predefined style.

### Markers

Markers, sometimes referred to as pins, are used to show specific locations on a map. We can use these to identify locations for our users. Documentation on markers can be found [here](https://docs.mapbox.com/mapbox-gl-js/api/#marker). To place a marker on a map, a marker object must be created. The methods ```.setLngLat()``` and ```.addTo()``` must be called on this marker object to set the position of the marker and add it to the map. The former method takes in the longitude and latitude coordinates for the marker while the latter takes in whatever variable refers to the map object. The following code included in the above example will place a marker on The Alamo:

```
var marker = new mapboxgl.Marker()
.setLngLat([-98.4916, 29.4260])
.addTo(map);

```

#### Popups

Popups (info windows for Google Maps) are the info boxes that appear on a map and may describe a given location. See the documentation [here](https://docs.mapbox.com/mapbox-gl-js/api/#popup). To add a simple popup to a map, a new popup object must be created and methods should be called, as with a marker, to set its position and place it on the map. Additionally, the methods ```.setHTML()``` or ```.setText()``` can be added to control the content inside an info window. The following example would add a popup over Codeup:

```
var popup = new mapboxgl.Popup()
.setLngLat([-98.489615, 29.426827])
.setHTML("<p>Codeup Rocks!</p>")
.addTo(map)

```

More often, popups are associated with a marker. If this is the case, there is no need to set the position for the popup. Comment out the popup for Codeup. To add a popup to The Alamo marker from above, add the following to your code:

```
var alamoPopup = new mapboxgl.Popup()
.setHTML("<p>Remember The Alamo!</p>")

marker.setPopup(alamoPopup)

```

Now the popup info window is toggleable by clicking on the marker it is attached to.

### Geocoder API

#### Geocoding

Mapbox provides a separate API for making geocode requests (see the documentation [here](https://docs.mapbox.com/api/search/#geocoding)). Geocoding is the process of finding the coordinates (latitude and longitude) of a specific physical address. For example, a call to the Geocoder API for Codeup's address, "600 Navarro St #350, San Antonio, TX 78205," would give us back a longitude and latitude of ```[-98.489615, 29.426827]```.  While Mapbox plugins can be used to add user interfaces that interact with this API, calls to the API can be made directly. You will be provided with methods to make these API calls more streamlined. How these methods are implemented will be covered in more detail later in the curriculum. To use these methods, make sure you create a JavaScript file called ```mapbox-geocoder-utils.js``` in your Mapbox projects containing the following JavaScript:

```js
"use strict";

/***
 * geocode is a method to search for coordinates based on a physical address and return
 * @param {string} search is the address to search for the geocoded coordinates
 * @param {string} token is your API token for MapBox
 * @returns {Promise} a promise containing the latitude and longitude as a two element array
 *
 * EXAMPLE:
 *
 *  geocode("San Antonio", API_TOKEN_HERE).then(function(results) {
 *      // do something with results
 *  })
 *
 */
function geocode(search, token) {
    var baseUrl = 'https://api.mapbox.com';
    var endPoint = '/geocoding/v5/mapbox.places/';
    return fetch(baseUrl + endPoint + encodeURIComponent(search) + '.json' + "?" + 'access_token=' + token)
        .then(function(res) {
            return res.json();
        // to get all the data from the request, comment out the following three lines...
        }).then(function(data) {
            return data.features[0].center;
        });
}


/***
 * reverseGeocode is a method to search for a physical address based on inputted coordinates
 * @param {object} coordinates is an object with properties "lat" and "lng" for latitude and longitude
 * @param {string} token is your API token for MapBox
 * @returns {Promise} a promise containing the string of the closest matching location to the coordinates provided
 *
 * EXAMPLE:
 *
 *  reverseGeocode({lat: 32.77, lng: -96.79}, API_TOKEN_HERE).then(function(results) {
 *      // do something with results
 *  })
 *
 */
function reverseGeocode(coordinates, token) {
    var baseUrl = 'https://api.mapbox.com';
    var endPoint = '/geocoding/v5/mapbox.places/';
    return fetch(baseUrl + endPoint + coordinates.lng + "," + coordinates.lat + '.json' + "?" + 'access_token=' + token)
        .then(function(res) {
            return res.json();
        })
        // to get all the data from the request, comment out the following three lines...
        .then(function(data) {
            return data.features[0].place_name;
        });
}
```

Here is example code using the Geocode helper function to log the coordinates of Codeup and then recenter the map to focus on Codeup:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Geocode Examples</title>
    <!-- Mapbox CSS -->
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.0/mapbox-gl.css' rel='stylesheet' />
    <!-- Custom CSS -->
    <style>
        #map {
            /* the width and height may be set to any size */
            width: 100%;
            height: 700px;
        }
    </style>
</head>
<body>

<h1>My First Mapbox Map!</h1>

<!-- The HTML element that serves as the Mapbox container -->
<div id='map'></div>

<!-- Mapbox JS -->
<script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.0/mapbox-gl.js'></script>

<!-- Mapbox Geocoder Util Methods -->
<script src="mapbox-geocoder-utils.js"></script>

<!-- Custom JS -->
<script>
    mapboxgl.accessToken = YOUR_TOKEN_HERE;
    var map = new mapboxgl.Map({
        container: 'map',
        style: 'mapbox://styles/mapbox/streets-v9',
        zoom: 10,
        center: [-98.4916, 29.4252]
    });

    // the  geocode method from mapbox-geocoder-utils.js 
    geocode("600 Navarro St #350, San Antonio, TX 78205", YOUR_TOKEN_HERE).then(function(result) {
        console.log(result);
        map.setCenter(result);
        map.setZoom(20);
    });

</script>
</body>
</html>

```

#### Reverse Geocoding

Reverse geocoding is the process of converting coordinates (latitude and longitude) into a physical address. For example, entering a coordinates object to the reverseGeocode method ```{lng: -98.4861, lat: 29.4260}``` would yield the physical address for the Alamo:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Geocode Examples</title>
    <!-- Mapbox CSS -->
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.0/mapbox-gl.css' rel='stylesheet' />
    <!-- Custom CSS -->
    <style>
        #map {
            /* the width and height may be set to any size */
            width: 100%;
            height: 700px;
        }
    </style>
</head>
<body>

<h1>My First Mapbox Map!</h1>

<!-- The HTML element that serves as the Mapbox container -->
<div id='map'></div>

<!-- Mapbox JS -->
<script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.0/mapbox-gl.js'></script>

<!-- Mapbox Geocoder Util Methods -->
<script src="mapbox-geocoder-utils.js"></script>

<!-- Custom JS -->
<script>
    var accessToken = YOUR_TOKEN_HERE;
    mapboxgl.accessToken = accessToken;
    var map = new mapboxgl.Map({
        container: 'map',
        style: 'mapbox://styles/mapbox/streets-v9',
        zoom: 10,
        center: [-98.4916, 29.4252]
    });

    // reverse geocode method from mapbox-geocoder-utils.js
    reverseGeocode({lng: -98.4861, lat: 29.4260}, accessToken).then(function(results) {
        // logs the address for The Alamo
        console.log(results);
    });

</script>
</body>
</html>

```

#### Bonus Example: Geocoding to Place a Marker

We can combine what we know about creating markers and geocoding addresses. Lets place a marker and popup window on UTSA based on the physical address of the 1604 campus:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Map Marker Example</title>
    <!-- Mapbox CSS -->
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.0/mapbox-gl.css' rel='stylesheet' />
    <!-- Custom CSS -->
    <style>
        #map {
            /* the width and height may be set to any size */
            width: 100%;
            height: 700px;
        }
    </style>
</head>
<body>

<h1>My First Mapbox Map!</h1>

<!-- The HTML element that serves as the Mapbox container -->
<div id='map'></div>

<!-- Mapbox JS -->
<script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.0/mapbox-gl.js'></script>

<!-- Mapbox Geocoder Util Methods -->
<script src="mapbox-geocoder-utils.js"></script>

<!-- Custom JS -->
<script>

    var accessToken = YOUR_ACCESS_TOKEN_HERE;

    mapboxgl.accessToken = accessToken;

    var map = new mapboxgl.Map({
        container: 'map',
        style: 'mapbox://styles/mapbox/streets-v9',
        zoom: 10,
        center: [-98.4916, 29.4252]
    });

    var alamoInfo = {
        address: "The Alamo, San Antonio",
        popupHTML: "<p>Remember the Alamo!</p>"
    };

    function placeMarkerAndPopup(info, token, map) {
        geocode(info.address, token).then(function(coordinates) {
            var popup = new mapboxgl.Popup()
                .setHTML(info.popupHTML);
            var marker = new mapboxgl.Marker()
                .setLngLat(coordinates)
                .addTo(map)
                .setPopup(popup);
            popup.addTo(map);
        });
    }

    placeMarkerAndPopup(alamoInfo, accessToken, map);

</script>
</body>
</html>

```

In the above example, a custom function, placeMarkerAndPopup, has been added to allow multiple markers to be easily placed. A challenge here is how to keep track of all marker objects once they are created. This is essential if we need to change the markers in some way after they are added. We could push multiple marker objects on to a global array of markers. The limitation of this approach is that it would be populated asynchronously, making it hard to predict how long our code would need to wait before accessing these marker objects. We will learn more about a convenient wrapper for asynchronous JavaScript code later in the curriculum (JavaScript Promises). For now, wrapping any calls to a global array of markers in a setTimout will offer a limited solution to this problem.


### Exercises

Please follow the instructions below. Remember to commit your changes after each step. At the end of the exercise, push your changes to GitHub.

1. Generate a Mapbox API Key using the steps from above
1. Create a new file named ```mapbox_maps_api.html``` and add a map using the next steps.
1. Generate a map that shows the city with your favorite restaurant using geocoding.
1. Redraw the map of the above location at zoom levels 5, 15, and 20. Do this by simply changing the value of zoom level where the map properties are initially set and refresh the page to see the changes. Can the zoom be changed programmatically after the initial map is drawn?
1. Create a marker on your map of the exact location of your favorite restaurant set the zoom to allow for best viewing distance.
1. Create a popup with the name of the restaurant.
1. Make sure the info window does not display until the marker has been clicked on.
1. Refactor your code to display at least three of your favorite restaurants with information about each. Create an array of objects with information about each restaurant to accomplish this. Use a ```.forEach()``` loop rather than a for loop.

### Bonuses (roughly in ascending order of challenge)

- Info windows can contain basic HTML, not just plain text. Add some additional information about your restaurant to the popup such as why it is your favorite, dishes you like, images, etc.
- Add a select input that allows the user to change the zoom level to 5, 15, or 20.
- Add a text box for the user to enter an address that will use geocoding to center the map and place a marker on that location.
- Add a button that will hide all markers.
- Using this [marker animation example](https://docs.mapbox.com/mapbox-gl-js/example/animate-marker/) as a starting point, animate a marker to bounce up and down. Alter the animation to stop after two seconds. Make the amount of bounce animation scale according to zoom level.
- Replace the generic marker icon with an image that is more appropriate for each restaurant. A tutorial for this can be found [here](https://docs.mapbox.com/help/tutorials/custom-markers-gl-js/).