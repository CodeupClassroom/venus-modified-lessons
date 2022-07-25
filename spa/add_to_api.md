# Inserting Data into an API

While Jalopy has a built-in mechanism for binding one or more URLs to a given screen, often you need to use a URL after the screen has loaded. A normal use case for this is for adding data. E.g., the user enters his/her data into a blank form, clicks the submit button, and the application sends the data to the API to add the data to the database. 

## A new dog fact screen

Let's add a new screen for inserting a new dog fact. Before we jump right in, consider the end-user. How would they want to enter a new dog fact? Some possibilities are:
1. a menu choice in the nav bar
2. a link on the Dog Fact screen
3. a button on the Home page, etc.

There is no one right way to provide the user access to that functionality. There is only the way that best suits your particular application's users. In our application, we will add a link at the bottom of the Dog Fact screen that will navigate our application to an Insert Dog Fact screen.

Add the following `<a>` to the top OR bottom of your `DogFactsView` HTML function in `DogFacts.js`. Sometimes, I like my add "button" to be at the bottom of the list of records, and sometimes at the top.
```html
<a data-link href="/insert-dog-fact">Insert Dog Fact</a>
```

Create a new view component file called `AddDogFact.js`. Add the following code to it.
```js
import createView from "../createView.js"

export default function InsertDogView(props) {
    return `
<form class="container">
    <h1>New Dog Fact</h1>
    <form>
        <label for="dogFactText" class="form-label">Dog fact</label>
        <input class="form-control" list="datalistOptions" id="dogFactText" placeholder="Enter a new dog fact">
        <button class="form-control" id="insert-btn">Insert Fact</button>
    </form>
</div>
`;
}

export function InsertDogFactEvents() {
}
```

Ignore the `import` statement for now. We will use it after sucessfully inserting a dog fact into the API.

In `router.js`, add a new import line at the top of the file:
```js
import InsertDogView, {InsertDogFactEvents} from "./views/AddDogFact.js";
```
and add a new routing entry:
```js
'/insert-dog-fact': {
    returnView: InsertDogView,
    state: {},
    uri: '/insert-dog-fact',
    title: 'Insert Dog Fact',
    viewEvent: InsertDogFactEvents
},
```
Note that `state` remains an empty object for this screen. This is because we will not use Jalopy to automatically fetch data for this screen. We will instead use `fetch` in our screens JS function to `POST` the new dog fact to the API.

Add an event listener for the "Insert Fact" button. When clicked, use `fetch` to POST the new fact to the API. In the JS function for the view, add this code:

```js
export function InsertDogFactEvents() {
    const insertBtn = document.querySelector("#insert-btn");
    insertBtn.addEventListener("click", function(event) {
        const factText = document.querySelector("#dogFactText").value;
        const requestOptions = {
            method: "POST",
            headers: {
                'Content-Type': 'application/json',
                'Authorization': DOG_QUOTE_API_KEY
            },
            body: JSON.stringify([factText])
        }
        fetch("https://dogfacts.fulgentcorp.com:12250/v1/facts", requestOptions)
            .then(function(response) {
                if(!response.ok) {
                    console.log("add dog fact error: " + response.status);
                } else {
                    console.log("add dog fact ok");
                    createView("/dogs");
                }
            });
    })
}
```

We start the JS function by adding a click event listener to the "Insert Fact" button. After saving the input text to a variable, we construct an options object for the request. The options tell `fetch` that this request is a `POST` request, the body data type is JSON, identifies us with our API key, and provides the body data as a string. Note that the body's data must be inside a string. This is a naive requirement of our dog facts API.

Finally, we call the dog facts API and provide the request options. If the `fetch` succeeds, we log the success to the console and change to the DogFacts view. Otherwise, we log the failure to the console and stay on the InsertDogFact screen. 

## Exercise

1. Create an Add Quote link or button on your Quotes screen that navigates the application to an `/addQuote` route.
2. Create an AddQuote screen with a skeleton HTML function and JS function. You should also import `createView` at the top, like we did in the dog facts example.
3. Update `router.js` to route `/addQuote` to the AddQuote screen.
4. Build out the rest of the AddQuote screen, providing inputs for the quote AND the author. 
5. In your event listener for adding a quote, you will need to create a JavaScript object representing the new quote, and then `JSON.stringify` that object into the body of the request (just as we did in the dog fact example above). 
Prior to stringification, the new quote object should look like this:
```js
{
    quote: "the text of the quote to add",
    author: "the author's name or Anonymous"
}
```
6. Humans are notorious for giving bad data! Validate the inputs. If the trimmed value of the author is an empty string, then save the author's name as "Anonymous". If the trimmed value of the quote is an empty string, then abort the save and tell the user that the quote cannot be blank. You can use Jalopy's `showNotification` function, if you wish. Don't use `console.log` to notify the user.
7. Report all other errors trying to save the quote to the user. You can test this by intentionally misspelling the quotes API URL.
8. If the quote saves successfully, redirect your screen by calling `createView` and returning to the `/quotes` screen.
