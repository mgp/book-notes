## The Swift Programming Language: Language Guide

### [The Basics](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html)

#### Constants and Variables
* You declare constants with the `let` keyword and variables with the `var` keyword.
* In practice, it is rare that you will need a type annotation. It may be useful if you are declaring a variable and no initial value is provided.
* To use string interpolation, wrap the variable name in parentheses and escape it with a backslash before the opening parenthesis.

#### Comments
* You can nest multiline comments.

#### Numeric Literals
* Both integers and floats can be padded with extra zeroes and can contain `_` characters to help with readability.

#### Numeric Type Conversion
* Prefer `Int` for even non-negative values. This means that integer constants and variables are immediately interoperable in your code and will match the inferred type for integer literal values.
* Conversion between integer and floating-point must be explicit, regardless of the direction. An exception is made for numeric literals, which do not have an explicit type in and of themselves.

#### Tuples
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

### [Basic Operators](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html)

#### Arithmetic Operators
* The assignment operator in Swift does not return a value, so you cannot accidentally use `=` when you mean `==`.
* The `%` operator is the remainder operator, and not a modulo operator.
* Given a negative first operand, the remainder operator can yield a negative value. But the sign of the second operand for the remainder operator is ignored, so `a % b` and `a % -b` always yield the same answer.
* The remainder operator can also accept a second operand that is a floating point number.
* It is recommended that you use the prefix version of the increment and decrement operators, unless you need the specific behavior of their postfix forms.

#### Compound Assignment Operators
* The compound assignment operators do not return a value.

#### Comparison Operators
* The identity operators `===` and `!==` test whether two object references both refer to the same object instance.

#### Nil Coalescing Operator
* The *nil coalescing operator* `(a ?? b)` unwraps an optional `a` if it contains a value, or returns a default value `b` if `a` is `nil`. If `a` is not `nil`, then `b` is not evaluated.

#### Range Operators
* The *closed range operator* `a...b` is inclusive of `b`, while the *half-open range operator* `a..<b` is exclusive of `b`.

### [Strings and Characters](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html)

#### Strings Are Value Types
* The `String` type is a value type, and such a value is copied when it is passed to a function or method, or when it is assigned to a constant or value.

#### Working with Characters
* You can create a stand-alone character constant or variable from a single-character string literal by providing a `Character` type annotation.

#### String Interpolation
* A string literal can interpolate not just constants, variables, and literals, but expressions such as `\(Double(multiplier) * 2.5)`.

#### Unicode
* A string literal can include an arbitrary Unicode scalar as `\u{n}`, where `n` is between one and eight hexadecimal digits, such as `"\u{2665}"`.
* Every `Character` instance is a single *extended grapheme cluster*, which is a sequence of one or more Unicode scalars that (when combined) produce a single human-readable character.
* The use of extended grapheme clusters for `Character` values means that string concatenation and modification may not always affect the character count of a `String` instance.

#### Comparing Strings
* Two `String` or `Character` values are considered equivalent if their extended grapheme clusters are *canonically equivalent*, meaning they have the same linguistic meaning and appearance.

#### Unicode Representations of Strings
* Methods `utf8` and `utf16` return code units for UTF-8 and UTF-16, while method `unicodeScalars` returns 21-bit `UnicodeScalar` values, which is equivalent to UTF-32 encoding.
