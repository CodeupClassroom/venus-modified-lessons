# Selectors

There are a variety of ways to select elements in the DOM. We will cover methods for retrieving elements by their id, class, or tag name. We will also cover the direct access of form inputs.

`document.getElementById()`

`document.getElementsByClassName(<class>)`

`document.getElementsByTagName(<tag>)`

`document.querySelector(<selector>)`

`document.querySelectorAll(<selector>)`

`document.forms.<form name>.<form field name>`



[Click this for the original v2 DOM lesson](https://java.codeup.com/javascript-i/bom-and-dom/dom/)


### Venus: do NOT go below this point for this lesson

TODO: the below exercise needs to be split up between the selectors and properties lessons

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

