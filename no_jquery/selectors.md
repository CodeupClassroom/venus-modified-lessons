# jQuery Selectors

jQuery makes selecting elements in the DOM very simple and has a powerful and
optimized selector engine.

Though there are many types of selectors available through jQuery, we will be
concentrating on the basic (and most commonly used) selectors:

|     Selector      |              Syntax               |                         Description                          |
|     --------      |              :----:               |                         -----------                          |
|    ID Selector    |               `#id`               |    Selects a single element with the given id attribute.     |
|  Class Selector   |             `.class`              |          Selects all elements with the given class.          |
| Element Selector  |             `element`             |        Selects all elements with the given tag name.         |
| Multiple Selector | `selector1[, selector2[, ...]]` | Selects the combined results of all the specified selectors. |
|   All Selector    |                `*`                |                    Selects all elements.                     |

You might notice that jQuery selectors look very similar to CSS selectors.
That's intentional! Part of the power of jQuery is that it allows us to write
(almost all) CSS selectors in our JavaScript to manipulate the DOM.

The general syntax for manipulating elements with jQuery selectors is:

```js
$('selector')
```

There are several important things to note here:

- the selector is written inside a string
- the string is inside of `$()`, which is just calling a function named `$`
- this will return us a jQuery object

`$` is a valid identifier in javascript, which means we can name our functions
or variables `$` (although you probably shouldn't). `$` is a commonly used
shorthand for `jQuery`, and using either one will accomplish the same thing. So
these two code blocks are functionally identical:

```js
$('selector')
```

```js
jQuery('selector')
```

All selector expressions return us a jQuery object, which is just an object that
represents zero or more HTML elements, with all of the methods of jQuery
attached to it. We will learn about these methods in greater detail in future
lessons. For now, we need to know the basics of two methods:

- `.html`: returns the HTML contents of selected element or the first element in
  a collection. Similar to the `innerHTML` property we previously covered.
- `.css`: allows us to change CSS properties for a given element or elements.
  Similar to the `style` property previously discussed.

You'll see both of these used in examples in the following sections where
we talk about each type of selector in more detail.

## ID Selector

The syntax for selecting an element by `id` is:

```js
$('#some-id');
```

To demonstrate this, we will start with a basic page:

```html
<h1 id="codeup">Hello Codeup</h1>
```

Next, let's add some JavaScript that uses jQuery to alert the contents of the
`h1` element.

```js
var contents = $('#codeup').html();
alert(contents);
```

Here, we used the jQuery alias `$` and an id selector to match the id `codeup`.
We then chained the `html()` method to get the contents of the HTML element as a
string and then assigned that string to the variable `content`.  We will learn
more about the `html()` method later.

Alerting the `contents` variable shows us the text inside the heading element
with the `id` of `codeup`.

## Class Selector

Classes, unlike ids, can be used multiple times within a document. Selecting
elements by class is a simple way to manipulate multiple DOM elements at one
time.

### Using the Class Selector

Consider the following HTML with several elements with the same class name.

```html
<h1>Example Page</h1>
<div class="important">
    <h3>NOTICE</h3>
    <p>This is an important message!</p>
</div>
<div class="not-important">
    <h2>Lorem Ipsum</h2>
    <p>
        Lorem ipsum dolor sit amet, incididunt ut <span class="important">labore et dolore magna aliqua.</span>, quis ut aliquip ex ea commodo.
    </p>
</div>
<p class="important">This is also very important.</p>
```

We will add some jQuery to select all the elements with a class of `important`
and use the `.css` method to change the background color to yellow.

```js
$('.important').css('background-color', 'yellow');
```

[JS Bin example](http://jsbin.com/topupe/1/edit?js,output)


## Element Selector

Selecting elements by `id` and `class` is very convenient, but sometimes we need
to get all the elements that are of the same type. The element selector allows
us to select elements based on their tag name.

### Using the Element Selector

Like the `id` and `class` selectors, we can use a simple CSS selector in our
call to jQuery:

```js
$('tag_name')
```

For example, we could get all the paragraph elements like this:

```js
$('p')
```

Looking at our previous example HTML page:

```html
<h1>Example Page</h1>
<div class="important">
    <h3>NOTICE</h3>
    <p>This is an important message!</p>
</div>
<div class="not-important">
    <h2>Lorem Ipsum</h2>
    <p>Lorem ipsum dolor sit amet, incididunt ut <span class="important">labore et dolore magna aliqua.</span>, quis ut aliquip ex ea commodo.</p>
</div>
<p class="important">This is also very important.</p>
```

We could make all of our paragraph fonts larger using jQuery. Later, when we
learn about events, we could create buttons to increase and decrease font size.

```js
$('p').css('font-size', '14px');
```

[JS Bin Example](http://jsbin.com/gayir/1/edit?js,output)

## Multiple Selector

Sometimes we want to select multiple different elements using different
selectors. While we could create a different selector for each, jQuery allows
us to use multiple selectors at one time to get a single result.

### Using the Multiple Selector

We can use the multiple selector by separating our selectors by commas:

```js
$("selector1, selector2, ...")
```

If we wanted to get all the elements with a class of `important` and all
paragraph elements, we could use this selector:

```js
$('.important, p')
```

Going back to our example HTML page, we could select all elements with a class
of `important` *and* all paragraph elements and make their background color
yellow:

```html
<h1>Example Page</h1>
<div class="important">
    <h3>NOTICE</h3>
    <p>This is an important message!</p>
</div>
<div class="not-important">
    <h2>Lorem Ipsum</h2>
    <p>Lorem ipsum dolor sit amet, incididunt ut <span class="important">labore et dolore magna aliqua.</span>, quis ut aliquip ex ea commodo.</p>
</div>
<p class="important">This is also very important.</p>
```

```js
$('.important, p').css('background-color', '#FF0');
```

[JS Bin Example](http://jsbin.com/qejeli/1/edit?output)

## The All Selector

If we want to select all the elements on a page, we can use the *all selector*:

```js
$('*')
```

A common use is to put a border around every element to help see the layout of a
page. We could create a thin red line around all of our elements using the
*all* selector and some CSS:

```js
$('*').css('border', '1px solid #F00');
```

[JS Bin Example](http://jsbin.com/fivucu/1/edit?js,output)

## Further Reading

- [The jQuery Object](http://learn.jquery.com/using-jquery-core/jquery-object/)
- [Full Documentation for Selectors](http://api.jquery.com/category/selectors/)
- [Basic Selectors](http://api.jquery.com/category/selectors/basic-css-selectors/)
    - [ID Selector](http://api.jquery.com/id-selector/)
    - [Class Selector](http://api.jquery.com/class-selector/)
    - [Element Selector](http://api.jquery.com/element-selector/)
    - [Multiple Selector](http://api.jquery.com/multiple-selector/)
    - [All Selector](http://api.jquery.com/all-selector/)

## Exercises

Continue working in `jquery_exercises.html` for these exercises. Make sure you are committing your changes to GitHub.

Id Selectors

1. Create content in your HTML file using at least the following elements: `h1`, `p`, `ul`, `li`, `div`.

1. Add several attributes to your elements; you will need both `id` and `class` attributes.

1. Use jQuery to select an element by the `id`.  Alert the contents of the element.

1. Update the jQuery code to select and alert a different `id`.

1. Use the same `id` on 2 elements. How does this change the jQuery selection?

1. Remove the duplicate `id`.  Each `id` should be unique on that page.

Class Selectors

1. Remove your custom jQuery code from previous exercises.

1. Update your code so that at least 3 different elements have the same class named `codeup`.

1. Using jQuery, create a border around all elements with the class `codeup` that is 1 pixel wide and red.

1. Remove the class from one of the elements. Refresh and test that the border has been removed.

1. Give another element an `id` of `codeup`.  Does this element get a border now?

Element Selectors

1. Remove your custom jQuery code from previous exercises.

1. Using jQuery, set the font-size of all `li` elements to `20px`.

1. Craft selectors that highlight all the `h1`, `p`, and `li` elements.

1. Create a jQuery statement to alert the contents of your `h1` element(s).

Multiple Selectors

1. Combine your selectors that highlight all the `h1`, `p`, and `li` elements.

