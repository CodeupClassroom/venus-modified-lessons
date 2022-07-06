# Introduction to Jalopy

## First step: bare (BEAR) clone Jalopy

1. Clone the repo: git@github.com:CodeupClassroom/jalopy.git
2. Change to the directory where you cloned the repo
3. Remove its local .git folder: `rm -Rf .git`
4. Create your own `jalopy` repo on Github
5. Create a new local .git folder: `git init`
6. Add all files for committing: `git add .`
7. Do a first commit: `git commit -m "first commit"`
8. Add a remote (follow your GH repo's instructions for adding the `origin` remote)
9. Push your `jalopy` repo to GH

Jalopy has a starter application ready for viewing. Run it by displaying its `index.html` in your browser.

## Digging into Jalopy

Jalopy is Codeup's own toy SPA framework that handles view components and client-side routing.

### View Components

A view component is a chunk of code whose purpose is to provide and support all, or some part of, a screen. SPA frameworks have a standard structure for coding view components, which allows devs to easily build new screens and insert them into the SPA. The downside is that a dev must first understand the standard structure. In Jalopy, a view component is a single file that has the HTML and JavaScript for a single screen. 


### Routing

When a user navigates to a new page in an SPA, the application intercepts the click (via an event listener) and tries to handle the navigation change itself. If the SPA can handle the navigation change, it tells the view component that contains the new page's HTML and JS to execute, modifying the DOM without causing a page refresh (no blinking or flashing). The end result of SPA routing is smoother and faster navigation. 

As with all frameworks, there is a learning curve to being able to use it effectively. Over the next few lessons, you will:

1. learn about all the parts/files of Jalopy
2. modify an existing view component
3. add a new view/screen to Jalopy's starter application
4. connect a view to an API as a source of data

## Next Lesson

[Jalopy's Composition](jalopy_parts.md)