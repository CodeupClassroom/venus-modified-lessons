# Ajax Requests using JavaScript's Fetch API

Ajax is a way to communicate with a server, sending or retrieving data, without refreshing the current webpage. Once the request is done we can alter the DOM with the requested information or result. The quintessential example of this is [Google Maps](https://www.google.com/maps). Each time you drag the map or press a button, the map information is changed without refreshing the entire view.
This can provide a very rich user experience.

In this lesson we will learn how to use JavaScript's `fetch` API to send Ajax requests, and how to work with information returned from those requests. Note that from this point on, we will simply refer Ajax as `fetch`.

## Basic Request

The simplest way to issue a `fetch` request is the following:

```js
fetch("/some-url")
```

This will issue a `GET` request to your server, asking for the file stored at `/some-url`. What if we needed to use a `POST` request instead? Or if we wanted to pass some additional data with the request? For these purposes, and others, `fetch` accepts a JavaScript object of additional options as a second argument.

## Fetch Options

The easiest way to manipulate the fetch options is to pass a JavaScript object like the following:

```js
fetch("/some-url", {
    method: "POST",
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify(someData)
});
```

In this example, we are sending a `POST` request rather than
`GET` and passing some additional information to the server that tell's the server the format of the request's content and some data. Some of the most common options are:

- `method` &mdash; The type of HTTP request to send to the server. Can be `"GET"`,
  `"POST"`, `"PUT"`, or `"DELETE"`. The default is `"GET"`.
- `body` &mdash; Data to be included with the request. Typically this will be a
  JavaScript object. If the request type is `GET` the data will be encoded into
  the URL being requested. Otherwise, it is included with the request behind the
  scenes. NOTE: if the data is complex like JSON arrays or objects, then it MUST be stringified.
- `credentials` &mdash; If a server requires authentication information, that information can be passed in the `credentials` option.
- `headers` &mdash; an object of whose key value pairs represent custom HTTP headers to
  send along with the request

## Handling Responses

Now that we can send requests to the server, how do we handle the data that comes back? It is important to be aware that `fetch` requests are done *asynchronously*. This means that even though the request is fired off when we call `fetcch()`, JavaScript does not sit and wait for the response to come back. The server could reply in a fraction of a second, or it could reply in half a minute!

It is tempting to think we can write something along the lines of:
```js
// THIS WILL *NOT* WORK

// Send fetch request
var data = fetch("/some-url");
// Handle data from Ajax request
doSomething(data);
```

But just because a fetch request has been sent is no guarantee that it has come back from the server by the time the next line of JavaScript is executed. In our
example above, `doSomething` will be called before JavaScript has even finished sending the request to the server! Instead, we must explicitly specify a function to be called once the response has come back.

The example below makes a `GET` request to the pokeAPI using the `/pokemon` endpoint for data regarding Charmander. 

```js
fetch('https://pokeapi.co/api/v2/pokemon/charmander').then(function(response) {
    console.log(response);
})
```

A function that we create to be called when some process completes is called a *callback function*. The primary way to attach a callback to your `fetch` request is to tack `.then()` to the end of your request and pass your callback to it, like the following:

```js
fetch("/some-url").then(function(response) {
    alert("fetch call completed successfully!");
    console.log("Response status: " + response.status);
});
```

`.then()` is a method that accepts a callback function as an argument. `fetch` will then call that function once the request has come back successfully.

The callback function has a single parameter:

- `response`: the ENTIRE response from the server

You can check the success or failure of the request by evaluating `response.ok`. If it is true, then the response was successful. If not, something happened. The exact response code is accessible via `response.status`. 

`response.body` contains the data returned from the request, BUT it may not be immediatedly useful to you. If the response format is JSON, it must first be processed via `response.json()`. Notice that this is a method call. `response.json()` returns the contents of the response's body as JSON.

**IMPORTANT** `json()` is an asynchronous method, just like `fetch()`. Therefore, the returned data from `json()` is only accessible via its `.then()` method.

Following is an example of using `fetch` to request information about a Pokemon character. 

```js
fetch('https://pokeapi.co/api/v2/pokemon/charmander')
        .then(function(response) {
            // can't use the response's data yet. have to json() it
            return response.json();
        }).then(function(data) {
            // at this point, we can use the response's data
            console.log(data);
        });
```

This process may seem a little long-winded, and you are not wrong!. `fetch` alternatives like jQuery's `ajax` and `axios` make many vanilla JavaScript tasks, like HTTP requests, simpler and easier to understand. While we are teaching you the basic JavaScript way of performing these tasks, we encourage you to broaden your toolset by playing around with other JavaScript libraries, like jQuery.

In addition to `then()`, `fetch` provides two other methods for executing code based after a request has been made.

- `.catch` &mdash; Executes the given function after the request encounters an error. 
- `.finally` &mdash; Executes the given function after the request finishes, whether it be successful OR unsuccessful.

`.catch()` takes a callback function as an argument. The callback function has a single parameter, `error`, which provides information about the error that caused the failure.

`.finally()` takes a callback function as an argument and the callback function has no parameters. 

An example combining all three callback types might look like:

```js
fetch("/some-url").then(function(response) {
    if(!response.ok) {
        console.log("The request did not succeed!");
    }
    return response.json();
}).then(function(data) {
    console.log("At last, we have the JSON data we want: " + data);
}).fail(function(error) {
    console.log("ERROR!!!");
    console.log("Error message: " + error.message);
}).always(function() {
    console.log("This function always runs!");
});
```

Notice that only two of those callbacks will ever be called; `.then()` and `.catch()` are MUTUALLY EXCLUSIVE, so there should never be a case where **both** are called. `.finally()`, as the name implies, is always called.

Remember that any anonymous function can be replaced with a named function, and as our code starts to get more complex, it usually makes sense to do so. Here is a small example using named functions with comments that contain examples of
what the function might be used for:

```js
function onSuccess(response) {
    // display the requested data to the user
}

function onFail(error) {
    // tell the user something went wrong, and to try again later
}

function stopLoadingAnimation() {
    // the request is no longer pending, hide the loading spinner
}

fetch("/some-url")
    .then(onSuccess)
    .catch(onFail)
    .finally(stopLoadingAnimation);
```

## async/await

An alternative to handling asynchronous methods in callback functions is to tell JavaScript to **wait** until the asynchronous method finishes using the `await` keyword.

NOTE: using `await` will require the containing function to be declared with the keyword `async` just before the keyword `function`. This includes using `await` inside an IIFE.

Example:
```js
    // use async/await to make it a simpler function
    async function getPokeStats(pokeName) {
        const response = await fetch('https://pokeapi.co/api/v2/pokemon/' + pokeName);
        return response.json();
    }

    getPokeStats("pikachu").then(function(pokeStats) {
        console.log(pokeStats);
    });

    // or this
    console.log(await getPokeStats("pikachu"));
```

## Further Reading

- [Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

- [Comparison of async/await with .then()](https://www.smashingmagazine.com/2020/11/comparison-async-await-versus-then-catch/)

- [Axios](https://axios-http.com/docs/intro)

## Exercises

### Ajax Store

1. Download and save [ajax-store.html](/examples/javascript/ajax-store.html).
1. Create a `data` directory and download [inventory.json](/examples/javascript/inventory.json) to that folder.
1. Your online tool store should load data from the JSON file using a get request and append the data to the table. As explained in the lesson, you will need to use `fetch` and either a couple of `.then()` calls, or `async/await`.
1. Add some new entries to inventory.json and see how the data on the page gets updated.
1. Add a "Refresh" button that will load inventory.json for you without having to refresh the entire page.
1. Make your store look better using custom CSS and/or Bootstrap

### Ajax Blog

1. Create a new html file called `ajax-blog.html`.
1. At minimum, your Ajax blog will need an empty `<div>` with an id of `posts`.
1. Make it pretty with custom CSS and/or Bootstrap.
1. Download [blog.json](/examples/javascript/blog.json) to your `data` directory from before.
1. Use Ajax to load the contents of `blog.json` and add the data from it to your `#posts` div.
1. Add additional entries to `blog.json` and make sure your changes are reflected on the page.
