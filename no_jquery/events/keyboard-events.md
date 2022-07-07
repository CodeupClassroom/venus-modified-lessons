# Keyboard Events

Events include keyboard events like a key being pressed or when a key is up or down.

In this lesson we will learn how to write event handlers for the following events:

- `key down` &mdash; Bind an event handler to the "keydown" JavaScript event or trigger that event on an element.
- `key press` &mdash; Bind an event handler to the "keypress" JavaScript event or trigger that event on an element. Some special keys like Escape and Backspace do not trigger this event.
- `key up` &mdash; Bind an event handler to the "keyup" JavaScript event or trigger that event on an element.
- removing an event listener

### key down

Use an event type of `keydown`.

~~~html
<p>Please type something into this form:</p>
<form>
    <input type="text" id="textfield" name="textfield" placeholder="Type Here"/>
</form>
~~~

~~~js
document.querySelector('#textfield').addEventListener('keydown', function() {
    alert('A key was pushed down!');
});
~~~

### keypress

Keypress is the same as keydown, with one small exception: modifier keys (shift,
control, escape, etc) being pressed will trigger the `keydown` event, but will
**not** trigger `keypress` events.

Use the event type of `keypress` for key press events.

### keyup

The `keyup` event is very similar to the `keydown` event, except `keyup` will trigger an event when a key is released.

~~~js
document.querySelector('#textfield').addEventListener('keyup', function() {
    alert('A key was released!');
});
~~~

## Removing a listener

The `.removeEventListener` method removes event listeners from a specified element.
We can use it to remove event listeners for a specific event.

```js
function myKeyPressFunction(event) {
    console.log("key press: " + event.code);
}

document.querySelector("body").removeEventListener("keypress", myKeyPressFunction);
```

Note: all of the event listener methods we have covered so far apply to single elements. We can add and remove event listeners to multiple elements by using the `.querySelectorAll()` method and iterating over the resulting array.

For example:

```js
document.querySelectorAll(".myBox").forEach(function (obj) {
        obj.addEventListener("mouseenter", function (event) {
            console.log("mouse enter happens");
        });
    });
```

## Further Reading

- [Full event reference](https://developer.mozilla.org/en-US/docs/Web/Events)

## Exercises

The [Konami Code](http://en.wikipedia.org/wiki/Konami_Code) was a cheat code that appeared on a number of games from Konami. [Contra](http://en.wikipedia.org/wiki/Contra_%28video_game%29) was one of the early NES games to have featured the Konami Code. Contra would start you with 3 lives per game, but after the start screen, if you entered the Konami Code, you would get 30 lives!

The Konami Code: Up, Up, Down, Down, Left, Right, Left, Right, B, A ( &uarr; &uarr; &darr; &darr; &larr; &rarr; &larr; &rarr; B A )

Create a file named `konami.html` using the template below. This code will log a number reference that is tied to each key that is entered on your keyboard. Open the console and view the output as you type on the keyboard.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Konami Code</title>
</head>
<body>
    <h1>Konami Code</h1>

    <script>
        "use strict";

        document.addEventListener("keyup", function(event) {
            console.log(event.key);
        });
    </script>
</body>
</html>
```

1. Allow the user to enter the Konami Code: "&uarr; &uarr; &darr; &darr; &larr; &rarr; &larr; &rarr; b a enter" (the return key)
1. Find the matching sequence using the code above for each key in the Konami Code.
1. Don't worry about capital `a` or `b` just check for lowercase.
1. After the correct Konami Code sequence is inputted, have the script alert the user:
__"You have added 30 lives!__ Other ideas:
    1. Change the background screen.
    1. Play a sound.
    1. Be creative!
1. Happy Playing[.](https://www.youtube.com/watch?v=g_kMGkGYNJM)
