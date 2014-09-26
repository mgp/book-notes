## The Swift Programming Language: Language Guide

### [The Basics](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html)

##### Constants and Variables
* You declare constants with the `let` keyword and variables with the `var` keyword.
* In practice, it is rare that you will need a type annotation. It may be useful if you are declaring a variable and no initial value is provided.
* To use string interpolation, wrap the variable name in parentheses and escape it with a backslash before the opening parenthesis.

##### Comments
* You can nest multiline comments.

##### Numeric Literals
* Both integers and floats can be padded with extra zeroes and can contain `_` characters to help with readability.

##### Numeric Type Conversion
* Prefer `Int` for even non-negative values. This means that integer constants and variables are immediately interoperable in your code and will match the inferred type for integer literal values.
* Conversion between integer and floating-point must be explicit, regardless of the direction. An exception is made for numeric literals, which do not have an explicit type in and of themselves.

##### Tuples
* You can decompose tuples. If you only need some parts of the tuple's values, ignore parts of the tuple with the `_` character when decomposing.
* If the elements in a tuple are unnamed, you can access them with index numbers starting at zero, e.g. `tupleName.0`. If they are named, you can access them by their element names, e.g. `tupleName.firstElementName`.
* Tuples are not suited for the creation of complex data structures. Use them primarily for returning multiple values from a function.

##### Optionals
* An optional lets you indicate the absence of a value for any type at all, without the need for special constants like `NSNotFound`.
* You can only assign `nil` to an optional variable. You cannot use it with nonoptional constants and variables. 
* The value `nil` is not the same in Objective-C as it is in Swift. In Swift, `nil` is not a pointer like it is in Objective-C. Instead, it is an absence of a value of a certain type.
* Once you're sure that an optional does contain a value, you can access that value by appending `!` to the end of the optional's name. This is called *forced unwrapping* of its value.
* Optional binding, written as `let constantName = someOptional` or `var constantName = someOptional`, can be used with `if` and `while` statements to check for a value inside an optional and extract it as a constant in a single action.
* If the conversion that is part of optional binding is successful, then the constant is initialized with the value inside the optional, and there is no need to use the `!` suffix.
* An *implicitly unwrapped optional* has a `!` instead of a `?` after the type that you want to make optional. Use this if an optional will always have a value after it's first set, and you want to remove the need to check and unwrap the optional's value on each access.
* If you try to access an implicitly unwrapped optional when it doesn't contain a value, you trigger a runtime error, much like if you place a `!` after a normal optional missing a value.


### [Basic Operators](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html)

##### Arithmetic Operators
* The assignment operator in Swift does not return a value, so you cannot accidentally use `=` when you mean `==`.
* The `%` operator is the remainder operator, and not a modulo operator.
* Given a negative first operand, the remainder operator can yield a negative value. But the sign of the second operand for the remainder operator is ignored, so `a % b` and `a % -b` always yield the same answer.
* The remainder operator can also accept a second operand that is a floating point number.
* It is recommended that you use the prefix version of the increment and decrement operators, unless you need the specific behavior of their postfix forms.

##### Compound Assignment Operators
* The compound assignment operators do not return a value.

##### Comparison Operators
* The identity operators `===` and `!==` test whether two object references both refer to the same object instance.

##### Nil Coalescing Operator
* The *nil coalescing operator* `(a ?? b)` unwraps an optional `a` if it contains a value, or returns a default value `b` if `a` is `nil`. If `a` is not `nil`, then `b` is not evaluated.

##### Range Operators
* The *closed range operator* `a...b` is inclusive of `b`, while the *half-open range operator* `a..<b` is exclusive of `b`.


### [Strings and Characters](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html)

##### Strings Are Value Types
* The `String` type is a value type, and such a value is copied when it is passed to a function or method, or when it is assigned to a constant or value.

##### Working with Characters
* You can create a stand-alone character constant or variable from a single-character string literal by providing a `Character` type annotation.

##### String Interpolation
* A string literal can interpolate not just constants, variables, and literals, but expressions such as `\(Double(multiplier) * 2.5)`.

##### Unicode
* A string literal can include an arbitrary Unicode scalar as `\u{n}`, where `n` is between one and eight hexadecimal digits, such as `"\u{2665}"`.
* Every `Character` instance is a single *extended grapheme cluster*, which is a sequence of one or more Unicode scalars that (when combined) produce a single human-readable character.
* The use of extended grapheme clusters for `Character` values means that string concatenation and modification may not always affect the character count of a `String` instance.

##### Comparing Strings
* Two `String` or `Character` values are considered equivalent if their extended grapheme clusters are *canonically equivalent*, meaning they have the same linguistic meaning and appearance.

##### Unicode Representations of Strings
* Methods `utf8` and `utf16` return code units for UTF-8 and UTF-16, while method `unicodeScalars` returns 21-bit `UnicodeScalar` values, which is equivalent to UTF-32 encoding.


### [Collection Types](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html)

##### Mutability of Collections
* Arrays and dictionaries assigned to variables are mutable, while arrays and dictionaries assigned to constants are immutable.

##### Arrays
* The type of values stored by an array is specified by an explicit type annotation or type inference, and does not have to be a class type.
* To specify the type of an array, prefer the shorthand form `[ElementType]` over the full form `Array<ElementType>`.
* Thanks to type inference, you don't have to write the type of an array if you're initializing it with a literal containing values of the same type.
* You can also use subscript syntax to change a range of values at once, even if the replacement set of values has a different length than the range you are replacing.
* The `removeAtIndex` and `removeLast` methods return the removed item.
* Use the global `enumerate` function to iterate over tuples containing each item and its corresponding index.
* To initialize an empty array, use the syntax `[ElementType]()`. If the context provides type information about an array variable, you can assign it simply `[]`.

##### Dictionaries
* To specify the type of a dictionary, prefer the shorthand form `[KeyType: ValueType]` over the full form `Dictionary<KeyType, ValueType>`.
* Thanks to type inference, you don't have to write the type of a dictionary if you're initializing it with a literal containing keys of the same type, and values of the same type.
* Unlike using subscript syntax, method `updateValue(forKey:)` returns the old value after performing an update. It returns an *optional* value of the dictionary's value type.
* You can also use subscript syntax to remove a key-value pair from a dictionary by assigning a value of `nil` for that key.
* If you need to use a dictionary's keys or values with an API that takes an `Array` instance, initialize a new array with the `keys` or `values` property.
* To initialize an empty dictionary, use the syntax `[KeyType: ValueType]()`. If the context provides type information about a dictionary variable, you can assign it simply `[:]`.
* Any type conforming to protocol `Hashable` can be used as a dictionary key. All of Swift's basic types are hashable by default, excluding enumeration member values with associated values.


### [Control Flow](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html)

##### For Loops
* If you don't need access to the current value during each iteration of a `for`-`in` loop, use the `_` character in place of a loop variable.
* Each item in the dictionary is returned as a `(key, value)` tuple when the dictionary is iterated over using `for`-`in`, and you can decompose that tuple's members as explicitly named constants.

##### Conditional Statements
* Every `switch` statement must be exhaustive, meaning every possible value must be matched by a case. If this is not appropriate, you can use `default` to specify a catch-all case.
* Unlike `switch` statements in C and Objective-C, `switch` statements in Swift do not fall through the bottom of each case and into the next one by default.
* Multiple matches for a single `switch` case can be separated by commas, and can be written over multiple lines if the list is long.
* Use tuples to test multiple values in the same `switch` statement, where each element can be tested against a different value or range of values. Or use the `_` identifier to match any possible value.
* A `switch` case can employ *value binding* to bind the value or values it matches to temporary consonants or variables for use in the body of the case.
* A `switch` case can use a `where` clause to check for additional conditions.

##### Control Transfer Statements
* A `switch` case that is empty or contains only a comment is a compile-time error. Use a `break` statement, which ends execution of a case immediately, to ignore such a case.
* A `switch` statement does not fall through the bottom of one case and into the next one, unless that case ends with the `fallthrough` keyword.
* The `fallthrough` keyword does not check the case conditions for the next `switch` case that it causes execution to fall into.
* You can mark a loop or `switch` statement with a *statement label*, and use this label with the `break` or `continue` statement to end or continue the execution of the labeled statement.


### [Functions](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html)

##### Function Parameters and Return Values
* Functions without a defined return type return a special value of type `Void`, which is simply an empty tuple that can be written as `()`.
* If the function returns a tuple, and its return type specifies names for the tuple values, then the tuple's members do not need to be named where the tuple is returned from the function.

##### Function Parameter Names
* Precede a local parameter name with an *external parameter name* if you want users of your function to provide parameter names when they call your function.
* If you provide an external parameter name for a parameter, that external name must always be used when you call the function.
* Use external parameter names whenever the purpose of a function's arguments would be unclear to someone reading your code for the first time.
* Prefix a local parameter name with `#` to generate an external parameter with the same name.
* Place parameters with default values at the end of a function's parameter list. This ensures that all calls to the function use the same order for their non-default arguments.
* Swift provides an automatic external variable name for any parameter that has a default value, ensuring that the argument for that parameter is clear in purpose if a value is provided.
* A function may have at most one variadic parameter, and it must always appear last in the parameter list, to avoid ambiguity when calling the function with multiple parameters.
* Variable parameters give you a new modifiable copy of the parameter's value for your function to use. They exist only for the lifetime of that function call.
* You can only pass a variable as the argument for a parameter with the `inout` keyword, and you must prefix the variable name with `&`.

##### Function Types
* The *function type* is made up of the parameter types and the return type of a function.
* To use a function type as the return type of another function, write a complete function type immediately after the return arrow of the returning function.

##### Nested Functions
* An enclosing function can also return one of its nested functions to allow the nested function to be used in another scope.


### [Closures](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html)
* Global functions are closures that have a name and do not capture any values, while nested functions are closures that have a name and can capture values from their enclosing function.

##### Closure Expressions
* *Closure expressions* provide several syntax optimizations for writing closures in a shortened form without loss of clarity or intent.
* The `in` keyword indicates that the definition of the closure's parameters and return type has finished, and that the body of the closure is about to begin.
* If Swift can infer the parameter types and return types of a closure, such as when it's passed to a method, then all the types can be omitted, as well as the enclosing parentheses and the return arrow.
* It is always possible to infer the parameter types and return type when passing a closure to a function as an inline closure expression.
* Single-expression closures can implicitly return the result of their single expression by omitting the `return` keyword from their declaration.
* Swift automatically provides shorthand argument names `$0`, `$1`, etc. These let you omit the closure's argument list from its definition, and the number and type of the shorthand argument names will be inferred from the expected function type.

##### Trailing Closures
* If a closure expression is provided as the function's only argument and you provide that expression as a trailing closure, you do not need to write `()` after the function's name when you call the function.
* Trailing closures are most useful when the closure is sufficiently long that it is not possible to write it inline on a single line.

##### Capturing Values
* In closures, Swift determines what should be captured by reference and what should be copied by value. It also handles all memory management for disposing captured values when no longer needed.
* If you assign a closure to a property of a class instance, and the closure captures that instance by referring to the instance or its members, you will create a strong reference cycle between the closure and the instance.

##### Closures Are Reference Types
* Functions and closures are reference types, and so by assigning a closure to two different constants or variables, both of those constants or variables will refer to the same closure.


### [Enumerations](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html)

##### Enumeration Syntax
* Unlike C and Objective-C, enumeration members do not assume integer values. Instead, they are fully-fledged values in their own right with an explicitly-defined type.
* To define multiple member values on a single line, prefix them with `case` and separate them by commas.
* Once the enumeration type of a variable is known, you can drop the enumeration type name when assigning its value, such as `directionToHead = .East`.

##### Matching Enumeration Values with a Switch Statement
* A switch statement must be exhaustive when considering the members of an enumeration. Use a `default` case to cover any members that are not addressed explicitly.

##### Associated Values
* Enumerations can serve as *discriminated unions* or *tagged unions*, where they can store associated values of any given type, and the value types can be different for each enumeration member.
* Each enumeration member defines only the *type* of associated values, but not actual values themselves.
* Extract each associated value as a constant (using `let`) or as a variable (using `var`) for use within the `case` body of a `switch` statement.
* If all of the associated values for a enumeration member are extracted as constants, or if all are extracted as variables, you can place a single `var` or `let` annotation before the member name.

##### Raw Values
* Unlike associated values, enumeration members with *raw values* are all of the same type (constrained to strings, characters, integer, or floating-point number types), come prepopulated, and the values must be unique.
* When integers are used for raw values, they auto-increment if no value is specified for some of the enumeration members.
* Use `toRaw` and `fromRaw` to convert between enumeration members and raw values. The latter returns an *optional* enumeration member, for when no member with the given raw value is found.


### [Classes and Structures](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html)

##### Comparing Classes and Structures
* Unlike structures, classes can use inheritance, participate in type casting, have deinitializers, and participate in reference counting.
* Unlike Objective-C, Swift allows you to set sub-properties of a structure property directly.
* All structures have an automatically-generated *memberwise initializer*, which allows initializing the properties of an instance by name.

##### Structures and Enumerations Are Value Types
* A *value type* is a type whose value is copied when it is assigned to a variable or constant, or when it is passed to a function.
* All the basic types in Swift are value types, and are implemented as structures.

##### Classes Are Reference Types
* Classes are *reference types*, which are not copied when they are assigned to a variable or constant, or when they are passed to a function.

##### Choosing Between Classes and Structures
* Consider value types for simple collections of data, any properties are themselves value types, and the structure does not need to inherit properties or behavior from another type.

##### Assignment and Copy Behavior for Strings, Arrays, and Dictionaries
* Unlike `NSString`, `NSArray`, and `NSDictionary` in Foundation, the `String`, `Array`, and `Dictionary` types in Swift are implemented as structures.
* Swift only performs an actual copy of such types when it is absolutely necessary, and you should not avoid assignment to try and preempt this optimization.


### [Properties](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html)

##### Stored Properties
* You can set and then modify the initial value for a stored property during initialization, even if it is a constant.
* When an instance of a value type is declared as constant using `let`, then all of its properties are constant, and you cannot modify even those properties that are declared using `var`.
* A *lazy stored property* is prefaced by the `lazy` keyword, and its initial value is not calculated until the first time that it is used.
* Swift unifies the concepts of properties and instance variables from Objective-C into a single property declaration. A property does not have a corresponding instance variable, and the backing store for a property is not accessed directly.

##### Computed Properties
* Classes, structures, and enumerations can define *computed properties*, which don't actually store a value, but instead provide a getter and an optional setter.
* If a computed property's setter does not define a name for the new value to be set, a default name of `newValue` is used.
* You must declare a *read-only computed property* with the `var` keyword, because its value is not fixed. The `let` keyword is only used for constant properties, to indicate that their values cannot change.
* You can simplify the declaration of a read-only computed property by removing the `get` keyword and its braces.

##### Property Observers
* The `willSet` property observer is passed the new property value as a constant parameter. If you omit the parameter name, it will assume the default parameter name of `newValue`.
* The `didSet` property observer is passed the old property value as a constant parameter. If you omit the parameter name, it will assume the default parameter name of `oldValue`.
* If you assign a value to a property within its own `didSet` observer, the new value that you assign will replace the one that was just set.

##### Global and Local Variables
* Global constants and variables are always computed lazily, similar to lazy stored properties. But local constants and variables are never computed lazily.

##### Type Properties
* A *type property* belongs to the type itself, and not any one instance. A constant type property is similar to a static constant in C, while a variable type property is similar to a static variable in C.
* Unlike stored instance properties, stored type properties must always have a default value. This is because the type itself doesn't have an initializer that can assign a value to such a property at initialization time.
* You define type properties for value types with the `static` keyword, and type properties for class types with the `class` keyword. 


### [Methods](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Methods.html)
Methods are functions that are associated with a particular type.

##### Instance Methods
* As in Objective-C, the name of a method in Swift typically refers to the method’s first parameter using a preposition such as `with`, `for`, or `by`.
* Unlike functions, the first parameter name in a method has a local parameter name by default, and the second and subsequent parameter names have both local and external parameter names by default.
* You can provide an external parameter name for the first parameter either explicitly or by prefixing the `#` symbol, and suppress the external names of subsequent parameters by using the `_` symbol.
* An instance method has implicit access to all other instance methods and properties of that type, but you can use `self` to refer to a property of the instance instead of an equivalent parameter name.
* To modify the values of a value type, such as a structure or enumeration, its method must be declared as `mutating`. Any changes made are written back when the method ends.
* Mutating methods can assign an entirely new instance to the implicit `self` property. For example, a mutating method of an enumeration can set `self` to be a different member of the same enumeration.

##### Type Methods
* Unlike Objective-C, in Swift you can define type-level methods for all classes, structures, and enumerations.

### [Subscripts](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Subscripts.html)

##### Subscript Syntax
* Subscript definitions using the `subscript` keyword can be read-write or read-only. This behavior is communicated by a getter and a setter method in the same way as for computed properties.
* Like with computed properties, you can drop the `get` keyword for read-only subscripts, and the setter can use a default parameter named `newValue` if no parameter is specified.

##### Subscript Usage
* Subscripts can return an optional type to model the fact that a value will not exist for every parameter value, such as an index or key.

##### Subscript Options
* Subscripts can take any number of parameters of any type, and return any type. They can use variable parameters and variadic parameters, but cannot use `inout` parameters or provide default parameter values.
* *Subscript overloading* allows you to define multiple subscript implementations, and a caller will invoke the appropriate one based on the types of the value or values between the braces.

### [Inheritance](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html)

##### Defining a Base Class
* Swift classes do not inherit from a universal base class.

##### Overriding
* To override a characteristic that would otherwise be inherited, you prefix your overriding definition with the `override` keyword. This not only clarifies, but guards against errors.
* An overridden subscript for `someIndex` can access the superclass version of the same subscript as `super[someIndex]` from within the overriding subscript implementation.
* You can provide a custom getter or setter to override any inherited property, regardless of whether the inherited property is implemented as a stored or computed property at source.
* You can present an inherited read-only property as a read-write property by providing both a getter and a setter in your subclass property override, but you cannot present an inherited read-write property as a read-only property.
* If you provide a setter as part of a property override, you must also provide a getter for that override. Simply return `super.someProperty` if you don't want to modify its value.
* You cannot add property observers defined by `willSet` or `didSet` to inherited constant stored properties or inherited read-only computed properties, because their values cannot be set.
* You cannot provide both an overriding setter and an overriding property observer for the same property. Instead, simply observe any value changes from within the custom setter.

##### Preventing Overrides
* Put the modifier `final` before the introducer keyword of a method, property, or subscript to prevent it from being overridden.
* Put the modifier `final` before the `class` keyword in a class definition to prevent subclassing.
