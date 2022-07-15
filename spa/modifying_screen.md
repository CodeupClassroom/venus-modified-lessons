# Modifying a Jalopy Screen

In this lesson, we will make a few changes to the About screen/view component. Recall that the file responsible for the About screen is `js/views/About.js`.

## Changing a view component's HTML

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

## Changing a view component's JavaScript

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

## Changing view component function names

`router.js` is responsible for connecting a view component's HTML and JS functions to the route that transitions to that view component. Thus, any time you create or modify a view component's HTML or JS function names, you must also modify the view component's routing information in `router.js`.

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

### Tracking the version

Use the application's version number in `public_constants.js` as a simple way of tracking which version of the software is currently being run in the browser. Dev companies have a variety of techniques for using version numbers. We will simply increment the version number each time we commit our work. 

- Open the file `public_constants.js`.
- Increment the value for the constant FRONTEND_VERSION to 0.0002
- Run your web app and check the version number in the footer
- Add, commit, and push

## Exercise

Let's modify the Home screen. 

1. completely remove all code from `HomeEvents()`
2. replace the `h1` text with "Hello Jalopy!" and center it
3. create your own CSS file called `my-css.css` and include it in `index.html`
4. add an `img` of a jalopy above the `p`. Use the file `assets/jalopy1.jpeg` as the source of the image. 
5. You may experience problems with IntelliJ finding the `assets` folder from the root of the web application. If so, you should add a constant in `public_constants.js` and prepend it to the jalopy image path. Later, when we deploy the application to a proper web server, we will set that constant to an empty string.
6. Give the jalopy's `img` a max-width of 300px and center it in the page. Use your CSS file for the styling.
7. Change the `p` text to "Welcome to my Jalopy application!"
8. Add a button under the `p` with an id of `img-button`
9. Add an event listener for the button so that each time the button is clicked, the image `src` cycles through the images `jalopy1.jpeg`, `jalopy2.jpeg`, and `jalopy3.jpeg`. Therefore, the first time the button is clicked, the image should show `jalopy2.jpeg`. When the image is showing `jalopy3.jpeg` and the button is clicked again, the image should show `jalopy1.jpeg`.
10. Increment the app version number to 0.0003

## Next Lesson

[Adding a new screen](add_screen.md)