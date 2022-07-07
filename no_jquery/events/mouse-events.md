# Mouse Events

## Event Handler Functions

It is worth reviewing event handler functions here before we discuss how to attach them with jQuery. Recall that:

- an *event listener*, or *event handler* function is a *callback* function that is called when an event happens
- the handler function is passed the event object when it is called
- the event object contains details about the event that occurred, as well as methods for modifying the event, for example, `preventDefault`
- the event object can be omitted from the function definition if it is not used
- we can attach an anonymous function or a named function as a callback function
- any anonymous function can be refactored to be a named function

## Common Mouse Events
We can act on events like a user clicking a mouse, hovering over an element, or bringing areas in and out of focus.

In this lesson we will cover the different types of mouse events.

First, let's see how to add an event listener using JavaScript. 

- `.addEventListener(<event type>, function() {});`

This method is called on an object reference returned from `.querySelector()`.

`<event type>` is a string parameter that describes the type of event to which you wish to add the listener function.  

`function` is the function that you wish to execute when the specified event type occurs on the selected DOM object.

- `click` &mdash; Bind an event handler to the "click" JavaScript event or trigger that event on an element.
- `dblclick` &mdash; Bind an event handler to the double-click JavaScript event or trigger that event on an element.
- `mouseenter` &mdash; Bind an event to be executed when the mouse pointer enters the element.
- `mouseleave` &mdash; Bind an event to be executed when the mouse pointer leaves the element.

##Examples:

### click
Let's start with some basic HTML:

~~~html
<h1 id="codeup">Hello Codeup</h1>
~~~

Then we will use vanilla JS to attach an event listener that runs when our `h1` with the id of `codeup` is clicked:

~~~js
document.querySelector('#codeup').addEventListener("click", function() {
    alert('h1 with id "codeup" was clicked');
});
~~~

Clicking on the `h1` will now show an alert box.

### dblclick

`dblclick` is the same as `click`, but, as the name implies, the event will be triggered when the element is double clicked.

We could update our click example to use double clicks as follows:

~~~html
<h1 id="codeup">Hello Codeup</h1>
~~~

~~~js
document.querySelector('#codeup').addEventListener('dblclick', function(e) {
    alert('h1 with the id of "codeup" was double clicked!');
});
~~~

Clicking on the `h1` will now do nothing, but double clicking will show an alert box:

### mouseenter

`mouseenter` is called when the mouse enters the element.

The syntax for `mouseenter` is:

~~~js
document.querySelector( selector ).addEventListener('mouseenter', handlerIn);
~~~

`mouseleave` is very similar, except that the given function executes when the pointer leaves the element.

~~~js
document.querySelector( selector ).addEventListener('mouseleave', function(event) {
    console.log("leaving " + this);
});
~~~


Here we are using `this` to reference the selected DOM element inside our listener function.  When we are inside of a _callback function_ for an event, `this` will always refer to the selected DOM element that triggered the event.

## Further Reading

TODO:
- [MDN mouse events](https://developer.mozilla.org/en-US/docs/Web/Events/Creating_and_triggering_events)

## Exercises

Use the file `dom_exercises.html` for these exercises.  Commit your changes to GitHub.

1. Remove your custom event listener code from previous exercises.

1. Add code that will change the background color of an `h1` element when clicked.

1. Make all paragraphs have a font size of `18px` when they are double clicked.

1. Set all `li` text color to red when the mouse is hovering; reset to black when it is not.
