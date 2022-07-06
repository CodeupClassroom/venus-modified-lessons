# Modifying a Jalopy Screen

In this lesson, we will make a few changes to the About screen/view component. Recall that the file responsible for the About screen is `js/views/About.js`.

### Changing a view component's HTML

Let's modify the HTML to use a Bootstrap grid to display information about the application in the first row, and some information about the application's 3 developers (fake) in the second row.

We will also add a button to the bottom of the screen that will change the about information from the "lorem" text to "Hello world!". 

Finally, we will add an anchor to link to the `/` route (i.e., Home). Jalopy will intercept this request and handle the navigation internally, instead of requesting the page from a web server.

Here is the resulting `About()` function:

```js
export default function About(props) {
    // language=HTML
    return `
<header>
    <h1>About Page</h1>
</header>
<main>
    <div class="container-fluid">
        <div class="row">
            <div class="col-12">
                <p id="about-text">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aliquid animi illo possimus quis sit. Amet, aspernatur autem cum dicta doloremque eveniet fugit hic id mollitia neque nesciunt, obcaecati similique, voluptate.</p>
            </div>
        </div>
        <div class="row">
            <div class="col-4">
                <img src="https://fer-uig.glitch.me?uuid=1" alt="Fake dev 1">
                <p>Fake Dev 1</p>
            </div>
            <div class="col-4">
                <img src="https://fer-uig.glitch.me?uuid=2" alt="Fake dev 2">
                <p>Fake Dev 2</p>
            </div>
            <div class="col-4">
                <img src="https://fer-uig.glitch.me?uuid=3" alt="Fake dev 3">
                <p>Fake Dev 3</p>
            </div>
        </div>
        <div class="row">
            <div class="col-12">
                <button id="change-about-text">Change About Text</button>
            </div>
            <div class="col-12">
                <a href="/" data-link>Go Home</a>
            </div>
        </div>
        <div
    </div>
</main>
    `;
}
```

NOTE: any anchor that you wish to use for Jalopy navigation needs to have the `data-link` attribute.

### Changing a view component's JavaScript

Remove the call to `showNotification()`. You can also delete the import at the top of the file.

The button with id `change-about-text` will change the text for the element with id `about-text` to "Hello World!". Since this functionality is pretty specialized, it makes sense to roll it into is own function and call that function from the button's event listener. 

Here is the resulting JavaScript:

```js
function changeAboutText() {
    document.querySelector("#about-text").innerText = "Hello World!";
}

export function AboutEvents() {
    document.querySelector("#change-about-text").addEventListener("click", changeAboutText);
}
```

Since the outside world does not need to call `changeAboutText()`, it does not need to be exported.

### Changing view component function names

`router.js` is responsible for connecting a view component's HTML and JS functions to the route that transitions to that view component. Thus, any time you create or modify a view component's HTML or JS functions, you must also modify its routing information in `router.js`.

Let's change the About HTML function's name from `About` to `AboutView`. 
1. change the function's name in `js/views/About.js`
2. change the function's name for the `/about` route in `js/router.js`

The resulting code looks like this:

```js
export default function AboutView(props) {
    // language=HTML
    return `
...
```
(from file `js/views/About.js`)

```js
import Home, {HomeEvents} from "./views/Home.js";
import AboutView, {AboutEvents} from "./views/About.js";

...

'/about': {
    returnView: AboutView,
    state: {},
    uri: '/about',
    title: 'About',
    viewEvent: AboutEvents
},
```
(from file `js/router.js`)


## Exercise

## Next Lesson
