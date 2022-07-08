# DOM Traversal Techniques

There are times when you may need to reference an HTML element from another element. Because the DOM is expressed as a tree of nodes (i.e., HTML elements), you can reach any element from any other element. Be aware though that DOM trees can be VERY extensive, so it may involve some pretty complex statements. In these situations, consider using `.getElementById()` or `.querySelector()` instead.

![Example of DOM tree](pic_htmltree.gif)
Example of DOM tree from: https://www.w3schools.com/jsref/prop_element_children.asp

## Elements vs. Nodes
The DOM is a complex creature. It allows us to store and access information beyond just the HTML elements, capturing a variety of information in an HTML document as generic `nodes`. A DOM `node` can be an HTML element, or other things like text or comments. In this course, we are only interested in HTML elements, and will be using element versions of the DOM's traversal properties. For more information about DOM nodes, please see the Further Reading section at the end of this lesson.

## Traversing HTML Elements using JavaScript

The traversing techniques covered in this lesson are:

- `.parentElement` &mdash; Get the parent of an element.
- `.children` &mdash; Get the children of an element.
- `.firstElementChild` &mdash; Access the first child of an element's children.
- `.lastElementChild` &mdash; Access the last child of an element's children.
- `.nextElementSibling` &mdash; Access the element's immediately following sibling.

### .parentElement

> Get the parent of an element.

```js
let box1 = document.querySelector("#box1");
let parent = box1.parentElement;
```

`.parentElement` only returns the parent (or ancestor) one level above the selected element. If you wanted to access an element's grandparent, you can add it's parentElement in the statement. E.g.,

```js
let box1 = document.querySelector("#box1");
let grandParent = box1.parentElement.parentElement;
```


Consider the following HTML structure:

```html
<h1>Parks List</h1>
<div id="national-parks-div">
    <h3 id="national-parks-heading">National Parks</h3>
    <ul id="national-parks">
        <li>Arches</li>
        <li>Badlands</li>
        <li>Carlsbad Caverns</li>
        <li>Denali</li>
        <li>Everglades</li>
    </ul>
</div>
<div id="state-parks-div">
    <h3 id="state-parks-heading">State Parks of Texas</h3>
    <ul id="state-parks-texas">
        <li>Abilene</li>
        <li>Big Bend</li>
        <li>Choke Canyon</li>
        <li>Davis Mountains</li>
        <li>Enchanted Rock</li>
    </ul>
</div>
```

We can change the background color of the state parks `ul` parent (i.e., the containing `div`) with the following code:

```js
let stateParksUL = document.querySelector("#state-parks-texas");
stateParksUL.parentElement.style.backgroundColor = "yellow";
```

### .children

> Get the children of an element.

The `.children` property provides access to all of an element's direct child elements. The children are accessible as an array-like structure using normal array indexing. HOWEVER, you cannot use the `.forEach()` method on `.children`. Thus, if you want to process ALL of an element's children, use a basic `fori` loop on the element's `.children` property.

The syntax to access an element's children is: 

```js
let myChildren = myElement.children;
```

Consider the following HTML structure:

```html
<h1>Parks List</h1>
<div id="national-parks-div">
    <h3 id="national-parks-heading">National Parks</h3>
    <ul id="national-parks">
        <li>Arches</li>
        <li>Badlands</li>
        <li>Carlsbad Caverns</li>
        <li>Denali</li>
        <li>Everglades</li>
    </ul>
</div>
<div id="state-parks-div">
    <h3 id="state-parks-heading">State Parks of Texas</h3>
    <ul id="state-parks-texas">
        <li>Abilene</li>
        <li>Big Bend</li>
        <li>Choke Canyon</li>
        <li>Davis Mountains</li>
        <li>Enchanted Rock</li>
    </ul>
</div>
```

To change the font weight of all of the state park `li` elements, we could use the following code:

```js
let stateParksLIs = document.querySelector("#state-parks-texas").children;
for(let i = 0; i < stateParksLIs.length; i++) {
    stateParksLIs[i].style.fontWeight = "bold";
}
```


### .firstElementChild

> Provides access to the first child of a given element.

The syntax is simply `.firstElementChild`.

Using the example HTML from above, we could change the background color of the first state park `li` using this code:

```js
let stateParksUL = document.querySelector("#state-parks-texas");
stateParksUL.firstElementChild.style.backgroundColor = '#FF0';
```

### .lastElementChild

> Provides access to the last child of a given element.

The syntax is simply `.lastElementChild`.

Using the example HTML from above, we could change the background color of the last state park `li` using this code:

```js
let stateParksUL = document.querySelector("#state-parks-texas");
stateParksUL.lastElementChild.style.backgroundColor = 'goldenrod';
```



### .nextElementSibling

> Get the immediately following sibling of a given element.

Similar to the other traversal techniques, the syntax is simply `.nextElementSibling`

If we wanted to give the `ul` with the id of `national-parks` a salmon
background color when the sibling h3 is clicked, we could do it like so:

```html
<h1>Parks List</h1>
<div id="national-parks-div">
    <h3 id="national-parks-heading">National Parks</h3>
    <ul id="national-parks">
        <li>Arches</li>
        <li>Badlands</li>
        <li>Carlsbad Caverns</li>
        <li>Denali</li>
        <li>Everglades</li>
    </ul>
</div>
<div id="state-parks-div">
    <h3 id="state-parks-heading">State Parks of Texas</h3>
    <ul id="state-parks-texas">
        <li>Abilene</li>
        <li>Big Bend</li>
        <li>Choke Canyon</li>
        <li>Davis Mountains</li>
        <li>Enchanted Rock</li>
    </ul>
</div>
```

```js
let natlParksHeading = document.querySelector("#national-parks-heading");
natlParksHeading.nextElementSibling.style.backgroundColor = "salmon";
```

While you could also target the `ul` with it's id, the above example
demonstrates how to use the `.nextElementSibling` property.

## Further Reading

Documentation for the techniques covered in this lesson:

- [`DOM element child methods`](https://www.w3schools.com/jsref/prop_element_children.asp)

- [`DOM navigation`] (https://www.w3schools.com/js/js_htmldom_navigation.asp)

## Exercises

1. Open the file named [parks.html](parks.html) for editing.  Commit all changes to
   GitHub.

1. Under the FAQ, add 3 unordered lists like above.  Each list should contain a
   national park name in an `h3` element, and a `ul` of 4 facts about the park.

1. Create a button that, when clicked, makes the last `li` in each `ul` have a
   yellow background.

1. When any `h3` is clicked, the `li`s underneath it should have a `fontWeight` of "bold". Hint: you should use `this` in the event listener to access the next sibling of the `h3` that is clicked. You can get to the corresponding `ul` by traversing to the `h3`'s sibling and then it's children.

1. When any list item is clicked, the first `li` of that list item's parent `ul` should have a
   font color of blue. Hint: you should again rely on `this` in the `li` event handler.

### Bonus

Create 3 divs that each look like a picture frame. Each one should have a unique background image and a button underneath that swaps the image from the frame. Use traversing properties to accomplish this.

The rules are the following:

- When the left picture's button is clicked, the left and center pictures swap images.
- When the center picture's button is clicked, the center picture swaps randomly with either the left OR right pictures.
- When the right picture's button is clicked, the right and center pictures swap images.

Hint: swap the values of the images' `src` attributes.

[See here for a demonstration.](/img/traversing-bonus.gif)
