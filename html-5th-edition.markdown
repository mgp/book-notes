## HTML 5th Edition

by Elizabeth Castro

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/HTML-XHTML-CSS-Sixth-Edition/dp/0321430840)!*

### Chapter 1
* Elements are block or inline; block elements are displayed on new lines, while inline elements are not. This is the difference between `div` and `span`.
* If you save your document in UTF-8, and declare it as such when serving it, you don't have to use character references like `&copy`, with the exception of `&amp` for `&`.
* XHTML requires the `DOCTYPE` and `html`, `head`, and `body` elements in a document, and that every attribute has a value (e.g. `noshade="noshade"`).
* If a browser finds a `DOCTYPE`, then it uses standards mode; otherwise, it uses quirks mode.
* A stylesheet has multiple rules. Each rule is composed of a selector, and between the curly brackets, a collection of declarations separated by semicolons. Each declaration has a property name, followed by a colon, and one or more values.
* CSS uses inheritance, specificity, and location to resolve rule conflicts:
* Many CSS properties affect not only the elements defined by the selector but also their descendants; this is called inheritance.
* If multiple rules apply to an element and conflict with each other, the more specific rules take precedence (e.g. a rule for an id overrides a class); this is called specificity.
* If this cannot decide which rule to apply, the last rule takes precedence; this is called location.
* When declaring the length of an element, `em` is usually equal to the element's font size.

### Chapter 3
* Declare the page encoding in UTF-8 with `<meta http-equiv="content-type" content="text/html; charset=utf-8" />` in the `<head>` element. Starting the document with `<?xml version="1.0" encoding="UTF-8"?>` will put IE6 into quirks mode.
* The `id` attribute automatically turns the element into an anchor, to which you can directly link.
* A `span` has no inherent formatting, while a `div` only has the line break as inherent formatting.
* You can use the `title` attribute to add a tool tip to anything; to suppress IE from using alt as a tooltip for images, use an empty title, i.e. `title=""`.

### Chapter 4
* The `tt` element forces monospaced font; when displaying computer code, the `code` attribute is equivalent, but gives semantic meaning.
* To strikethrough text, use the `del` element.
* Use the `abbr` or `acronym` tag (if the abbreviation an be pronounced as a word) with the `title` attribute to explain their contents when hovering.

### Chapter 5
* If a browser is asked to show a color outside its range, it can mix two colors (called ditherhing) or show the closest available one (called shifting).
* The web-safe or browser-safe colors are the 216 colors that are not reserved by the browser and are supported by both Windows and Mac.
* Each frame in GIF format is limited to 256 colors, although these colors can be selected from a much larger palete.
* The PNG format is not limited to 256 colors, and like GIF is lossless and allows for transparency, but unlike GIF cannot have multiple frames.
* The JPEG or JPG format, typically used for photos, uses lossy compression and so originals should be saved in a format like TIFF.

### Chapter 6
* Add the `width` and `height` attributes to the `img` element so that the browser can display the text while the image loads.
* Use the `br` element with the `clear` attribute so that the following elements will not display until the specified margin (`left`, `right`, or `all`) is clear.

### Chapter 7
* Open a link in a new window by adding `target="_blank"` to the link element.
* Links with the same value for `target` will all open in the same window.
* Add the `tabindex` attribute between `0` and `32767` to links to define a custom tab order; links with the same index value are accessed in the order in which they appear.

### Chapter 8
* A descendant selector of the form `A B` matches when an element `B` is an arbitrary descendant of some ancestor element `A`. 
* The universal selector is `*`, and `A * B` matches a `B` element that is a grandchild or later descendant of an `A` element.
* The child selector of the form `A > B` matches when an element `B` is a child of an `A` element.
* The adjacent sibling selector of the form `A + B` matches when an element `B` is an adjacent sibling of an `A` element.
* The attribute selector `A[foo]` matches any `A` element with attribute `foo` set; `A[foo~="bar"]` restricts to only `foo` attributes containing `bar`, and `A[foo="bar"]` restricts to only `foo` attributes equal to `bar`.
* Pseudo-classes select markup; the selector `A:first-child` chooses only the `A` elements that are the first child of some parent element.
* When setting link properties, i.e. the `a` tag, define them in the order `link`, `visited`, `focus`, `hover`, `active`.
* Pseudo-elements select content; the selector `A:first-line` or `A:first-letter` chooses the first line (which can change as the browser resizes) or first letter of any `A` element.

### Chapter 13
* To change the marker, set the `list-style-type` property with values `disc`, `circle`, `square`, `decimal`, `upper-alpha`, `lower-alpha`, `upper-roman`, or `lower-roman`.
* To use a custom image, set the `list-style-image` property with a value as the image URL; the image should be no larger than 15 x 15 pixels.
* The `dl` element begins a definition list, suited for glossaries, where terms are in `dt` and definitions are in `dd`, alternating as necessary.
* When styling nested lists, the selectors should reflect the types of nested lists, e.g. `ol li` and `ol ol li`.

