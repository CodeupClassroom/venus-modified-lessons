# Adding a Screen to a Jalopy Application

In this lesson, we will a new screen/view component to our Jalopy project. The process is similar to modifying an existing screen. But in addition to modifying a few of the existing fileds, we will create a brand new file: the view component for the new screen. Generally, the steps are:

1. make the new view file in the `./views` directory, giving the view component an HTML function and a JavaScript function. These functions should just be placeholder functions for this step, with minimal, or no content.
2. add a routing entry for the new screen in `router.js`
3. add one or more navigation links in existing screens
4. try out your new screen
5. go back to thew view component and add content/functionality to the screen

## Step 1: make the new view component file
Let's add a new page that will display a single fact about cats.

Create a new file: `views/CatFacts.js` with the following code:
```js
export default function CatFactsView(props) {
    return `<h1>Cat Facts</h1>`;
}

export function CatFactsEvents() {
}
```
`CatFactsView(props)` is the HTML function for the screen and `CatFactsEvents()` is the JavaScript function. We have to `export` both functions so that the framework can call them when the user navigates to the screen. `props` is a parameter containing any data that needs to be passed to the HTML function (more about this next lesson).

## Step 2: add a routing entry
Open the file `router.js` and add the following line to the end of the `imports` at the top of the file:
```js
import CatFactsView, {CatFactsEvents} from "./views/CatFacts.js";
```
This line of code imports the HTML and JavaScript functions from the view component. Next, add a routing entry for the Cat Facts screen. 
```js
        '/cats': {
            returnView: CatFactsView,
            state: {},
            uri: '/cats',
            title: 'Cat Facts',
            viewEvent: CatFactsEvents
        },
```
`'/cats'` is the route that will navigate the application to the Cat Facts screen. `returnView` tells the framework which function will be called to generate the HTML part of the screen. We will discuss `state` in the next lesson. `uri` should match the route. `title` is what you want to display on the browser title bar when the screen is loaded. `viewEvent` tells the framework which function will be called after the screens DOM is loaded. This function is normally used to initialize screen fields and add event listeners to DOM elements.

## Step 3: add navigation links
There are different ways to cause navigation to the new screen: a link in the menu, a link in page content, redirection, etc. Navigation specifics depend on the application and its developers. We will extend the starter Jalopy menu with a link for the new screen.

Open the file `views/partials/Navbar.js. Replace the last three lines of the file with this:
```js
    html += `<a class="jalopy-nav" href="/cats" data-link>Cat Facts</a>`;
    html += `</nav>`;
    return html;
}
```
This code adds a link to the end of the menu that will route to the Cat Facts screen.

## Step 4: try it out
Run your application in the browser and view your handiwork! It should look something like this:

![Jalopy add screen page](jalopy_add_screen.png)

## Step 5: add more content to the view component

Finally, let's jazz up the Cat Facts screen by displaying a cat fact from https://alexwohlbruck.github.io/cat-facts/docs/endpoints/facts.html

Replace your `CatFactsView` function with this:
```js
export default function CatFactsView(props) {
    return `
<div class="container">
    <h1>Cat Facts</h1>
    <div class="card">
        <div class="card-body">
            <p class="cat-fact">It has been scientifically proven that stroking a cat can lower one's blood pressure.</p>
        </div>
    </div>
</div>
`;
}
```

Check out your work!

## Adding some JavaScript

Lastly, let's hide the fact and add a button. When the button is clicked, the fact will show itself. For convenience and brevity, we will use inline CSS to hide the fact. Here's the code for the entire `views/CatFacts.js`:

```js
export default function CatFactsView(props) {
    return `
<div class="container">
    <h1>Cat Facts</h1>
    <div class="card">
        <div class="card-body">
            <p class="cat-fact" style="visibility: hidden">It has been scientifically proven that stroking a cat can lower one's blood pressure.</p>
        </div>
    </div>
    <button class="form-control" id="show-fact-btn">Show Fact</button>
</div>
`;
}

export function CatFactsEvents() {
    const btn = document.querySelector("#show-fact-btn");
    btn.addEventListener("click", function(event) {
        const facts = document.querySelectorAll(".cat-fact");
        for (let i = 0; i < facts.length; i++) {
            facts[i].style.visibility = "";
        }
    });
}
```

## Exercise

Let's add a Quotes screen, where we will display quotes from famous people.







