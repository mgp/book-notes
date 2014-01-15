## Bulletproof Ajax

by Jeremy Keith

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/Bulletproof-Ajax-Jeremy-Keith/dp/0321472667)!*

### Chapter 1: What is Ajax?
* pg 8: A tiny, practically invisible inline frame, or hidden `iframe`, can get data from the server; Javascript can use that data to update the main page.
* pg 9: Ajax isn't tied to XML -- you can use any structured format that carries data.

### Chapter 2: JavaScript and the Document Object Model
* pg 19: Variables assigned `null` and `0` are treated as `false` in boolean expressions.
* pg 24: Use `===` to check if two values are not just equal but identical; use `!==` to check if they are not identical.
* pg 29: Functions can be assigned to variables; to store a reference to a function with arguments defined, use an anonymous function.
* pg 31: Functions and methods belonging to a class are called methods and properties.
* pg 32: Native objects are part of the core Javascript language; host objects are provided by the environment in which JavaScript is running.
* pg 39: Calling `getElementsByTagName("*")` returns only element nodes, while the `childNodes` property contains text nodes and attributes.

### Chapter 3: XMLHttpRequest
* pg 52: For `XMLHttpRequest`, the `onreadystatechange` event handler is triggered everytime the `readyState` property changes.
* pg 55: The `open` method will not initiate a request; the `send` method must be invoked.
* pg 56: If POSTing data, use the `setRequestHeader` method to set the `Content-type` header to `application/x-www-form-urlencoded`.
* pg 57: Every browser finishes with a `readyState` value of `4` when the request is completed.
* pg 59: If the server responds with MIME type `text/xml` or `application/xml` the response is in the `responseXML` property instead of `responseText`.

### Chapter 4: Data Formats
* pg 73: Returned XML data can be traversed using the DOM methods you would use to traverse an HTML document.
* pg 77: JSON, pronounced like the name Jason, does not need to be interpreted by JavaScript because it is JavaScript.
* pg 80: The JSON stored in `responseText` is interpreted as an object literal and converted using the `eval` function.
* pg 82: The `send` method of `XMLHttpRequest` can only access the domain serving up the Javascript file being executed.
* pg 83: Using DOM scripting, you can dynamically create a `script` element with a Javascript URL from another domain; that script can modify your DOM.
* pg 89: HTML fragments returned via Ajax can be inserted wholesale into your document by assigning the `innerHTML` property of an element.

### Chapter 5: Hijax
* pg 98: Use the `onload` event handler to invoke the DOM methods in your JavaScript only once the page has finished loading.
* pg 103: After building a web page without Ajax, apply Ajax to scenarios where a page is reloaded but only partially modified.
* pg 112: Given a `form` element, its `elements` property contains all its inputs.
* pg 115: Do not rely on Javascript for form validation, but use it to get and display a validation response from the server.

### Chapter 6: Ajax Challenges
* pg 124: If you're not going to try for graceful degradation, consider technologies beyond Ajax like Flash.
* pg 126: Applications using Ajax should provide feedback to the user that some action is under way.
* pg 130: It's also good to show that something just happened, using something like the yellow fade technique.
* pg 136: If the user would want to bookmark the changed state of a page, don't replace full page refreshes with Ajax.
* pg 137: With Ajax, wireframes representing web page must be ditched for prototypes that add dynamic elements through simple DOM scripting.

### Chapter 7: Ajax and Accessibility
* pg 141: Most screen readers don't interact with the DOM, but take a snapshot of the document and put it in a "virtual buffer" for manipulation.
* pg 143: Screen readers show a bias toward the creating or updating of tables, and focusable elements like form fields and links.
* pg 143: Assign an element a tabindex of `-1` to make it focusable and attract the screen reader without upsetting the default tab order.
* pg 144: Put an opt-in checkbox off-screen that only screen-readers can find and toggle, which generates alert messages for Ajax updates.
* pg 149: Web browsers can't detect screen-readers, but Flash can, and can notify JavaScript of this.

### Chapter 8: Putting It All Together
* pg 171: Functions within an object can access variables declared within the same object using closures.
* pg 182: In the `onreadystatechange` method, display an error message with DOM scripting if the status code is not `200` or `304`.
* pg 184: Use `setTimeout` to call abort on the `XMLHttpRequest` object if the request is taking too long, and `clearTimeout` if the request completes.

### Chapter 9: The Future of Ajax
* pg 193: If you use a JavaScript library, consider its file size, as the client will need to download whatever you choose.

