# Inserting Data into an API

While Jalopy has a built-in mechanism for binding one or more URLs to a given screen, often you need to use a URL after the screen has loaded. A normal use case for this is for adding data. E.g., the user enters his/her data into a blank form, clicks the submit button, and the application sends the data to the API to add the data to the database. 

## A new dog fact screen

Let's add a new screen for inserting a new dog fact. Before we jump right in, consider the end-user. How would they want to enter a new dog fact? Some possibilities are:
1. a menu choice in the nav bar
2. a link on the Dog Fact screen
3. a button on the Home page, etc.

There is no one way to provide the user access to that functionality. There is only the right way for your particular user. In our application, we will add a  link at the bottom of the Dog Fact screen that will navigate our application to an Insert Dog Fact screen.

Add the following `<a>` to the bottom of your `DogFactsView` HTML function in `DogFacts.js`.
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

Lastly, add an event listener for the "Insert Fact" button. When clicked, use `fetch` to POST the new fact to the API. In the JS function for the view, add this code:

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

