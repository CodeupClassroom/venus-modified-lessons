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

### An alternative method for modifying the DOM

In many situations, you can also modify child elements by directly modifying the `innerHTML` property of the parent element. 

For example:

```js
const newP = `
<p class="bg-warning">I am not a real Paragraph until innerHTML changes</p>
`;
document.querySelector("#my-paragraphs").innerHTML += newP;
```

While this may be a simpler approach for creating elements, it is not necessarily simpler for removing or replacing elements. Furthermore, you cannot affect the nodes directly until after the `innerHTML` property is changed. The elements do not exist in the DOM until `innerHTML` changes.

## Further Reading

[MDN removeChild](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild)

[MDN replaceChild](https://developer.mozilla.org/en-US/docs/Web/API/Node/replaceChild)

## Exercise

1. Download [THIS FILE](dom_add_remove.html) into your codeup-web-exercises project.
3. When the user clicks the "Add Todo" button, add a new Todo card as a child of the div with id `my-todos`. 
    -   The text in the input with id `add-todo-text` should be the text of the newly added todo. 
    -   The new todo MUST have a button (or something similar) labeled "Delete Todo". When a delete button is clicked, it deletes the todo in which the button is contained. Whenever a new todo is created, you must add an event listener to the new delete button that will call the deleteTodo function.
    -   Use the HTML of the starting todo in the code as a model for additional todos. 
    -   There are a couple of ways you can add the todo.
        i.  use `document.createElement()` and work with DOM element references. This is kind of a long way as there are several embedded divs.
        ii.  concatenate the HTML text of the new card to the `innerHTML` of `my-todos`. This way is very simple *BUT* you can't modify the elements until after the `innerHTML` property is changed AND you will have to re-add the click listener for ALL the todos. Setting the `innerHTML` property of `my-todos` removes the previous listeners.
4. When a todo's delete button is clicked, it removes the todo div (i.e., the card) in which the button is contained. 
    -   Use DOM traversal to delete the card containing the clicked button. Because we used a pretty Bootstrap card for the todo, we have travel a little ways up the DOM tree to get at the delete button's containing card!
    -   IMPORTANT: Be sure to add the delete button listener to the first todo that is already in the DOM when the page loads!
5. Let's modularize the code nicely. Put all of the code responsible for creating a new todo in the function called `addTodo` and all the code responsible for deleting a todo in the function called `deleteTodo`. Your handlers will call those functions. You don't need to pass anything into `addTodo`. Use the automatically provided `event` parameter in `deleteTodo` to get a reference to the delete button that clicked. Use `event.target` for that.

And YES, you can change the way this exercise looks! Make it pretty!

### Bonus
1.  If the user tries to add a todo without any text, tell the user that the todo's text cannot be blank and do not add the todo. This is called a `validator`.
2. Only allow 10 todos. If the user tries to add more than 10 todos, they are presented with a snarky response along the lines of "Try making a Todone first."


Read up on event.target. It's handy!
[MDN Event.target](https://developer.mozilla.org/en-US/docs/Web/API/Event/target)