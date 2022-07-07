# Adding and removing elements

### createElement

A method on `document` that returns a new element of the given type (i.e., tag name). The element may still need to have additional properties set and is NOT automatically placed in the DOM.

`document.createElement(<element tag name>)`

```js
let myNewP = document.createElement("p");
myNewP.innerText = "I'm a new paragraph";
myNewP.classList.add("bg-primary");
```

### appendChild

Adds an element to the DOM as a child of the given parent element. 

`<parent element>.appendChild(<new child element>)`

```js
document.querySelector("#my-paragraphs").appendChild(myNewP);
```

### removeChild

Removes a given child element from the parent element.

`<parent element>.removeChild<child element to remove>)`

```js
```

### replaceChild

Replaces the first argument in the given parent with the second argument.

`.replaceChild()`

```js
```