## ppk on JavaScript, 1/e

by Peter-Paul Koch

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/ppk-JavaScript-1-Peter-Paul-Koch/dp/0321423305)!*

### Chapter 1: Purpose
* pg 6: A fat client emphasizes JS and reduces load times, thereby increasing usability; a thin client uses little JS and thereby increases accessibility.
* pg 10: ECMA standardized the JS core language, but the W3C created the DOM specification which includes event handling and CSS modification.
* pg 15: JS cross-window communication is allowed only when the pages in both windows come from the same domain (not even related subdomains).
* pg 15: Documents in different subdomains can communicate with each other by setting document.domain to some value part of their real domains.

### Chapter 2: Context
* pg 29: In 2002 Stuart Langridge coined the term "unobtrusive scripting" which is where the JS will hook itself into a page for enhanced usability.
* pg 35: Add and remove DOM elements in a form with JS instead of simply hiding them because hidden fields are submitted too.
* pg 38: Don't use the `javascript` pseudo-protocol link which is a no-op for noscript users; instead, add an `onclick` handler with a default value.
* pg 41: The JS event equivalents of CSS `:hover` are not `mouseover` and `mouseout`, but `mouseenter` and `mouseleave` which are only supported by IE.
* pg 41: A CSS dropdown menu triggered by a `li:hover` style cannot accommodate keyboard users because they can't focus on `li`, whereas a JS one could.
* pg 46: No `mouseover` event will ever trigger if the user doesn't use a mouse -- to assist keyboard users, use a `focus` event.
* pg 47: Screen readers read a page from top to bottom, so modifying a part of the page that is already read will be missed by the reader.
* pg 53: If you have a link with an `onclick` handler but no sensible `href` value, just generate the link with JS so noscript users never see it.
* pg 55: If redirecting a user, use `location.replace()` instead of assigning `location.href` to gracefully handle the use clicking the Back button.
* pg 57: Don't use the `noscript` tag -- the user agent may know enough JS to ignore it, but not all the functionality you actually need.

### Chapter 3: Browsers
* pg 69: The rendering engine for Internet Explorer is called Trident.
* pg 77: Don't write a script for one browser first and add support for other browsers later on; solve nasty incompatibilities at the start, not the end.
* pg 81: When doing object detection, always try and use the standards-compliant method first, and fall through to any proprietary ones.
* pg 88: Don't do browser detection; but if you need some sort of introspection, always use `navigator.userAgent`.
* pg 97: The `alert` method is useful for debugging, but tiresome if in a loop and you've found the problem; in this case, use `confirm` to break out of it.

### Chapter 4: Preparation
* pg 112: Custom attributes fail (X)HTML validation, can can encode name-value pairs for hooks not easily expressed in a `class` attribute.
* pg 121: The `script` tag in `head` can include scripts from other domains, since this is decided by the Webmaster and implicitly judged safe.
* pg 122: If using XHTML, the content of the `script` tag must be defined as `CDATA`, which tells the parser not to parse the contents.
* pg 128: Running JS initialization code using `window.onload` waits for all images to load; to avoid this, put the `script` tag before the closing `body` tag.

### Chapter 5: Core
* pg 155: The value `undefined` is returned when a) a variable is declared but unassigned; b) you access an `undefined` property of an object; c) a function argument is defined, but no value is passed it to it.
* pg 155: The `typeof` operator yields `"function"` on functions when they're really objects, and yields `"object"` when applied to `null`.
* pg 160: When converting to `boolean`, `null`, `undefined`, `0`, `NaN`, and the empty string are `false`; everything else is `true`.
* pg 160: To convert a string to a number, multiply by `1` or subtract `0`; to convert a variable to a boolean, apply the `!` operator twice.
* pg 166: Comparison operators will interpret (i.e. convert) their operands to numbers if they can, otherwise they perform lexicographic comparison.
* pg 172: JS makes no distinction between integers and floating point values; they're both numbers.
* pg 176: The `toFixed` method of a number converts it to a string with the specified number of decimals.
* pg 177: The `parseInt` and `parseFloat` will parse the leading number of a string that can contain non-numeric characters.
* pg 189: The operators `&&` and `||` don't return `true` or `false`, but the value of the last expression they evaluated.
* pg 198: The value after a `case` statement must have the same data type as the variable in the `switch`; no data-type conversion happens.
* pg 208: You can `continue` or `break` from an outer loop by using a label.
* pg 209: Unlike Java, you can not catch particular exception types -- you can only do `catch (e)`, which is the exception object.
* pg 217: Javascript functions run in the scope in which they are defined, not in the scope from which they're executed.
* pg 218: A nested function has access only to the final value of a local variable; beware of them accessing loop counters, etc.
* pg 221: Predefined JS objects are either internal (e.g. the `String` objects) or host objects (created by Javascript's host, the browser).
* pg 225: The `this` keyword always refers to the object the method is defined on.
* pg 226: Client-side Javascript binds treats global variables as properties of the `window` object.
* pg 235: The W3C DOM `nodeList` data type resembles an array, but is read-only and lacks all the methods defined by `Array`.

### Chapter 6: BOM
* pg 242: When the user loads a new page, the `window` object is wiped clean but not destroyed, so references remain intact.
* pg 245: After one window opens another, if the browser hasn't had time to download the page yet, its properties may be unavailable.
* pg 251: The `window.open` method returns a reference to the new window, which has a property `opener` referring to the caller window.
* pg 255: When the user closes a window, its `window` object remains available, but its `closed` property is set to `true`.
* pg 257: Every `window` object has a `close` method; some browsers won't execute it, others open a dialog box for permission.
* pg 261: You can only read the length of `window.history`, not its URLs; to navigate, use its `go`, `back`, and `forward` methods.
* pg 265: The `focus` method brings a window to the front of the stacking order, while `blur` hides the window.
* pg 268: The first argument of `setTimeout` can be a string that is interpreted as a JS command, or a plain function reference.
* pg 270: The browser may set `lastModified` as either January 1 1970 or the current date if the server does not send the header.
* pg 275: A cookie is stored in a name/value pair, where the name is what makes a cookie unique for a given path.
* pg 276: If a page sets a cookie, only pages in that directory and its subdirectories can access them unless its path is changed.
* pg 277: If a cookie is created in a subdomain, but its domain is changed to the top-level domain, it is now associated with all subdomains.
* pg 278: When `document.cookie` is assigned to, a new cookie is created; when read, it contains all cookies separated by semicolons.
* pg 281: A resource loaded from a third party domain allows that domain to set cookies, but modern browsers refuse them by default.

### Chapter 7: Events
* pg 288: The `click` event is the only accessible mouse event, firing when the user has focused on an element and presses Enter.

### Chapter 8: DOM
* pg 356: In XML, `getElementById` searches through attributes with the type (not the name) id, as defined in the DTD.
* pg 359: The `children` node list contains only the element nodes (not text nodes) in `childNodes`, but it is not part of the DOM or fully supported.
* pg 362: The `document.documentElement` and the `document.body` allow access to the `html` and `body` tags, respectively.
* pg 375: The `document.createTextNode` argument cannot contain HTML entities; instead, use the `innerHTML` property.
* pg 376: The `cloneNode` method will not clone event handlers, so you must redefine them on the clone.
* pg 378: To work with IE, add generated table elements to a `tbody`, and create radio buttons with `innerHTML` instead of the DOM.
* pg 382: Assigning `innerHTML` is 3 to 30 times faster than using pure DOM methods.
* pg 392: HTML attributes are nodes in the DOM, while JS properties belong to the object representing the tag. 
* pg 393: Some JS properties are mapped to HTML attributes, e.g. the inline style, event handlers, and form fields, links, and images.
* pg 395: IE does not support empty text nodes, while all other browsers do.
* pg 398: The best defense against empty text nodes is to not use `previousSibling`, `nextSibling`, or `childNodes` at all.
* pg 401: A `nodeList` is dynamic, so beware of introducing errors by adding or removing elements as you iterate over it.
* pg 408: In the level 0 DOM, the `elements` associative array of a form maps a value to all radio buttons with that value as its name.
* pg 410: In the level 0 DOM, for a `select` box, assigning `null` to an element deletes it, while assigning an element inserts it.
* pg 416: You can check whether an element is in DOM hyperspace by checking if it has a parent node.

