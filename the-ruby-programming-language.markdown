## The Ruby Programming Language

by David Flanagan and Yukihiro Matsumoto

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177)!*

### Chapter 1: Introduction
* pg 2: Every value is an object, even simple numeric literals and the values `true`, `false`, and `nil`.
* pg 3: Parentheses are usually optional and commonly omitted, and so method invocations can look like references to named fields or named variables of an object.
* pg 4: Symbols are immutable, interned strings, and so can be compared by identity rather than textual content.
* pg 4: Double quoted strings can include arbitrary Ruby expressions delimited by `#{` and `}`, and interpolated values are converted to strings by calling `to_s`.
* pg 5: Strings, arrays, and streams use the `<<` operator for an append operation.
* pg 6: Methods can be defined on individual objects by prefixing the name of the method with the object, and are called _singletonmethods_.
* pg 6: Methods that end with `=` are special because they can be invoked using assignment syntax.
* pg 7: Global variables are prefixed with `$`, instance variables with `@`, and class variables with `@@`.
* pg 7: The `..` operator has an inclusive right endpoint, while the `...` operator has an exclusive right endpoint.
* pg 7: `Regexp` and `Range` objects define `===` for membership, which is what the `case` statement also uses, and so it is called the _case equality operator_.
* pg 10: Even core classes in Ruby are open, and so any program can add methods to them.
* pg 10: Strings in Ruby are mutable, and so a string literal in a loop evaluates to a new object on each iteration.
* pg 11: When evaluating conditional expressions, the value `nil` is treated as `false`, and any other value is the same as `true`.
* pg 11: In Ruby 1.9, the MRI (Matz's Ruby Implementation) was merged with YARV (Yet Another Ruby Virtual machine) that executes to bytecode before execution.
* pg 12: Unlike `print`, `puts` ends with a newline, and its alternative `p` converts objects to strings using `inspect` which can return more programmer-friendly representations.
* pg 13: When using `ri`, you must use `::` to refer to a class method or `#` to refer to an instance method if both are defined with the same name.
* pg 14: Ruby 1.9 knows how to find installed gems on its own, and you do not need to put `require rubygems` in your programs that use gems.
* pg 15: You can use the `gem` method before calling `require` to load a specific version of a gem other than the most recent one.

### Chapter 2: The Structure and Execution of Ruby Programs
* pg 26: Ruby has no equivalent of `/* */` that allows embedding a comment in code.
* pg 27: Embedded documents defined with `=begin` and `=end` allow defining multi-line comments easily, and are usually intended for use by a source code postprocessing tool.
* pg 27: The `rdoc` tool converts comments to HTML or prepares them for display using `ri`, and works with comments using `#`, or embedded documents that start with `=begin rdoc`.
* pg 28: Identifiers that begin with a capital letter are constants, but the Ruby interpreter will issue a warning and not an error if their values are altered.
* pg 30: A number of Ruby's operators are implemented as methods so that classes can redefine them for their own purposes.
* pg 31: Ruby does not complain if you prefix keywords with `@`, `@@`, or `$` and use them as instance, class, or global variable names, but clearly don't do that.
* pg 31: Important features are implemented as methods of the `Kernel`, `Module`, `Class`, and `Object` classes, so their identifiers should be treated as reserved as well.
* pg 32: Ruby continues parsing a statement on the next line if the first line is not complete.
* pg 32: To prevent automatically terminating a statement, you can escape a line break with a backslash.
* pg 33: In Ruby 1.9, if the first nonspace character is a period, then the line is considered a continuation line, which allows creation of fluent APIs.
* pg 33: If warnings are enabled with `-w`, Ruby issues a warning whenever it finds ambiguous code from omitting parentheses in method calls.
* pg 34: In Ruby the term methods is used, not functions, procedures, or subroutines.
* pg 35: Nested blocks of code are indented two spaces relative to their delimiters by convention.
* pg 35: Formal blocks may be delimited with curly braces, or they may be delimited with the keywords `do` and `end`.
* pg 36: The `require` method prevents any given module from being loaded more than once.
* pg 38: UTF-8 encoded files can identify their encoding if the first three bytes are 0xEF 0xBB 0xBF, which is called the "Byte Order Mark," or BOM.
* pg 38: In Ruby 1.9, the language keyword `__ENCODING__` evaluates to the source encoding of the currently executing code.
* pg 38: The default external encoding is what Ruby uses when reading from files and streams; to convert that text to a single common encoding, specify the default internal encoding.
* pg 39: Query the default external and internal encodings using methods `Encoding.default_external` and `Encoding.default_internal`, respectively.

### Chapter 3: Datatypes and Objects
* pg 42: If a value fits into a `Fixnum`, it is stored as such; otherwise, it is automatically converted to a `Bignum`.
* pg 43: Underscores may be inserted into integer literals, and this feature can be used as a thousands separator.
* pg 43: Integer literals starting with `0x` or `0X` are hexadecimal, ones starting with `0b` or `0B` are binary, and ones starting with `0` and no subsequent letter are octal.
* pg 44: Floating point values require that digits appear before the decimal point.
* pg 44: Integer division by zero causes a `ZeroDivisionError` to be thrown, while floating-point division by zero simply returns the value `Infinity`.
* pg 45: Integer values can also be indexed like arrays to query, but not set, individual bits.
* pg 45: When one of the operands in integer division is negative, Ruby rounds towards negative infinity instead of toward `0` like C and Java.
* pg 45: The sign of the result of a modulo operation in Ruby is that of the second operand, instead of the first operand like C and Java.
* pg 47: In single-quoted strings, a backslash is not special if the character that follows is anything other than a quote or a backslash.
* pg 47: To break a single-quoted string across multiple lines, break it into multiple adjacent string literals while escaping the newlines between the literals.
* pg 48: Curly braces may be omitted when the expression to be interpolated in a double-quoted string literal is a reference to a global, instance, or class variable.
* pg 48: Ruby uses the `%` operator to perform `sprintf`-style string interpolation, just like Python does.
* pg 48: Double-quoted string literals may span multiple lines, and line terminators become part of the literal unless escaped by a backslash.
* pg 51: The sequence `%q` follows single-quoted rules, and the sequence `%Q` follows double-quoted rules.
* pg 51: A string literal formed with `%q` or `%Q` continues until a matching unescaped delimiter is found, and the delimiters `(`, `[`, `<` pair with `)`, `]`, and `>`, respectively.
* pg 53: Text using sequence `%x` or between backticks is treated as a double-quoted string literal and run as a shell command by `Kernel.``, which runs it as a shell command.
* pg 53: Every time Ruby encounters a string literal, it creates a new object, and so avoid using literals in loops for efficiency.
* pg 54: Single characters can be included literally by preceding them with `?`, but since characters are strings with length 1 in Ruby 1.9, this has little use.
* pg 55: If the righthand operand of a string’s `<<` operator is an integer, it is taken to be a Unicode codepoint, and the corresponding character is appended.
* pg 56: Ruby does not throw an exception if you access a character beyond the end of the string, but instead returns `nil`.
* pg 57: Reading `s[s.length]` returns `nil`, while reading `s[s.length, 1]` returns an empty string and will not work for indexes greater than `s.length`.
* pg 57: Assigning `s[s.length]` is an error, while assigning `s[s.length, 0]` appends to the end of the string.
* pg 58: Indexing a string with another string is only really useful when you want to replace the matched string with some other string.
* pg 58: The `each_char` iterator may be more efficient than using the `[]` operator, and complements the `each_byte` and `each_line` iterators.
* pg 59: In Ruby 1.9, string elements are characters which are strings of length 1, unlike 1.8 which assumes ASCII, where elements are numbers representing byte values.
* pg 60: If a string consists of only one-byte characters, random access is efficient; otherwise Ruby 1.9 must sequentially iterate to the character.
* pg 60: Encoding of string literals is the same as the source file encoding, except that literals containing `\u` escapes are always encoded in UTF-8.
* pg 60: The `ASCII-8BIT` encoding in Ruby 1.9 has the alias `BINARY` and is equivalent to the encoding in Ruby 1.8, while the `US-ASCII` encoding is true 7-bit ASCII.
* pg 61: The `force_encoding` method does not perform validation, which is done by the `valid_encoding?` method.
* pg 62: If a string you call `encode` on consists of unencoded bytes, you need to specify the encoding by which to interpret those bytes before transcoding them.
* pg 63: To obtain the encoding for the current locale, call `Encoding.locale_charmap` and pass the resulting string to the `Encoding.find` factory method.
* pg 64: Attempting to read an index with `index >= size` or `index < -size` returns `nil` as opposed to raising an exception.
* pg 64: Assigning an element to an index beyond the end of the array automatically extends the array with `nil` elements.
* pg 64: The `%w` and `%W` sequences express array literals whose elements are strings without spaces, and follow the same rules as `%q` and `%Q`.
* pg 65: Arrays indexed with a negative value cannot be used on the left side of assignment statements; to prepend, index with the two integers `0,0`.
* pg 65: When replacing a subarray by indexing with two integers, if the right side is a single element, the brackets `[` and `]` are optional.
* pg 66: Treating arrays as unordered sets, the boolean operators `|` and `&` perform union and intersection, while `-` is like set difference but may contain duplicate elements.
* pg 67: Ruby 1.9 supports a succinct hash literal syntax where the keys are symbols, and the colon moves to the end of the hash key and replaces the arrow.
* pg 68: If you define a new class that overrides the `eql?` method,  you must also override the `hash` method or else instances of the class will not work as keys.
* pg 68: As a special case, Ruby makes private copies of all strings used as hash keys; otherwise, if you mutate a key, you must call the `rehash` method after doing so.
* pg 69: A value can only be used as a range endpoint if it responds to the comparison operator `<=>`.
* pg 69: A range where the class of the endpoints defines a `succ` method is called a discrete range, and can be iterated using `each`, `step`, and `Enumerable` methods.
* pg 69: To invoke a method on a range literal, you must parenthesize the literal, or else the method invocation is actually on the range endpoint.
* pg 70: A continuous membership test compares against the range endpoints, while a discrete membership test uses `succ` to enumerate all values and is potentially more expensive.
* pg 70: In Ruby 1.9, unless the endpoints are numeric, `include?` and `member?` perform a discrete membership test, while `cover?` performs a continuous membership test.
* pg 71: The `%s` sequence defines a literal syntax for symbols the same way that `%q` and `%Q` does for strings.
* pg 71: The `intern` and `to_sym` methods convert strings to symbols, and the `to_s` and `id2name` methods convert back.
* pg 72: There is no `Boolean` class in Ruby, and `TrueClass` and `FalseClass` both have `Object` as their superclass.
* pg 73: The `new` method of the `Class` class allocates memory to hold the new object, and then initializes its state by invoking its `initialize` method.
* pg 74: By default the `Object` class implements method `hash` by returning the object’s ID, which is also returned by the `object_id` method.
* pg 75: The `instance_of?` method tests if an object is a given class, while `is_a?`, `kind_of?`, and the `===` operator test if an object is an instance or a subclass.
* pg 76: The `equal?` method tests whether two values refer to exactly the same object, and by convention, subclasses never override it.
* pg 77: The `==` operator behaves like `equal?` unless overridden, and typically checks equality while performing needed type conversion, like between a `Fixnum` and `Float`.
* pg 77: The `eql?` method behaves like `equal?` unless overridden, and is typically used as a strict version of `==` that performs no type conversion.
* pg 78: If the `<=>` comparator cannot make a meaningful comparison, it should return `nil`.
* pg 79: The `Comparable` mixin defines the operators `<`, `<=`, `==`, `>=`, and `>` in terms of `<=>`, as well as the `between?` operator.
* pg 79: The `to_s`, `to_i`, `to_f`, and `to_a` methods allow explicit conversion to `String`, `Integer`, `Float`, and `Array`, respectively.
* pg 80: The default `inspect` method simply calls `to_s`, but should be overridden to return a representation useful for Ruby developers and not end users.
* pg 81: The `Kernel` module defines the functions `Array`, `Float`, `Integer`, and `String`, which attempt to perform conversions that in some cases use both implicit and explicit conversion methods.
* pg 81: Numeric objects have a `coerce` method tries to convert an argument to its own type, or else it converts both itself and the argument to a more general compatible type.
* pg 82: By default both `clone` and `dup` return a shallow copy, but if an `initialize_copy` method is provided, they allocate a new, empty instance and call that.
* pg 83: The `clone` method copies both the frozen and tainted state of an object, whereas `dup` only copies tainted state, and `clone` also copies any singleton methods, whereas `dup` does not.
* pg 84: Once an instance of a class is frozen, it cannot be thawed, and a copy created with `clone` will also be frozen, while a copy created with `dup` will be thawed.
* pg 84: The tainted property allows tracking of user input and data derived from it, and the trusted property allows tracking of untrusted data.

### Chapter 4: Expressions and Operators
* pg 86: Everything in Ruby, including class and method definitions, can be evaluated as an expression and can return a value.
* pg 87: Variables whose names begin with an underscore or lowercase letter are local variables, defined only within the current method or block.
* pg 88: If the interpreter hasn’t seen an assignment to a variable, it treats an expression as a method invocation, and raises a `NameError` if no such method exists.
* pg 88: A variable comes into existence when the interpreter sees an assignment for that variable, even if the assignment is not actually executed.
* pg 89: Global functions and constants are defined and looked up within the `Object` class, and so omitting the lefthand side of `::` is actually the same as using `Object::`.
* pg 89: Unlike variables that come into existence when the interpreter sees them, constants do not exist until a value is assigned to them, and so no constants are uninitialized.
* pg 90: A method name can be separated from the object it is invoked on with `::` instead of `.`, but this is rare because it looks more like a constant reference expression.
* pg 90: Methods in `Kernel` are global functions, and because global functions are methods of `Object`, they can be invoked in any context regardless of the value of `self`.
* pg 91: Ruby objects expose only methods to the outside world, and it is intentional that attribute accessor methods without arguments look like variable references.
* pg 92: By itself, `super` passes the arguments of the current method to the method with the same name in the superclass.
* pg 94: Assignment to constants is not allowed within the body of a method, since Ruby assumes that methods are intended to be executed more than once.
* pg 95: When an object has both a getter method and a setter method, it is called an attribute.
* pg 97: When using the `||=` idiom for assigning a default value, if the left operand is not `nil` or `false`, then no assignment is actually performed nor setter method is invoked.
* pg 97: Given a single lvalue and multiple rvalues, an array creating all the rvalues is assigned to the lvalue.
* pg 98: Following a lvalue with a comma performs parallel assignment and discards superfluous rvalues, which is useful for unpacking an array.
* pg 99: In Ruby 1.9 a list of rvalues can have any number of splats, but you cannot perform a "double splat" on a nested array.
* pg 99: Any ravlue that defines a `to_a` method can be prefixed with a splat, otherwise no expansion is performed and the splat evaluates to the object itself.
* pg 99: In Ruby 1.9 a list of lvalues may include one splat operator at any position in the list.
* pg 100: If a parallel assignment is prefixed with the name of a method, Ruby will interpret the commas as method argument separators rather than lvalue and rvalue separators.
* pg 102: In general, classes may define their own arithmetic, ordering, and equality operators, but they may not redefine the various boolean operators.
* pg 103: Unary plus and minus have the method names `+@` and `-@` to disambiguate themselves from the binary plus and minus methods.
* pg 103: The exponentiation operator handles fractional and negative exponents.
* pg 104: When an array is multiplied by a string, the result is the same as calling its `join` method and passing that string as an argument.
* pg 104: The `String`, `Array`, and `IO` classes define `<<` as the append operator, as do other "appendable" classes like `Queue` and `Logger`.
* pg 105: The boolean operators `&&` and `||` are more efficient than `&`, `|`, and `^` because they do not evaluate their righthand operand unless otherwise needed.
* pg 105: The `Module` class, which is the superclass of `Class`, defines `A < B` as `true` if class `A` is a subclass or descendant of class `B`.
* pg 106: The `Module` comparison operators return `nil` between classes where neither one is a subclass of the other.
* pg 106: `String` defines the pattern matching operator `~=` so that it expects a `RegExp` as its argument, and `RegExp` defines it so that it expects a `String`.
* pg 106: There is no `!==` operator; if you want to negate `===`, you must do it yourself.
* pg 107: An idiomatic use of `&&` is to only access an attribute of an object if that object is not `nil`.
* pg 108: One idiomatic use of `||` is to return the first non-`nil` value in a series of alternatives.
* pg 108: The `and`, `or`, and `not` operators are low precedence versions of `&&`, `||`, and `!`, and are even lower than the assignment operator.
* pg 109: The `and` and `or` operators have the same precedence, with `not` slightly higher, which is unlike `&&` having a higher precedence than `||`.
* pg 110: A flip-flop expression is false until the lefthand expression evaluates to `true`, and remains `true` until the righthand expression evaluates to `true`.
* pg 110: When a `..` flip-flop flips to `true`, it immediately evaluates its righthand expression to see if it should flop back to `false`, while a `...` flip-flop waits until the next evaluation.
* pg 112: Since method names can end in a question mark, you must put parentheses around the first operand of the ternary operator or add a disambiguating space.
* pg 113: Although assignment operators cannot be redefined as methods, compound assignment operators like `+=` use redefinable operators like `+`.
* pg 113: The `defined?` method returns `nil` given an undefined variable or method, or if the operand is an expression that uses `yield` or `super` in an inappropriate context.
* pg 115: It is better to think of method invocation as a special kind of expression than to think of `()` as a method-invocation operator.

### Chapter 5: Statements and Control Structures
* pg 118: The expression of a conditional must be separated from the body by a newline or semicolon or the keyword `then`.
* pg 121: Using `if` as a statement modifier does not allow any kind of `else` clause.
* pg 124: Comma separated expressions in the `when` clause act like the `||` operator, as if any expression evaluates to `true`, its body is executed.
* pg 126: If a `when` clause contains multiple expressions separated by commas, the `===` operator is invoked on each one.
* pg 126: Expressions in `when` clauses do not have to be compile-time constants, so no lookup table is used, and performance is comparable to an `if` statement and `elsif` clauses.
* pg 128: When using `while` or `until` as a modifier, if the loop body is between the `begin` and `end` keywords then the body runs at least once like a `do`/`while` loop, but this usage is discouraged.
* pg 129: The `for`/`in` loop calls the `each` method of the specified object.
* pg 129: A loop variable in a `for` loop remains defined after the loop exits, as does new variables defined within the body of the loop.
* pg 130: Iterators are methods that interact with a block of code that follows them by use of the `yield` statement, and do not need to serve an iteration or looping function.
* pg 132: In general, `n.times` is equivalent to `0.upto(n-1)`.
* pg 132: The `Enumerable` module implements the method `each_with_index`, and the rhyming `collect` (also called `map`), `select`, `reject`, and `inject` methods on top of the `each` method.
* pg 133: If no initial accumulator value is provided to `inject`, it is assigned the first element in the enumerable object, and the block is called first with the second element.
* pg 134: You can pass a hash literal without curly braces around it to `yield`, but you cannot pass a block, i.e. you cannot pass a block to a block.
* pg 135: The `block_given?` method and its synonym `iterator?` determines whether there is a block associated with the invocation.
* pg 135: The `to_enum` and `enum_for` methods return enumerators that can serve as immutable proxy objects, instead of requiring defensive copies.
* pg 136: The built-in iterator methods of Ruby 1.9 automatically return an enumerator when invoked with no block, bypassing the need to use `to_enum` or `enum_for`.
* pg 137: In Ruby 1.9, enumerators are also external iterators, where the client can control iteration using the `next` method until `StopIteration` is raised.
* pg 138: The `Kernel.loop` method in Ruby 1.9 includes an implicit `rescue` clause and exits cleanly when `StopIteration` is raised.
* pg 138: In general, if new invocations of each on the underlying `Enumerable` object do not restart iteration from the beginning, then calling `rewind` will not restart it either.
* pg 141: Omitting parentheses around method arguments and using curly brace delimiters will associate a block with the last method argument instead of the method itself.
* pg 142: A `return` inside a block causes the containing method to return; use `next` instead, or rely on the block returning the value of the last expression evaluated.
* pg 142: While variables created in a block are undefined outside the block, assigning to a variable already defined outside the block does not create a new block-local variable.
* pg 143: Follow the list of block parameters with a semicolon and a list of block local variables to prevent inadvertently clobbering the value of some existing variable.
* pg 145: In Ruby 1.9, prefix a block parameter with `*` to assign it an array with multiple yielded values, or else extra values are silently discarded as if `,` were appended.
* pg 145: In Ruby 1.9, the final block parameter may be prefixed with `&` to indicate that it is to receive any block associated with the invocation of the block.
* pg 147: The `return` statement always causes the lexically enclosing method, or the method that the block appears inside of when viewing the source code, to return.
* pg 148: The `break` statement transfers control out of the block, out of the iterator that invoked it, and to the first expression following the invocation of the iterator.
* pg 148: A `break` statement can be used with a single expression or multiple expressions; this value or array of values becomes the value of the loop or the iterator return value.
* pg 151: The `redo` statement transfers control to the first expression of the body, and does not retest the loop condition or fetch the next element from an iterator.
* pg 151: The `redo` statement is uncommon, but is useful to recover from errors when prompting a user for input.
* pg 153: Unlike a labeled break, `throw` and `catch` are not restricted to loops, and can propagate up the call stack to cause a block in an invoking thread to exit.
* pg 154: If `throw` is called, by default the return value of the corresponding `catch` is `nil`, unless you pass a second argument to `throw`.
* pg 155: The `StandardError` exception type is for recoverable errors; all other subclasses of `Exception` represent more serious ones that programs do not attempt to recover.
* pg 157: `raise` with no arguments creates a `RuntimeError`, while `raise` with a single string argument creates a `RuntimeError` with that as its message.
* pg 157: If the first argument to `raise` is an object with an `exception` method, it is called and its return value is raised; `Exception` defines such a method.
* pg 158: The global variable `$!` refers to the `Exception` object that is being handled, but a better alternative is to specify a name in the clause itself.
* pg 160: To define a catch-all `rescue` clause that handles any exception not handled by previous clauses, use `rescue Exception => ex` as the last clause.
* pg 161: When the `retry` statement is used in a `rescue` clause, it reruns the block of code to which the `rescue` is attached.
* pg 162: Putting code in an `else` clause differs from appending it to the end of the `begin` clause in that any exceptions raised in it are not handled by the `rescue` statements.
* pg 163: An `ensure` clause can cancel exception propagation by raising a new exception, or executing a control statement like `return`, `break`, or `next`.
* pg 163: If the body of a `begin` statement includes a `return` statement, then if the `ensure` clause has a `return` statement of its own, it changes the return value of the method.
* pg 164: The `rescue`, `else`, and `ensure` keywords can be used as clauses of a `def` statement, a `class` statement, or a `module` statement.
* pg 165: `rescue` can be used as a statement modifier, but must be used alone, with no exception class names and no variable names.
* pg 166: The `at_exit` method of `Kernel` allows registering blocks of code to execute just before the interpreter exits, where the block registered first is executed last.
* pg 167: The `value` method of a `Thread` object returns the value returned by the associated block, and the caller will block until it becomes available.
* pg 168: While arguments to the first call of `resume` become block parameters, afterward arguments to `resume` become the return value of `Fiber.yield`, and vice versa.
* pg 171: Avoid using the additional features of fibers found in the `fiber` module, as they are so powerful that misusing them can crash the VM.
* pg 172: Continuations were part of the core platform in 1.8 but have been replaced by fibers in 1.9, and should now be considered a curiosity.

### Chapter 6: Methods, Procs, Lambdas, and Closures
* pg 176: Blocks and methods are not objects that Ruby can manipulate, but they can be represented by instances of `Proc` and `Method`, respectively.
* pg 178: To return more than one value, separate the values by commas and explicitly use `return`, or create an array of values explicitly without using `return`.
* pg 179: Singleton methods are prohibited on `Symbol` and `Numeric` objects, because Ruby treats `Fixnum` and `Symbol` values as immediate values and not object references.
* pg 180: The `undef` statement cannot be used to undefine a singleton method in the way that `def` can be used to define such a method.
* pg 180: Methods that end with `?` are called predicates, and are not required to return `true` or `false`.
* pg 181: Methods that end with `!` should be used with caution, and are not required to be mutators.
* pg 181: Operator methods `[]` and `[]=` are special because they can be invoked with any number of arguments, and for `[]=`, the last argument is the value assigned.
* pg 182: The keyword `alias` lets a Ruby method have two different names, but method overloading is not permitted.
* pg 183: Omitting the parentheses in method invocations gives the illusion of property access.
* pg 184: When you do use parentheses in a method invocation, the opening parenthesis must immediately follow the method name with no intervening space.
* pg 184: To reduce confusion, always use parentheses around a method invocation if any of the arguments use parentheses for expression grouping.
* pg 186: Argument defaults don’t need to be constants, and can refer to instance variables and to previous parameters in the parameter list.
* pg 186: Ruby 1.9 allows ordinary parameters to appear after parameters with defaults, but all parameters with defaults must be adjacent in the parameter list.
* pg 187: In Ruby 1.9, a parameter with `*` must appear after any parameters with defaults specified, but it may be followed by additional ordinary parameters.
* pg 187: In Ruby 1.9, enumerators are splattable objects and can be used with `*`.
* pg 189: To better support hashes for named arguments, Ruby allows you to omit the curly braces around a hash literal that is the last argument; this is called a bare hash.
* pg 189: If you omit the parentheses in a method call, then you must omit the curly braces of the hash, or else Ruby thinks you’re passing a block to the method.
* pg 190: Block arguments prefixed with `&` must be the last objects in a method definition.
* pg 191: Even if a block has been converted to a `Proc` object and passed as an argument, it can still be invoked as an anonymous block using `yield`.
* pg 191: If you find yourself using two identical blocks, create a `Proc` to represent the block, and use the single `Proc` object twice.
* pg 192: You can use `&` before any object with a `to_proc` method, such as `Method` or `Symbol`.
* pg 192: The `to_proc` method of `Symbol` returns a `Proc` object that invokes the named method of its first argument, and passes the remaining arguments as parameters.
* pg 192: While procs have block-like behavior and lambdas have method-like behavior, both are instances of the class `Proc`.
* pg 193: All `Proc` objects have a `call` method that, when invoked, runs the contained by the block from which the proc was created.
* pg 193: If `Proc.new` is invoked without a block within a method that does have an associated block, then it returns a proc representing that block.
* pg 195: A lambda literal argument list requires parentheses if it includes a semicolon and block-local variable names, and there must be no preceding space.
* pg 195: Lambda literals create `Proc` objects, so you must precede the definition with `&` to pass one to where a block is expected, but regular block syntax is simpler.
* pg 196: Instead of invoking `call`, Ruby 1.9 allows you to invoke a `Proc` using parentheses prefixed with a period, like the method name is missing.
* pg 196: Only if a proc has a `*`-prefixed final argument, its `arity` method returns a negative number `n`, where the number of required arguments is `~n` or `-n-1`.
* pg 197: The `==` method only returns `true` if one `Proc` is a clone or duplicate of the other.
* pg 197: Calling a proc is like yielding to a block, whereas calling a lambda is like invoking a method; you can test if a `Proc` object is a lambda by calling its `lambda?` method.
* pg 198: If a proc executes a `return` statement after its lexically enclosing method has already returned, it raises a `LocalJumpError`, so omit the `return` or use a lambda instead.
* pg 199: It never makes sense to have a top-level `break` statement in a proc created by `Proc.new`, as `new` is the iterator that `break` returns from.
* pg 199: A lambda is method-like; if `break` is used without an enclosing loop or iteration, it acts just like `return`.
* pg 200: A proc uses yield semantics to assign arguments, which is similar to parallel assignment and is flexible, while a lambda uses invocation semantics, which is inflexible.
* pg 202: If a method returns more than one closure, pay attention to the variables they use, as it’s easy to introduce bugs if they are unintentionally sharing variables, or state.
* pg 203: The `Kernel.binding` method returns a `Binding` object that represents the bindings in effect at whatever point you call it.
* pg 203: Invoking a `Method` object is less efficient than invoking it directly.
* pg 204: To use a `Method` where a true `Proc` is required, call its `to_proc` method, or prefix the method object with `&` as shorthand.
* pg 204: An `UnboundMethod` cannot be invoked and does not define a `call` method; you must first convert it to a `Method` object using `bind`.

### Chapter 7: Classes and Modules
* pg 215: The `class` keyword creates a new constant to refer to the class, which is why all class names must begin with a capital letter.
* pg 216: The `initialize` method is automatically made private, so an object can call `initialize` on itself, but you cannot explicitly call `initialize` on an object.
* pg 218: Assignment expressions only invoke a setter method when used with an object, so using a setter without `self` in an instance method creates a new local variable.
* pg 218: The `attr_reader` and `attr_writer` methods create getters and setters respectively, while `attr_accessor` creates both.
* pg 219: The getter and setter methods defined with `attr_reader` or `attr_accessor` are just as fast as hardcoded ones.
* pg 221: If a binary operator of a class expects an argument of a different type, and the operator is called on that different type, the `coerce` method can reverse the operand order.
* pg 223: Duck typing should not be supported in `==` or `eql?` methods, which should return `false` instead of raising `NoMethodException` if accessors are missing.
* pg 224: A `==` method should be defined in terms of `==` operators, and an `eql?` method should be defined in terms of `eql?` methods.
* pg 224: A good general purpose hash function is to start with the value `17`, and multiply that value by `37` before a new variable’s hash is added.
* pg 225: Ideally the `==` and `<=>` operator should have consistent definitions of equality, but it is not required.
* pg 226: When defining a mutator method, add `!` to the end if there is a non-mutating version of the same method.
* pg 226: `Struct` is a class that creates other classes, with read and write accessors, a `==` operator and `to_s` methods, and other useful methods.
* pg 227: If you assign an unnamed class object to a constant, the name of that constant becomes the name of a class.
* pg 228: When defining class methods, you can use `self` instead of the class name, which is slightly less clear but applies the DRY principle.
* pg 230: If constants of a class refer to instances of the class, they must be defined after the `initialize` method of the class.
* pg 232: Class instance variables are just instance variables, so you can use `attr_reader` and `attr_writer` to define them in the context defined by the syntax `class << self`.
* pg 232: A private method `m` must be invoked in a functional style as `m`, and not as `o.m` or even `self.m`, but a protected method can be explicitly invoked on any class instance.
* pg 233: One object can call another’s protected method if it is defined in a class that both objects are instantiated from, or are subclasses of.
* pg 233: The `public`, `protected`, and `private` methods, when invoked with the names of one or more methods, alter the visibility of the named methods.
* pg 234: Use `private_class_method` to make a class method private; you can use this to make the new method private if you define factory methods.
* pg 235: You can also subclass a struct-based class in order to add methods other than the automatically generated ones.
* pg 238: Private methods cannot be invoked from outside the class that defines them, but they are inherited by subclasses, which can invoke them and override them.
* pg 238: To avoid accidentally overriding a private method of a class you didn’t write, extend its functionality by encapsulating and delegating to it.
* pg 239: Because a bare `super` has a special meaning, you must use a pair of empty parentheses to pass zero arguments from a method that has one or more arguments.
* pg 239: When invoking a class method with an explicit receiver, always invoke the class method on the class that defines it for clarity.
* pg 240: Instance variables are unrelated with inheritance, so a subclass cannot "shadow" an instance variable in a superclass, and is another reason to prefer encapsulation and delegation.
* pg 241: A subclass can alter a class variable defined and used by a superclass, but class instance variables are not inherited, and so this is a strong reason to prefer them.
* pg 241: Constants are looked up in the lexical scope of where they are used before the inheritance hierarchy, and so inherited methods will not find a redefined constant in a subclass.
* pg 242: The `Class#new` method is inherited by all class objects and is used to initialize new instances, while `Class::new` can be used to create new classes.
* pg 244: The `initialize_copy` method can rely on `dup` or `clone` to perform a deep copy.
* pg 244: A class that defines an enumerated type can use `private_class_method` on `:new` and `:allocate`, and `private` on `:dup` and `:clone` to prevent unwanted instances.
* pg 246: Note that if you have disabled the `clone` and `dup` methods for a class, you must implement custom marshalling methods, or else a copy can still be made.
* pg 246: Add `require 'singleton'` and then `include Singleton` into your class to properly implement a singleton using the techniques described before.
* pg 248: `Class` is a subclass of `Module`; both classes and modules can be used as namespaces, but classes cannot be used in mixins as modules can.
* pg 249: A class or module within another has no special access to the class or module it is nested within.
* pg 250: Nesting classes keeps the namespace tidy, but does not actually make the nested class private in any way.
* pg 250: While `include` is usually used as if it were a language keyword, it is a private instance method of `Module`, implicitly invoked on `self`.
* pg 251: You can include one module within another, and while every class is a module, the arguments to `include` must be modules, not classes.
* pg 251: To make instance methods of a specified module into singleton methods of the receiver object, use `Object.extend`.
* pg 252: When defining a module function, avoid using `self`, because the value of `self` will depend on how it is invoked.
* pg 253: Ruby 1.9 defines a `require_relative` method, which works like `require` but ignores the load path and searches relative to the directory from which the invoking code was loaded.
* pg 254: The current working directory `.` in a load path is the directory from which a user invokes a Ruby program, and not the directory in which the program is installed.
* pg 256: The `autoload` function allows lazy-loading a library when a given constant, typically a class or module name, is first encountered.
* pg 257: Singleton methods of an object are not defined by an object, but must be associated with a class; this is called the eigenclass, singleton class, or metaclass.
* pg 258: When you open the eigenclass of an object using `class <<`, `self` refers to the eigenclass object.
* pg 258: When performing a method lookup, Ruby searches the instance methods of any modules included by a class before searching the superclass.
* pg 260: Invoking a class method performs a standard method lookup on the object that represents the class, and so proceeds through `Class`, `Module`, `Object`, and `Kernel`.
* pg 261: The eigenclass of a class is the eigenclass of its superclass, and to find a method, Ruby first checks the singleton methods of eigenclasses before looking for instance methods.
* pg 262: Constants defined in enclosing modules are found before those in included modules, and modules included by a class are searched before the superclass of the class.

