# Single-Page Web Applications (SPA)

## Definition

Historically, whenever an end-user of a web application would click on a link to a new page of the application, or refresh data that have changed on the currently viewed page, the user's browser would make a new request of the web server to send the new page, if the page could not be loaded from the cache. This is known as Multi-Page Application (MPA) design and it has a few drawbacks:

- the request for the new page and its response must travel over a network like the internet, which is very slow compared to other data storage like SSDs and RAM.
- puts more load on the web server.
- does not more fully utilize the user's computer and its resources, like large amounts of RAM and processors.
- a page refresh causes the browser to blink or flash as the DOM is COMPLETELY rebuilt.

![SPA vs. MPA](/img/spa-mpa-lifecycle.jpeg)
<p><em>(above image swiped from https://lvivity.com/single-page-app-vs-multi-page-app)</em></p>

In the above diagram, an MPA must request a page refresh from the web server when data change. An SPA uses AJAX to fetch the data from the server and modifies its DOM. An SPA results in a more efficient use of network bandwidth and faster page updates. SPAs also do not flash or blink since they do not need to rebuild the entire DOM.

## Further Reading

- [MDN SPA Definition](https://developer.mozilla.org/en-US/docs/Glossary/SPA)

- [Wikipedia SPA Definition](https://en.wikipedia.org/wiki/Single-page_application)

- [SPA vs. MPA](https://lvivity.com/single-page-app-vs-multi-page-app)