## JavaScript Patterns

by Stoyan Stefanov

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752)!*

#### Chapter 1: Introduction
* pg 3: Native objects are described by the ECMAScript standard and are either built-in or user-defined; host objects are defined by the host environment.
* pg 4: A prototype is an object, not a class or anything special, and every function has a `prototype` property.

#### Chapter 2: Essentials
* pg 12: Globals declared with `var` outside any functions cannot be deleted; any implied globals are properties of the global object, and can be deleted.
* pg 14: All variable declarations are hoisted to the top of a function, so referencing a variable declared later yields `undefined`, and not a global value with the same name.
* pg 16: `HTMLCollection` objects are live queries against the document, and so iterating without caching their length is expensive.
* pg 21: To avoid confusion caused by implied typecasting, use the `===` and `!==` operators that check both the values and the type of the expressions compared.
* pg 22: Passing strings to `setInterval`, `setTimeout`, and the `Function` constructor is similar to using `eval` and should be avoided.
* pg 26: To avoid ambiguity with Javascript's semicolon insertion mechanism, put the opening curly brace on the same line as the previous statement.
* pg 34: At the expense of some verbosity, the YUIDoc system is language-agnostic and can document code in any language.
* pg 36: If using a global variable more than once in a function, assign it to a local variable so it can be effectively minified.

#### Chapter 3: Literals and Constructors
* pg 40: A trailing comma after the last name-value pair in an object literal produces errors in IE, so avoid it.
* pg 42: Invoking a constructor with `new` creates an empty object, referenced by `this`, that inherits from the method prototype.
* pg 48: To test if an object is an array, check whether `Object.prototype.toString` returns `"[object Array]"` when applied to it.
* pg 51: Use the `RegExp` constructor when the pattern is known only at runtime; otherwise, use the literal form.
* pg 52: Number, string, and boolean primitives are automatically converted to their wrapper objects when methods on them are called.
* pg 53: You can only assign properties to the wrapper objects for number, string, and boolean primitives.

#### Chapter 4: Functions
* pg 60: Function declarations and named function expressions have a `name` property, which is useful for both debugging and recursion.
* pg 62: Function declarations and expressions are hoisted, but expressions will only have their declaration hoisted, not their definition.
* pg 68: Self-defining functions overwrite themselves to do less work later, but erase method properties, while references to the old definition may still exist.
* pg 70: Immediate functions are a good place to put initialization code that won't pollute the global namespace and will only run once.
* pg 74: Initialization tasks in an anonymous object with a method to be called immediately adds structure, but may not be minified effectively.
* pg 76: To serialize an object or array of objects as a key in a cache, use JSON.

#### Chapter 5: Object Creation Patterns
* pg 91: Assigning modules required to local variables lists dependencies in one place, runs faster, and is minified more effectively.
* pg 94: Immediate functions that return object literals can provide the scope in which private members of the literal exist.
* pg 98: Object literals with methods created by immediate functions can be assigned to method prototypes or namespace objects to create modules.
* pg 100: A module can return a method to create a constructor; inside the module, the constructor prototype can be set up appropriately.
* pg 106: If a public static method is a method added to a constructor, then this inside the static method will refer to the constructor method.
* pg 109: A constant can be implemented as a private property with only a getter, but simply using variables in all caps is likely enough.

#### Chapter 6: Code Reuse Patterns
* pg 116: Prefer talking about constructors in JavaScript instead of classes, which can mean different things to different people.
* pg 125: By introducing a proxy between a constructor and its parent prototype, no constructors have the same prototype, avoiding mistakes by modification.
* pg 132: In the prototypical inheritance pattern, inheriting from the prototype object of the existing constructor avoids inheriting "own" properties.
* pg 136: To re-use only a few methods of an object, don't inherit, but invoke `call` or `apply` to borrow them by binding your own object as this.

#### Chapter 7: Design Patterns
* pg 146: When returning a singleton upon every use to `new`, wrap the instance in an immediate function to hide it.
* pg 153: A decorated object can be the prototype of another object that overrides methods and calls the prototype copies appropriately.
* pg 160: A proxy can lazily initialize a "real subject" or cache values computed by it, saving resources.
* pg 172: By copying (mixing in) the properties of a publisher object, you can turn any object into a publisher with subscribers.
* pg 178: The observer program promotes looser coupling, but shuns sequential code execution which could be more understandable.

#### Chapter 8: DOM and Browser Patterns
* pg 184: Selector APIs that accept a CSS selector string and return matching DOM nodes are faster than selecting the nodes yourself.
* pg 185: To minimize repainting, add node batches using document fragments, and update nodes by cloning a root and then replacing it.
* pg 192: Methods that perform feature detection can rewrite themselves to use the supported method once found without any checks.
* pg 195: If you only need to send data to the server, use an image beacon, and ensure it returns a 204 (No Content) response.
* pg 197: Set the expires header value of served JavaScript far in the future, and adopt a naming and versioning pattern for the files.
* pg 201: Dynamically created `script` tags do not stop further page downloads until they are downloaded and executed.
* pg 202: To insert a new `script` element safely, insert it immediately before an existing `script` element in the document.
* pg 205: To preload a script, create an `object` element with a data attribute with no width and height, or use an image beacon in IE.

