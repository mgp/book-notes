## The Swift Programming Language: Language Guide

### [The Basics](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html)

### Constants and Variables
* You declare constants with the `let` keyword and variables with the `var` keyword.
* In practice, it is rare that you will need a type annotation. It may be useful if you are declaring a variable and no initial value is provided.
* To use string interpolation, wrap the variable name in parentheses and escape it with a backslash before the opening parenthesis.

### Comments
* You can nest multiline comments.

### Numeric Literals
* Both integers and floats can be padded with extra zeroes and can contain `_` characters to help with readability.

### Numeric Type Conversion
* Prefer `Int` for even non-negative values. This means that integer constants and variables are immediately interoperable in your code and will match the inferred type for integer literal values.
* Conversion between integer and floating-point must be explicit, regardless of the direction. An exception is made for numeric literals, which do not have an explicit type in and of themselves.

### Tuples
* You can decompose tuples. If you only need some parts of the tuple's values, ignore parts of the tuple with the `_` character when decomposing.
* If the elements in a tuple are unnamed, you can access them with index numbers starting at zero, e.g. `tupleName.0`. If they are named, you can access them by their element names, e.g. `tupleName.firstElementName`.
* Tuples are not suited for the creation of complex data structures. Use them primarily for returning multiple values from a function.

#### Optionals
* An optional lets you indicate the absence of a value for any type at all, without the need for special constants like `NSNotFound`.
* You can only assign `nil` to an optional variable. You cannot use it with nonoptional constants and variables. 
* The value `nil` is not the same in Objective-C as it is in Swift. In Swift, `nil` is not a pointer like it is in Objective-C. Instead, it is an absence of a value of a certain type.
* Once you're sure that an optional does contain a value, you can access that value by appending `!` to the end of the optional's name. This is called *forced unwrapping* of its value.
* Optional binding, written as `let constantName = someOptional` or `var constantName = someOptional`, can be used with `if` and `while` statements to check for a value inside an optional and extract it as a constant in a single action.
* If the conversion that is part of optional binding is successful, then the constant is initialized with the value inside the optional, and there is no need to use the `!` suffix.
* An *implicitly unwrapped optional* has a `!` instead of a `?` after the type that you want to make optional. Use this if an optional will always have a value after it's first set, and you want to remove the need to check and unwrap the optional's value on each access.
* If you try to access an implicitly unwrapped optional when it doesn't contain a value, you trigger a runtime error, much like if you place a `!` after a normal optional missing a value.

