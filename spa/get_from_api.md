# Adding API Calls to a Jalopy Application

The Jalopy framework provides an automatic way for a route to fetch data and pass it into the view's HTML function. 

## Specifying a route's API endpoint

In `router.js`, the route entry for a screen has a field for the URL that can provide data for that route's designated screen. The `state` property of the route entry can be used for one, or more, URLs. For example:

```js
        '/dogs': {
            returnView: DogFactsView,
            state: {
                dogFacts: "https://dog-api.kinduff.com/api/facts?number=1"
            },
            uri: '/dogs',
            title: 'Dog Facts',
            viewEvent: DogFactsEvents
        },
```

Modify the dog facts route entry in `router.js` with the above change to `state`.

### The `state` property

Just before calling the HTML function for the view component, Jalopy checks the `state` property of that view's routing entry. If there are one or more URLs in the the `state` property, Jalopy uses `fetch` to send a request to each of the URLs. The resulting data from each request is packaged into a `props` object and associated with the key that is provided for its corresponding `state` entry. The final `props` object is then passed to the view's HTML function as a `props` parameter. 

For example, in the above `router.js` entry, Jalopy will fetch data from the URL https://dog-api.kinduff.com/api/facts?number=1 and store the result in a `props` object with a key of `dogFacts`. The data that is associated with the key is whatever data is returned from the API. When `DogFactsView` is called, it will receive a `props` object as a parameter that could look like this: (note that the actual dog fact is randomized)

```js
{
    dogFacts: {
        facts: 
            [
                "One female dog and her female children could produce 4,372 puppies in seven years."
            ],
        success: true
    }
}
```
From the above example, `props` is an object with one property: `dogFacts`. The `dogFacts` property has a value that is an object, containing 2 properties:
- a `facts` property that is an array containing a single string (the one dog fact).
- a `success` property that has a boolean value of true.
Jalopy DID NOT determine the structure of the data that is received from the dog facts API. The called API determines the structure of the data. Jalopy simply attaches the received data, as is, to the `props` object with the key you give the API's URL in the route entry in `router.js`.

## Working with `props`
A view's HTML function automatically receives `props` as a parameter. As stated previously, the structure of `props` depends entirely on the results returned from the view's associated APIs. 

In the dog facts example, if we wanted to display the fetched dog fact in our, our `DogFactsView` function would look like this:
```js
export default function DogFactsView(props) {
    return `
<div class="container">
    <h1>Dog Facts</h1>
    <div class="card">
        <div class="card-body">
            <p class="dog-fact" style="visibility: hidden">${props.dogFacts.facts[0]}</p>
        </div>
    </div>
    <button class="form-control" id="show-fact-btn">Show Fact</button>
</div>
`;
}
```
Note the use of the template string placeholder which injects the `props` data into the view's HTML.

## Exercise

1. Add the following URL to the `state` property of your quotes view route entry in `router.js`:
https://quotes.fulgentcorp.com:12250/api/v1/quotes?random=true&limit=10
Be sure to give it an appropriate key in `state`, like `quotes`
2. In your quote view's HTML function, remove your previous fake data. Iterate over the `props` parameter to display the quotes data from the API in your HTML. Similar to the fake data, display the quote and the quote's author.
