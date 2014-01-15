## Learning Python, 4th Edition

by Mark Lutz

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/Learning-Python-Edition-Mark-Lutz/dp/1449355730)!*

### Chapter 3: How You Run Programs
* pg 98: A Python file without the `.py` extension and with the executable bit set can be run as a shell script if the hash bang is present in the first line.
* pg 108: Reloads aren't transitive, so reloading one module won't reload any modules it imports, so avoid the temptation to launch by imports and reloads.

### Chapter 4: Introducing Python Object Types
* pg 131: Objects can be printed with full precision, in as-code `repr` form, and in a human-readable form using `str`.
* pg 135: Generic operations that span multiple types show up as built-in functions or expressions, but type-specific operations are method calls.
* pg 137: Raw string literals, which start with the letter `r`, disable the backslash escape mechanism.
* pg 146: An object follows the iteration protocol if it responds to the `iter` built-in with an object that advances in response to `next`.
* pg 150: Files provide an iterator that automatically reads line by line in `for` loops and other contexts.

### Chapter 5: Numeric Types
* pg 159: Integers are automatically converted to long integers when their values overflow 32 bits, you never need to type the letter `L` yourself.
* pg 169: Python allows chaining of comparison statements, so `x == y < z` is equivalent to `(x == y) and (y < x)`.
* pg 170: While `//` performs floor division given one or more float operands, the result is still a float.
* pg 175: It's best practice and less error prone to prefix octal values with `0o` instead of `0`.
* pg 186: The greater-than and less-than operators return whether a set is a superset or a subset of another.
* pg 187: Methods of set like union and intersection accept not only sets, but any iterable type.
* pg 189: Sets can only contain immutable, or hashable, object types, and so you must use tuples instead of lists and dictionaries as values.

### Chapter 6: Dynamic Typing Interlude
* pg 197: The header field of every variable has a type designator and a reference counter.
* pg 201:  Variables are always pointers to objects, not labels of changeable memory areas: setting a variable to a new value causes the variable to reference an entirely different object.
* pg 203: Copying a list is most commonly done by slicing without specifying the start or end index; to copy a set or dictionary, use its `copy` method.

### Chapter 7: Strings
* pg 211: In strings, `\xhh` stores a character with with hex value `hh`, `\xooo` stores a character with octal value `ooo`, and `\0` stores the null character without ending the string.
* pg 214: A raw string cannot end in a single backslash; because the backslash escapes the following quote character, you still must escape the quote character to embed it in the string.
* pg 215: To temporarily comment out a block of code, surround it in triple quotes.
* pg 227: The `replace` method takes an optional parameter that limits the number of replacements made.
* pg 228: The mutable `bytearray` object allows editing a string without exploding it as a list and, after performing edits, concatenating the characters back together using `join`.
* pg 232: As every type of object can be converted to a string (the one used when printing), every object type works with the `%s` conversion code.
* pg 234: Replacing a width or precision value with `*` in a format string uses the next value in the inputs to the right of the `%` operator, allowing them to be determined at runtime.
* pg 235: When using a dictionary literal to the right of the `%` operator, key names enclosed with parentheses in the format string will be replaced with their corresponding values.
* pg 246: The three major type categories in Python are numbers, sequences, and mappings; operations for one kind in a category work the same on any other kind in the category.

### Chapter 8: Lists and Dictionaries
* pg 255: Slice assignment is powerful, but in practice most programmers use concatenation, and the `insert`, `pop`, and `remove` methods to replace, expand, and shrink lists.
* pg 257: In sorts, the `key` argument gives a one-argument function that returns the value to be used in sorting, but this does not modify any values to be sorted.
* pg 265: Keys in dictionaries don't need to be strings, but must be immutable objects.
* pg 269: The dictionary constructor can take a sequence of binary tuples representing key-value pairs, which is useful if you build up such pairs at runtime using `zip`.
* pg 273: Both the key and value views of a dictionary reflect changes dynamically, but only the keys view are set-like and support common set operations.

### Chapter 9: Tuples, Files, and Everything Else
* pg 278: To create a one-item tuple, follow the element with a comma, and surround them together with parentheses.
* pg 283: To open a file for both reading and writing, add `+` to the mode string.
* pg 284: The `readline` method of a file returns the empty string when the end of the file is reached; empty strings in the file come back containing just a newline character.
* pg 296: To recursively traverse an object and copy all its parts, use the `deepcopy` method of the `copy` module.
* pg 297: Nonnumeric mixed-type comparisons are errors in Python 3.0; in Python 2.6, but use a fixed but arbitrary ordering rule.
* pg 299: Numbers are treated as true if nonzero, while other objects are true if non-empty.
* pg 302: The type objects representing the of the built-in object types are available through built-in names like `dict`, `set`, `list`, `str`, `tuple`, `int`, `float`, and so forth.

### Chapter 10: Introducing Python Statements
* pg 321: To span a statement across multiple lines, enclose part of it in a bracketed pair such as parentheses, square brackets, or curly braces.
* pg 322: Don't end lines in a backslash so that they can span multiple lines; they are difficult to notice, and the solution is brittle as no spaces are allowed afterward.
* pg 327: An `else` block that follows an `except` block will execute if no exception is raised in the `try` part.

### Chapter 11: Assignments, Expressions, and Prints
* pg 332: Beyond tuple and list assignments, any sequence of names can be assigned to any sequence of values where positions are matched.
* pg 335: When assigning nested sequences Python unpacks their parts according to their shape.
* pg 342: Augmented assignments run faster, because the left operands is evaluated only once, and automatically perform in-place changes if available instead of making slower copies.
* pg 345: Because module names in `import` statements become variables in your scripts, variable name constraints extend to your module filenames too.
* pg 346: Names with a single underscore are not imported by a `from module import *` statement.
* pg 348: Methods are attributes of a class and referenced using dot notation, while functions are not.
* pg 353: In Python 2.6, ending a `print` statement with a comma suppresses the newline character.
* pg 356: To redirect output from `print` without reassigning `sys.stdout`, begin a `print` statement with `>>`, followed by an object that has a `write` method.

### Chapter 12: `if` Tests and Syntax Rules
* pg 369: Python 2.6 has a `-t` command-line flag that warns if tabs and spaces are mixed, and a `-tt` flag that issues errors on such code.
* pg 373: The `and`, `or` operators don't return `True` and `False`, but the last operand evaluated for which the result of the expression is known.

### Chapter 13: `while` and `for` Loops
* pg 384: The loop `else` clause is also run if the body of the loop is never executed, as you don't run a `break` in that event either.
* pg 387: When a `for` loop exists, the assignment target from the `for` loop header is still in scope and refers to the last item visited.
* pg 397: To skip items in a sequence, apply a slice expression to it in the loop header; only manually apply indexes from `xrange` if the copy from a slice is expensive.

### Chapter 14: Iterations and Comprehensions, Part 1
* pg 406: The `iter` built-in calls `__iter__()` on an object to acquire an iterator; the next built-in calls `__next__()` on an iterable object to get the next element.
* pg 411: List comprehensions run faster than manual `for` loop statements because their iterations are performed at C language speed inside the interpreter.
* pg 416: Everything in Python's built-in toolset that scans an object from left to right is defined to use the iteration protocol, such as the `list` and `tuple` functions, or even sequence assignments.
* pg 422: An object that is its own iterator usually returns itself from a call to `iter`; objects that are not their own iterator may return different iterators that remember their positions independently.

### Chapter 15: The Documentation Interlude
* pg 429: Comments coded as strings in module files, functions, and class statements before any executable code are called docstrings and available through the `__doc__` attribute of each.
* pg 432: The built-in `help` function invokes PyDoc to extract docstrings and format them for display on your screen.

### Chapter 16: Function Basics
* pg 451: `def` lines are not evaluated until they are reached and run, and the code inside each `def` is not evaluated until the functions are later called.
* pg 455: Your code should not care about specific data types; code to object interfaces in Python, not data types.
* pg 456: Classes implement the in operator either by providing the specific `__contains__` method or by supporting the general iteration protocol with the `__iter__` method.

### Chapter 17: Scopes
* pg 461: Any type of assignment within a function classifies a name as local, but in-place changes to objects do not classify names as locals.
* pg 469: Don't directly assign to a global in another module; have that module provide a simple setter function for its global so that it is a known point of interface and that the value can change.
* pg 475: Enclosing scope variable values are looked up when a nested function is later called; to remember a changing loop counter, for example, use default arguments which are evaluated when the nested function is created.
* pg 478: In Python 3.0 `nonlocal` restricts scope lookup to just enclosing `def` statements, requires that the names already exist there, and allows them to be assigned.
* pg 484: To workaround the lack of `nonlocal` in Python 2.6, nested functions can propagate values between calls by using an attribute on the function.

### Chapter 18: Arguments
* pg 492: When tuples are passed as arguments to functions, receive the tuple as a single variable and unpack it in the body of the function.
* pg 497: Because they subvert the normal left-to-right positional mapping, keywords allow us to essentially skip over arguments with defaults.
* pg 500: In a `def` header, `*` and `**` collect positional arguments in a tuple and keyword arguments in a dictionary, while in the call it unpacks them from from a sequence and from a dictionary.
* pg 502: When using keyword-only arguments in Python 3.0, a `*` character by itself in the arguments list indicates that a function does not accept a variable-length argument list.
* pg 504: In a function header, keyword-only arguments must be coded before the `**args` arbitrary keywords form and after the `*args` arbitrary positional form, when both are present.
* pg 511: To detect superfluous keywords without using required keywords, use `dict.pop()` to delete fetched entries, and check if the dictionary is not empty at the end.

### Chapter 19: Advanced Function Topics
* pg 523: The code object of a function, with the name `__code__`, allows introspection of a function's local variables and arguments.
* pg 527: A `lambda` introduces a local scope much like a nested `def`, which automatically sees names in enclosing functions, the module, and the built-in scope (via the LEGB rule).
* pg 533: The `map` built-in expects an *n*-argument function for *n* sequences; it sends items taken from sequences in parallel as distinct arguments to the function.
* pg 534: The `reduce` built-in allows an optional third argument placed before the items in the sequence to serve as a default result when the sequence is empty.

### Chapter 20: Iterations and Comprehensions, Part 2
* pg 540: You can code any number of nested `for` loops in a list comprehension, and each may have an optional associated `if` test.
* pg 543: A list comprehension can be used as a sort of column projection by unpacking each row into a tuple and evaluating the desired value.
* pg 545: Functions containing a `yield` statement are compiled specially as generators; when called, they return a generator object that supports the iteration interface.
* pg 548: When a value is passed to the `send` method of a generator, its code is resumed, and the `yield` expression returns the value passed to `send`, thereby creating a communication channel.
* pg 551: Both generator functions and generator expressions are their own iterators and thus support just one active iteration.
* pg 554: If `map` is called with `None` as its function and with more than one sequence, it acts like `zip`, but pads results with `None` if lengths differ instead of truncating.
* pg 559: Set and dictionary comprehensions are just syntactic sugar for passing generator expressions to the type names, which works because generators are iterable.
* pg 571: Any assignment in a function body makes that name local when compiled, and so even reading a global of the same name before the assignment raises an exception.
* pg 572: Default argument values are evaluated and saved when a function is compiled, and so their state persists between function.

### Chapter 21: Modules: The Big Picture
* pg 587: The byte code of top-level files is used internally and discarded; byte code of imported files is saved in files to speed future imports.
* pg 588: Python first looks for imported files in the home directory, which is either the directory containing the top-level file, or the current working directory if working interactively.
* pg 591: The empty string in `sys.path` denotes the current working directory.
* pg 593: Third-party installers can use `distutils` to put themselves automatically on the module search path; the emerging open source `eggs` system adds dependency checking.

### Chapter 22: Module Coding Basics
* pg 598: `import` and `from` are implicit assignments, where `import` assigns an entire module object to a single name, and `from` assigns one or more names to another module.
* pg 600: The `from` statement has more serious issues when used in conjunction with the `reload` call, as imported names might reference prior versions of objects.
* pg 608: The `reload` method takes a module, not its name, and changes it in place so that clients which qualify attributes with module names are affected, but not use importing using from.

### Chapter 23: Modules Packages
* pg 615: Python runs all code in the `__init__.py` files required in each directory named in a package `import` statement.
* pg 617: Each directory name in package import path becomes a variable assigned to a module object whose namespace is initialized by all the assignments in that directory's `__init__.py` file.
* pg 626: A package import in Python 2.6 searches the package first for a module; in 3.0, absolute imports skip the package directory, while "relative" import syntax searches it first and only.
* pg 629: An absolute import from a package can still load a module from locations other than the intended standard library, such as the current working directory.

### Chapter 24: Advanced Module Topics
* pg 643: The as extension is useful for providing a short, simple name for an entire directory path when using the package import.
* pg 644: Each module has a `__name__` attribute, visible as a global name; using this with `sys.modules`, you can change local and global variables of the same name inside a function.
* pg 651: Code inside a function body doesn't run until the function is called; because names in a function aren't resolved until the function actually runs, they can usually forward reference.
* pg 653: Using `from *` is dangerous because it can overwrite variables in your scope, but never use it more than once in a file, as unqualified function names must all lead back to the same file. 
* pg 655: Recursive from imports may not work, as statements in a module may not all have been run when it imports another module that imports it back.

### Chapter 25: OOP: The Big Picture
* pg 667: By redefining and replacing the attribute, a subclass effectively customizes what it inherits from its superclasses.
* pg 669: If there is more than one superclass listed in parentheses in a class statement, they will be searched for attributes in their left-to-right order.

### Chapter 26: Class Coding Basics
* pg 680: Classes and instances are linked namespace objects in a class tree that is searched by inheritance.
* pg 688: Operator overloading is useful when passing a user-defined object to a function that was coded to expect the operators available on a built-in type, but use it judiciously.
* pg 690: Each instance has a link to its class for inheritance through its `__class__` attribute, which in turn has a `__name__` attribute.

### Chapter 27: A More Realistic Example
* pg 700: Using Python 3.0's `print` function call syntax in Python 2.6 will turn multiple items into a tuple; avoid the extra parentheses portably by using formatting to yield a single object to print.
* pg 705: The `__repr__` method provides low-level details for developers, and is what the interactive prompt uses to echo results.
* pg 707: Calling a method on an instance is equivalent to calling it on the class and passing in the instance explicitly as `self`; this approach can be used to call methods of a superclass.
* pg 712: A subclass is not required to call a superclass' constructor, thereby replacing its logic altogether instead of augmenting it.
* pg 715: In 3.0, built-in operations do not route their implicit attribute fetches through `__getattr__` (for undefined attributes) or `__getattribute__` (for all attributes), making delegation more verbose.
* pg 716: The built-in `object.__dict__` attribute provides a dictionary with one key/value pair for every attribute attached to a namespace object, including modules, classes, and instances.
* pg 720: Python automatically expands method names beginning with two underscores to include the enclosing class's name, making them unique and eliminating collisions through inheritance.
* pg 725: Pickleable classes must be coded at the top level of a module file accessible from a directory listed on the `sys.path` module search path, or an object cannot be unpickled.

### Chapter 28: Class Coding Details
* pg 734: Any sort of statement can be nested inside a class body; these statements run when the class statement itself runs.
* pg 735: Inheritance searches occur only on attribute references, not on assignment: assigning to an object's attribute always changes that object, and no other.
* pg 738: Static methods allow you to code methods that do not expect instance objects in their first arguments; class methods receive a class when called instead of an instance.
* pg 744: The `@abstractmethod` decorator from the `abc` package disallows making an instance of a class unless the method is defined lower in the class tree.
* pg 750: While inspecting `__dict__` will not find inherited attributes, the `dir` built-in method also collects inherited attributes automatically and sorts them.

### Chapter 29: Operator Overloading
* pg 762: The `__index__` method in Python 3.0 isn't for index interception, but instead returns an integer value for an instance when needed and is used by built-ins that convert to digit strings.
* pg 764: In an iteration context, Python first looks for an `__iter__` method that supports `__next__` until `StopIteration` is raised, or else falls back on `__getitem__` eventually raising `IndexError`.
* pg 771: The `__setattr__` method intercepts all attribute assignments, and must assign any instance attributes through the attribute dictionary `__dict__`, so it doesn't enter an infinite loop.
* pg 774: While printing falls back on `__repr__` if no `__str__` is defined, the inverse is not true, so `__repr__` may be best if you want a single display for all contexts.
* pg 777: Right-side methods are only needed when you need operators to be commutative, and then only if you need to support such operators at all.
* pg 781: There are no implicit relationships among the comparison operators; consequently, both `__eq__` and `__ne__` should be defined to ensure that both operators behave correctly.
* pg 782: The `__cmp__` method is used as a fallback in Python 2.6 if more specific methods are not defined, but it is removed in Python 3.0.

### Chapter 30: Designing with Classes
* pg 801: A method for use only within a mix-in class can use the double underscore prefix to ensures that it won't interfere with other names in the tree, but don't clutter your code with it.
* pg 809: In Python 2.6, attribute search proceeds in a depth-first fashion, while in Python 3.0 it proceeds in a breadth-first fashion.
* pg 813: If iterating through all visible attributes of an instance using `dir`, you must retrieve their values using `getattr` and not `__dict__` to handle any inherited attributes.
* pg 814: Displaying the value of a method triggers the `__repr__` of the method's class in order to display the class, which can cause an infinite loop if `__repr__` itself is trying to display the value.

### Chapter 31: Advanced Class Topics
* pg 829: `bool` is a subclass of `int` with two instances (`True` and `False`) that behave like the integers `1` and `0` but inherit custom string representation methods that display their names.
* pg 832: With new-style classes, the type of a class instance is not the "instance" type, but the class it's created from and equivalent to its `__class__` attribute.
* pg 837: You can force the selection of an attribute from anywhere in an inheritance tree by assigning the one you want at the place where the classes are mixed together.
* pg 839: The implied object superclass in the new-style model provides default methods for a variety of built-in operations, including `__str__` and `__repr__`, so they never call `__getattr__`.
* pg 841: Extra attributes in new-style classes can still be accommodated by including `__dict__` in `__slots__`, in order to allow for an attribute namespace dictionary.
* pg 844: If any argument to the `property` built-in is passed as `None` or omitted, instead of using a default implementation, that operation is not supported.
* pg 855: Class methods always receive the lowest class in an instance's tree, and so may be better suited to processing data that differs for each class in a hierarchy versus static methods.
* pg 862: A class attribute can be read through an instance of that class, but trying to assign to a class attribute through an instance just creates an instance variable of the same name.
* pg 865: Method `def`s cannot see the local scope of the enclosing class; they can only see the local scopes of enclosing `def`s.

### Chapter 32: Exception Basics
* pg 878: Unlike other languages, in Python exceptions can also be used to signal valid conditions without you having to pass result flags around a program or test them explicitly.

### Chapter 33: Exception Coding Details
* pg 890: In Python 3.0, catching an exception named `Exception` has almost the same effect as an empty `except`, but ignores exceptions related to system exits.
* pg 892: The `else` clause ensures that `except` handlers will run only for real failures in the code you're wrapping in a `try`, not for failures in the `else` case's action.
* pg 897: Ordering must go `try`, `except`, `else`, `finally`, and there may be zero or more `except`, but there must be at least one `except` if an `else` appears.
* pg 900: The `raise` statement can accept an instance, a class for creating an instance from its no-argument constructor, or no argument to reraise the most recently raised exception.

### Chapter 34: Exception Objects
* pg 913: The `sys.exc_info` method is useful for empty `except` clauses that cannot determine the type of exception raised because they have no exception instance to lookup on.
* pg 918: In Python 2.6, user-defined exceptions should at least extend the `Exception` root class instead of being coded in the classic style, so they will be caught when catching `Exception`.
* pg 920: When adding custom attributes besides args to exceptions, define `__str__`; omitting it will not invoke `__repr__` as a fallback, but invoke the superclass version in `Exception`.

### Chapter 35: Designing with Exceptions
* pg 930: When any object is potentially a valid return value, exceptions can be used to signal unusual or nonerror conditions.
* pg 934: The older `sys.exc_type` and `sys.exc_value` functions return the most recent exception type and value globally, while the preferred `sys.exc_info()` is per-thread.
* pg 935: Resist using an catch-all `except` clause, especially when not in the top-level method and there may be try handlers higher up in the exception nesting structure.
* pg 939: The Python standard library is regularly run through PyChecker before release; use it with PyLint.

