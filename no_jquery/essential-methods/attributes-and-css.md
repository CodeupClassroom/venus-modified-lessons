# Attributes and Properties

jQuery provides many different methods for manipulating element attributes, but we will be covering just the most commonly used methods.

`.innerHTML`

`.innerText`

`.hasAttribute(<attribute name>)`

`.getAttribute(<attribute name>)`

`.setAttribute(<attribute name>, <value>)`

`.classList`

`.classList.add(<class name>)`

`.classList.remove(<class name>)`

`.classList.toggle(<class name>)`


## CSS attributes

`.style`

This accesses INLINE STYLES ONLY for the element. 




[Click this for the original v2 DOM lesson](https://java.codeup.com/javascript-i/bom-and-dom/dom/)

### Venus: do NOT go below this point for this lesson

TODO: be sure to cover classList.toggle(<classname>);

## Exercises

1. Create a new file named `parks.html`. Commit all changes to GitHub.

1. In a HTML structure, create a [definition list](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl) containing 10 FAQs about national parks.

1. Add a class to all `dd` elements named `invisible`.

1. Create some CSS rules that set elements with the `invisible` class to `visibility: hidden;`. What is the difference between `visibility: hidden;` and `display: none;`?

1. Add an anchor on the page and JavaScript that will toggle the class `invisible` on and off for all `dd` elements.

**Bonus**

1. When any of the `dt` elements is clicked, the element that was clicked should have a background color of "green".
