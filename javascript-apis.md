## `XMLHttpRequest` 

(Note: The name is historical. ``XMLHttpRequest` ` can be used to retrieve `JSON` instead of `XML`, and it can be used over protocols besides http.)

Unless otherwise stated, "fetch" refers to a generic high-level process, not the Fetch API. A request goes in, and a response comes out. The abstraction is simple, but the low-level details are not. The request can be of any method. The response can be of any data type, though it's usually text in `JSON` format.

`XMLHttpRequest`  can be used to fetch information from a server. Fetching is not limited to GET requests. Rather it should be thought as the glue that binds requests and responses. `XMLHttpRequest`  API has similiar functionality to the Fetch API. In fact, `XMLHttpRequest`  initiates the same low-level fetching algorithm.

`var xhr = new XMLHttpRequest()` is a common abbreviation to use. req, request, and client are also common. This doc will alternate between them.

API calls should initialize XHRs with `xhr.open(method, url, async)`. async defaults to true. 

API calls should specify an `.onload` callback with `xhr.onload = function() {}` or `xhr.onload = handler`

API calls should specify an `.onerror` callback with `xhr.onerror = function() {}`

API calls should be sent with the `xhr.call()` method.

Successful calls should have a 3-digit req.status and additional info in `req.statusText`. (E.g. `req.status` -> 200 or `req.statusText` -> `"Error: Not Found"`)

Successful GET requests should have info in the `req.responseText`. It will usually be a str  in `JSON` format.

An example of all of the above:

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

