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
const myOtherP = document.querySelector("#my-other-p");
document.querySelector("#my-paragraphs").removeChild(myOtherP);
```

### replaceChild

Replaces the SECOND argument (old child) in the given parent with the FIRST argument (new child).

`<parent element>.replaceChild(<new child>, <old child>)`

```js
// replace myNewP with myAwesomeP
let myAwesomeP = document.createElement("p");
myAwesomeP.innerText = "I'm an EVEN BETTER paragraph";
myAwesomeP.classList.add("bg-danger");
document.querySelector("#my-paragraphs").replaceChild(myAwesomeP, myNewP);
```

## Further Reading

[MDN removeChild](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild)

[MDN replaceChild](https://developer.mozilla.org/en-US/docs/Web/API/Node/replaceChild)

## Exercise

TODO: a really cool exercise to practice how to really manipulate the DOM!