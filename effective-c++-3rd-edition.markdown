## Effective C++, Third Edition

by Scott Meyers

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/Effective-Specific-Improve-Programs-Designs/dp/0321334876)!*

### Chapter 1: Accustoming Yourself to C++

#### Item 1: View C++ as a federation of languages
* There is C, object-oriented C++, templates and template metaprogramming, and the STL.

#### Item 2: Prefer `const`, `enum`, and `inline` to `#define`
* Unlike when using a `#define`, if an error occurs when using a constant, then its name is included in the error message.
* For a constant pointer in the header file, declare both the pointer and the data it points to as `const`.
* In-class initialization is allowed only for integral types, and only for constants.
* When using a macro, remember to parenthesize all the arguments, and beware expressions being evaluated multiple times if used as arguments.

#### Item 3: Use `const` whenever possible
* Using `const` is wonderful because it allows the compiler to enforce a semantic constraint.
* Declaring an `iterator` `const` means it isn't allowed to point to something different, but whatever it points to may be modified.
* One of the hallmarks of good user-defined types is that they avoid gratuitous incompatibilities with built-in types.
* A `const` member function can overload a non-`const` member function, and the former will be used on `const` objects.
* Bitwise `const` is C++'s definition of `const`. Logical `const` is when bits in the object are changed, but in ways that the client cannot detect.
* The `mutable` keyword frees non-static data members from the constraints of bitwise `const`.
* By calling `static_cast` to add `const` to `this`, and then `const_cast` to remove `const` from the return value, the overloaded non-`const` function can call the `const` one.

#### Item 4: Make sure that objects are initialized before they're used
* Reading uninitialized values yields undefined behavior, so always initialize objects before you use them.
* Always listing every data member on the initialization list avoids having to remember which data members may go uninitialized.
* Within a class, data members are initialized in the order they're declared, and not in their order on the initialization list.
* If a non-local static object in one translation unit uses a non-local static object in another translation unit, it may be uninitialized, because their initialization order is undefined.
* Using `static` objects defined in functions eliminates this problem, and if you never call such a function, you don't even construct its `static` object.

### Chapter 2: Constructors, Destructors, and Assignment Operators

#### Item 5: Know what functions C++ silently writes and calls
* A compiler declares a copy constructor, copy assignment operator, and destructor if you don't declare them yourself, as well as a default constructor if you declare none.
* The generated destructor is not virtual unless the class inherits from a base class with a virtual destructor.
* You must define the copy constructor yourself if the class contains a reference member or a `const` member.

#### Item 6: Explicitly disallow the use of compiler-generated functions you do not want
* Declare the copy constructor and the copy assignment operator private to prevent the compiler from generating its own version.
* To stop member and friend functions from still calling them, don't actually define them; this generates an error during the linking stage.

#### Item 7: Declare destructors virtual in polymorphic base classes
* When a derived class object is deleted through a pointer to the base class with a non-virtual destructor, results are undefined, but typically the derived part isn't destroyed.
* If the class isn't intended to be a base class, making the destructor virtual increases its size, as this adds a `vptr` (virtual table pointer) and `vtbl` (virtual table).
* The `string` class and STL container types (`vector`, `list`, `set`, etc.) lack virtual destructors, and so should never be inherited from.

#### Item 8: Prevent exceptions from leaving destructors
* Depending on the circumstances, if two destructors simultaneously emit exceptions, program execution either terminates or yields undefined behavior.
* One option is to terminate the program in the destructor, thereby preventing any undefined behavior.
* The second option is to swallow the exception, but only if the program can reliably continue after the exception was ignored.
* Good practice is to try and move the operation that can generate an exception to outside the destructor.

#### Item 9: Never call virtual functions during construction or destruction
* During base class construction, virtual function calls never go down into a derived class, because an object is not of a derived class until its constructor begins execution.
* The same reasoning applies during destruction.
* You must ensure that your constructors or destructors don't call virtual functions, and that all the functions they call obey the same constraint.

#### Item 10: Have assignment operators return a reference to `*this`
* This is what all built-in types do, thereby allowing chained assignments, and it applies to all assignment operators (such as `operator+=`).

#### Item 11: Handle assignment to self in `operator=`
* Code that operates on references or pointers to multiple objects of the same type must consider that the objects might be the same.
* Guard against self-assignment by checking the argument's address against `this` at the top of `operator+=`.
* In many cases, a careful ordering of statements can yield code that guards against both exceptions and self-assignment.
* Another alternative that guards against both exceptions and self-assignment is the "copy and swap" technique, which is covered in Item 29.

#### Item 12: Copy all parts of an object
* When you add new data members to a class, be sure to update its copy constructor and copy assignment operator accordingly.
* A copying function should copy all local data members, and also invoke the appropriate copying function in all base classes.

### Chapter 3: Resource Management

#### Item 13: Use objects to manage resources
* A thrown exception or a premature `return`, `continue`, or `goto` statement might preclude execution from eaching a `delete` statement.
* By putting resources inside objects like `auto_ptr`, we can rely on C++'s destructor invocation to ensure that the resources are released.
* Resource Acquisition Is Initialization, or RAII, means acquiring a resource and initializing its managing object happen in the same statement.
* Copying an `auto_ptr` sets it to null. While this enforces an object being managed by only one `auto_ptr`, you cannot use them in STL containers.
* There is no `auto_ptr` or `shared_ptr` for dynamically allocated arrays because you should be using `vector` instead.

#### Item 14: Think carefully about copying behavior in resource-managing classes
* For resources that are not heap-based, smart pointers like `auto_ptr` and `shared_ptr` are inappropriate as resource handlers.
* Policies for copying an RAII object include prohibiting copying, and reference counting, copying, and transferring ownership of the underlying resource.

#### Item 15: Provide access to raw resources in resource-managing classes
* An implicit conversion function on the RAII class can make access to the raw resource easier, but this increases the chance of errors.
* Often an explicit conversion function simply named `get` is preferable, even if clients must explicitly call it each time.
* Returning the raw resource violates encapsulation, but RAII classes don't exist simply to encapsulate it, but to ensure that it is released.

#### Item 16: Use the same form in corresponding uses of `new` and `delete`
* Using the expression `delete` when `delete []` should be used results in undefined behavior.
* The memory for an array usually includes the size of the array, so that the `delete` operator knows how many destructors to call.
* Use the same form of `new` in all constructors that initialize a pointer member, or else you may use the wrong form of `delete`.
* The `string` and `vector` classes largely eliminate the need to dynamically allocate an array.

#### Item 17: Store `new` objects in smart pointers in standalone statements
* Compilers are given less leeway in reordering operations across statements than within them.

### Chapter 4: Designs and Declarations

#### Item 18: Make interfaces easy to use correctly and hard to use incorrectly
* The type system is your primary ally in preventing undesirable code from compiling.
* Restrict what can be done with a type. A common way to impose restrictions is to add `const` wherever you can.
* Avoid gratuitous incompatibilities with the built-in types so that interfaces behave consistently, thereby reducing mental friction.
* Any interface that requires clients to remember to do something is prone to incorrect use, because clients can forget to do it.
* In many applications, the additional runtime costs of resource managers are unnoticeable, but the reduction in client errors will be noticed by everyone.

#### Item 19: Treat class design as type design
* Approach class design with the same care that language designers lavish on the design of built-in types.
* Good types have a natural syntax, intuitive semantics, and one or more efficient implementations.
* If you inherit from existing classes, you are constrained by their design, particularly by whether their functions are virtual or not.
* Guarantees made with respect to performance, exception safety, and resource usage impose constraints on your implementation.
* If you're defining a whole family of types, you don't want to define a new class, you want to define a new class template.
* If you're only subclassing so you can add functionality to an existing class, consider non-member functions or templates instead.

#### Item 20: Prefer pass-by-reference-to-`const` to pass-by-value
* Passing by reference eliminates the *slicing problem*, where passing a derived class object to a function that accepts a base class object calls the base class copy constructor.
* References are typically implemented as pointers, so passing built-in types like `int` by value is usually more efficient.
* Implementers of iterators and function objects ensure that they are efficient to copy and are not subject to the slicing problem.
* Avoid passing a user-defined type by value because while its size may be small now, that is subject to change with its implementation.

#### Item 21: Don't try to return a reference when you must return an object
* A function should never return a reference or pointer to a local object that is destroyed when the function exits.
* Returning a reference from a function like `operator*` is incorrect. Such a function must return a new object.
* In some cases the construction and destruction of such a return value can be safely eliminated by the compiler.

#### Item 22: Declare data members private
* Many data members should be hidden, and rarely does every data member require a getter and a setter.
* Only functional interfaces makes it easy to notify other objects when variables are read or written, to verify class invariants and function pre- and postconditions, to perform synchronization in threaded environments, etc.
* Public means unencapsulated, which means an unchangeable implementation, especially for classes that are widely used.
* Protected data is as unencapsulated as public data, since changing such a data member could break all derived classes that use it, which is an unknowably large amount of code.

#### Item 23: Prefer non-member non-friend functions to member functions
* The more functions that can access data, the less the data is encapsulated, and the harder it is to change the characteristics of the data.
* Unlike a member function, a non-member non-friend function doesn't increase the count of functions that can access the private parts of a class.
* Consider putting the non-member function in the same namespace as the class it operates on.
* Partitioning functionality in a namespace across multiple files promotes clean organization, and clients can freely add more functions to the namespace.

#### Item 24: Declare non-member functions when type conversions should apply to all parameters
* For overloaded operators, compilers will also look at non-member functions accepting two parameters in the namespace or global scope.
* Parameters are eligible for implicit type conversion only if they are listed on the parameter list. The object on which the member function is invoked is never eligible.
* To support mixed mode arithmetic with operator overloading, make operators non-member functions accepting both operands as arguments.
* The opposite of a member function is a non-member function, not a friend function. Avoid friend functions when you can.

#### Item 25: Consider support for a non-throwing `swap`
* If using the "pimp idiom," define a member function named `swap` that does the actual swapping, then specialize `std::swap` to call that member function.
* It's okay to totally specialize templates in `std`, adding new templates, classes, functions, or anything else to `std` results in undefined behavior.
* In addition to the specialization of `std::swap`, write a non-member version of `swap` in the same namespace of your class.
* By prefacing a call to `swap` with `using std::swap`, compilers will look for the namespace definition first, then the specialization in `std`, and finally the general form.

### Chapter 5: Implementations

#### Item 26: Postpone variable definitions as long as possible
* Postponing declaring a variable until you have its initialization arguments avoids unnecessary default constructions.
* Assigning a variable defined outside a loop is more efficient than initializing it on ever iteration, because it avoids destructing when each iteration completes.

#### Item 27: Minimize casting

* Casts can subvert the type system, which is there to ensure that you're not trying to perform any unsafe or nonsensical operations on objects.
* Only use `reinterpret_cast` for low-level casts in low-level code, such as from a pointer to an `int`. It yields unportable results.
* Use `static_cast` to force implicit conversions as well as the reverse of such conversions, with the exception of `const` to non-`const`.
* Prefer the explicit, new-style casts. They are easier to search for, and their narrow purpose makes it possible for compilers to diagnose usage errors.
* Type conversions of any kind, either explicit via casts or implicit by compilers, often lead to additional code that is executed at runtime.
* Avoid making assumptions about how things are laid out in C++. Making casts based on such assumptions leads to undefined behavior.
* A `dynamic_cast` can have a significant runtime cost. If you need to perform derived class operations through a pointer or reference to the base class, explore alternative designs.
* Casts should be isolated as much as possible, typically hidden inside functions whose interfaces shield callers from the work done inside.

#### Item 28: Avoid returning "handles" to object internals
* A data member is only as encapsulated as the most accessible function returning a reference to it.
* Returning `const` references to data members can still lead to *dangling handles*, which refer to parts of objects that don't exist any longer.
* Functions that return a handle to an object internal are the exception and not the rule.

#### Item 29: Strive for exception-safe code
* Exception-safe functions don't leak resources, and don't allow data structures or objects to enter a corrupted state.
* The *basic guarantee* ensures that the program remains in a valid state if an exception is thrown, but that exact state may ot be predictable. The *strong guarantee* ensures that the state is unchanged.
* Often you can't guarantee that no exceptions are thrown, because anything that dynamically allocates memory can throw a `bad_alloc` exception.
* When functions have side-effects on non-local data instead of operating exclusively on local state, it's much harder to offer the strong guarantee.
* If a system has even a single function that's not exception-safe, then the system as a whole is not exception-safe.
* A function's exception-safety guarantee is a visible part of its interface, and you should choose it as deliberately as you choose all other interface aspects.

#### Item 30: Understand the ins and outs of inlining
* Compiler optimizations are typically designed for stretches of code that lack function calls, so inlining allows more optimizations.
* Inlined code can also lead to additional paging, a reduced instruction cache hit rate, and their accompanying performance penalties.
* The `inline` keyword is a request that compilers may ignore, and only the most trivial functions that are not virtual may be inlined.
* Even empty constructors and destructors are unlikely to be inlined, as they implicitly call the constructors of base classes and data members.
* Debuggers have trouble with inlined functions. For example, you can't set a breakpoint in a function that isn't there.

#### Item 31: Minimize compilation dependencies between files
* Instead of trying to forward-declare parts of the standard library, use the proper `#include` statements and be done with it.
* You never need a class definition to declare a function using that class. Forward declare the class, and shift the burden of including its definition to clients that call the function.
* A class that employs the pimpl idiom is often called a *handle class*.
* If a function is declared as pure virtual in an interface class, then there is no need to include the keyword `virtual` in its declaration in the subclass.
* Handle classes and interface classes decouple interfaces from implementations and thereby reduce compilation dependencies between files.

### Chapter 6: Inheritance and Object-Oriented Design

#### Item 32: Make sure public inheritance models "is-a"
* The most important rule in object-oriented programming in C++ is that public inheritance means "is-a".
* There is no one ideal design for all software. The best design depends on what the system is expected to do, both now and in the future.
* Good interfaces prevent invalid code from compiling. Prefer design that rejects operations during compilation than one that rejects at runtime.
* A class named `Square` extending `Rectangle` is a classic example of the fragile nature of class hierarchies.

#### Item 33: Avoid hiding inherited names
* If a function in a derived class has the same name as a function in the base class, it hides all overloaded forms of that function in the base class.
* To preserve the is-a relationship of inheritance, the derived class must include a `using` declaration to inherit all overloaded forms of a function with a given name.

#### Item 34: Differentiate between inheritance of interface and inheritance of implementation
* Pure virtual functions must be redeclared by any concrete class that inherits them.
* A simple virtual function allows a derived class to inherit a function interface as well as an implementation.
* A pure virtual function can still have an implementations of its own. A subclass must redeclare it, but it can call down to this "default" implementation.
* A non-virtual function serves a mandatory implementation, and should never be redefined in a derived class.
* Don't blindly declare all member functions virtual, and don't blindly declare all member functions non-virtual. Consider each one individually.

#### Item 35: Consider alternatives to `virtual` functions
* When a non-virtual member function calls a private virtual member function, subclasses can redefine the latter. This is a form of the template method design pattern.
* The only way to resolve the need for non-member functions to access the non-public parts of a class is to weaken its encapsulation.
* The `tr1::function` class can refer to anything that *acts* like a function and returns a type *convertible* to the specified type.
* The "standard" strategy pattern offers the possibility that an existing strategy can be tweaked via defining a subclass.

#### Item 36: Never redefine an inherited non-virtual function
* Non-virtual functions are statically bound, so calling one on a base class pointer or reference uses the base class implementation, and not any derived class implementation.
* A non-virtual function is an invariant over specialization for the base class, and so no derived class should try to redefine it.
* Item 7, which warns against not specifying a virtual destructor in a base class, is a special case of this item.

#### Item 37: Never redefine a function's inherited default parameter value
* An object's dynamic type is determined by the object to which it currently refers, which in turn determines how it will behave.
* Default parameters are statically bound, so invoking a virtual function defined in a derived class uses a default parameter value from the base class.
* To avoid duplication of the default parameter, use the non-virtual interface idiom, where the default parameter is only defined once in the public non-virtual function.

#### Item 38: Model "has-a" or "is-implemented-in-terms-of" through composition
* Composition in the application domain expresses a has-a relationship, while in the implementation domain it expresses an is-implemented-in-terms-of relationship.
* Inappropriate subclassing violates the is-a principle, and should be replaced with composition.

#### Item 39: Use private inheritance judiciously
* With private inheritance, you cannot assign a derived object to a base class pointer, and protected and public members in the base class are private in the derived class.
* Use composition whenever you can, and use private inheritance whenever you must.
* Use private inheritance if two classes don't have an is-a relationship, but one needs to access the protected members or redefine a virtual function in the other.
* If a class privately inherits from another and defines a virtual function, its own subclasses can redefine that function, even if they can't call it.

#### Item 40: Use multiple inheritance judiciously
* If a function is defined in two base classes, then a call in the derived class to that function is always ambiguous, even if only one definition is accessible.
* Virtual inheritance prevents replicating data in the base class when multiple inheritance is used, but it's slower and creates larger objects.
* The one reasonable of multiple inheritance is to combine public inheritance of an interface with private inheritance of an implementation.
* If the only design you can come up with involves multiple inheritance, you should think a little harder.

