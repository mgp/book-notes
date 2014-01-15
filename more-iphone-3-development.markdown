## More iPhone 3 Development

by David Mark and Jeff LaMarche

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/More-iPhone-Development-Tackling-Beginning/dp/143022505X)!*

### Chapter 11: MapKit
* pg 369: When testing if a coordinate is within a map view’s displayed region, be sure to account for the international date line.
* pg 370: The annotation view is the object that gets displayed on the map, not the floating window (or callout) that is displayed when it’s selected.
* pg 373: Your map view’s delegate is notified with all annotations; test the annotation class and return `nil` for any classes you don’t recognize.
* pg 376: If you need to track the user’s location, let the map view do it for you.
* pg 378: You can redefine properties to be more permissive than declared in a protocol or in a superclass.
* pg 379: A newline in a string will be stripped out when displayed in the annotation’s callout view.
* pg 385: The location manager may give you stale data from the cache at first; you can ignore it and wait until fresh data arrives.

### Chapter 14: Keeping Your Interface Responsive
* pg 458: Non-repeating timers are not typically used; instead, call the method `performSelector:withObject:afterDelay`.
* pg 472: Accessors declared `atomic autorelease` returned values, so if another thread sets a new value through a mutator, the old value is not deallocated under a thread still working with it.
* pg 473: Most of UIKit is not thread safe; don’t set or retrieve values or work with outlets from threads other than the main thread.
* pg 475: When subclassing `NSOperation`, the main method must create its own `autorelease` pool, and wrap all logic in a `@try` block so no uncaught exceptions kill the application.
* pg 478: Sending a `cancel` message to an operation only sets the property; it is up to the main method to check whether it has been cancelled and stop appropriately.
* pg 489: Key value observation enables notification whenever objects are changed.

