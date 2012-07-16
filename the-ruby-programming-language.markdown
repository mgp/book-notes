## The Ruby Programming Language

by David Flanagan and Yukihiro Matsumoto

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

