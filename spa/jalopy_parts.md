# Jalopy's Composition

You will immediately notice that there are quite a few files involved in Jalopy's framework and the starter application. In this lesson, we will identify each of the files and its purpose.

### Top-level files

`index.html` The entry point for running the Jalopy starter application. 

### CSS files

`css/jalopy_starter.css` Contains a few simple and helpful CSS styles for the Jalopy starter application. Feel free to override or remove these.

### Framework files

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

### Application-specific files
