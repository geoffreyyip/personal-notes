## `XMLHttpRequest` 

*Note: The name is historical. ``XMLHttpRequest` ` can be used to retrieve `JSON` instead of `XML`, and it can be used over protocols besides http.*

*Unless otherwise stated, "fetch" refers to a generic high-level process, not the Fetch API. A request goes in, and a response comes out. The abstraction is simple, but the low-level details are not. The request can be of any method. The response can be of any data type, though it's usually text in `JSON` format.*

*`XMLHttpRequest`  can be used to fetch information from a server. Fetching is not limited to GET requests. Rather it should be thought as the glue that binds requests and responses. `XMLHttpRequest`  API has similiar functionality to the Fetch API. In fact, `XMLHttpRequest`  initiates the same low-level fetching algorithm.*

`var xhr = new XMLHttpRequest()` is a common abbreviation to use. req, request, and client are also common. This doc will alternate between them.

API calls should initialize XHRs with `xhr.open(method, url, async)`. async defaults to true. 

API calls should specify an `.onload` callback with `xhr.onload = function() {}` or `xhr.onload = handler`

API calls should specify an `.onerror` callback with `xhr.onerror = function() {}`

API calls should be sent with the `xhr.call()` method.

Successful calls should have a 3-digit req.status and additional info in `req.statusText`. (E.g. `req.status` -> 200 or `req.statusText` -> `"Error: Not Found"`)

Successful GET requests should have info in the `req.responseText`. It will usually be a str  in `JSON` format.

An example of all of the above:

```javascript
function requestHandler() {
  if (this.status === 200 && this.responseText != null) {
    // success!
    var data = `JSON`.parse(this.responseText);
      }
  else {
    // we reached the server, but there is no response
  }
}

var client = new XMLHtmlRequest();
client.onload = requestHandler;
client.open('GET', 'foobar.html');
client.send();
```

### Lifecycle of a XMLHttpRequest

`client.readyState` must return one of the following values:

- `0` or unsent
- `1` or opened
- `2` or headers_received
- `3` or loading
- `4` or done

**unsent** means when the request has just been constructed.

**opened** means when all the request parameters (open(method, url)) have been set, but the request has not been sent (in other words, set but not yet sent)

**headers_received** means the request has finished all its redirects, and the client has received the complete headers information

**loading** means the request is currently loading the response body

**done** means the request-response cycle is complete (can indicate success or failure)

`XMLHttpRequest` s can only be sent once. 

readystatechange event will fire at least once each time the `XMLHttpRequest`  object changes states. It may fire multiple time in the `send()` method for compatibility reasons.

**load** will fire when the full request is completed and completed successfully. It will not fire while the response body in in the middle of loading, and it will not fire when the request fails. Usually, this indicates the transition between loading and done.

### XMLHttpRequest Upload

All `XMLHttpRequest`  objects have an associated `XMLHttpRequestUpload` object defined within. It records no information by default. You can set a flag to gain transmission info upon process request end-of-body, or when the full request has been sent to the servers.

### Response attributes

There is no centralized response attribute. Instead you have a collection of mostly readonly variables like `responseText`, `responseType`, and `getAllRequestHeaders`.

(There is, in fact, a request.response attribute, but it sits alongside the other properties. `request.response` returns `responseText` when `responseType === 'text'` and returns an object otherwise. There are a few objects that can be returned, but those are usually not important to know. Note that response refers to the body, and does not include the headers.)

## Fetch API

*Unless otherwise stated, "fetch" refers to a generic high-level process, not the Fetch Standard or the Fetch API. A request goes in, and a response gets returned. The abstraction is simple, but the low-level details are not. The request can be of any method. The response can be of any data type, though it's usually text in `JSON` format.*

*There are three interfaces. `Request`, `Response`, and `Headers` . When capitalized and code-fenced, these words refer to the Javascript objects.*

### Description

Fetch API is a promise-based version of `XMLHttpRequest`. Ultimately, they're two different ways of implementing the same AJAX functionality and it's really a matter of preference. Arguments for Fetch API are the cleaner interface and the separation of concerns.

### Two Gotchas

* The Promise returned from `fetch()` will **not** reject on HTTP error status (e.g. 404 error codes). It will resolve normally, but with `res.ok` set to false. 
* By default, `fetch()` will **not send or receive cookies** unless a credentials header is specified. This helps with CORS.

### `Headers`  Interface

Constructing a `Headers` object from scratch is simple. Pass in either a hash map or an array of ordered pairs. This data will get set as the header list property of the `Headers` object.

```Javascript
var meta = { 
  "Content-Type": "text/xml", 
  "Breaking-Bad": "<3" 
}
new Headers(meta)

// The above is equivalent to
var meta = [
  [ "Content-Type", "text/xml" ],
  [ "Breaking-Bad", "<3" ]
]
new Headers(meta)
```

`Headers` will also have a **guard** property that sets various limitations on what mutation operations are allowed on the `Headers` object. `none` is the default for a constructed object. 

### `Request` Interface

`Request` objects have an associated **request**, `Headers` and a **body**. **request** and **body** are defined below in the [Fetch Standard](#fetch-standard).

`new Request()`  accepts one or two arguments:

- **input**, typically a URL. 
- **init**, an optional argument that contains custom settings to apply to the request

Commonly used options are `method`, `headers`, `body`, `mode`, and `credentials`.

`mode` defaults to `cors`. `method` defaults to GET. `credentials` defaults to false.

You may want to change `mode` to either `no-cors` or `same-origin`. You may want to change `method` to POST or PUT or HEAD. You may want to change `credentials` to `include`.

### `Response` Interface

`Response` objects have an associated **response**, `Headers` and a **body**. **response** and **body** are defined below in the [Fetch Standard](#fetch-standard).

You rarely need to construct a `Response` from scratch.

Neat trick: `response.ok` is a succint way of checking if a **response** came back with a successful HTTP code.

```javascript
// the new way
if (res.ok) {
  console.log('success');
}

// the old way
if (res.status >= 200 && res.status < 300) {
  console.log('success')
}
```

An even neater trick is that `Response` has built-in transform methods to return `.json(),` `.text()`, `.blob()`, etc.

```javascript
// the new way
const data = res.json();

// the old way
const data = JSON.parse(res.responseText);
```

### `Fetch()`, the part you actually care about

`fetch()` accepts one or two arguments:

* **input**, typically a URL. 
* **init**, an optional argument that contains custom settings to apply to the request

`fetch()` returns a `Promise` that resolves to a `Response`.

Note, this is the exact same interface as `Request`. That's because `fetch()` actually generates a `Request` for you, and then does stuff on top of that.

You can pass a pre-defined `Request` to `fetch()`. It creates an almost identical copy, but with some properties changed for security.

```Javascript
// How to GET data
const user = 'trevordmiller';
const url = `https://api.github.com/users/${user}`;

fetch(url)
  .then(response => response.json())
  .then(json => dataHandler)
  .catch(error => console.log(error));

// How to POST data
const url = 'https://randomuser.me/api';

let data = {
  name: 'Sara'
};

let fetchData = {
  method: 'POST',
  body: data,
};

fetch(url, fetchData)
  .then(res => responseHandler);
```

## Fetch Standard

*Unless otherwise stated, "fetch" refers to a generic high-level process, not the Fetch Standard or the Fetch API. A request goes in, and a response gets returned. The abstraction is simple, but the low-level details are not. The request can be of any method. The response can be of any data type, though it's usually text in `JSON` format.*

*There are three interfaces. `Request`, `Response`, and `Headers` . When capitalized and code-fenced, these words refer to the Javascript objects. Generally, there's no reason to construct them directly. You'll usually interact with these objects through the Fetch API. `fetch()` is used to initiate a `Request`, return a `Response`, and modify the `Headers`  list for both `Request` and `Response`. They're useful for offline purposes though.*

### Methods

By default, `Request` is set to `mode: "no-cors"`, or **no cross-origin**. For these, you may only send GET, HEAD or POST methods. 

To change the `mode`, you must specify it as an option in your second parameter to the `Request` constructor. 

```Javascript
var req = new Request('example.html', { mode: "cors" });
```

### Headers

**Header** refers to a name-value pair. **Headers** refers to a **header list**, or a collection of name-value pairs. There can be duplicate keys.

By default, `Request` is set to `mode: "no-cors"`, or **no cross-origin**. You are limited in what headers you may send, and you are limited in what headers you will receive back.

Some headers are **forbidden header names** in that they cannot be modified programmatically. They can only be changed by the user agent. (Technically, you can modify `Header` properties if you're creating the object from scratch and `guard == 'none'`. But there's usually no reason to. And once you send the `Request`, `guard` will automatically get set as something else. And you can't modify `guard` once it's been set.)

### Bodies

A body consists of:

* **Stream**
* **Transmitted Bytes**
* **Total Bytes**
* **Source**

**Stream** and **Source** are initially null. Both **Bytes** variables are initially 0.

### Requests

Requests are inputs to the Fetch specification. Request parameters must be set before being sent from the client. Some parameters may change during redirects. (E.g. Methods can change to `GET` during redirects.)

### Responses

Responses are results of the Fetch specification. A response will evolve over time and so not all fields will be available right away. 

Typically, a response will queue **process response**, **process response end-of-body**, and **process response done** tasks.

### CORS Protocol

This allows for sharing response across different origins, and adds functionality on top of the existing HTTP protocol. Both client and server must supply `Access-Control-*` parameters to make `CORS` work.

