## The Ruby Programming Language

by David Flanagan and Yukihiro Matsumoto

### Chapter 1: Introduction
* pg 2: Every value is an object, even simple numeric literals and the values `true`, `false`, and `nil`.
* pg 3: Parentheses are usually optional and commonly omitted, and so method invocations can look like references to named fields or named variables of an object.
* pg 4: Symbols are immutable, interned strings, and so can be compared by identity rather than textual content.
* pg 4: Double quoted strings can include arbitrary Ruby expressions delimited by `#` and `}`, and interpolated values are converted to strings by calling `to_s`.
* pg 5: Strings, arrays, and streams use the `<<` operator for an append operation.
* pg 6: Methods can be defined on individual objects by prefixing the name of the method with the object, and are called _singletonmethods_.
* pg 6: Methods that end with `=` are special because they can be invoked using assignment syntax.
* pg 7: Global variables are prefixed with `$`, instance variables with `@`, and class variables with `@@`.
* pg 7: The `..` operator has an exclusive right endpoint, while the `...` operator has an inclusive right endpoint.
* pg 7: `Regexp` and `Range` objects define `===` for membership, which is what the `case` statement also uses, and so it is called the _case equality operator_.
* pg 10: Even core classes in Ruby are open, and so any program can add methods to them.
* pg 10: Strings in Ruby are mutable, and so a string literal in a loop evaluates to a new object on each iteration.
* pg 11: When evaluating conditional expressions, the value `nil` is treated as `false`, and any other value is the same as `true`.
* pg 11: In Ruby 1.9, the MRI (Matz's Ruby Implementation) was merged with YARV (Yet Another Ruby virtual machine) that executes to bytecode before execution.
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

