## iOS Programming: The Big Nerd Ranch Guide, 4/e

by Christian Keur, Aaron Hillegass, and Joe Conway

### Chapter 1: A Simple iOS Application

* The lefthand side of Xcode shows the navigator area.
* XIB stands for XML Interface Builder. It is pronounced "zib". You create and configure objects, and then save them into an archive, or the XIB file.
* In Interface Builder, the utility area is to the right of the editor. It is divided into the inspector and library sections.
* XIB files are complied into NIB files that are smaller and easier for the application to parse. That is copied into the application's bundle.
* A connection allows two objects to communicate. There are two kinds: An outlet points to an object, while an action defines a callback.
* Select the File's Owner object, and then the connections inspector to view all wired outlets and actions.
* The App Delegate manages the single top-level `UIWindow`. Assign its `window.rootViewController` to specify the starting view controller.
* The App ID in you provisioning profile must match the bundle identifier of your application. But a development profile can use an App ID of `*` and match any bundle identifier.
* The provisioning profile refers to a developer certificate, a single App ID, and a list of device IDs that the application can be installed on.
* The developer certificate is on your Mac's keychain. The provisioning profile belongs to your development device and your computer.
* When deploying to the App Store, an application must have an icon for every device class on which it can run.
* The `Images.xcassets` file contains an `AppIcon` section where you can place the various icons for your application.
* A good launch image is a content-less screenshot of the application. Although not required, you can provide one for each class of device.

### Chapter 2: Objective-C

* In Objective-C, we typically put an underscore at the beginning of an instance variable, or *ivar*, name.
* A method is a chunk of code to be executed, while a message is the act of acting a class or object to execute a method.
* A message sent to `nil` is ignored. So if your program is not doing anything when you expect something, an unexpected `nil` is usually the culprit.
* Perform fast iteration over an array while attempting to add or remove objects will cause an exception to be thrown.
* In Objective-C, the name of a getter method is just the name of the instance variable that it returns.
* Use accessor methods to access instance variables, even inside a class. Do not refer to them by name, which is prefaced with an underscore.
* All initializers of a class call its designated initializer, which in turn calls the designated initializer of the superclass.
* The `instancetype` keyword can only be used for return types, and matches the return type of the receiver. `init` methods always return `instancetype`.
* In Objective-C, class methods that return an object of their type (like `stringWithFormat`) are called *convenience methods*.
* You can use `array[index]` as shorthand for `objectAtIndex:`, `insertObject:atIndex:`, and `replaceObjectAtIndex:withObject:`.
* Objective-C has no notion of namespaces. Prefix class names with three letters to keep them distinct. Two-letter prefixes are reserved by Apple.
* Precompiled header files saves us from parsing and compiling standard headers repeatedly, but Apple will eventually replace them with the `@import` directive.

### Chapter 3: Managing Memory with ARC

* A variable that does not take ownership of an object is called a *weak reference*. This helps avoid a *strong reference cycle*, or *retain cycle*.
* To decide what reference in a cycle should be weak, look for a parent-child relationship. A parent should own its child, but a child should never own its parent.
* Preface the instance variable declaration with `__weak` to convert it to a weak reference.
* A weak reference knows when the object that it points to is destroyed, and responds by setting itself to `nil`. This avoids a dangling pointer.
* When using `@property`, the name of the generated instance variable is prefixed by an underscore.
* By default, properties are declared `atomic`, `readwrite`, and `strong`. We explicitly declare the `strong` attribute, in case a new default attribute value is chosen later.
* When a property points to an instance of a class that has a mutable subclass, you should set its memory management attribute to `copy`.
* To prevent needless copying, immutable classes implement `copy` to quietly return a pointer to the original and immutable object.
* If you implement a custom setter and a custom getter on a `readwrite` property, or a custom getter on a `readonly` property, then you must declare your own instance variable.
* You can use this technique to define accessor methods that are not backed by any instance variable, or backed by some property of a contained object.
* ARC was born when the Clang static analyzer became so good that it could insert all the `retain` and `release` messages automatically.
* Inside an `@autoreleasepool` directive, any newly instantiated object returned from a method that does not have `alloc` or `copy` in its name is put into an autorelease pool.

### Chapter 4: Views and the View Hierarchy

* Each view draws itself by rendering itself to a *layer*, which is an instance of `CALayer`. These layers are then composited together on the screen.
* The `x` and `y` of `CGPoint` and `width` and `height` of `CGRect` are specified in points, not pixels, so they are consistent across different resolutions.
* A pixel is equal to half a point on Retina, and equal to one point on non-retina. When printing to paper, an inch is 72 points long.
* The `drawRect:` method renders the view onto its layer. `UIView` subclasses override this method to perform custom drawing.
* The `bounds` rectangle is in the view's own coordinate system, for drawing itself. The `frame` rectangle is in the superview's coordinate system, for positioning the subview.
* Instances of `UIBezierPath` define and draw lines and curves that you can use to make shapes.
* To set the color drawn by the `stroke` method of `UIBezierPath`, send the message `setStroke` of a `UIColor` instance.
* `CGContextRef` is the graphics context, which holds the drawing properties (like the pen color and line thickness) and the memory that is being drawn upon.
* The system creates a `CGContextRef` instance before calling `drawRect:`, and composites that instance after the method completes executing.
* The current context is an application-wide pointer that is assigned just before `drawRect:` is called. You can retrieve it by calling `UIGraphicsGetCurrentContext()`.
* A Core Graphics type ending in `Ref` is a pointer typedef. If you create such an object with a function that has `Create` or `Copy` in the name, then you must pass it to the matching `Release` function.
* Some Core Graphic features cannot be "unset." Instead, you must pass `currentContext` to `CGContextSaveGState` beforehand, and then to `CGContextRestoreGState` afterward.

### Chapter 5: Views: Redrawing and UIScrollView

* When the user touches a view, it is sent the message `touchesBegan:withEvent:`.
* After the run loop dispatches an event, it invokes `drawRect:` on all dirty views. Send the message `setNeedsDisplay` to a view to mark it as dirty.
* If you call `setNeedsDisplayInRect:` instead, that `CGRect` method is passed to `drawRect:`, which you can use to optimize re-drawing. But most developers don't bother with this.
* The `contentSize` of a `UIScrollView` is the size of the area that it can be used to view. This is usually the size of its subview.
* After adding multiple subviews to a `UIScrollView`, you can set its `pagingEnabled` property to page between them, which snaps its viewing port to a subview. 

### Chapter 6: View Controllers

* A view controller's `view` is not created until it needs to appear on screen.
* When a view controller initializes its view hierarchy from a NIB file, you do not override `loadView`. The default implementation loads the NIB file.
* Declare outlets as `weak`. This ensures that, when memory is low, destroying `view` also destroys its subviews and avoids memory leaks.
* When a view controller is contained by a `UITabBarController`, its `tabBarItem` property appears in the tab bar for that purpose.
* Calling `init` calls the designated initializer `initWithNibName:bundle:`. It looks for a NIB with the class name in the main bundle, which it specifies by passing `nil` as the bundle.
* To preserve the benefits of lazy loading, you should never access the `view` property of a view controller in `initWithNibName:bundle:`.
* Destroying and reloading the view of a view controller due to memory constraints is not the typical behavior on newer devices.
* When calling `valueForKey:`, key-value coding will first look for a corresponding getter method, followed by an instance variable.
* Retina display is 640x1136 pixels on a 4" display, and 640x960 pixels on a 3.5" display, while earlier devices have a display of 320x480 pixels.
* By appending the suffix `@2x` to a higher resolution image, method `imageNamed:` of `UIImage` loads the file that is appropriate for the particular device.

### Chapter 7: Delegation and Text Input

* `UIResponder` is an abstract class that defines methods for handling events, such as touch events, motion events, and remote control events.
* Touch events are sent directly to the corresponding view. For other event types, `UIWindow` has a `firstResponder` pointer that specifies their handler.
* Text fields and text views show and hide the keyboard upon becoming and resigning the first responder. Most views refuse to become the first responder so that they do not steal focus.
* By default, protocol methods are required, but you can precede a list of optional methods with the directive `@optional`.
* If a method in a protocol is required, then a message will be sent to it without first creating a `SEL` instance and sending message `respondsToSelector:`.
* You can declare that an object conforms to a protocol in the private extension of that class, as opposed to in its header file.
* A class that uses a delegate must declare it as a weak reference in order to prevent strong reference cycles.
* The `textFieldShouldReturn:` method must explicitly dismiss the keyboard by sending the message `resignFirstResponder` to the text field.
* If your application is throwing exceptions and you're not sure why, adding an exception breakpoint will help you pinpoint what's going on.
* The `UIApplicationMain` method creates an instance of `UIApplication`, which maintains the run loop.
* Next, `UIApplicationMain` creates an instance of the delegate class, assigns it to `delegate` of the `UIApplication` instance, and sends a message to `application:didFinishLaunchingWithOptions:`.

### Chapter 8: UITableView and UITableViewController

* The designated initializer of `UITableViewController` is `initWithStyle:`.
* To return a read-only view of an `NSMutableArray`, define a private `NSMutableArray` property, and then define an `NSArray` property with the `readonly` attribute that returns it.
* The `contentView` of a `UITableViewCell` contains the three subviews `textLabel`, `detailTextLabel`, and `imageView`.
* Call the `registerClass:forCellReuseIdentifier:` of `UITableView` to specify which kind of cell it should instantiate if there are no cells with a given identifier in the reuse pool.
* To define a placeholder in a code snippet from the library, place its name between `<#` and `#>`.

### Chapter 9: Editing UITableView

* The `strong` attribute must be used with properties that reference top-level objects in the XIB file.
* To resize an empty view in Interface Builder, go to the Simulated Metrics section, and select None for the Size option.
* To load a NIB file manually, send `loadNibNamed:owner:options:` to `NSBundle`, where the owner is substituted for the File's Owner placeholder.
* A `UITableViewController` automatically sets the `editing` property of its table view to match its own `editing` property.
* To add a cell, a button above the cells of the table view is usually for adding a record for which there is a detail view.
* To add a cell, a cell with a green plus sign is usually for adding a new field to a record.
* The `view` and `tableView` properties of `UITableViewController` refer to the same view.
* When you implement `tableView:commitEditingStyle:forRowAtIndexPath:`, swipe-to-delete also works automatically.
* In the call to `tableView:moveRowAtIndexPath:toIndexPath:`, the row of the new path is that after the item has been removed from its existing path.
* In editing mode, the `UITableView` will not display the reordering controls if its data source does not implement `tableView:moveRowAtIndexPath:toIndexPath:`.
