# Ajax request

> [AJAX is] ... a set of web development techniques that uses various web technologies on the client-side to create asynchronous web applications. With Ajax, web applications can send and retrieve data from a server asynchronously (in the background) without interfering with the display and behaviour of the existing page. By decoupling the data interchange layer from the presentation layer, Ajax allows web pages and, by extension, web applications, to change content dynamically without the need to reload the entire page. In practice, modern implementations commonly utilize JSON instead of XML.
> <footer><cite>[Ajax (programming) - Wikipedia](http://en.wikipedia.org/wiki/Ajax_%28programming%29)

Ajax stands for Asynchronous JavaScript and XML. Ajax was originally intended to work with XML coming back from the server, but this does not mean you *must* serve up XML for the requests. As Wikipedia mention above, JSON notation has been widely adopted for sending and retrieving data for Ajax requests.

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

## JSON vs XML

As mentioned previously, most devs favor JSON over XML these days. XML is more wordy than JSON. Raw JSON is very readable.

In the following example, we request an anime quote from a web-accessible anime quote API. The quote that the API returns contains 3 pieces of information:
- Anime: Hunter X Hunter
- Character: Killua Zoldyck
- Quote: "Assassination — It's the family trade. We all take it up. My folks see me as an exceptional prospect. But I don't see that I should have to live up to their expectations."

Below is a visual comparison of what the quote data would look like returned in both XML and JSON formats.

### XML version
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<animequote>
	<anime>Hunter X Hunter</anime>
	<character>Killua Zoldyck</character>
	<quote>Assassination — It's the family trade. We all take it up. My folks see me as an exceptional prospect. But I don't see that I should have to live up to their expectations.</quote>
</animequote>
```

### JSON
```json
{
	"anime":"Hunter X Hunter",
	"character":"Killua Zoldyck",
	"quote":"Assassination — It's the family trade. We all take it up. My folks see me as an exceptional prospect. But I don't see that I should have to live up to their expectations."
}
```