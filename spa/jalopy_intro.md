# Introduction to Jalopy

## First step: bare (BEAR) clone Jalopy

### NOTE: VENUS WILL NOT BARE CLONE THIS PROJECT. VENUS WILL INSTEAD DOWNLOAD THE ZIP FILE FROM GITHUB AND UNZIP IT INTO THEIR CODEUP-WEB-EXERCISES PROJECT

1. Clone the repo: https://github.com/CodeupClassroom/jalopy
2. Change to the directory where you cloned the repo
3. Remove its local .git folder: `rm -Rf .git`
4. Create your own `jalopy` repo on Github
5. Create a new local .git folder: `git init`
6. Add all files for committing: `git add .`
7. Do a first commit: `git commit -m "first commit"`
8. Add a remote (follow your GH repo's instructions for adding the `origin` remote)
9. Push your `jalopy` repo to GH

There is one required file for Jalopy that is not included in the repo as it is supposed to contain private configuration info, like API keys. To provide you with an initial version of this file, create the file `js/private_constants.js` and copy the following code into it.

```js
const BACKEND_HOST = "";
```

We will explain the purposes of all of the Jalopy files in the next section.

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

## How to NOT use IntelliJ as a web server

You may have noticed at this point that refreshing your browser while viewing a Jalopy page causes a "404" error. This is due to IntelliJ launching the Jalopy application. 

IntelliJ tries to make your webdev life easier by acting like a tiny web server so you can quickly and easily run your web pages while you are developing them. However, you can quickly bump your head on some of its limitations, primarily how it serves up assets, like images. 

The Mac has a built-in Apache web server that you can use, in place of IntelliJ. Unfortunately, it is a bit tricky to configure. Instead, let's use nginx!

### Step 1: Install nginx
`brew install nginx`


### Step 2: Find the nginx config file path
`nginx -t`

It is usually "/usr/local/etc/nginx/nginx.conf"


### Step 3: Edit the nginx config file

Change the port:

```
server {
	listen 9000;
```

Change the location of the web root:

```
location / {
	root	<path to the directory where jalopy's index.html file lives>
```

Add a few aliases rules below the current `location`. The aliases tell nginx how to route browser refreshes back to the Jalopy application's router.

```
        location /dogs {
                alias   <path to jalopy's index.html's directory>;
                index  index.html index.htm;
        }

        location /about {
                alias   <path to jalopy's index.html's directory>;
                index  index.html index.htm;
        }

        location /quotes {
                alias   <path to jalopy's index.html's directory>;
                index  index.html index.htm;
        }

        location /register {
                alias   <path to jalopy's index.html's directory>;
                index  index.html index.htm;
        }

        location /login {
                alias   <path to jalopy's index.html's directory>;
                index  index.html index.htm;
        }
```

NOTE: anytime you add a new route in a Jalopy application, you should add another alias in your nginx configuration file so that browser refreshes on that screen do not give you a `404` response.

### Step 4: Start or restart nginx
`nginx`
If typing the above gives you an "(48: Address already in use)" error, then type the following:
`nginx -s stop; nginx`


### Step 5: Cross your fingers and try it out

Visit `http://localhost:9000` in your browser.



## Next Lesson

[Jalopy's Composition](jalopy_parts.md)