## Functional JavaScript: Introducing Functional Programming with Underscore.js

by Michael Fogus

#### Chapter 1: Introducing Functional JavaScript

* Whereas the object-oriented approach tends to break problems into groupings of "nouns," or objects, a functional approach breaks the same problem into groupings of "verbs," or functions.
* Functional programming is what happens when you take a system built in an imperative way and shrink explicit state changes to the smallest possible footprint to make it more modular.
* Practical functional programming is not about eliminating state change, but instead about reducing the occurrences of mutation to the smallest area possible for any given system.
* A higher-order function is a function that takes a function and returns a new function.
* An optimization tool can eliminate code that is not called, or is "dead," through a process known as *code elision*.
* Functional programming may yield slower code. But write beautiful code first, and then only if your code is slow, optimize it for speed by reverting to imperative structure.
* Functional programming builds functions for abstractions, and uses existing functions and passes existing functions to other functions to build more complex abstractions.

#### Chapter 2: First-Class Functions and Applicative Programming

* Boiling down the definition of a functional programming language to its essence, it is one that facilitates the use and creation of first-class functions.
* A higher-order function is a function that can take a function as an argument, or return a function as a result.
* Metaprogramming is a programming paradigm that manipulates the basis of the language's execution model.
* Imperative code operates at a precise level of detail that makes them one-shot implementations at best. This level of detail is often better for compilers than for programmers.
* Because JavaScript is oriented around objects, it must have semantics for self-references, which conflict with the notion of functional programming.
* The term *function* means a function that exists on its own, while *method* means a function that is created in the context of an object.
* Applicative programming is defined as the calling of one function by another function, where the former is provided as an argument to the latter -- such as `map`, `filter`, and `reduce`.
* Functional programming is collection-centric. As Alan Perlis said, "It is better to have 100 functions operate on one data structure, than 10 functions on 10 data structures."
* The `groupBy` function in Underscore takes a collection and a criteria function, and returns an object where the keys are the criteria points returned by the function, and their associated values are the elements that matched.
* The `countBy` function is similar to `groupBy`, but it returns an object with keys of the match criteria associated with its count.
* Functional programming is often about defining a chain of functions, one called after the other, each gradually transforming the result from the last to reach a final solution.
* Instances with accessor methods lock data up into per-piece of information micro-languages. The associative data technique, by contrast, structures named values to form higher-level data models, accessed in uniform ways.
* Functional programmers think deeply about their data, the transformations occurring on it, and the hand-over formats between the layers of their applications.

#### Chapter 3: Variable Scope and Closures

* The term "scope" has various meaning among JavaScript programmers, but this book defines it as the variable value resolution scheme, or the lexical bindings.
* Lexical scope refers to the visibility of a variable and its value analogous to its textual representation, or in accordance with its surrounding source code.
* Dynamic scoping uses a global table of named values, which any function can modify. Its trouble therefore lies in that the value of any given binding cannot be known until the caller of any given function is known.
* JavaScript makes selective use of dynamic scoping by allowing manipulating of the `this` reference through the use of `apply` or `call`.
* In JavaScript, all `var` declarations in a function body are implicitly moved to the top of the function, which is called *hoisting*.
* A closure is a function that captures the external bindings contained in the scope in which it was defined for later use, even after that scope has completed.
* Free variables are those variables that will be closed over in the creation of a closure. They are carried along for later use in inner functions that are returned by their enclosing functions.

#### Chapter 4: Higher-Order Functions

* A higher-order function is a first-class function that either takes a function as an argument, or returns a function as a result.
* By changing a function so that it exposes a function instead of a value, you open up a world of flexibility to the call site.
* A feed-forward function is where the result of some call to a passed function is fed into the next call of the function as its argument.
* While you can directly invoke a particular method on an instance, a functional style prefers functions that take the invocation target as an argument.
* You might want to create a function that returns another function because the arguments to the higher-order function "configure" the behavior of the returned function.
* Closing over a free variable ensures that the client of the returned function cannot manipulate the value of that variable.
* A *referentially transparent* function relies only on its arguments for the value that it will return. This allows you to replace any call to a function with its expected value without breaking your program.
* The functional style has a propensity to build higher-level parts from lower-level functions.
* Returning a function from another function, and taking advantage of captured arguments along the way, is known as currying.
* We can view functional programming as a virtual assembly line, where data is fed into one end of a functional "machine," transformed and optionally validated along the way, and returned at the end as something else.

#### Chapter 5: Function-Building Functions

* Polymorphic functions are functions that exhibit different behaviors based on their arguments.
* The essence of functional composition is using existing parts in well-known ways to build up new behaviors.
* Mutation is a low-level operation. It is acceptable if it happens within the confines of a function and if it never escapes.
* For every logical parameter, a curried function will keep returning a gradually more configured function until all parameters have been filled.
* One reason for currying from the right is that partial application handles working from the left, and between them, they have both directions covered.
* A currying function that takes a function and returns a function expecting only one parameter can be used to ignore additional optional parameters expected by the function.
* If an API uses higher-order functions, then curried functions, at least to one parameter, are appropriate.
* JavaScript works against currying because it allows a variable number of arguments to functions, which exhibit different behavior given their number of type.
* Partial application does not work with one function at a time, but instead stores the partially applied arguments for later execution, given the remaining arguments.
* Use of partial application can lead to fluent APIs, which in turn makes your code declarative, specifying what is to happen rather than how.
* Functions that compose other functions should themselves compose.

#### Chapter 6: Recursion

* Recursion is important because it applies a single abstraction to multiple subsets of a common problem, it can hide mutable state, and it can implement laziness and infinitely large structures.
* When writing a self-recursive function, know when to stop, decide how to take one step, and break the problem into that step and a smaller problem.
* An accumulator argument is a common technique in recursion for communicating information from one recursive call to the next.
* If the last action of a function is a recursive call, then it is tail recursive. This means that there is no way the function body will be used again, and its resources can be deallocated.
* Using a nested function is a common way to hide accumulators in recursive calls.
* Two or more functions that call one another are known as mutually recursive.
* To avoid making too many recursive calls and "blowing the stack," a function can return another function that wraps the call to the mutually recursive function instead of calling it directly.
* A *trampoline* repeatedly calls the return value of a function until its no longer a function, and can thereby flatten out the recursive calls.
* Recursion is a low-level operation and should be avoided if possible; instead, combine higher-order functions to create new functions.

#### Chapter 7: Purity, Immutability, and Policies for Change

* A pure function returns a result that is calculated only from the values of its arguments, and cannot rely on or change state outside of its body.
* When you attempt to test functions that rely on the vagaries of external conditions, then all test cases must set up those same conditions for the very purpose of testing.
* Because JavaScript passes object references around, every function that takes an object or an array is subject to impurity.
* Pure functions allow for the easy composition of functions and makes replacing any given function in your code with an equivalent function, or even the expected value, trivial.
* By viewing the function as the basic unit of abstraction, the implementation details of functions are irrelevant as long as they do not "leak out."
* If you compose two pure functions, then the resulting function is also pure. Therefore try and create pure abstractions around impure ones.
* Immutable objects should take their values at construction time and never change again afterward. All operations on them should return new instances.
* A language like JavaScript can only provide so much safety, and the burden is therefore on us to adhere to strictures that ensure that our programming practices are as safe as possible.
* Factory methods allow returning different types for different construction arguments, allow changing the returned type without changing each call site, are a seam for preconditions, and are composable.
* The way to control the scope of change is to isolate the thing that changes. Instead of changing an object in-place, a better strategy may be to hold the object in a container and change that instead.

#### Chapter 8: Flow-Based Programming

* In Underscore, the `tap` method allows you to inspect an intermediate value belonging to the wrapper object used by `chain`.
* A function that wraps some behavior for later execution is typically called a *thunk*.
* jQuery promises are intended to provide a fluent API for sequencing asynchronous operations that run concurrent to the main program logic.
* Unlike a chain of lazily evaluated thunks, a jQuery promise calculates a value that is available on demand after the promise is executed.
* The primary problem of method chains is very often they mutate some common reference from one call to the next, whereas functional APIs perform nondestructive transformations that return new values.
* Indirection and deep function nesting can obscure how functional programming transforms data from one function to the next, but pipelines can make this data flow more explicit.
* Composing functions that accept parameters and return values of different "shapes" is possible using intermediate state and functions that return answers and update that state separately.
