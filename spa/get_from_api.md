# Adding API Calls to a Jalopy Application

The Jalopy framework provides an automatic way for a route to fetch data and pass it into the view's HTML function. 

## Specifying a route's API endpoint

In `router.js`, the route entry for a screen has a field, called `state`, for one or more URLs that are endpoints for providing data to the route's designated screen. Furthermore, an endpoint can be specified with a custom header that will be sent to the API as part of the request. A common use-case for this is providing an API key to the API's endoint. For example:

```js
        '/dogs': {
            returnView: DogFactsView,
            state: {
                dogFacts: {
                    url: "https://dogfacts.fulgentcorp.com:12250/v1/facts?random=true&limit=10",
                    headers: {
                        'Accept': 'application/json',
                        'Authorization': DOG_QUOTE_API_KEY
                    }
                }
            },
            uri: '/dogs',
            title: 'Dog Facts',
            viewEvent: DogFactsEvents
        },
```

The "Accept" header tells the API that the requesting application wants the response to be in a JSON format. The "Authorization" header provides the API key to the server for identification. 

Modify the dog facts route entry in `router.js` with the above change to `state`.

### The `state` property

Just before calling the HTML function for the view component, Jalopy checks the `state` property of that view's routing entry. If there are one or more URLs in the the `state` property, Jalopy uses `fetch` to send a request to each of the URLs. The resulting data from each request is packaged into a `props` object and associated with the key that is provided for its corresponding `state` entry. The final `props` object is then passed to the view's HTML function as a `props` parameter. 

For example, in the above `router.js` entry, Jalopy will fetch data from the URL https://dogfacts.fulgentcorp.com:12250/v1/facts?random=true&limit=10 and store the result in a `props` object with a key of `dogFacts`. The data that is associated with the key is whatever data is returned from the API. When `DogFactsView` is called, it will receive a `props` object as a parameter that could look like this: (note that the actual dog fact may be randomized)

```json
{
    dogFacts: 
        [
            "All dogs can be traced back 40 million years ago to a weasel-like animal called the Miacis which dwelled in trees and dens. The Miacis later evolved into the Tomarctus, a direct forbear of the genus Canis, which includes the wolf and jackal as well as the dog.",
            "Ancient Egyptians revered their dogs. When a pet dog would die, the owners shaved off their eyebrows, smeared mud in their hair, and mourned aloud for days.",
            "Small quantities of grapes and raisins can cause renal failure in dogs. Chocolate, macadamia nuts, cooked onions, or anything with caffeine can also be harmful."
        ]
}
```
From the above example, `props` is an object with one property: `dogFacts`. The `dogFacts` property has a value that is an array containing an array of strings. Each element in the array is a dog fact from the API.

Jalopy DID NOT determine the structure of the data that is received from the dog facts API. The called API determines the structure of the data. Jalopy simply attaches the received data, as is, to the `props` object with the key you give the API's URL in the route entry in `router.js`.

## Working with `props`
A view's HTML function automatically receives `props` as a parameter. As stated previously, the structure of `props` depends entirely on the results returned from the view's associated APIs. 

In the dog facts example, if we wanted to display some of the fetched dog facts in our, our `DogFactsView` function would look like this:
```js
export default function dogFactsHTMLFunction(props) {
    return `
<div class="container">
    <h1>Dog Facts</h1>
    <div class="card">
        <div class="card-body">
            <p class="dog-fact" style="visibility: hidden">${props.dogFacts[0]}</p>
        </div>
    </div>
    <div class="card">
        <div class="card-body">
            <p class="dog-fact" style="visibility: hidden">${props.dogFacts[1]}</p>
        </div>
    </div>
    <div class="card">
        <div class="card-body">
            <p class="dog-fact" style="visibility: hidden">${props.dogFacts[2]}</p>
        </div>
    </div>
    <button class="form-control" id="show-fact-btn">Show Fact</button>
</div>
`;
}
```
Note the use of the template string placeholder which injects the `props` data into the view's HTML.

## Improving the HTML Function

Instead of manually placing single elements into the screen's HTML, let's use a loop to automate the inserting of dog facts.

```js
export default function dogFactsHTMLFunction(props) {
    let html = `
<div class="container">
    <h1>Dog Facts</h1>
    `;
    for(let i = 0; i < props.dogFacts.length; i++) {
        html += `
    <div class="card">
        <div class="card-body">
            <p class="dog-fact" style="visibility: hidden">${props.dogFacts[i]}</p>
        </div>
    </div>
        `
    }
    html += `
    <button class="form-control" id="show-fact-btn">Show Fact</button>
</div>
`;
    return html;
}
```

## Cleaning up the HTML function

Using a loop to add all of the dog facts to the HTML is swell, but the resulting code is kind of ugly, especially with all of that HTML mixed in with JavaScript. Let's pull out the card creation code into separate functions so that each function is very specialized and its code only focuses on a single level of detail. E.g., 
- the HTML function focuses on putting together the overall HTML for the `DogFacts` screen. 
- the next level of HTML assembly detail is a function that focuses on putting together the HTML for all of the dog fact cards and will return the result to the higher-level HTML function.
- the deepest level of detail is a function that focuses on putting together the HTML for a SINGLE dog fact card.

Notice how the level of focus goes from high-level (the HTML function itself), to low-level (how to create the HTML for a single dog fact card). The higher-level functions depend on the lower-level functions BUT DO NOT care about the details of HOW the lower-level functions do what they do. 

Here is the resulting code for the HTML function and its two supporting functions:

```js
// highest level of detail: the HTML function
// focuses on the HTML for the overall screen 
export default function dogFactsHTMLFunction(props) {
    return `
<div class="container">
    <h1>Dog Facts</h1>
    
    ${makeDogFactCards(props.dogFacts)}
    
    <button class="form-control" id="show-fact-btn">Show Facts</button>
</div>
`;
}

// next lower level of detail: the function for all of the card HTML
// uses a loop to iterate over the array and concats each card as it is made
// by the lowest level function
function makeDogFactCards(dogFacts) {
    let html = "";
    dogFacts.forEach(function(dogFact) {
        html += makeDogFactCard(dogFact);
    });
    return html;
}

// the lowest level function: assembles the HTML for a single dog fact card
// using the dog fact that is passed in as a parameter
function makeDogFactCard(dogFact) {
    return `
<div class="card">
    <div class="card-body">
        <p class="dog-fact" style="visibility: hidden">${dogFact}</p>
    </div>
</div>
        `;
}
```


## Exercise

1. Add the following URL to the `state` property of your quotes view route entry in `router.js`:
https://quotes.fulgentcorp.com:12250/v1/quotes?random=true&limit=10
Be sure to give it an appropriate key in `state`, like `quotes`
2. In your quote view's HTML function, remove your previous fake data. Iterate over the `props` parameter to display the quotes data from the API in your HTML. Similar to the fake data, display the quote and the quote's author.

## Next Lesson

[Inserting data using an API](add_to_api.md)