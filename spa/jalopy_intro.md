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


## Digging into Jalopy

Jalopy is Codeup's own toy SPA framework that handles view components and client-side routing.

### View Components

A view component is a chunk of code whose purpose is to provide and support all, or some part of, a screen. SPA frameworks have a standard structure for coding view components, which allows devs to easily build new screens and insert them into the SPA. The downside is that a dev must first understand the standard structure. In Jalopy, a view component is a single file that has the HTML and JavaScript for a single screen. 


### Routing