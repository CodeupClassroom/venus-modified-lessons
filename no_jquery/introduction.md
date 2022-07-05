# Introduction to jQuery

## What is an API

API stands for _application programming interface_. An API is any way for one application or program to communicate with another one. There are tens of thousands of APIs in the world, many of which are actually built into our programming languages and tools. For example, later in this course you will be writing code that communicates with a MySQL database. The way we will do this is through an API provided by MySQL.

When we talk about APIs in JavaScript, typically we are talking about services that can send or retrieve data over the internet using ajax. Again, there are [thousands](http://catalog.data.gov/dataset?sort=views_recent+desc&res_format=api), upon [**thousands**](http://www.programmableweb.com/category/all/apis?data_format=21175%2C21173%2C21190) of open APIs you can use with JavaScript. Most of these use [*REpresentational State Transfer (REST)*](http://en.wikipedia.org/wiki/Representational_state_transfer#Architectural_constraints) for communication. Later in this course you will learn how to make your own RESTful API, but communicating with another company's API will give us a good preview of how the process works.

## Using jQuery

Before we can begin using jQuery in our applications, we need to include the jQuery source. This can be done by either installing locally or linking to a public CDN.

### Installing jQuery

We can install jQuery locally or use a content delivery network (CDN).



#### Downloading jQuery

We can download different packages for jQuery from the [official jQuery download page](http://jquery.com/download/).

There are 3 versions of jQuery available at this time:

- jQuery 1.x - Use this if you need to support IE 6, 7, or 8
- jQuery 2.x - Smaller and faster that version 1.x with the same API, but no support for IE 6, 7, or 8
- jQuery 3.x - Newest version, a few differences in the API from version 1.x or 2.x, some performance improvements

Each version has two different release types: compressed (production) and uncompressed (development).

We will be using the uncompressed copies for our applications and will switch them out for the compressed versions when we launch our product on a production server.

You can download a copy of jQuery locally by going to [the jquery download page](https://code.jquery.com/jquery/), right clicking the "uncompressed" link, and choosing "Save Link As...".

For this course we'll recommend that you use the [most recent 3.x release](https://code.jquery.com/jquery-3.6.0.js).

To use in our application, we can now just copy the file to our `js` folder and reference it using a `<script>` element like this:

~~~html
<script src="/js/jquery.js"></script>
~~~

#### Using jQuery from a CDN

jQuery provides a CDN release for production. It is available in the minified version and is easy to include using a `<script>` element.

~~~html
<script src="https://code.jquery.com/jquery-2.2.4.min.js" integrity="sha256-BbhdlvQf/xTY9gja0Dq3HiwQF8LaCRTXxZKRutelT44=" crossorigin="anonymous"></script>
~~~

This line will retrieve the jQuery source code from the CDN provided by jQuery
itself.

### The jQuery object

We will be using the `jQuery` object to find and create HTML elements from the DOM.

The jQuery object is [defined in the API documentation](http://api.jquery.com/jQuery/):
> Return a collection of matched elements either found in the DOM based on passed argument(s) or created by passing an HTML string.

In jQuery, we commonly use the dollar sign `$`, an alias of `jQuery` to reference the jQuery object, as we will see below.

### Document Ready

JavaScript provides the ability to natively determine if the window has finished loading using the `window.onload` event handler.

~~~js
window.onload = function() {
    alert( 'The page has finished loading!' );
}
~~~

While this is a good way to make sure the page is fully loaded and ready for DOM manipulation, it waits until all images have finished downloading. We usually do not need the image files to be fully downloaded to begin manipulating DOM objects.

Therefore, we can pass an anonymous handler function into the jQuery instance. This will fire as soon as the DOM is ready to be manipulated, _before_ all the images have completed downloading.

~~~js
$(function() {
    // code goes here
});
~~~

Passing an anonymous function in `$()` will execute our code when the document is ready for JavaScript manipulation:

~~~js
$(function() {
    alert( 'The DOM has finished loading!' );
});
~~~

If we put our JavaScript at the bottom of our document, it will already be loaded by the time the JavaScript is loaded. Using the function above will ensure the DOM is loaded before running.

## A Note on Code Examples

Throughout the rest of this section, you will see various code blocks of HTML
and JavaScript (and even some CSS!). In all of these examples, unless otherwise
stated, it is assumed that the HTML code is part of a larger document that
provides `script` tags that link to jQuery and an external js file.

All JavaScript code snippets should be assumed to be inside a larger file with
`"use strict";` at the top and inside a `$(function(){})` function.

HTML template

```html
<!DOCTYPE html>
<html>
<head>
    <title>Curriculum Code Sample</title>
</head>
<body>

    <!-- INSERT HTML CODE SAMPLE HERE -->

    <script type="text/javascript" src="js/jquery.js"></script>
    <script type="text/javascript" src="js/my-custom-javascript.js"></script>"
</body>
```

`my-custom-javascript.js`

```js
"use strict";

$(function() {

    // INSERT JAVASCRIPT CODE SAMPLE HERE

});
```

You will also see links to [JSBin](http://jsbin.com/), which is a platform that
allows us to quickly create and embed front-end code samples. You are highly
encouraged to play around with the js bin examples to get a better understanding
of the lessons. Play around with the code! Try to break the existing code! Then
go and figure out why it broke.

## Exercises

Commit each step to GitHub.

1. Create a new file named `jquery_exercises.html`.

1. Add a basic HTML structure and insert the jQuery CDN at the bottom of the body.

1. Add an alert to tell you the DOM has finished loading. Check for expected results.
