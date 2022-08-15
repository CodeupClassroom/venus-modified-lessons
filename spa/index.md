# Single-Page Web Applications (SPA)

## Definition

Historically, whenever an end-user of a web application would click on a link to a new page of the application, or refresh data that have changed on the currently viewed page, the web server responsible for serving up the application would send the new page, or re-send the current page with the new data (use of cache notwithstanding) to the end-user's browser. This is known as Multi-Page Application (MPA) design and it has a few drawbacks:

- the request for the new page and its response must travel over a network like the internet, which is very slow compared to other data storage like SSDs and RAM.
- puts more load on the web server.
- does not more fully utilize the user's computer and its resources, like large amounts of RAM and processors.
- a page refresh causes the browser to blink or flash as the DOM is COMPLETELY rebuilt.

![SPA vs. MPA](/img/spa-mpa-lifecycle.jpeg)
<p><em>(above image swiped from https://lvivity.com/single-page-app-vs-multi-page-app)</em></p>

In the above diagram, an MPA must request a page refresh from the web server when data change. An SPA uses AJAX to fetch the data from the server and modifies its DOM. An SPA results in a more efficient use of network bandwidth and faster page updates. SPAs also do not flash or blink since they do not need to rebuild the entire DOM.

## Building an SPA

To build an SPA, we need to use a framework that handles some of the tasks required for an SPA to function. 

Some popular SPA frameworks are:

- [React](https://reactjs.org/)
- [Angular](https://angular.io/)
- [Vue](https://vuejs.org/)

We will be using our own toy SPA framework (Jalopy), but becoming familiar with how Jalopy works will make learning one or more of the other SPA frameworks easier.

## Further Reading

- [Intro to SPAs](https://www.bloomreach.com/en/blog/2018/what-is-a-single-page-application)

- [SPA vs. MPA](https://lvivity.com/single-page-app-vs-multi-page-app)

- [MDN SPA Definition](https://developer.mozilla.org/en-US/docs/Glossary/SPA)

- [Wikipedia SPA Definition](https://en.wikipedia.org/wiki/Single-page_application)

## Next Lesson

[Intro to Jalopy (our SPA Framework)](jalopy_intro.md)
