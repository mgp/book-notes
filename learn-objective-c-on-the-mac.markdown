## Learn Objective-C on the Mac

by Mark Dalrymple and Scott Knaster

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/Learn-Objective-C-Mac-For-iOS/dp/1430241888)!*

### Chapter 2: Extensions to C
* pg 8: Files ending in `.m` are handled by the Objective-C compiler.
* pg 9: The `#import` directive is like `#include`, but doesn't require the programmer to use an `#ifndef` directive so that a file is included only once.
* pg 9: A framework is a collection of parts -- header files, libraries, images, sounds, and more -- collected together into a single unit.
* pg 10: Each framework has a master header file that includes all the framework's individual header files.
* pg 13: `BOOL` is just a type definition for the signed character type, where `YES` is `1` and `NO` is `0`.
* pg 16: Don't compare a `BOOL` value directly to `YES`, as some methods might return values that assume that anything non-zero is treated as `YES`.

### Chapter 3: Introduction to Object-Oriented Programming
* pg 39: `id` is a generic type that's used to refer to any kind of object; it's simply a pointer.
* pg 41: Each object has a reference to its class; when an object receives a message, the object delegates to its class, which finds and executes the correct code.
* pg 46: The argument type or return type in a method signature is always in parentheses.
* pg 47: If a method takes an argument, it has a colon; if it takes no arguments, it has no colons.
* pg 48: Defining a method solely in the `@implementation` directive does not make it private; the method is still accessible due to the language's dynamic nature.

### Chapter 4: Inheritance
* pg 62: There is no support for multiple inheritance, but you can reap some of its benefits by using categories and protocols.
* pg 68: `NSObject` declares one instance variable, named `isa`, which holds the pointer to the object's class.
* pg 69: To fix the fragile base class problem, the 64-bit Objective-C runtime in Leopard use indirection for determining ivar locations instead of adding offsets to `self`.
* pg 70: To call the superclass version of a method, send an appropriate message to `super`.

### Chapter 5: Composition
* pg 75: The description method in your class defines how it is printed by `NSLog` with the `%@` format specifier.
* pg 76: Pointers are automatically initialized to `nil` (a zero value) when declared.
* pg 77: When a constructor calls its superclass constructor with `[super init]`, it's convention to assign the result to `self`.
* pg 80: Getter methods shouldn't start with `get`; in Cocoa, this means the value is returned via a pointer passed in as an argument.

### Chapter 6: Source File Organization
* pg 88: Using a `.mm` file as an extension tells the compiler you've written your code in Objective-C++, which allows using C++ and Objective-C together.
* pg 90: In Xcode, groups allow organizing files together but do not equate to directories on your hard drive.
* pg 92: In an `#import` statement, a header file in angle brackets is a read-only system header file, while one in quotes is local to your project.
* pg 95: A class is forward referenced with `@class`, which is put between the `import` statements and the `@interface` keyword.

### Chapter 7: More About Xcode
* pg 106: When typing a class or method name, press the escape key to see all code completion options.
* pg 108: To move between placeholders after code completion, press Control and forward slash.
* pg 111: Edit all in Scope will allow renaming a local variable in a function, while the Refactor option allows renaming an entire class.
* pg 115: To search for classes, methods, or properties, press Command, Option, Shift, and D.
* pg 119: Comments with `TODO:`, `FIXME:`, `!!!:` and `???:` are indexed automatically by the function menu.
* pg 120: The split box beneath the lock will horizontally split the window; holding Option will split the window vertically.
* pg 122: Holding Control and double-clicking on a symbol will issue a documentation seach for that symbol.
* pg 124: Disable a breakpoint by clicking on it symbol, or remove it by dragging the symbol out of the gutter.
* pg 129: Hold control and press period to cycle forward through code completion options; adding shift will cycle backward.

### Chapter 8: A Quick Tour of the Foundation Kit
* pg 131: Cocoa is two different frameworks: Foundation and Application Kit, where the former has useful low-level, data-oriented classes and types.
* pg 135: Declaring a method with a plus sign makes it a class method.
* pg 136: The `isEqualToString` method of a `NSString` compares string values; using `==` will only compare the pointers.
* pg 141: Because inheritance works with class methods and instance methods, `stringWithFormat` is available on class `NSMutableString`.
* pg 143: A class name with `CF` means it's part of the Core Foundation framework and written in C; CF and Cocoa objects are "toll-free bridged," so they can be used interchangeably.
* pg 147: When `NSEnumerator` returns `nil`, iteration has finished, and so you can't store `nil` in arrays.
* pg 149: The initializer lists for both `NSArray` and `NSDictionary` have `nil` as their last value.
* pg 150: Extending `NSString`, `NSArray`, or `NSDictionary` is problematic because they're implemented as class clusters, or many classes behind a common interface.
* pg 152: `NSValue` allows putting structs into an `NSArray` or `NSDictionary`; the `@encode` compiler directive creates the required string describing the type for you.

### Chapter 9: Memory Management
* pg 163: The `retain` method returns an `id`, so you can chain it with other messages.
* pg 167: If a setter method retains its argument and then releases its current value, it guards against the retain count hitting `0` when passing the current value as the argument.
* pg 169: An autorelease pool is created on an event, and destroyed when the event ends.
* pg 172: If you get a hold of an object in any way other than `new`, `alloc`, or `copy`, assume it has a retain count of `1` and has been autoreleased.
* pg 175: Autorelease pools are kept in a stack; sending an `autorelease` message puts the receiver into the topmost pool.
* pg 177: For iPhone programming, Apple recommends you avoid using `autorelease` in your own code, and convenience functions that give you autoreleased objects.

### Chapter 10: Object Initialization
* pg 179: The `alloc` method initializes all memory to zero, all numbers as `0`, all booleans as `NO`, and all pointers as `nil`.
* pg 181: If a new object is returned from a superclass `init` method, `self` must be updated so any subsequent instance variable references affect the right memory locations.
* pg 183: The `init` methods are ordinary methods that follow a naming convention; you can provide as many convenience initializers as needed.
* pg 198: All initializer methods of a class use the designated initializer to do initialization work, and subclasses use the superclass' designated initializer as well.

### Chapter 11: Properties
* pg 205: The `@property` keyword in the header file includes the type, while the `@synthesize` keyword does not.
* pg 208: Always specify `copy` in properties for string attributes; a common error is to use a string from a UI component, which are mutable and change as the user types.
* pg 212: A default decoration for the `@property` keyword is `assign`; alternatively you can use `retain` or `copy`.
* pg 213: Passing `nil` to a setter generated from the property will release the previous value if needed.

### Chapter 12: Categories
* pg 226: Using categories you can split a class' implementation across multiple files, and even among frameworks.
* pg 227: To simulate a private method, exclude it from the `.h` file, and add it to a category at the top of the implementation file while leaving the definition in the header's `@implementation` part.

### Chapter 13: Protocols
* pg 235: You adopt a protocol by listing its name in your class' interface declaration; then your class conforms to the protocol.
* pg 239: When implementing `copyWithZone`, send the `allocWithZone` message to `[self class]` to create a class of the right subtype.
* pg 242: A `copyWithZone` method should send a `copyWithZone` message to its parent class if available, but send a `copy` message to objects that compose it.
* pg 246: Specifying protocol names on instance variables and methods arguments ensures that the assigned variables conform to those protocols.

### Chapter 14: Introduction to the AppKit
* pg 252: `IBOutlet` is defined to be nothing, so it disappears upon compiling; `IBAction` is defined to be `void`, and so the associated method returns nothing.
* pg 253: Nib files, which stands for NeXT Interface Builder, are binary files that serialize objects, and `.xib` files are XML that are compiled into nib files.
* pg 260: When making connections in Interface Builder, drag from the object that needs to know something to the object it needs to know about.
* pg 262: When an object recreated from a nib file is initialized, all its `IBOutlet` instances are `nil`; you must wait until `awakeFromNib` is called to do any work with them.

### Chapter 15: File Loading and Saving
* pg 266: `NSTimeInterval` is simply a `typedef` of `double` representing some interval of seconds.
* pg 268: Property list files can be converted between XML and compressed binary format using the `plutil` command.
* pg 270: The `NSCoder` class allows for converting objects to `NSData` and back; `NSData`, along with `NSArray`, `NSDictionary`, `NSString`, `NSNumber`, and `NSDate`, can be used in property lists.
* pg 273: If a parent class adopts `NSCoding`, `initWithCoder` should call the superclass implementation of `initWithCoder` first; otherwise, simply call `init`.

### Chapter 16: Key-Value Coding
* pg 280: KVC automatically boxes and unboxes scalar values into `NSValue` or `NSNumber` when neccessary.
* pg 281: Methods like `valueForKey` and `setValue:forKey` will use getters and setters if they exist, and will access the variables directly otherwise.
* pg 283: If a key path includes an array or to-many attribute, the remaining part of the key is sent to every object in the array, and the values are returned in an array.
* pg 288: KVC operators are always used with `valueForKeyPath`.
* pg 294: When printing, `"<null>"` is the `[NSNull null]` object which could be returned as a value from a `NSDictionary`, while `"(null)"` is `nil` and could mean the key did not exist.

### Chapter 17: NSPredicate
* pg 301: You cannot use `$VARIABLE` for key paths in a `NSPredictate` string, only values; use the `%K` format specifier to substitute key paths.
* pg 303: Curly braces denote an array, even though they are printed as having parentheses.
* pg 305: String relational operators can be decorated with `[c]` for case-insensitivity, and `[d]` for diacritic-insensitivity (i.e. removing accents), or both.

