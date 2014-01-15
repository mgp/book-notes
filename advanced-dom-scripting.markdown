## Advanced DOM Scripting

by Jeffrey Sambells and Aaron Gustafson

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/AdvancED-DOM-Scripting-Dynamic-Techniques/dp/1590598563)!*

### Chapter 1: Do It Right with Best Practices
* pg 11: Instead of using inline event attributes, use unobtrusive techniques to add the event handlers when the window loads.
* pg 14: Instead of browser sniffing to find what features you can use, use capability detection and check for the existence of methods directly.
* pg 16: Don't require Javascript for content, as search engine crawlers will miss it.
* pg 21: When creating your namespace with its methods, check that it doesn't already exist if your library is already included from another file.
* pg 28: The display property can be toggled between `none` and empty (the string `''`), which is the default.
* pg 31: Prefer single quotes in Javascript, and leave the double quotes for XHTML attributes as its specification requires.
* pg 39: The `hasOwnProperty` method returns `true` if the object has the property or method specified, ignoring any inherited properties or methods.

### Chapter 2: Creating Your Own Reusable Objects
* pg 63: Static members exist only on a specific instance of an object, by manual assignment and not through any prototype property.
* pg 67: Private methods are defined within a constructor or another method, but not available externally through any instance.
* pg 68: Privileged methods are defined within the constructor using the `this` keyword, and can access private methods while being publicly accessible.
* pg 70: When you declare function `foo() { ... }`, you can call the function before its definition. If you use var `foo = function() { ... }`, you can't.
* pg 74: When a method of an object is used as an event handler, you can bind `this` to the object again using `call` or `apply`.

### Chapter 3: Understanding the DOM2 Core and DOM2 HTML
* pg 91: DOM Level 2 builds on Level 1 by specifying mouse-related events and the ability to access and manipulate CSS styles.
* pg 94: Internet Explorer 7 only supports some of DOM Level 1, but supports some features of Level 2 (like events) in its own way.
* pg 101: IE will only create a text node in the DOM if it contains something other than whitespace.
* pg 105: The `nodeValue` property of a `Node` returns the value of an attribute node or the content of a text node, and `null` in most other cases.
* pg 111: Attributes are implementations of the Core `Attr` interface and are contained in a `NamedNodeMap` in a node's `attribute` property.
* pg 113: The `attributes.getNamedItem` method will work on any node, while the `getAttribute` method only works on instances of `Element`.
* pg 117: Don't modify the prototype of any DOM classes like `Node`; it's flaky across browsers, and may override any future additions to the DOM specification.
* pg 122: The `Document` class has a `getElementsByTagName` like `Element` does, but technically it's different; it also adds `getElementById`.
* pg 126: The `HTMLDocument` class inherits from `Document`, and adds properties like `title`, `referrer`, `body`, `images`, `forms`, and `anchors`.
* pg 127: The `HTMLElement` class inherits from `Element`, and adds properties like `id`, `title` (for tooltip rollovers), and `className`.
* pg 133: The DOM names CSS properties like `font-size` as `fontSize`, and `class` as `className`.

### Chapter 4: Responding to User Actions and Events
* pg 154: The `mousemove` event has no specified period or distance in which it fires.
* pg 156: A `click` and `dblclick` event will only happen if the mouse remains stationary; regardless of the mouse motion, a `mousedown` and `mouseup` event occurs.
* pg 159: Key-related events apply only to the `document` object.
* pg 161: The `focus` and `blur` evens only apply to `label`, `input`, `select`, `textarea`, and `button` form elements.
* pg 182: Event listeners registered using the W3C event model do not necessarily execute in the order in which they're added.
* pg 183: Beware that the `load` event is only executed after all the images have been loaded as well.
* pg 196: A simple drag-and-drop effect can be created with a `mousemove` event listener that moves an element to the cursor's location.
* pg 197: Keyboard commands are not in the DOM2 Events specification, but all browsers supply the Unicode value of a key press in the `keyCode` property.

### Chapter 5: Dynamically Modifying Style and Cascading Style Sheets
* pg 205: `CSSStyleSheet` represents a stylesheet with `CSSStyleRules`, while the `CSSStyleDeclaration` class represents an object's style property.
* pg 214: A `CSSStyleDeclaration` instance of an element only contains the inline style properties, and hence does not contain the overall computed style.
* pg 216: The only time mixing style and scripting is acceptable practice is in the case of positioning, e.g. drag-and-drop.
* pg 220: Use the `className` property to assign classes, as `setAttribute` requires an attribute name of `'class'` in W3C and `'className'` in IE.
* pg 222: Setting the `rel` attribute of a stylesheet link to `"alternate stylesheet"` will load the stylesheet but disable it; Javascript can enable it later.
* pg 228: A new stylesheet can be used by dynamically creating a new link element referencing the stylesheet and adding it to the `head` tag.
* pg 232: The W3C browsers contain the rules in a stylesheet in the variable `cssRules`, while IE contains them in `rules`.
* pg 233: The W3C `insertRule` method combines the selector name and its definition in a single argument, while the `addRule` method of IE combines them.
* pg 238: The computed style of an element is accessible through `document.defaultView.getComputedStyle` in W3C, and `element.currentStyle` in IE.

### Chapter 7: Adding Ajax to the Mix
* pg 286: IE 7 and later support the `XMLHttpRequest` object, while earlier versions must use the `Microsoft.XMLHTTP` ActiveX component.
* pg 289: The `onreadystatechange` listener doesn't receive any arguments, so the `XMLHttpRequest` object must be defined in the same scope for it to be available.
* pg 289: If the response has MIME type `application/xml`, the response will be interpreted as XML and not HTML, so the DOM2 HTML methods won't work.
* pg 291: Within the `onreadystatechange` property, the `this` keyword refers to the method itself and not the `XMLHttpRequest` object.
* pg 298: To parse JSON from an Ajax response, prefer a JSON parser over `eval` unless the information in the response is entirely controlled by you.
* pg 307: To bypass cross-site restrictions, dynamically generated `script` elements can run with a source from any domain and their Javascript will run normally.
* pg 328: Replies to concurrent requests may come back out of order; guard against this causing inconsistent application behavior.

