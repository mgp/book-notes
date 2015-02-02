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
* Becuase JavaScript is oriented around objects, it must have semantics for self-references, which conflict with the notion of functional programming.
* The term *function* means a function that exists on its own, while *method* means a function that is created in the context of an object.
* Applicative programming is defined as the calling of one function by another function, where the former is provided as an argument to the latter -- such as `map`, `filter`, and `reduce`.
* Functional programming is collection-centric. As Alan Perlis said, "It is better to have 100 functions operate on one data structure, than 10 functions on 10 data structures."
* The `groupBy` function in Underscore takes a collection and a criteria function, and returns an object where the keys are the criteria points returned by the function, and their associated values are the elements that matched.
* The `countBy` function is similar to `groupBy`, but it returns an object with keys of the match criteria associated with its count.
* Functional programming is often about defining a chain of functions, one called after the other, each gradually transforming the result from the last to reach a final solution.
* Instances with accessor methods lock data up into per-piece of information micro-languages. The associative data technique, by contrast, structures named values to form higher-level data models, accessed in uniform ways.
* Functional programmers think deeply about their data, the transformations occurring on it, and the hand-over formats between the layers of their applications.
