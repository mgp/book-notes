## Auto Layout Guide

from https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/AutoLayoutConcepts/AutoLayoutConcepts.html

### Auto Layout Concepts

* Solve a system of constraints to determine your layout.
* Every constraint is of the form `y = m*x + b`, where y and x are attributes of views, and m and b are floating point values.
* Use attributes `leading` and `trailing` instead of `left` and `right` to deal with RTL languages.
* Constraints are cumulative, and do not override each other. You must remove the first constraint manually.
* Intrinsic content size is when a leaf-level view, such as a button, knows more about its size than the code that positions it. The element will grow and shrink appropriately with different content.

### Working with Auto Layout Programmatically

* In the visual format string, `-` denotes a standard Aqua space, which is 8.0 between sibling views and 20.0 between a view and its superview.
* The `NSDictionaryOfVariableBindings` method creates a dictionary where the keys are the same as the corresponding value’s variable name.
* If the bounds transform of a container changes, then a space like `-80-` scales too.

### Resolving Auto Layout Issues

* Auto Layout issues occur when there are too few constraints, or the system contains conflicting or ambiguous constraints.

#### Content Size Is Undefined

* Container views may use their content to determine their own size at runtime.
* Placeholder constraints address this, such as a minimum width for a view. This placeholder is removed at build time, when your view defines its size.

#### The Size of Custom View Content Is Unknown

* Unlike standard views, custom views have no defined intrinsic content size.
* Use a placeholder intrinsic content size to indicate the custom view’s content size. Again, this is removed at runtime.

#### Debugging in Code

* Method `_subtreeDescription` of `NSView` creates a textual description of the view hierarchy.
* Call method `constraintsAffectingLayoutForAxis:` to get constraints affecting a particular orientation. The debugger can print their details.
* On OS X, you can visualize the constraints by calling `visualizeConstraints:`.

### Auto Layout by Example

#### Using Scroll Views with Auto Layout

* The size of the content inside of a scroll view is determined by the constraints of its descendants.
* You must make sure you create constraints for all the subviews inside a scroll view.
* There cannot be any missing constraints, starting from one edge of the scroll view to the other.

#### Spacing and Wrapping

* The height of the spacer views can be any value, including 0. However, you must create constraints for the height of the views.

### Implementing a Custom View to Work with Auto Layout

* A custom view class must provide enough information so that the Auto Layout system can make the correct calculations and satisfy the constraints.

#### A View Specifies Its Intrinsic Content Size

* The `intrinsicContentSize` method of a view tells the layout system that it contains content that the layout system doesn't understand, and provides to the layout system the intrinsic size of that content.
* Method `intrinsicContentSize` can return absolute values for its width and height, or return `NSViewNoInstrinsicMetric` for either value to indicate no intrinsic metric for a dimension.
* For example, a button returns an intrinsic size with a width and height large enough not to clip the text or image. A horizontal slider has an intrinsic height, but no intrinsic width.

#### Views Must Notify Auto Layout If Their Intrinsic Size Changes

* If the content of a view changes, which in turn changes its intrinsic size, then it must call `invalidateIntrinsicContentSize`.

### Appendix A: Visual Format Language

* You cannot express a fixed aspect ratio using the visual format language; you must instead create a `NSLayoutConstraint` using `constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:`.
