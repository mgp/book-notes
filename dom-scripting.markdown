## DOM Scripting

by Jeremy Keith

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/DOM-Scripting-Design-JavaScript-Document/dp/1430233893)!*

### Chapter 1
* ECMAScript is the proper name of Javascript, which is not related to Java.
* DHTML was a shorthand term for describing the combination of HTML, CSS, and Javascript.
* DOM is generic: a model that can be used by any programming language to manipulate any document in any markup language.

### Chapter 2
* Semicolons aren't required in Javascript unless multiple statements appear on one line, but are good practice.
* Use C or C++ commenting styles, which are `//comment` and `/*comment*/`, respectively.
* Declare numeric or associative arrays by using the same `Array` class.
* Prepending a variable declaration with `var` will treat the variable as a local variable.
* Objects have properties (variables) and methods (actions as functions).
* Native objects are those part of the Javascript library, such as `Array`, `Date`, and `Math`.
* Host objects are those supplied by the web browser, such as `Form`, `Image`, and `Element`.

### Chapter 3
* The properties and methods of the window object are often referred to as the Browser Object Model.
* Element attributes are represented by attribute nodes, or children of the element, in the DOM.
* Method `getElementsByTagName` accepts the wildcard value `*`.
* Method `getAttribute` will return `null` if an attribute does not exist.

### Chapter 4
* In an `onclick` handler, you can use the `this` keyword to refer to the element node.
* Returning `false` from the `onclick` handler of an `img` element will disable the default behavior of following the `src` URL.
* The `body` element of the document can be retrieved using `document.body`.
* Whitespace and line breaks between elements in the HTML source are interpreted in nodes in the `childNodes` array.
* The `nodeType` attribute has 12 values, where `1` is an element node, `2` is an attribute node, and `3` is a text node.
* The `nodeValue` attribute contains the text value of a text node.

### Chapter 5
* Making your web site navigable by users without Javascript is called graceful degradation.
* The JavaScript pseudo-protocol allows invoking JavaScript from within a link, but does not allow graceful degradation.
* Likewise, defining a link as `#` and using an inline event handler does not allow graceful degradation.
* Progressive enhancement is using JavaScript and the DOM to enhance sites that are already functional without them.
* When `window.onload` is invoked, the document (and hence DOM) within it is guaranteed to exist.
* Testing whether the browser supports some DOM property or method in a conditional is called object detection.

### Chapter 6
* When applying an anonymous function, the `this` keyword will refer to the element it is bound to.
* To invoke multiple functions on loading, you must bind a single function to `window.onload` that invokes each.
* To be cautious, test not only whether DOM methods exist, but expected DOM nodes and their attributes exist.
* Don't use the `onkeypress` event handler -- it can be invoked even if the user presses the Tab key.
* The `onclick` event handler is also invoked if you press Return while tabbing from link to link.
* HTML-DOM extends the DOM Core with properties like `document.forms` and `document.images`.

### Chapter 7
* You must invoke the `document.write` method from a `script` element in the body, which is obtrusive, so avoid it.
* The `innerHTML` element isn't a web standard, and referencing the inserted elements still requires the DOM, so avoid it.
* Both `document.write` and `innerHTML` won't work with XHTML served with MIME type `application/xhtml+xml`.
* The `createElement` method creates an element not attached to the DOM tree, called a `DocumentFragment`.
* Text elements are created with method `createTextMode`, not `createElement`.
* There is an `insertBefore` method for inserting elements, but you have write your own `insertAfter` method.

### Chapter 8
* An abbreviation is any shortened version of a word, while an acronym is an abbreviation pronounced as a word.
* You can iterate through the keys of an `Array` object using the construct `for (variable in array)`.
* Sniffing for a specific browser name and version number is bound to cause problems and convoluted code.
* Using DOM you can add meaningful content from attributes that browsers typically ignore, like `cite` for `blockquote`.
* Convention for the `accesskey` attribute is `1` for home, `4` for search, and `9` for contact.
* The accessibility statement lists which access keys have been assigned on a page.

### Chapter 9
* Every element has a `style` property -- it is an object, and not a simple string.
* CSS properties are converted to camel case, so the `font-family` property becomes `element.style.fontFamily`.
* Usually style properties are returned in the same units in which they are set, for example `em` vs `px`.
* The DOM can only return inline style information, not style applied through external CSS files or `style` tags.
* But the DOM can retrieve styles that the DOM itself assigns through the `style` property.
* CSS pseudo-classes and DOM scripting can overlap when styling elements based on state, like `:hover` versus `:onmouseover`.
* The DOM can apply styles defined in CSS files by setting the element's `class` attribute, or its `className` style property.

### Chapter 10
* Elements have a position of `static` by default, but by using `relative` we can use the `float` property on them.
* The `setTimeout` method returns a handle to the function that can be cancelled using `clearTimeout`.
* Methods `parseInt` and `parseFloat` extract the number starting any string, e.g. `39 steps` converts to `39`.
* The `overflow` property specifies how to handle content that is larger than the containing element.
* To persist variables between method calls, set them as element properties instead of making them global.

### Chapter 11
* You can split your CSS into multiple files: one for layout, one for color, and one for typography.
* The address of the current window can be retrieved through property `window.location.href`.
* To frame a slideshow element, make the frame image an absolutely positioned child with a high z-index.
* Invoking the `focus` method on a form text element will place the cursor inside it.
* The `form.elements` array contains only descendant form elements, which differs from `form.childNodes`.

### Reference
* If the boolean argument to `cloneNode` is `true`, all children nodes are copied; if `false`, only its attributes are.
* The newly created node returned by `cloneNode` is not automatically added to the document.
* Methods `appendChild`, `insertBefore`, and `replaceChild` will move elements that already exist in the DOM.
* If the `hasChildNodes` property is `false`, `childNodes` is an empty array, and `firstChild` and `lastChild` are `null`.
* The `nodeName` property for a element node is its tag type, and for an attribute node is the attribute name.
* The `nodeValue` property for an attribute node is its value, and for a text node is its text, but is `null` for element nodes.
* You can't set `nodeValue` if it is already `null`, i.e. you can't assign a value to element nodes.
* The nodes adjacent to an element node can be retrieved with `nextSibling` and `previousSibling`.

