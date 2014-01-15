## jQuery in Action

Bear Bibeault and Yehuda Katz

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/jQuery-Action-Second-Edition-Bibeault/dp/1935182323)!*

### Chapter 1: Introducing jQuery
* pg 9: When `$` is passed a selector, the returned value contains matching elements in the order they appear in the document.
* pg 12: Passing a method to `$` executes the method when the DOM has loaded, and doesn't wait for all resources to load.
* pg 13: Passing a string containing HTML to `$` creates a document fragment that can then be added to the DOM.
* pg 15: A test against `null` will check whether a value is either `null` or `undefined`.

### Chapter 2: Selecting the elements upon which to act
* pg 21: The context for `$` can select or contain multiple DOM elements, which serve as multiple sub-trees to query on.
* pg 27: Filters `:first` and `:last` return a single element, while `:first-child` and `:last-child` return elements from different sets of siblings.
* pg 28: The `:nth-child` filter starts counting from 1 for CSS compatibility, while other filters start from 0.
* pg 29: An attribute selector will only match the initial element state; for the real-time state, use a `filter` selector.
* pg 38: Passing a negative value to the `get` method returns elements from the end of a wrapped set, unlike subscript notation.
* pg 39: The `eq` method returns a single element like `get`, but inside of a new wrapped set.
* pg 40: If no argument is passed to `index`, the ordinal index of the first element of the wrapped set within its siblings is returned.
* pg 45: The `not` method removes elements that match its argument, while the `filter` method retains matching elements.
* pg 50: The `contents` method of a wrapped set is frequently used to obtain the contents of `iframe` elements.
* pg 51: TODO: difference between children and find?

### Chapter 3: Bringing pages to life with jQuery
* pg 62: Removing an attribute doesn't remove any property in its corresponding DOM element, although its value may change. 
* pg 63: For attributes where presence decides behavior, passing `true` or `false` as the attribute value adds or removes it.
* pg 64: Use the data method to bind values to DOM elements without using expandos, which can lead to memory leaks.
* pg 69: No method for retrieving class names of an element is provided; you must split on the attribute value yourself, if it exists.
* pg 71: The `css` method returns the computed value of any CSS property as a string, which may converting into a number.
* pg 76: The `offset` and `position` methods must be used on visible elements; accuracy requires padding, margins, borders in pixels.
* pg 79: For methods setting element content, if a wrapped set contains multiple targets, the argument is cloned as necessary.
* pg 86: The `detach` method removes elements from the DOM but retains bound events or data; `remove` will unbind them as well.
* pg 89: The `replaceAll` method returns no reference to the replaced elements; to keep a reference, use `replaceWith`.
* pg 90: The `val` method returns the empty string if a form element isn't first in the wrapped set.

### Chapter 4: Events are where it happens!
* pg 95: There is no DOM Level 0, but the term is used to describe what was implemented prior to DOM Level 1.
* pg 109: By appending multiple suffixes to event names, you can put the handlers in multiple namespaces.
* pg 111: The `focus` and `blur` events do not bubble up the DOM tree, but the `focusin` and `focusout` events do.
* pg 114: Event property `keycode` isn't reliable across browsers for non-alphabetic characters, but the `which` property is.
* pg 115: Handlers returning `false` cancel default behavior and stop propagation up the tree, but allow immediate propagation.
* pg 116: A `live` event doesn't propagate; its handler runs only after the normal event reaches the context with which `live` was called.
* pg 118: Unlike `trigger`, the `triggerHandler` method invokes handlers without bubbling, semantic actions, or live events.
* pg 123: Unlike `mouseout`, the `mouseleave` event is not triggered when the cursor moves over the child of an element.
* pg 135: Custom events and using the `live` method promotes loose coupling effectively.

### Chapter 5: Energizing pages with animations and effects
* pg 139: Calling `show` on an element sets its display property to the element type default if no explicit value was provided.
* pg 142: A convention is to add the `$` character as a prefix or suffix to variable names that refer to a wrapped set.
* pg 149: The `show` or `hide` methods adjust width, height, and opacity when animated; `fadeIn` and `fadeOut` only adjusts opacity.
* pg 151: Unlike `fadeIn` and `fadeOut`, `fadeTo` never removes elements from the display, and doesn't remember the original opacity.
* pg 153: Stopping an animation does not revert changes to animated elements; their CSS values must be explicitly reset.
* pg 155: End values for animation can assigned `show`, `hide`, and toggle`, or relative values can be assigned with `+=` and `-=`.
* pg 164: To run all animations in a queue, a common idiom is to call the `dequeue` method at the end of a queued function.
* pg 167: A function added to the `fx` queue must call dequeue at the end for the animations queued behind it to execute.

### Chapter 6: Beyond the DOM with jQuery utility functions
* pg 170: Setting the `fx` flag to `false` will disable animations but still apply the desired effects.
* pg 176: The browser detection flags are deprecated, so abstract away browser detection using custom support flags.
* pg 178: The only argument to a `ready` handler is the jQuery object; naming the parameter `$` will hide any global reassignment.
* pg 185: If the callback to the `map` function for `arrays` returns an array, its values are appended to the returned array.
* pg 191: Passing any non-form elements in a wrapped set or array to `param` appends `&undefined=undefined` to the query string.
* pg 192: The `param` method can construct a query string from a nested object by using square brackets to identify children.
* pg 195: Use the `data` and `contains` methods instead of their wrapper equivalents when you have DOM element references.
* pg 199: The `parseJSON` method expects well-formed JSON, which is much more strict than JS expression notation.

### Chapter 8: Talk to the server with Ajax
* pg 240: If a response has a MIME type `text/xml`, `application/xml`, or ending in `+xml`, its body is parsed into `responseXML`.
* pg 244: The `load` method uses POST only if its parameters are in an array or object hash; if a string or omitted, it uses GET.
* pg 245: The `serialize` and `serializeAll` methods only collect successful form control values that participate in form submission.
* pg 253: Use the callback to inject the response of the get or post methods into the DOM; unlike `load`, it is not automatic.
* pg 264: Values passed to `ajaxSetup` aren't applied to `load`, and won't change the HTTP methods used by `get` and `post`.
* pg 265: Global Ajax events are broadcast to every element in the DOM; there is no hierarchy through which they bubble.
* pg 268: Global events `ajaxStart` and `ajaxStop` are triggered only once for a set of concurrent requests.
* pg 274: The `abbr` tag is used to identify abbreviations, but can serve as a hook for flyouts used to define terms.

### Chapter 9: Introducing JQuery UI: themes and effects
* pg 284: A theme's images folder must be in the same folder as its CSS file.
* pg 287: The `ui-widget` class identifies the container element that is the ancestor of all elements that comprise the widget.
* pg 288: Given the appropriate class name, rounded corners appear on any element, but only in supported browsers.
* pg 290: To edit a custom theme created by ThemeRoller, visit the URL embedded in a comment of its CSS file.

### Chapter 10: jQuery UI mouse interactions: Follow that mouse!
* pg 310: The `cancel` selector identifies elements on which drag operations cannot start, but the elements can still be draggable.
* pg 314: Passing `enabled` to `draggable` re-enables draggability, and does not bestow draggability on non-draggable elements.
* pg 319: When a `draggable` is released over a `droppable`, both the `drop` and `dropdeactivate` events occur.
* pg 321: If a class applied by `activeClass` or `hoverClass` does not take precedence, the `!important` CSS qualifier can override.
* pg 328: The `sortupdate` event may be the most important, as it only fires if the sort order of anything changes.
* pg 329: Passing 'toArray' and 'serialize' to sortable to retrieve the order of its children requires that each child has an `id` attribute.
* pg 335: Only the southeast corner of a resizable element has a grip icon; custom child elements must be created for the others.
* pg 342: The selectable tolerance 'fit' can be problematic without nested selectables because drags must begin within a selection.

### Chapter 11: jQuery UI widgets: Beyond HTML controls
* pg 348: The `buttonset` method acts on a set of buttons and themes them so that they appear as a cohesive unit.
* pg 352: No custom events are defined for jQuery UI buttons; standard events like `click` must be used.
* pg 359: A slider uses `a` elements for its handles so that they are focusable elements and can be controlled using the keyboard.
* pg 361: Only use a progress bar if a reasonable level of accuracy is possible; use another activity indicator otherwise.
* pg 374: If only one of `label` or `value` is specified for an autocomplete item, the provided data is used in both places.
* pg 387: Date pickers have no events; you must rely exclusively on the callbacks assigned to the options.
* pg 394: By default, any content loaded with Ajax will not be cached, so showing the tab will contact the server every time.
* pg 395: A spinner, or string displayed while an Ajax tab loads, requires a `span` element be the child of the tab's anchor element.
* pg 401: With the `navigation` option set, loading the page opens the panel whose anchor header matches that in `location.href`.
* pg 407: Once a dialog box is created, it doesn't need to be created again to be reopened after closing.
* pg 410: Override the `close` handler to prevent a dialog box from closing when it contains forms that fail validation.
* pg 411: The dialog box content element is set as the context for event handlers, not the created `div` that contains it.

