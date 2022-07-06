# Jalopy's Composition

You will immediately notice that there are quite a few files involved in Jalopy's framework and the starter application. In this lesson, we will identify each of the files and its purpose.

### Top-level files

`index.html` The entry point for running the Jalopy starter application. 

### CSS files

`css/jalopy_starter.css` Contains a few simple and helpful CSS styles for the Jalopy starter application. Feel free to override or remove these.

### Framework files

These files should always be present in a Jalopy application, although the contents may change to suit your specific application.

`js/auth.js` Contains functions for authenticating and identifying the logged-in user.

`js/createView.js` Orchestrates the generation of a screen from a given URI.

`js/fetchData.js` Uses `fetch` to call API endpoints associated with a given route and make the returned data available to view components via a `props` parameter.

`js/init.js` Adds the event listeners to the application when the browser loads the app and the page refreshes. The event listeners call `createView` when the DOM first loads and when links are clicked. This file is only executed when the page loads/refreshes.

`js/main.js` This file is the entry point for the application to the framework. It is directly loaded from `index.html` and passes the processing to `init.js`. This file is only executed when the page loads/refreshes.

`js/messaging.js` This file contains a helper function for displaying an application-level notification. 

`js/private_constants.js` This file contains global constants that WILL NOT BE PUSHED to Github. Put information like API keys and credentials that are ok for the end-user to have in this file. 

`js/public_constants.js` This file contains global constants that will be pushed to Github. Put non-sensitive information like version # and application switches in this file.

`js/render.js` This file modifies the DOM with the view component's DOM info. It also modifies the DOM with the contents of `Navbar.js` and the notification `div` from `messaging.js`. It also executes the view component's specific JavaScript-loading function.

`js/routing.js` This file is the routing table that maps routes (URIs) to view components and any API endpoints that will provide data to a view.

`js/views/partials/Navbar.js` This file contains the a function that returns the HTML for the framework's menu bar. It is passed `props` created by any API endpoints for the current route.

### Application-specific files

These files are the view components that are specific to the starter application. View components completely change based on the screens you have in your application. 

`js/views/About.js` This is the view component for the about screen.

`js/views/Error404.js` This is the 404 (not found) screen.

`js/views/Home.js` This is the Home screen.

`js/views/Loading.js` This is the Loading screen that `createView.js` automatically calls while the selected route/view component is loading. Once the selected view component finishes loading, its DOM replaces the loading screen's DOM.

`js/views/Login.js` This is a simple login screen that will be used when we start the Backend module. Ignore it for now.

`js/views/Register.js` This is a simple user registration screen that will be used when we start the Backend module. Ignore it for now.

`js/views/User.js` This is a simple user information screen that will be used when we start the Backend module. Ignore it for now.

### Dissecting a view component

Generally, a view component exposes two functions that Jalopy calls when rendering a view component to the DOM. The first function returns the HTML that will be plugged into the application's DOM, replacing whatever view component DOM is currently there. For simplicity, let's call this "the HTML function". The second function executes any JavaScript needed to prepare the view component, e.g., add event listeners to the view component's DOM elements. For simplicity, let's call this "the JS function".

Looking at the view component `js/views/About.js`, we see those two functions:

```js
import {showNotification} from "../messaging.js";

export default function About(props) {
    return `
        <header>
            <h1>About Page</h1>
        </header>
        <main>
            <div>
                <p>About page is here.</p>  
            </div>
        </main>
    `;
}

export function AboutEvents() {
    showNotification("Hey, a message!", "danger");
}
```

The `import` statement at the top of the screen makes the `showNotification` function available to the About screen. 

`export` is needed for any functions that want to callable from outside of this JavaScript file. At a minimum, the view component's HTML function and JS function should have the `export` keyword. 

`About(props)` is the view component's HTML function. It uses a template string to compose the HTML that it returns to the caller, i.e., the `render()` function in `render.js`. The `props` parameter contains any data returned from API endpoints specified for the view component in `router.js`. *IMPORTANT:* this function's name can be anything AS LONG AS `router.js` knows about it.

`AboutEvents()` is the view component's JS function. This function contains the JavaScript for preparing the view component's DOM, like attaching event handlers. *IMPORTANT:* this function's name can be anything AS LONG AS `router.js` knows about it.

## Next Lesson

[Modifying an existing screen](modifying_home.md)