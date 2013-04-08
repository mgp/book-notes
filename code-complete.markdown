## Code Complete

by Steve McConnell

### Chapter 5: Design in Construction

#### 5.1: Design Challenges

* Design is "wicked": You must "solve" the problem once in order to define it, and then solve it again to create a solution that works.
* Design relies on heuristics, and relies on trial-and-error.

#### 5.2: Key Design Concepts

* Managing complexity is the most important technical topic in software development.
* Complexity is reduced by dividing a system into subsystems that are ideally independent.
* Some desirable characteristics: simple, loosely-coupled, extensible, reusable, lean, stratified, standardized.
* Make subsystems meaningful by restricting communications and preventing cycles.

#### 5.3: Design Building Blocks: Heuristics

* Real-World Objects: Identify objects' public/protected/private attributes, then public/protected interfaces.
* Form abstractions at the right level, allowing you can ignore irrelevant details
* Encapsulate; abstraction provides a high level of detail, while encapsulation says you can't change levels.
* Inheritance.
* Information hiding promotes secrets: hiding complexity for easier understanding, or hiding sources of change so its effects are localized.
	* Asking what a class should hide cuts to the heart of interface design.
* Identify and isolate areas likely to change, like nonstandard langauge features, bad design or construction, or data-size constraints.
* Keep coupling loose; one module using some semantic knowledge of a module's inner workings is especially bad.
* Design patterns provide vocabulary for efficient communicationa, and embody accumulated wisdom over years.
* Other heuristics: aim for strong cohesion, use preconditions and postconditions, design for test, and keep design modular.
* Don't get stuck on a single approach; if you are stuck on all approaches, step away for a bit.

#### 5.4: Design Practices

* Iterate; when you come up with something that seems good enough, don't stop, but instead apply what you learned on a second design.
* Top-down design is a decomposition strategy, while bottom-up design is a composition strategy.
	* Top-down design is easy and you can defer construction details.
	* Bottom-up design typically results in early identification of needed utility functionality.
* Prototyping fails when developers don't write the absolute minimum code, and so don't treat the code as throwaway.
* Big design problems found not to come from bad designs, but from areas deemed too easy for any design at all.
* Capture design work in code comments, on a Wiki, with photos of whiteboards, or UML diagrams.

### Chapter 6: Working Classes

#### 6.1: Class Foundations: Abstract Data Types (ADTs)

* ADTs hide implemenation details, isolate changes, promote informative interfaces, highlight correctness, provide a private namespace, and build on lower-level data.
* A class is an ADT with inheritance and polymorphism added in.

#### 6.2: Good Class Interfaces

* Each class should implement only one ADT; mixed abstractions move implementation details to the public interface and complicate understanding.
* If a subset of a class' methods operate on a subset of its data, move the data and methods into a new class.
* Minimize assumptions by the programmer to use an interface; have the compiler or its own form enforce the requirements.
* Only add public members to a class that are consistent with its abstraction, even if a convenient utility method.
* Abstraction provides models allowing you to ignore implementation details, while encapsulation enforces this principle.
* Looking at a class' implementation to determine its use breaks encapsulation, and breaking abstraction isn't far behind.

#### 6.3: Design and Implementation Issues

* Don't re-use names of non-overridable methods from the base class in a derived class.
* Move common interfaces, data, and behavior as high as possible in the inheritance tree.
* Be wary of classes with only one instance (excluding singletons), and base classes with only one subclass.
* A subclass overriding a method to do nothing violates the interface contract and should be addressed in the base class.
* Inheritance works against managing complexity and so you should bias against it.
* Keep class interfaces small, the implementations insulated, and minimize its collaboration with other classes.

#### 6.4: Reasons to Create a Class

* The best reason to create a class is to hide information, thereby reducing complexity.
* Classes also isolate complexity, hide implementation details, streamline parameter passing, promote code resuse, and package related operations.
* Avoid god classes; if a class retrieves its data from and stores its data in a god class, move that data.

### Chapter 7: High-Quality Routines

* A bad routine has bad names, bad layout, multiple purposes, too many parameters, poor documentation, uses global variables, and doesn't defend against bad data.

#### 7.1: Valid Reasons to Create a Routine

* The most important reason to create a routine is to reduce complexity; using a routine doesn't require knowing its inner workings.
* Routines can encapsulate or hide the assumption about the order in which operations must be performed or routines are called.
* Putting complicated boolean tests in routines hides the details, summarizes its purpose, and emphasizes its significance. 
* No block of code is too small to put into a routine, especially if it improves readability.

#### 7.2: Design at the Routine Level

* A cohensive routine contains operations that are related; otherwise, it probably does more than one thing.

#### 7.3: Good Routine Names

* A routine with long, complicated name may stem from the routine doing too much; break the routine into multiple routines.
* Name functions after the returned value; name procedures with a strong verb and an object to provide its context.

#### 7.5: How to Use Routine Parameters

* Put parameters in input-modify-output order; if several routines use similar parameters, order the parameters consistently.
* If you consistently pass too many arguments to a function, the coupling among your routines is too tight.
* Pass the variables or objects that a routine needs to maintain its interface abstraction.

### Chapter 9: The Pseudocode Programming Process

#### 9.2: Pseudocode for Pros

* When writing pseudocode, avoid syntactic elements from the target programming language.
* Catching errors at the "least-value stage," or when the least effort has been invested, contributes to success.

#### 9.3: Constructing Routines by Using the PPP

* TODO

