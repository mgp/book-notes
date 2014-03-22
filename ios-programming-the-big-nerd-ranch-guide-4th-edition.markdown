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
