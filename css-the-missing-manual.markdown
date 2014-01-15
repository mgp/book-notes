## CSS: The Missing Manual

by David Sawyer McFarland

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/CSS3-Missing-David-Sawyer-McFarland/dp/1449325947)!*

### Chapter 3 - Selector Basics: Identifying What to Style
* pg 43: Tag selectors, sometimes called type selectors, are the selector rules applied to the HTML element names.
* pg 47: A `div` tag specifies a logical division on the page, and can contain several block-level elements, allowing you to group together logically related tags. A `span` allows you to apply a style to just part of a tag.
* pg 48: Use the ID selector to sidestep style conflicts, since web browsers give ID selectors priority over class selectors.
* pg 53: For multiple page elements to share some but not all of the same formatting properties, create a group selector with the shared formatting options, and then individual rules, with unique formatting, for each individual element.
* pg 55: The `:hover` pseudo-class can be used on elements other than links.
* pg 57: The `:first-child` pseudo-class is bound only to the name of the child element, never the parent, and so its use on tags other than list items can be tricky.
* pg 59: The child selector can be used to style nested lists easily, e.g. given a `ul` root with class `mainList`, the first level of nested lists can be styled as `ul.mainList > li > ul > li`.
* pg 60: The adjacent sibling selector can be used to change the margin between the first paragraph and its header, e.g. `h2 + p`.

### Chapter 4 - Saving Time with Inheritance
* pg 75: As a general rule, properties that affect the placement of elements on the page, or the margins, background colors, and borders of elements aren't inherited.

### Chapter 5 - Managing Multiple Styles: The Cascade
* pg 83: Some inherited properties don't "appear" to inherit because browsers have their owned predefined style for elements that are overriding inherited styles.
* pg 87: Inherited properties don't have any specificity value, so properties from a style with a large specificity like #banner will always be overridden by a style that directly applies to the tag.
* pg 90: Given a global stylesheet included on every page, use an internal stylesheet or an external stylesheet loaded later that selectively overrides the global styles.
* pg 91: To fine-tune designs on a page-by-page basis, apply different ID names to the body tags of different pages -- e.g. `#review`, `#story`, `#home` -- and then create descendant selectors from these names.

### Chapter 6 - Formatting Text
* pg 101: Fonts like Georgia, Times New Roman, and Times are best for long passages of text where the serifs gently lead the eye between letters; sans-serif fonts like Verdana, Tahoma, Arial, and Helvetica are best for headlines.
* pg 105: In most browsers, text is by default 16 pixels tall. Using percentages or ems for formatting keeps the page proportions if this default is overridden by the client.
* pg 107: Because font size is an inherited property, avoid making nested lists progressively bigger or smaller with percentages or ems by using a selector like `ul ul {font-size: 100%}`.
* pg 113: The default `line-height` is 1.2, and the leading, or space between lines, is calculated from it as the font size multiplied by `line-height - 1`.
* pg 114: Specify `line-height` by number, e.g. `line-height: 1.5`, so the percentage is inherited by all descendants. When using ems or percentages with `line-height`, the calculated value in pixels is inherited instead.
* pg 116: Percentages specifying margins or text-indent values are related to the width of the element and not the font size, and so are not encouraged.
* pg 122: To style bullets and numbers differently, apply a selector to each `ol` or `li` tag to style them, and then wrap the content of each list item in a `span` with an associated class style that reverses its effects.

### Chapter 7 - Margins, Padding, and Borders
* pg 136: Again, percentages for margins and padding are calculated relative to the width of the containing element. This includes top and bottom margins and padding.
* pg 137: When the bottom margin of one element touches the top margin of another, only the larger margin is applied. To fix this, use top or bottom padding instead.
* pg 138: Vertical margins for nested elements collapse into a single margin, so any margin assigned to an inner element is applied to the outer element. Instead, apply padding to the element containing the inner element.
* pg 139: Negative margins can be used to overlap elements, but you cannot specify negative padding to move an element's border over its own content.
* pg 140: Adding left or right margins to an inline element has no effect, but elements can be converted between block and inline using the `display:block` and `display:inline` properties.
* pg 146: When setting the `height` property of an element, a percentage value is based on the containing element's height, instead of its width as seen earlier.
* pg 148: Unless you know the exact dimensions of a content of a tag, don't specify its height or its contents may simply spill out of it.
* pg 154: A floated inline element is treated like a block-level element, so margins and padding can be applied to it. A floated block element should always have a `width` property.
* pg 155: Content will wrap around a floated element, but its background and border will appear under the floated element unless the `overflow:hidden` property is set on it.

### Chapter 8 - Adding Graphics to Web Pages
* pg 174: The URL specified with the `background-image` property is relative to the external stylesheet, and not the HTML page.
* pg 178: To add graphic backgrounds to banners and sidebars, use a `background-position` of `bottom`, `center`, or `top` with `background-repeat` as `repeat-x`, or `left`, `center`, or `right` with `repeat-y`.
* pg 179: Using pixels or ems with `background-position` specifies distance from the container's left edge and top edge. To specify distance from the bottom or right, use keywords or percentages.
* pg 180: Background images are usually not printed by the browser, so use the img tag for critical images.
* pg 191: You can apply two classes to one tag like `<div class="class1 class2">`. Use this to allow the second class to override specific properties contained in the first.
* pg 200: Background images are contained within the border, not the content, and so adding padding on the side of an element can make room for an adjacent image, potentially tiled.
* pg 202: To add graphic bullets to lists, use the `background` property instead of the `list-style-image` property for better positioning control.

### Chapter 9 - Sprucing Up Your Site's Navigation
* pg 210: The `a:link`, `a:visited`, `a:hover`, `a:active` styles have the same specificity; use the mnemonic LoVe/HAte to put them in the correct order.
* pg 214: You can't control the color, width, or style of a link underline, so remove it with `text-decoration: none` and give the link a `bottom-border` property.
* pg 218: To convert an unordered list into a navigation bar, remove the bullets with `list-style-type: none` and eliminate padding and margins.
* pg 219: For vertical navigation bars, styling each link in the list with `display:block` widens the link, or clickable area, to the width of the containing element.
* pg 223: When using inline list elements for horizontal navigation bars, add padding to the containing `ul` to accommodate the overflowing backgrounds and borders of inline links which cannot add padding.
* pg 228: You can't wrap an inline element like links around block elements, but you can assemble elements in a link in an inline fashion, and then style them with `display:block` when needed.
* pg 232: With the sliding doors technique, the thin left-hand graphic must be applied to the link so it appears above the wide background image on the encompassing `li` tag.
* pg 248: Using an em value for the width of buttons on a horizontal navigation bar allows them to scale gracefully with the client's font size.
* pg 249; When using floated list elements for horizontal navigation, they are removed from the encompassing `ul` tag in the normal flow, which shrinks to almost no height unless you float it as well.

### Chapter 11 - Building Float-Based Layouts
* pg 277: A majority of web pages use float-based layouts because while layouts based on absolute positioning are powerful, they are hard to achieve.
* pg 280: An elastic layout defines the page width in ems, and so if the user increases his or her font size, everything is kept in proportion.
* pg 284: Adding a margin to the `div` containing the main content of a page prevents it from wrapping underneath a sidebar that is floated.
* pg 286: By floating all columns and nesting floated `div` tags, your content can precede the sidebars in the source code, which will give it more weight by search engines and increases its accessibility.
* pg 289: Use the `min-width`, `max-width`, `min-height`, and `max-height` properties to prevent liquid layouts from falling apart when the resolution is too small or too large.
* pg 291: Given a fixed width design, negative margins pull an element that appears later in the HTML document over and past an element that precedes it.
* pg 295: A floated element can escape its container if it's taller than the container. To expand the container, float it, or add at the end of it an element with the `clear` property.
* pg 299: To create full-height columns for two opposing sidebars, wrap the sidebars in two nested `div` elements, and style the left column background to one `div`, and the right column background to the other.
* pg 301: A float drop can occur if floated element widths are set in percentages, and you don't account for the browser rounding up when calculating their actual number of pixels.
* pg 309: Unless floating an image that has a set width, always set a `width` for the floated element, or else the browser will set it based on its content.

### Chapter 12 - Positioning Elements on a Web Page
* pg 326: Don't try to apply both the `float` property and any other positioning other than `static` (the default positioning) or `relative` to the same style.
* pg 332: An absolutely positioned element is placed relative to the boundaries of its closest positioned ancestor.
* pg 333: Apply `relative` positioning to an element crates a new positioning context; all absolutely positioned elements nested within are relative to its position.
* pg 337: Setting an element's `display` property to `none` removes it without a trace, while setting its `visibility` to `none` leaves an empty hole where it was.
* pg 341: If using negative values to absolutely position an element, try adding margins to its positioned ancestor to ensure it does not hide content unintentionally.
* pg 343: Unlike a float-based layout, if a footer or lower HTML elements are not given the same contents as the main content, an absolutely positioned sidebar can grow to cover them.
* pg 348: Elements with fixed positioning are always relative to the browser window and cannot be scrolled, so content that doesn't fit in the browser window can't be seen.
* pg 351: Absolute positioning and negative margins can let elements "break out" of parent elements and remove the boxy look typical of CSS designs.
* pg 355: The `img` tag is self-closing, so to position a caption on top of an image, wrap the image in a `div` that is relatively positioned and nest the element containing the caption inside that.

### Chapter 14 - Improving Your CSS Habits
* pg 385: Name styles by purpose, and not by appearance or position.
* pg 388: After dividing your styles among multiple stylesheets to make them more manageable, your HTML can rely on a single stylesheet that uses the `@import` directive to include them all.
* pg 392: Different browsers can add different vertical margins to block-level elements, so remove padding and margins from those you use and then purposely add the amount you want with new styles.
* pg 396: Giving the `body` tag an `id` property and using it in descendant selectors allows you to highlight the current page in a navigation bar, for example.

