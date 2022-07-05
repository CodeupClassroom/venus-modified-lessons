# Ajax request

> Ajax is a group of interrelated Web development techniques used on the client-side to create asynchronous web applications. With Ajax, web applications can send data to and retrieve from a server asynchronously (in the background) without interfering with the display and behavior of the existing page. Data can be retrieved using the XMLHttpRequest object. Despite the name, the use of XML is not required (JSON is often used in the AJAJ variant), and the requests do not need to be asynchronous.
> <footer><cite>[Ajax (programming) - Wikipedia](http://en.wikipedia.org/wiki/Ajax_%28programming%29)

Ajax stands for Asynchronous JavaScript and XML. Ajax was originally intended to work with XML coming back from the server, but this does not mean you *must* serve up XML for the requests. JSON notation has been widely adopted for sending and retrieving data for Ajax requests.

## JavaScript Object Notation (JSON)

JavaScript Object Notation (JSON) is a data-interchange format that is easy for humans to read and write. It is widely used in cloud based services and web application programming interfaces (APIs).

Object literal and array literal notations were discussed in their respective sections. These same notations are used in JSON.

~~~js
// empty object in json form
{}

// empty array in json form
[]
~~~

Object properties in JSON must be double-quoted strings, and properties are separated by a commas.

~~~json
{
    "name1": "value1",
    "name2": "value2"
}
~~~

The value of a property can be one of the following:

- A quoted string
- A number
- An object
- An array
- true or false
- null

Here is an example utilizing all the possible types:

~~~json
{
    "stringProp": "stringValue",
    "numberProp": 1,
    "objectProp": {
        "prop": "value"
    },
    "arrayProp" : ["item1", "item2"],
    "arrayOfObjectsProp" : [
        {
            "prop": "value"
        },
        {
            "prop": "value"
        }
    ],
    "boolProp"  : true,
    "nullProp"  : null
}
~~~

The above example illustrates the use of different types within JSON. It also shows how objects and arrays can be nested to create complex data structures.

Note that there is no comma after the last property of an object.

You can find the [formal specification for JSON here](http://json.org/).

In the following sections, we will create and modify some JSON files by hand to get experience working with the format and the basics of Ajax requests. In the real-world, however, you will most likely not edit these files directly; rather, JSON would most likely be automatically generated, or sent and received by a web server.
