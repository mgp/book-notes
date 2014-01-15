## Beginning iPhone 3 Development: Exploring the iPhone SDK

by David Mark and Jeff LaMarche

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/Beginning-iPhone-Development-Exploring-SDK/dp/1430224592)!*

### Chapter 1: Welcome to the Jungle
pg 7: The iPhone doesn't write memory to a swap file, so usually only half the memory of a 128MB of 256MB device is left for you to use.

### Chapter 2: Appeasing the Tiki Gods
* pg 21: In IB, File's Owner is what loaded the nib file from disk and so “owns” the nib file; first responder is the current object with which the user is interacting.
* pg 23: UIKit on Cocoa Touch is like AppKit on Cocoa; the Foundation framework classes are shared between them, however.
* pg 26: Icons must be 57x57 PNG images, which should always be used as they are actually optimized by Xcode at build time.
* pg 29: Delete the iPhone Simulator folder from `~/Library/Application Support` to clear out old applications from the simulator's home screen.

### Chapter 3: Handling Basic Interaction
* pg 35: Put the `IBOutlet` keyword in the property declaration; putting it before the instance variable declaration will fail if you rename the variable using the `@synthesize` directive.
* pg 43: Use `autorelease` only when you absolutely have to, and not to simply save a line or two of code.
* pg 44: Every iPhone application has one `UIApplication` instance, which will call methods on its delegate at well-defined points during execution.
* pg 55: When associating buttons with actions, do so from the connections inspector window for the button.

### Chapter 4: More User Interface Fun
* pg 60: Controls are active, static (or inactive), or passive, which simply hold on to a value until ready for processing later.
* pg 64: Holding the option key when resizing a view reveals the distance from each edge; selecting Show Bounds Rectangles in the Layout menu reveals the true size of any view.
* pg 66: For performance: use an Alpha of 1; under Drawing uncheck Clear Context Before Drawing and Subviews; and don't select a Mode that resizes an image.
* pg 68: The text base guideline ensures that a label and the text entered into a text field are aligned on the bottom.
* pg 74: The container view can be changed from a `UIView` to the `UIControl` subclass to capture background taps and dismiss keyboards and number pads.
* pg 90: Use an action sheet if a choice must be made; to simply notify the user without giving a choice, use an alert sheet.
* pg 93: Most buttons on the iPhone are drawn using images; Apple provides many in the `UICatalog` program, which you can redistribute with your application.
* pg 94: Controls are either normal, highlighted, disabled, or selected, which is similar to highlighted but can persist even when the user is no longer directly using that control.
* pg 95: When a view gets unloaded, its `viewDidUnload` method should assign `nil` to its properties so that their retain counts reach zero and memory is freed.

### Chapter 6: Multiview Applications
* pg 119: A `UIView` or one if its subclasses that has a corresponding view controller is called a content view, because it's a container for application content.
* pg 123: The root controller is loaded when the application loads and is responsible for displaying the appropriate content view.
* pg 127: The application delegate, in `application:didFinishLaunchingWithOptions`, can add the view of the root controller to the application window.
* pg 131: Toolbar buttons support only a single target action at the moment equivalent to a Touch Up event on other iPhone controls.
* pg 137: The view output of a controller must be reconnected with the view in the nib if the underlying class of the file's owner is changed.
* pg 142: Caching an animation makes it smoother by taking a snapshot of the view when it begins, but you can't change the appearance of the view during the animation.

### Chapter 7: Tab Bars and Pickers
* pg 148: While a picker gets its data from a delegate, its datasource specifies how many components it has and how many rows per component.
* pg 153: Clicking on a tab in IB will first select the associated view controller; clicking again will selects the tab bar item itself.
* pg 161: `NSInteger` is defined as either `int` or `long`; the compiler automatically chooses whichever size is best for the platform you're compiling for.
* pg 162: Store data in property lists when you can; this will allow you to edit data without recompiling, and make internationalization easier.
* pg 175: A bundle is a special folder whose contents follow a specific structure; you'll typically use it to access items in your Resources folder.
* pg 179: Interface Builder will let you assign any font to a label, but the iPhone has a very limited selection of fonts.
* pg 188: Method `performSelector:withObject:afterDelay` is available on all objects and allows calling a method sometime in the future.
* pg 190: To add a framework, right-click on the Frameworks folder under the Groups & Files pane, and select Existing Frameworks from the Add submenu.

### Chapter 8: Introduction to Table Views
* pg 195: Grouped table views have rows grouped in rectangles with rounded corners; plain tables have no rounded rectangles.
* pg 196: The iPhone HIG explicitly states that grouped tables should not provide indexes.
* pg 200: Always try to reuse a cell by calling `dequeueReusableCellWithIdentifier`, ensuring that you guard against `nil` values.
* pg 203: `UIImage` uses a caching mechanism based on the filename, so calling it repeatedly will return a cached instance.
* pg 205: Unlike pickers, in a table the delegate configures the appearance of the table and handles certain interactions; it doesn't provide data.
* pg 210: To customize table cells, either add subviews to instances of that class, or subclass it.
* pg 240: Actions are executed a tiny bit sooner in `tableView:willSelectRowAtIndex` versus `tableView:didSelectRowAtIndex`.
* pg 241: Implement `searchBar:textDidChange` to show a live search, but test extensively on hardware first to ensure good performance.
* pg 242: The index will overlap with the search bar unless you explicitly disable indexing while the search bar has focus.
* pg 243: Putting a search bar above the table view consumes valuable screen real estate; instead, add a search icon to the index for easy access.

### Chapter 9: Navigation Controllers and Table Views
* pg 249: A detail disclosure button says that selecting the row will reveal detailed information about the row, while tapping the row can perform another action.
* pg 253: Subclassing `UITableViewController` will create a table view automatically, without the need for a nib file.
* pg 254: The root controller is the first controller loaded by the application; `UINavigationController` has a root controller which is the bottom of the navigation stack.
* pg 262: If passing data to a subcontroller, assign to temporary variables that are copied to outlets in `viewWillAppear`; outlets are `nil` until its nib file loads and `viewDidLoad` is called.
* pg 284: If allowing the user to rearrange items, don't flush data in `viewDidUnload`, or the new arrangement will be lost when returning.
* pg 287: To use the move control in the simulator, you must select one of its pixels with your single-pixel hot-spot cursor; it's easier with fat fingers on a real device.
* pg 294: If you implement edit mode to allow deletes, horizontal swipes across a row will also reveal the Delete button.
* pg 300: If using a table view to implement a detail view without any nib file, just recreate the child controller each time; attempting reuse can introduce complexities.
* pg 302: If a text field being edited scrolls off the screen, its updated value may be lost; implementing `UITextFieldDelegate` allows you to save whenever an edit is made.
* pg 309: Use `UIBarButtonItemStyleDone` for buttons that users tap when they are happy with their changes and ready to leave the view.
* pg 316: If a view is composed entirely of text fields, have the keyboard's return button move on to the next text field.

### Chapter 10: Application Settings and User Defaults
* pg 322: The settings application is a common UI for the User Defaults mechanism; immersive apps should provide their own preferences view as well.
* pg 332: The `Values` array of `PSMultiValueSpecifier` and the `TrueValue` and `FalseValue` of `PSToggleSwitchIdentifier` can store any type.
* pg 333: The `DefaultValue` of `PSMultiValueSpecifier` or `PSToggleSwitchIdentifier` must specify a value in `Values`, or in `TrueValue` or `FalseValue`.
* pg 336: The settings bundle can't read from your application's sandbox, so images required by a bundle must exist in the bundle.

### Chapter 11: Basic Data Persistence
* pg 352: Custom objects cannot be serialized into property lists, and neither can other delivered classes from Cocoa Touch.
* pg 359: Any class written to hold data should implement `NSCoding` and support archiving; also implement `NSCopying` for good measure.
* pg 361: Copied objects are implicitly retained and should therefore be released or autoreleased in the code that called `copy` or `copyWithZone`.
* pg 368: Aggregations in SQLite3 is several orders of magnitude faster than loading all objects into memory and doing it yourself.
* pg 370: A `NSString` is converted to a C-string using the `UTF8String` method; it is created from one using `initWithUTF8String`.
* pg 371: Confusingly, either `sqlite3_step` returns the next result from a query, or commits an update.
* pg 381: An entity is a description of an object you write in XCode, while managed objects are the concrete instances of the entity created at runtime.
* pg 382: A fetched property is where an attribute of an entity is not loaded until the attribute is actually needed.
* pg 383: The managed object context, which intermediates access to the persistent store, registers all changes with the undo manager up until the last time a save was performed.
* pg 385: New managed objects and retrieved managed objects are not saved until the managed object context retrieves a save message.

### Chapter 13: Taps, Touches, and Gestures
* pg 438: Taps are only kept track of when one finger is used; if it detects multiple touches, the tap count is reset to one.
* pg 439: Events are passed to a view, then its controller, and then the superview, and then its controller, all the way up to the `UIApplication` instance if not consumed.
* pg 440: If a gesture affects more than the object being touched, the event handling code belongs in the view controller's class; otherwise, it can go in the view itself.
* pg 441: All touches are passed to an event handler; it is up to that handler to filter touches outside of its view or desired scope.
* pg 449: Beware that touches are in an `NSSet` and can be reordered across calls, and one finger may touch before others in a multi-finger swipe, so `touchesBegan:withEvent:` has just one touch.
* pg 455: Delay actions performed on some number of taps by 0.4 seconds; if another tap occurs, incrementing the tap count, you can still cancel the previous action and schedule a different one.
* pg 464: Test your custom gesture to ensure that it does not conflict with other gestures in your application.

### Chapter 14: Finding Your Way Around With Core Location
* pg 465: Choose the minimum accuracy level needed and don't poll for location any more than absolutely necessary to reduce battery drain.
* pg 468: A negative value of horizontalAccuracy means the latitude and longitude cannot be relied on; a negative value of verticalAccuracy menas the altitude cannot be relied on.

### Chapter 15: Whee! Accelerometer!
* pg 478: The accelerometer increases y with upward force, which is what OpenGL ES expects, but must be translated when using Quartz 2D.
* pg 479: The accelerometer supports 100 updates per second second, although that throughput and the uniformity of time between updates are not guaranteed.
* pg 480: Light shakes register at 1.5 gs, and strong shakes at 2.0 gs, while the accelerometer can't seem to register anything beyond 2.3 gs.
* pg 482: Normally, use `motionBegan:withEvent` and `motionEnded:withEvent`; only use `UIAccelerometer` directly if you need more control over the shake gesture.
* pg 494: The `synthesize` directive will not overwrite any accessor or mutator methods you write; it will simply provide the ones you don't.
* pg 494: Nib files contain archived objects, so any additional initialization of those objects must be performed in `initWithCoder`.

### Chapter 16: iPhone Camera and Photo Library
* pg 499: Even though applications are sandboxed, your application still has access to the image library by way of an image picker.
* pg 503: If an image returned to your delegate comes from the camera, it is not stored in the photo library, and so your application must save it if needed.
* pg 504: Since `UIImagePickerController` subclasses `UINavigationController`, you must conform to both protocols `UIImagePickerControllerDelegate` and `UINavigationControllerDelegate`.
* pg 508: All iPhone OS devices support a photo library, which you can resort to if the device does not have a camera.

### Chapter 17: Application Internationalization
* pg 512: If an application can't find a language project that matches the language/region pair or just the language, it uses the resources from the development base language.
* pg 515: The `NSLocalizedString` macro is used with `genstrings` to create string files; at runtime, the macro looks for the best strings file to use, or simply returns the first argument as the string.
* pg 522: When dealing with locales, language codes are lowercase, and the optional country codes that follow are uppercase.
* pg 526: When adding Localizable.strings to your project, change the encoding to Unicode (UTF-16) when prompted, and then localize the file.

