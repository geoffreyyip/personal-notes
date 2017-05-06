### XMLHttpRequest

_(Note: The name is historical. XMLHttpRequest can be used to retrieve JSON instead of XML, and it can be used over protocols besides http.)_

`var xhr = new XMLHttpRequest()` is a common abbreviation to use. `req` and `request` are also common. This doc will use both abbreviations.

API calls should initialize XHRs with `xhr.open(method, url, async)`. `async` defaults to true. 

API calls should specify an `.onload` callback with `xhr.onload = function() {}`

API calls should specify an `.onerror` callback with `xhr.onerror = function() {}`

API calls should be sent with the `xhr.call()` method.

Successful calls should have a 3-digit `req.status` and additional info in `req.statusText`. (E.g. `req.status -> 200` or `req.statusText -> Error: Not Found`)

Successful GET requests should have info in the `req.responseText`. It will usually be a `str`  in SON format.
