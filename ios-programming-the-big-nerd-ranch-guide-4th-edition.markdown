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

### Chapter 10: UINavigationController
* The view of a `UINavigationController` always has two subviews: a `UINavigationBar` and the view belonging to the `topViewController`.
* A `UINavigationController` will automatically resize the view of a `UIViewController` on its stack so that it fits below the navigation bar.
* Do not position subviews near the top of a XIB file. A view will extend beneath either a `UINavigationBar` or beneath a `UITabBar`.
* If you display an `.m` file next to its XIB file using the assistant editor, you can Control-drag views from the XIB to the `.m` file to automatically create outlets.
* The shortcut Command-T opens up a new tab, while Command-Shift-} and Command-Shift-{ cycle through the tabs.
* When the message `endEditing:` is sent to a view, if it or any of its subviews is the first responder, it will resign its first responder status and dismiss the keyboard.
* Every `UIViewController` has a `navigationItem` property that supplies the navigation bar with the content that it needs to draw.
* For the `titleView` of a `UINavigationItem`, you can either use a basic string as the title or specify a `UIView` instance.
* `UIViewController` has an `editButtonItem` property that is of type `UIBarButtonItem`. When pressed, it sends the message `setEditing:animated:` to the controller.

### Chapter 11: Camera
* The `contentMode` property of `UIImageView` determines where to position and how to resize the content of its frame.
* You cannot use `UIImagePickerControllerSourceTypeCamera` on devices without cameras, and must send `isSourceTypeAvailable:` to `UIImagePickerController` first.
* To populate the photo library in the simulator, open Safari in the simulator, navigate to a page with an image, and save the image.
* Objects of type `NSUUID` represent a UUID and are generated using the time, a counter, and a hardware identifier, which is usually the MAC address from the WiFi device.
* While a `UIBarButtonItem` only sends its target an action message when it is pressed, `UIControl` can send messages in response to a variety of events.
* With the `#pragma mark` directive, you can specify just a divider with `-`, a label, or both by prefacing the label with `-`.
* Just like the class method `isSourceTypeAvailable:` returns whether a device has a camera, `availableMediaTypesForSourceType:` returns whether it can capture video.
* After the user takes a video, `imagePickerController:didFinishPickingMediaWithInfo:` is passed the path to the video in a temporary directory.

### Chapter 12: Touch Events and UIResponder
* If a finger moves outside the frame of the `UIView` that it began on, that view still receives `touchesMoved:withEvent:` and `touchesEnded:withEvent` messages.
* While the methods of `UIResponder` accept a set of `UITouch` instances, there are usually multiple responder methods with one or more touches.
* Once a touch begins, a `UIResponder` will ignore subsequent touches unless its `multipleTouchesEnabled` property is set.
* You can simulate touching with multiple fingers in the simulator by holding down the Option key as you drag.
* The `valueWithNonretainedObject:` method of `NSValue` is useful if you want to add an object to a collection without creating a strong reference to it.
* This method also allows objects that do not implement `NSCopying` to be used as keys in dictionaries.
* Every `UIResponder`, such as `UIView`, `UIViewController`, `UIWindow`, and `UIApplication`, has a pointer called `nextResponder`, forming a *responder chain*.
* By default, methods `touchesBegin:withEvent:`, `touchesMoved:withEvent:`, and `touchesEnded:withEvent` simply delegate to `nextResponder`.
* The `CGRectContainsPoint` method returns whether a given point is in a `CGRect` variable.

### Chapter 13: UIGestureRecognizer and UIMenuController
* A `UIGestureRecognizer` intercepts touches destined for a view, and so this view may not respond to typical `UIResponder` messages.
* Setting the `delaysTouchesBegan` property delays sending `touchesBegan:withEvent:` to its view if it is still possible for the gesture to be recognized.
* The method `requireGestureRecognizerToFail:` allows you to require a double-tap recognizer to fail before invoking a single-tap recognizer.
* The `locationInView:` method of `UIGestureRecognizer` returns the coordinate where the gesture occurred in the coordinate system of its view argument.
* For a `UIMenuController` to appear, a view that responds to at least one action message of its menu items must be a first responder of the window.
* If you have a custom view that needs to become the first responder, you must implement `canBecomeFirstResponder` to return `YES`.
* A long press gesture begins when the user holds a touch for 0.5 seconds, and ends when the touch ends.
* A gesture recognizer will send `gestureRecognizer:shouldRecognizeSimultaneouslyWithGestureRecognizer:` to its `delegate` if another gesture recognizer has recognized its gesture, too.
* The `translationInView:` method of `UIPanGestureRecognizer` returns a `CGPoint` containing how far the pan has moved in the coordinate system of its view argument.
* Pass `CGPointZero` to `setTranslation:inView:` of `UIPanGestureRecognizer` at the end of its action method to report the change in translation between action method calls.
* If a `UIResponder` implements a method in `UIResponderStandardEditActions`, it enables the corresponding default menu item in an unmodified menu controller.
* A `UIGestureRecognizer` for a discrete gesture like tap only enters the *recognized* state; it does not transition through the *begin*, *change*, and *end* states.
* A gesture recognizer can enter the *fail* state, such as when no amount of finger contortion can make the particular gesture, given their current position.

### Chapter 14: Debugging Tools
* The debug gauges in Xcode are based on the hardware running the simulator, which is undoubtedly more powerful than the iOS device.
* In the *Memory Report*, the *Memory* graph scales so that peak usage represents 100%.
* In the *Allocations* instrument, the *# Overall* column shows you how many instances of a class have been allocated -- even if they have since been deallocated.
* When you select a method, the percentage on each line of the method specifies the amount of memory that line allocates compared to the other lines.
* The *Generation Analysis* category, also called Heapshot Analysis, is very useful for inspecting what objects get allocated for a specific event.
* By choosing *Invert Call Tree* in the *Time Profiler*, you can see how much time is spent in each method call.
* If a system call takes a lot of time, you can change it so that the time spent in that method is attributed to the method that called it.
* The `mach_msg_trap()` method is where the main thread sits when waiting for input, and so it is not a bad thing to spend time in this function.
* `CGPoint` and `CGRect` are structures instead of Objective-C classes so they do not incur the overhead of sending messages for simple operations.
* A project has at least one *target*. You build and run the target, which produces a *product*, such as the application, a library, or a unit test bundle.
* Under *Build Settings*, setting *Analyze During 'Build'* will automatically run the static analyzer every time you build your application.

### Chapter 15: Introduction to Auto Layout
* A single application that runs natively on both the iPhone and the iPad is called a *universal application*.
* The iPhone 4S has 320x480 points, the iPhone 5 and later has 320x568 points, and all iPads have 768x1024 points.
* Constraints determine the layout attributes like the size, margins, center, and baseline of the alignment rectangle constructed by Auto Layout.
* The *nearest neighbor* of a view is the closest sibling view in a specified direction, or its superview if no such sibling view exists.
* Apple recommends that you add constraints using Interface Builder whenever possible, instead of defining them programmatically.
* Constraint problems include missing constraints, conflicting constraints, and constraints not matching a view's size and position on the canvas.
* By Control-dragging from one view to another on the canvas, you can specify a constraint between those views.
* Constraints have a priority value ranging from `1` to `1000`, where `1000` is a required constraint. This is their default value.
* To remedy an ambiguous layout, which occurs when there is more than one way to fulfill a set of constraints, add another constraint.
* To display an alternate layout resulting from ambiguous constraints, send the message `exerciseAmbiguityInLayout` to each subview.
* A misplaced view problem is when a view's frame in a XIB does not match its constraints.
* For a string representation of the view hierarchy, with views having ambiguous layout tagged, set a breakpoint and then enter into the debugger `po [[ UIWindow keyWindow] _autolayoutTrace]`.
* If you need completely different views depending on the device, create one XIB file with a `~iphone` suffix, and another with an `~ipad` suffix.

### Chapter 16: Auto Layout: Programmatic Constraints
* If creating and constraining an additional view to add to a view hierarchy that was created by loading a NIB file, override method `viewDidLoad`.
* Without setting `translatesAutoresizingMaskIntoConstraints` to `NO`, iOS creates constraints from the mask, which may conflict with other constraints.
* In the Visual Format Language, or VFL, the dash by itself sets the spacing to the standard number of points between views, which is 8.
* You should call `addConstraints:` on the closest common ancestor of all the views that are affected by the constraint.
* Unlike other constraints, intrinsic content size constraints have a *content hugging priority* and a *content compression resistance priority*.
* A content hugging priority of `1000` means that the view should never be allowed to grow larger than its intrinsic content size.
* A content compression resistance priority of `1000` means that the view should never be allowed to be smaller than its intrinsic content size.
* Before Auto Layout, each view had a resizing mask that constrained the relationship between a view and its superview.
* When debugging constraints, a constraint from the resizing mask has the type `NSAutoResizingMaskLayoutConstraint` instead of `NSLayoutConstraint`.

### Chapter 17: Autorotation, Popover Controllers, and Modal View Controllers
* *Device orientation* represents the physical orientation of the device. You can access it through the `orientation` property of `UIDevice`.
* The *interface orientation* is a property of the running application, and is determined by the location of the Home button.
* For the interface orientation to change, both the `rootViewController` of the application and the application itself must allow the new orientation.
* To match convention, a view controller on the iPad allows all four orientations, while one on the iPhone allows any orientation other than upside-down.
* A `UITabViewController` only supports an orientation if the view controllers for each of its tabs support it.
* The `statusBarOrientation` method of the `UIApplication` instance returns the interface orientation.
* Trying to instantiate `UIPopoverController` on anything but an iPad will throw an exception.
* Explicitly calling `dismissPopoverAnimated:` on a `UIPopoverController` will not send `popoverControllerDidDismissPopover:` to its delegate.
* The `presentingViewController` property of `UIViewController` allows a modally-presented view controller to communicate with the view controller that presented it.
* When a modally-presented view controller uses the `UIModalPresentationFormSheet` style, the presenting view controller is not sent `viewWillAppear:` or `viewDidAppear:`.
* The two relationships between view controllers are *parent-child* relationships and *presenting-presenter* relationships.
* `UINavigationController`, `UITabBarController`, and `UISplitViewController` are all *view controller containers*, where each has a `viewControllers` property.
* A *view controller container* subclasses `UIViewController` and selectively adds the views of `viewControllers` as subviews of its own view.
* The `parentViewController` property of `UIViewController` refers to the closest view controller ancestor in a *family*, or tree of view controller containers.
* With a modally-presented view controller, `presentingViewController` and `presentedViewController` are valid for each view controller in each family, and always refer to the oldest ancestor in the other family.

### Chapter 18: Saving, Loading, and Application States
* An `NSCoder` organizes the stream of data that it writes to the filesystem as a collection of key-value pairs.
* Holding down the Option key and clicking on a class name presents a pop-up window with a brief description and links to its header file and its reference.
* Although a XIB file is not a standard archive, archiving is how Interface Builder writes and reads XIB files.
* In the application sandbox, directories `Documents/` and `Library/Caches` contain data that persists between runs, but only the former is synchronized with iTunes.
* Static method `archiveRootObject:toFile:` of `NSKeyedArchiver` archives an object implementing `NSCoding` to the given filename.
* When an overlay to handle an SMS message, push notification, phone call, or alarm appears on top of your application, it is in the *inactive state*.
* An application spends about ten seconds in the *background state* before it enters the *suspended state*.
* The inactive state loses the ability to receive events, the background state loses visibility, and the suspended state loses the ability to execute code.
* The operating system terminates suspended applications as needed when available memory is low; such an application receives no notification of this.
* Transitioning to the background state is a good place to save outstanding changes, because it's the last time it can execute code before entering the suspended state.
* Calling `writeToFile:atomically:` of `NSData` with `YES` writes the data to a temporary file, and then renames that file to the first parameter.
* In addition to `self`, the implicit variable `_cmd` is the selector for the current method, and can be translated to a string using `NSStringFromSelector`.
* The `localizedDescription` method of `NSError` returns a human-readable description that is suitable for display to the user.
* Use `NSException` if the error lies with the programmer, and `NSError` if a request is reasonable but could not be fulfilled.
* To use `writeToFile:` and `initWithContentsOfFile:` with collection objects, they must contain only *property list serializable* objects, namely `NSString`, `NSNumber`, `NSDate`, `NSData`, `NSArray`, and `NSDictionary`.
* Files within the application bundle are read-only, and you cannot dynamically add files to the application bundle at runtime.

### Chapter 19: Subclassing UITableViewCell
* To subclass `UITableViewCell`, add subviews to its content view, which is resized when the user enters editing mode, for example.
* Function `UIGraphicsBeginImageContextWithOptions` creates a new `CGContextRef` that is offscreen, and sets it as the current context.
* Get a `UIImage` from the current context by calling `UIGraphicsGetImageFromCurrentImageContext`, and then `UIGraphicsEndImageContext` to clean up.
* To allow tapping an image to show a full-size image, add a transparent `UIControl` or `UIButton` on top of the thumbnail.
* When such a button is tapped, it invokes a method of the `UITableViewCell` subclass. This should invoke a block assigned by the controller to implement behavior.
* Blocks are created on the stack and not the heap, and so the assignment of a block to a `UIView` or persisted instance must use the `copy` attribute.
* Blocks own the objects that they capture from their enclosing scope, which can easily create a strong reference cycle.
* A block should instead capture a weak reference to whatever object owns it. When the block starts executing, assign that to a strong reference to ensure that exists while the block executes.
* A `UICollectionViewLayout` subclass controls the attributes of each cell, including its position and size, in a `UICollectionView`.
* A `UICollectionViewCell` has a content view with no subviews, and so you must subclass it if you are using a `UICollectionView`.

### Chapter 20: Dynamic Type
* When a font is requested for a given text style, the system will use the preferred text size to return the appropriately configured font for that style.
* To react to users' changes in text size, you must subscribe to `UIContentSizeCategoryDidChangeNotification` and reassign the preferred fonts.
* For a view to grow larger than its `intrinsicContentSize` in a given direction, there must be a constraint with a higher priority than that view's *Content Hugging Priority*.
* For a view to grow smaller than its `intrinsicContentSize` in a given dimension, there must be a constraint with a higher priority than that view's *Compression Resistance Priority*.
* Given the `preferredContentSize` property value of `UIApplication`, assign the `height` property of a `UITableView` the correct row height.
* The `awakeFromNib` method is called after unarchiving, and is a great place to do any additional UI work that cannot be done within the XIB file.
* Modifying the `frame` or `bounds` of a view will only persist until the next time the view is laid out based on the constraints that it has.
* Constraints are instances of `NSLayoutConstraint`, so just like you can create outlets to views, you can create outlets to constraints.
* A *placeholder constraint* in Interface Builder is only temporary and is removed at build time.
* Use placeholder constraints if adding a programmatic constraint creates a conflict, but removing the constraint in IB would warn about misplaced views or ambiguous layout.

### Chapter 21: Web Services and UIWebView
* `NSURLRequest` holds all data necessary to communicate with a server, including an `NSURL` instance, a caching policy, a timeout value, and any additional HTTP data.
* `NSURLSessionTask` encapsulates the lifetime of a single `NSURLRequest`, and has methods to cancel, suspend, and resume the request.
* `NSURLSession` acts as a configurable factory for `NSURLSessionTask` instances.
* To percent encode and decode a string, use methods `stringByAddingPercentEscapesUsingEncoding:` and `stringByRemovingPercentEncoding`.
* A `NSURLSessionTask` instance is always created in the suspended state, and so calling `resume` on the task will start the request.
* The main thread is also referred to as the *UI thread*, as any code that modifies the UI has to run on the main thread.
* By default, `NSURLSessionDataTask` runs the completion handler on a background thread, but you can transfer control to the main thread by calling `dispatch_async` with `dispatch_get_main_queue()`.
* To respond to an authentication challenge, an `NSURLSessionDataDelegate` can return a `NSURLCredential` containing a username and password.
* `NSURLRequest` will compute the size in bytes of its body, and automatically add a `Content-Length` header with this size.

### Chapter 22: UISplitViewController
* A `UISplitViewController` refers to an array of view controllers, but that array can only contain two view controllers.
* The `splitViewController` message of `UIViewController` returns the `UISplitViewController` it is a part of, even if it is not a member of the array of `UISplitViewController`.
* By supplying a `UIBarButtonItem` to the delegate of `UISplitViewController`, tapping this button displays the master view controller in a specialized `UIPopoverController`.
* If the detail view controller does not specify a title for the `UIBarButtonItem`, then it won't appear, unless the master view controller specifies a title for its `navigationItem`.
* This requires placing the detail view controller inside a `UINavigationController`, unless you instantiate your own `UINavigationBar` or `UIToolbar` to hold the `UIBarButtonitem`.

### Chapter 23: Core Data
* In Core Data, a table/class is called an *entity*, and its columns/properties are called its *attributes*.
* With a *transformable* attribute, a supplied `NSValueTransformer` subclass converts between instances of a class and `NSData` for storage in Core Data.
* An entity in a to-one relationship has a pointer to the related entity, while an entity with a to-many relationship has an `NSSet` containing all related entities.
* An `NSManagedObject` works a bit like a dictionary, holding a key-value pair for every property in the entity.
* If your model objects must do something in addition to holding data, subclass `NSManagedObject`, and associate that subclass with the entity in your model file.
* Adding a `NSManagedObject` to the database invokes its `awakeFromInsert` method, which can contain the logic that is normally found in an initializer.
* An `NSPersistentStoreCoordinator` uses your model file in the form of an `NSManagedObjectModel` to interact with a SQLite database.
* Calling `save:` on an `NSManagedObjectContext` will update all records in the SQLite database with any changes since the last time it was saved.
* You use an `NSPredicate` to select which instances an `NSFetchRequest` should return, but you can also use it with `filteredArrayUsingPredicate:` of `NSArray`.
* To persist a reorderable list to Core Data, add an `orderingValue` of type `double` to each entity. When inserting an element, assign its `orderingValue` to the mean of that of its neighbors.

