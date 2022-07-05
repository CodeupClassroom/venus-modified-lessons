# Attribute Methods

jQuery provides many different methods for manipulating element attributes, but we will be covering just the most commonly used methods.

Here are the methods we will cover in this lesson:

- `.html()` &mdash; Get the HTML contents of the first element in the set of matched elements or set the HTML contents of every matched element.
- `.css()` &mdash; Get the value of a style property for the first element in the set of matched elements or set one or more CSS properties for every matched element.
- `.addClass()` &mdash; Adds the specified class(es) to each of the set of matched elements.
- `.removeClass()` &mdash; Remove a single class, multiple classes, or all classes from each element in the set of matched elements.
- `.toggleClass()` &mdash; Add or remove one or more classes from each element in the set of matched elements, depending on either the class’ presence or the value of the switch argument.

Before we begin talking about the individual methods, we should first discuss
two concepts that relate to jQuery as a whole:

- Getters and Setters

    Many of the methods we will cover in this lesson can act as both *getters* and *setters*. This means that they can both find the an existing value, or modify that value. We will see an example of this when we talk about the `.html` method.

- Method Chaining

    Most of the jQuery methods return a jQuery object, specifically when they are used as setters. This means that we can continue to add, or *chain*, methods calls together so long as the methods we are calling return a jQuery object. We will see an example of this when we discuss the `.css` method.

Keep both of these concepts in mind as we continue through the lesson.

## .html()

The `.html()` method can be used both to get the current HTML of an element, and to change the HTML of selected element(s).

When no arguments are passed to `.html()`, it will retrieve the HTML contents of a selected element. We will start with a basic example and then add to it.

Using our previous example HTML:

~~~html
<h1 id="codeup">Hello Codeup</h1>
~~~

We could retrieve the contents of the `h1` element using this bit of jQuery:

~~~js
$('#codeup').html();
~~~

We can then assign the contents of the `h1` to a variable and alert it:

~~~js
var h1Contents = $('#codeup').html();
alert(h1Contents);
~~~

Wrapping this in a `click` event would give us some control over when the alert fires:

~~~js
$('#codeup').click(function() {
    var h1Contents = $('#codeup').html();
    alert(h1Contents);
});
~~~

If we had multiple `h1` elements on the page, and we only wanted to alert the contents of the one that was clicked, we could use `$(this)`:

~~~js
$('#codeup').click(function() {
    var h1Contents = $(this).html();
    alert(h1Contents);
});
~~~

Refactoring further, we could eliminate the temporary variable `h1Contents`:

~~~js
$('#codeup').click(function() {
    alert($(this).html());
});
~~~

[You can see an example on JS Bin here](http://jsbin.com/suveni/1/edit?js,output)

!!!warning ""
    `.html` will return the *only the HTML from the first matched element* when
    used as a getter.

The `.html()` method can also be used to set the HTML inside of a selected element. Instead of displaying the contents, we could have the contents change on click using this code:

~~~js
$('#codeup').click(function() {
    $(this).html('Codeup Rocks!');
});
~~~

Now when the element with the id of `codeup` is clicked, it's HTML is changed to
'Codeup Rocks!'.

[JS Bin Example](http://jsbin.com/wetav/1/edit?js,output)

Another method similar to `.html` is `.text`. We won't go into detail on the `.text` method here, but differences between `.text` and `.html` are comparable to the differences we saw earlier between the `innerText` and `innerHtml` properties.

### .css()

The `.css` method can be used to both retrieve the current value of a CSS property, or make changes to an element or elements' style. Let's look at some examples of each:

Find the current value for the width of an element:

```js
$('#my-element').css('width')
```

Change the font color for all `h2`s to 'firebrick'

```js
$('h2').css('color', 'firebrick')
```

When used to set a property, the `.css` method returns a jQuery object, so we
can *chain* multiple calls to the `.css` method.

```js
$('#my-element').css('color', 'firebrick').css('background-color', 'papayawhip')
```

To change multiple properties at once, we can chain calls to the `.css` method,
or pass an object as an argument. The object we pass must have keys designating
CSS properties, and values that we want to change.

We can rewrite the above chained method calls by using an object:

```js
$('#my-element').css({
    'color': 'firebrick',
    'background-color': 'papayawhip'
});
```

Of course, we could define an object ahead of time and then later use it in our
calls to `.css`:

```js
var highlightedStyles = {
    'color': 'red',
    'background-color': 'yellow',
    'font-size': '28px'
};
$('#my-element').css(highlightedStyles);
```

### .addClass()

Adding classes dynamically to elements is a great way to control the styling of a page and use event handlers to add functionality. The `.addClass` method allows us to do this.

The syntax for `.addClass()` is:

~~~js
.addClass( className )
~~~

Where `className` is a string that contains the name of the class to be added.

Earlier we highlighted text on the page; now, we can do so using a predefined class with styling.

We will next look at an example using pre-defined CSS classes

*For the sake of brevity all the HTML, CSS, and JavaScript is presented in the
same code block*

~~~html
<!DOCTYPE html>
<html>
<head>
    <title>Example jQuery Page</title>
    <style>
        .centered {
            text-align: center;
        }
        .highlighted {
            background-color: #FF0;
        }
    </style>
</head>
<body>
    <h1>Example Page</h1>
    <div class="centered important">
        <h3>NOTICE</h3>
        <p>This is an important message!</p>
    </div>
    <div class="not-important">
        <h2>Lorem Ipsum</h2>
        <p>Lorem ipsum dolor sit amet, incididunt ut <span class="important">labore et dolore magna aliqua.</span>, quis ut aliquip ex ea commodo.</p>
    </div>
    <p class="centered important">This is also very important.</p>
    <a href="#" id="highlight-important">Click to highlight important text.</a>

    <script src="/js/jquery.js"></script>
    <script>
        "use strict";

        $(document).ready(function() {
            $('#highlight-important').click(function(event) {
                event.preventDefault();
                $('.important').addClass('highlighted');
            });
        });
    </script>
</body>
</html>
~~~

The CSS tells us any element with class `.centered` will have center-aligned text and any element that uses class `.highlighted` will have a background color of yellow.

The JavaScript is adding a click event to the `a` element with the id `highlight-important`.  When the event handler is triggered, the first thing it runs is `event.preventDefault()`, which will stop the default behavior of the anchor element from firing.

After the event is updated, we then see the `.addClass()` method being used to add the `highlighted` class to the elements selected by `$('.important')`.

Clicking on the `highlight-important` link should add the `highlighted` class to all elements with a class of `important`, thereby making them have a yellow background color.

We can test this code below:

[JS Bin Example](http://jsbin.com/wimara/1/edit?js,output)

### .removeClass()

Above, we learned how to add a class to elements using `.addClass()`; here, we will learn how to remove classes from elements using `.removeClass()`.

The syntax for `.removeClass()` is the same as above:

~~~js
.removeClass( className )
~~~

Using the example from above, we could start with the `highlighted` class on by default and have the link remove the styling:

~~~html
<!DOCTYPE html>
<html>
<head>
    <title>Example jQuery Page</title>
    <style>
        .centered {
            text-align: center;
        }
        .highlighted {
            background-color: #FF0;
        }
    </style>
</head>
<body>
    <h1>Example Page</h1>
    <div class="centered important highlighted">
        <h3>NOTICE</h3>
        <p>This is an important message!</p>
    </div>
    <div class="not-important">
        <h2>Lorem Ipsum</h2>
        <p>Lorem ipsum dolor sit amet, incididunt ut <span class="important highlighted">labore et dolore magna aliqua.</span>, quis ut aliquip ex ea commodo.</p>
    </div>
    <p class="centered important highlighted">This is also very important.</p>
    <a href="#" id="highlight-important">Click to remove highlight from important text.</a>

    <script src="/js/jquery.js"></script>
    <script>
        "use strict";

        $(document).ready(function() {
            $('#highlight-important').click(function(event) {
                event.preventDefault();
                $('.important').removeClass('highlighted');
            });
        });
    </script>
</body>
</html>
~~~

[JS Bin Example](http://jsbin.com/xidiya/1/edit?js,output)

### .toggleClass()

The `.toggleClass` method allows us to add a class to an element or elements if the class is not already present, or to remove the class if it is present on the matched element(s).

The syntax for `.toggleClass()` is:

~~~js
.toggleClass( className )
~~~

If the class name is present, `.toggleClass()` will remove it; otherwise, the new class name will be added.

We can update our example to have the link add or remove highlighting on each click:

~~~html
<!DOCTYPE html>
<html>
<head>
    <title>Example jQuery Page</title>
    <style>
        .centered {
            text-align: center;
        }
        .highlighted {
            background-color: #FF0;
        }
    </style>
</head>
<body>
    <h1>Example Page</h1>
    <div class="centered important highlighted">
        <h3>NOTICE</h3>
        <p>This is an important message!</p>
    </div>
    <div class="not-important">
        <h2>Lorem Ipsum</h2>
        <p>Lorem ipsum dolor sit amet, incididunt ut <span class="important highlighted">labore et dolore magna aliqua.</span>, quis ut aliquip ex ea commodo.</p>
    </div>
    <p class="centered important highlighted">This is also very important.</p>
    <a href="#" id="highlight-important">Click to toggle the highlighting of important text.</a>

    <script src="/js/jquery.js"></script>
    <script>
        "use strict";

        $(document).ready(function() {
            $('#highlight-important').click(function(event) {
                event.preventDefault();
                $('.important').toggleClass('highlighted');
            });
        });
    </script>
</body>
</html>
~~~

As we see below, the link will now add or remove highlighting based on the present state. Viewing the element inspector in a browser will show the `highlighted` class being added and removed from the elements.

[JS Bin Example](http://jsbin.com/cufuy/1/edit?js,output)

## Further Reading

- [Full List of jQuery Attribue Methods](http://api.jquery.com/category/attributes/)
- [The `.html` method](http://api.jquery.com/html/)
- [The `.css` method](http://api.jquery.com/css/)
- [The `.addClass` method](http://api.jquery.com/addClass/)
- [The `.removeClass` method](http://api.jquery.com/removeClass/)
- [The `.toggleClass` method](http://api.jquery.com/toggleClass/)

## Exercises

1. Create a new file named `jquery_faq.html`. Commit all changes to GitHub.

1. In a HTML structure, create a [definition list](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl) containing 10 FAQs about national parks.

1. Add a class to all `dd` elements named `invisible`.

1. Create some CSS rules that sets elements with the `invisible` class to `visibility: hidden;`.

1. Update the page with jQuery to add a link that toggles the class `invisible` on and off for all `dd` elements.

**Bonus**

1. When any of the `dt` elements is clicked, the element that was clicked should be highlighted.
