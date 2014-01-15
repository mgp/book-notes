## Code Complete

by Steve McConnell

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/Code-Complete-Practical-Handbook-Construction/dp/0735619670)!*

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
* Information hiding promotes secrets: hiding complexity for easier understanding, or hiding sources of change so its effects are localized.
	* Asking what a class should hide cuts to the heart of interface design.
* Identify and isolate areas likely to change, like nonstandard language features, bad design or construction, or data-size constraints.
* Keep coupling loose; one module using some semantic knowledge of a module's inner workings is especially bad.
* Design patterns provide vocabulary for efficient communication, and embody accumulated wisdom over years.
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

* ADTs hide implementation details, isolate changes, promote informative interfaces, highlight correctness, provide a private namespace, and build on lower-level data.
* A class is an ADT with inheritance and polymorphism added in.

#### 6.2: Good Class Interfaces

* Each class should implement only one ADT; mixed abstractions move implementation details to the public interface and complicate understanding.
* If a subset of a class' methods operate on a subset of its data, move the data and methods into a new class.
* Minimize assumptions by the programmer to use an interface; have the compiler or its own form enforce the requirements.
* Only add public members to a class that are consistent with its abstraction, even if a convenient utility method.
* Abstraction provides models allowing you to ignore implementation details, while encapsulation enforces this principle.
* Looking at a class' implementation to determine its use breaks encapsulation, and breaking abstraction isn't far behind.

#### 6.3: Design and Implementation Issues

* Don't re-use names of non-overrideable methods from the base class in a derived class.
* Move common interfaces, data, and behavior as high as possible in the inheritance tree.
* Be wary of classes with only one instance (excluding singletons), and base classes with only one subclass.
* A subclass overriding a method to do nothing violates the interface contract and should be addressed in the base class.
* Inheritance works against managing complexity and so you should bias against it.
* Keep class interfaces small, the implementations insulated, and minimize its collaboration with other classes.

#### 6.4: Reasons to Create a Class

* The best reason to create a class is to hide information, thereby reducing complexity.
* Classes also isolate complexity, hide implementation details, streamline parameter passing, promote code reuse, and package related operations.
* Avoid god classes; if a class retrieves its data from and stores its data in a god class, move that data.

### Chapter 7: High-Quality Routines

* A bad routine has bad names, bad layout, multiple purposes, too many parameters, poor documentation, uses global variables, and doesn't defend against bad data.

#### 7.1: Valid Reasons to Create a Routine

* The most important reason to create a routine is to reduce complexity; using a routine doesn't require knowing its inner workings.
* Routines can encapsulate or hide the assumption about the order in which operations must be performed or routines are called.
* Putting complicated boolean tests in routines hides the details, summarizes its purpose, and emphasizes its significance. 
* No block of code is too small to put into a routine, especially if it improves readability.

#### 7.2: Design at the Routine Level

* A cohesive routine contains operations that are related; otherwise, it probably does more than one thing.

#### 7.3: Good Routine Names

* A routine with long, complicated name may stem from the routine doing too much; break the routine into multiple routines.
* Name functions after the returned value; name procedures with a strong verb and an object to provide its context.

#### 7.5: How to Use Routine Parameters

* Put parameters in input-modify-output order; if several routines use similar parameters, order the parameters consistently.
* If you consistently pass too many arguments to a function, the coupling among your routines is too tight.
* Pass the variables or objects that a routine needs to maintain its interface abstraction.

### Chapter 8: Defensive Programming

#### 8.1: Protecting Your Program From Invalid Inputs

* To handle "garbage-in," check values from external sources, values of method parameters, and then decide how to handle bad inputs.

#### 8.2: Assertions

* Use error handling code for conditions you expect to occur; use assertions for conditions that should never occur.
* Use assertions to document precondition and postconditions. Don't put executable code in one.

#### 8.3: Error Handling Techniques

* Correctness means never returning an inaccurate result; robustness means always trying to do something that allows the program to keep running.
* Beware using a neutral value, substituting the next valid data, returning the last result, or substituting the closest legal value.

#### 8.4: Exceptions

* Exceptions weaken encapsulation by requiring the caller to know which exceptions might be thrown from the code that's called.

#### 8.5: Barricade Your Program to Contain the Damage Caused by Errors

* If public methods of a class checking and sanitizing data, then private methods can assume it is safe.
* Convert data to the proper type ASAP; otherwise you increase complexity and increase the chance that someone can crash your program.

#### 8.7: Determining How Much Defensive Programming to Leave in Production Code

* Remove code that results in hard crashes in production; but during development, this is invaluable for debugging.

### Chapter 9: The Pseudocode Programming Process

#### 9.2: Pseudocode for Pros

* When writing pseudocode, avoid syntactic elements from the target programming language.
* Catching errors at the "least-value stage," or when the least effort has been invested, contributes to success.

#### 9.3: Constructing Routines by Using the PPP

* TODO

### Chapter 10: General Issues in Using Variables

#### 10.3 Guidelines for Initializing Variables

* Ideally, declare and define each variable close to where it's used, following the principle of proximity.
* Initialize named constants once; initialize variables with executable code, such as in a `Startup()` method.

#### 10.4: Scope

* A span is the number of lines between successive variable uses; live time is the number between its first use and its last.
* To help minimize scope, begin with most restricted visibility, and expand the variable's scope only if necessary.
* Maximizing scope may make programs easy to write, but a program in which any method can use any variable at any time is harder to understand.

#### 10.6: Binding Time

* The earlier the binding time, the lower the flexibility and the lower the complexity. So add only as much flexibility as needed.

#### 10.8: Using Each Variable for Exactly One Purpose

* Use variables for only one purpose; avoid having different values for the variable mean different things (like negative values).

### Chapter 11: The Power of Variable Names

#### 11.1: Considerations in Choosing Good Names

* A good name tends to express the "what" more than the "how."
* To avoid `numSales` versus `saleNum` confusion, consider variable names like `salesTotal`, `salesCount`, and `salesIndex`.

#### 11.2: Naming Specific Types of Data

* Intermediate variables do not warrant a name like `temp`. Such a name may indicate that we aren't sure of their real purposes.
* Give boolean variables names that imply true or false, like `sourceFileFound` or `isStatusOk` instead of `sourceFile` and `status`.

#### 11.4: Informal Naming Conventions

* Variable names can contain: The variable contents, the kind of data, and the scope or visibility of the variable.

#### 11.6: Creating Short Names That Are Readable

* When shortening variable names, don't remove just one letter, be consistent, create pronounceable names, and avoid mispronunciation.

#### 11.7: Kinds of Names to Avoid

* Avoid names with similar meanings, like `fileNumber` and `fileIndex`.
* Avoid names with different meanings but similar names, like `clientRecs` and `clientReps`.

### Chapter 12: Fundamental Data Types

#### 12.1: Numbers in General

* By replacing magic numbers with constants, changes are reliable, changes can be made easily, and your code is more readable.

#### 12.3: Floating-Point Numbers

* To increase accuracy when adding numbers with differing magnitudes, add them starting with the smallest values.
* Avoid equality operations and anticipate rounding errors; cope by switching to greater precision, BCD, or integer variables.

#### 12.4: Characters and Strings

* To avoid endless strings in C, initialize strings to null, and use `strncpy()` instead of `strcpy()`.

#### 12.6: Enumerated Types

* Use enumerated types for more type-checking, and as a richer alternative to boolean variables.
* Explicitly assign their values to specify first and last values for iteration, and an invalid or "null" type.

#### 12.8: Arrays

* In C, use or define an `ARRAY_LENGTH()` macro as `#define ARRAY_LENGTH(x) (sizeof(x) / sizeof(x[0]))`.

#### 12.9: Creating Your Own Types

* Don't name a type created using `typedef` after the underlying data type, and don't refer to predefined types.

### Chapter 13: Unusual Data Types

#### 13.1: Structures

* By passing only one or two fields from a structure into a method, you promote information hiding *from* the method.

#### 13.2: Pointers

* Symptoms of pointer errors tend to be unrelated to causes of pointer errors.
* By isolating pointer operations to methods, you minimize the possibility of propagating careless mistakes through your program.
* Allocating dog tags allow you to check for freeing memory twice, or overwriting memory beyond the last byte.
* Free pointers at the same scoping level as they were allocated, such as in the same method, or a constructor/destructor pair.
* Set a pointer to `NULL` after deallocation; writing to it produces an error, and deallocating twice is more easily caught.
* In C++, a reference cannot point to `NULL` and the object it refers to cannot be changed.
* In C, you can use `char` or `void` pointers for any type of variable.

#### 13.3: Global Data

* Passing a global variable to a method, and then referring to both the parameter and global variable is especially tricky.
* Initialization order among different "translation units," or files, is not defined in languages like C++.
* Try to contain a global variable as a class variable, and provide an accessor for any other code that needs it.
* Replace global data with access methods to centralize control over it and protect yourself against changes.
* Build access methods at the level of the problem domain rather than at the level of the implementation details.

### Chapter 14: Organizing Straight-Line Code

#### 14.1: Statements That Must Be in a Specific Order

* Organize code so that dependencies are obvious; if one method initializes data, create and call an `Initialize()` method.

#### 14.2: Statements Whose Order Doesn't Matter

* Statements that operate on the same data, perform similar tasks, or have ordering dependencies should appear together.

### Chapter 15: Using Conditionals

#### 15.1: `if` statements

* For both readability and performance, write the nominal path through the code first, then the unusual cases.
* Simplify complicated conditional expressions with calls to methods that return boolean values.

#### 15.2: `case` statements

* Some `case` statements only work on data of certain types; don't create a "phony variable" to use a case statement.
* Use the `default` statement to detect legitimate defaults, or to detect errors, and nothing else.

### Chapter 16: Controlling Loops

#### 16.1: Selecting the Kind of Loop

* If you don't know ahead of time exactly how many times you’ll want the loop to iterate, use a `while` loop.
* Don't code a "loop and a half"; instead, loop forever and break in the middle.
* Keep `for` loops simple; if you're explicitly changing the index value, consider a `while` loop.

#### 16.2: Controlling the Loop

* Put initialization code immediately before the loop.
* Use `for (;;)` or `while (true)` to write an infinite loop; don't fake it by iterating to a large number.
* Reserve the `for` loop header for initializing the loop, terminating it, and moving toward termination.
* Keep statements that control the loop, or move it toward termination, near its beginning or end.
* Don't change the index of a `for` loop to make it terminate.
* Avoid using the loop index value after the loop; instead assign a final value to a variable at the appropriate point inside the loop.
* Using a `break` forces the person reading your code to look inside the loop for an understanding of the loop control.
* Inefficient programmers experiment randomly until they find something that works, perhaps replacing a bug with a more subtle one.

### Chapter 17: Unusual Control Structures

#### 17.2: Recursion

* For most situations, recursion produces very complicated solutions that chew up stack space. Use it selectively, and prefer iteration.
* For local variables in recursive functions, use `new` to create objects on the heap as opposed to on the stack.

#### 17.3: `goto`

* The use of a `goto` statement defeats some compiler optimizations, which rely on orderly flow control.
* The `try`-`finally` construct can sometimes be used to perform the error cleanup that a `goto` sometimes performs.
* Measure the performance of any `goto` statement used to improve efficiency.

### Chapter 18: Table-Driven Methods

#### 18.1: General Considerations in Using Table-Driven Methods

* With a table driven method, you must address how to look up entires, and what data should be stored in the table.

#### 18.2: Direct Access Tables

* A table driven approach generates less code and is easier to change without the need to recompile.
* Put a lookup key transformation in its own method to guard against different transformations in different places.

#### 18.4: Stair-Step Access Tables

* This puts the upper end of consecutive ranges into a table, and works well with a binary search for larger lists.

### Chapter 19: General Control Issues

#### 19.1: Boolean Expressions

* Even if a complicated conditional expression is only used once, moving it into its own method is useful for improving readability.
* Organize numeric tests so that they follow points on a number line.
* Comparing a character against `\0` instead of `0` reinforces that the expression works with character data instead of logical data.

#### 19.3: Null Statements

* Null statements are uncommon, so make them obvious, such as a comment inside the braces explaining why one is used.

#### 19.4: Taming Dangerously Deep Nesting

* Use a `break` block, try to flatten, move some of the nested blocks into their own methods, or use polymorphism.
* Complicated code is a sign that you don't understand your program well enough to make it simple.

#### 19.5: A Programming Foundation: Structured Programming

* The core of structured programming is the simple idea that a program should use single-entry, single-exit control constructs.

### Chapter 23: Debugging

#### 23.1: Overview of Debugging Issues

* If you don't know what you're telling the computer to do, you're programming by trial and error, and defects are guaranteed.
* If your code has a bug, don't blame the compiler, and don't blame the computer. It's your fault.

#### 23.2: Finding a Defect

* Locating a defect is like using the scientific method: gather data, formulate a hypothesis, and prove it.
* An error that doesn't occur predictably usually results from an initialization error or dangling pointer problem.
* Don't just find a test case that produces the error; reduce the test case to the simplest form possible.
* If the data doesn't fit the hypothesis, don't discard the data; instead, ask why it doesn't fit, and create a new hypothesis.
* Use all available tools to find an error: interactive debuggers, static analysis, memory inspection, and so on.
* You often discover your own defect in the act of explaining it to another person. Or try taking a break from the problem.
* If you have a syntax error, try removing part of the code and compiling again.

#### 23.3: Fixing a Defect

* Understand and fix the problem. Don't fix the symptom; such solutions are incomplete and unmaintainable.
* After you make a fix, check it again, and look for similar defects.

#### 23.4: Psychological Considerations in Debugging

* Choose variable names that can be easily differentiated from each another.

#### 23.5: Debugging Tools, Obvious and Not-So-Obvious

* Set your compiler's warning level to the highest setting, and treat them as errors so that you fix them.
* Debuggers allow full examination of data, including structured and dynamically allocated data.
* The debugger isn't a substitute for good thinking; the most effective solution is using both together.

### Chapter 24: Refactoring

#### 24.1: Kinds of Software Evolution

* If you treat modifications as opportunities to tighten up the original design of the program, quality improves.
* You know much more after you've written a program; use what you've learned to improve it.

#### 24.3: Reasons to Refactor

* Duplicate, long, or nested code, and poor cohesion, abstraction, encapsulation, or information hiding are good reasons.
* Never write speculative code or design ahead; it adds complexity, is likely untested, and is unlikely to meet requirements.

#### 24.4: Specific Refactorings

* Data level refactorings include naming constants, renaming variables, and introducing intermediate variables.
* Statement level refactorings include simplifying boolean expressions, using `break` or `return`, and swapping conditionals and polymorphism.
* Routine level refactorings include extracting methods, combining them by parameterizing them, and passing fields or complete objects.
* Class implementation refactorings include pulling up or pushing down data or methods, and separating or combining classes with subclasses.
* Class interface refactorings include swapping inheritance with delegation, and encapsulating or exposing member variables.
* System level refactorings include swapping factory methods with constructors, and error codes with exceptions.

#### 24.5: Refactoring Safely

* Keep refactorings small, do one at a time, review the changes thoroughly, and test them.

#### 24.6: Refactoring Strategies

* Refactor as you add code, and target error-prone or high-complexity modules especially.

### Chapter 25: Code-Tuning Strategies

#### 25.1: Performance Overview

* Performance is only loosely related to code speed; when you focus on speed, you ignore other quality characteristics.
* The mere act of making resource goals explicit improves the likelihood that they'll be achieved.
* Code tuning is the practice of modifying correct code so that it runs more efficiently.

#### 25.2: Introduction to Code Tuning

* Complete the code first, and then perfect it. By the Pareto principle, the part that needs to be perfect is usually small.
* For a given language, compiler, architecture, and hardware, you must always measure performance to evaluate your changes.
* Don't optimize bottlenecks as you go, as they'll monopolize your attention, and they'll likely not be the biggest ones anyway.
* Optimizing compilers are better at optimizing straightforward code than they are at optimizing tricky code.

#### 25.3: Kinds of Fat and Molasses

* Operations that swap pages of memory are slow; for example, consider the iteration order of a nested loop.
* Errors like logging debug information to a file, leaking memory, and bad schema design can also affect performance.
* Polymorphic method calls are slightly more expensive than not, while transcendental math functions are extremely expensive.

#### 25.4: Measurement

* Use the number of CPU clock ticks allocated to your program rather than the time of day, so that it's unaffected by multitasking.

### Chapter 26: Code-Tuning Techniques

* Optimizations degrade the internal structure of a program, otherwise they'd be considered standard coding practice.

#### 26.1: Logic

* Use `break` or `return` in loops, order conditional tests by frequency, replace conditionals with table lookups, and evaluate lazily.

#### 26.2: Loops

* Bottlenecks in a program are often inside loops because these loops are executed many times.
* Putting loops inside conditionals instead of conditionals inside loops is faster, but two loops must now be maintained.
* Don't compute the same value inside a loop repeatedly.
* Put the loop with the one with the largest iteration bound on the inside, so that it contributes less to the total iterations.
* Replace an expression that multiplies the loop index by a factor with addition on each iteration and a cumulative sum.

#### 26.3: Data Transformations

* Prefer integers to floating point numbers when you can.
* Similar to minimizing accesses to pointers, introduce temporary variables to minimize repeated access to array elements.
* Introduce supplementary data or indexes, or cache the data or memoize results.

#### 26.4: Expressions

* Initialize constant values at compile time instead of at runtime.
* By replacing common subexpressions with an intermediate variable, you also improve the readability of a program.

#### 26.6: Recoding in Assembler

* Save the assembler output from your compiler, and use it as a starting point for any optimization.

### Chapter 31: Layout and Style

#### 31.1: Layout Fundamentals

* The Fundamental Theorem of Formatting is that good visual layout shows the logical structure of a program.
* The smaller part is writing code that the computer can read; the larger part is writing code that others can read.
* Structure helps experts to perceive, comprehend, and remember important features of programs.

#### 31.2: Layout Techniques

* A paragraph of code should be identified with a blank line, and contain statements that accomplish a single task.
* Use more parentheses than you think you need.

#### 31.4: Laying Out Control Structures

* Blank lines can improve code by opening up natural spaces for comments.
* For complicated conditional expressions, put each group of related expressions on its own line if possible.

#### 31.5: Laying Out Individual Statements

* Break up a multi-line statement so that the first line is blatantly incorrect syntactically if it stood alone.
* When breaking a line, keep related elements together, such as array references, method arguments, and so on.
* With one statement per line, code reads from top to bottom, and mapping line numbers to statements is unambiguous.
* C++ does not define the order in which terms in an expression or arguments to a method are evaluated.
* If your list of variables is so long that alphabetical ordering helps, your method is probably too big.
* In C++, putting the asterisk next to the type name wrongly suggests that all variables on the line are pointers.

#### 31.6: Laying Out Comments

* Preceding comments with a blank line helps the reader scan the code.

#### 31.8: Laying Out Classes

* Put only one class in each file unless you have a compelling reason to do otherwise, but group the methods of each.
* When separating methods or parts, blank lines are easy to type and look at least as good as any other separator.
* Define an ordering, such as the file description before `#include` statements, before `enum` definitions, and so on.

### Chapter 32: Self-Documenting Code

#### 32.2: Programming Style as Documentation

* The main contributor to code-level documentation isn't comments, but good programming style.

#### 32.3: To Comment or Not to Comment

* Good comments don't repeat the code, but clarify its intent, or explain it at a higher level of abstraction.
* If some code is difficult to comment, either it's bad code or you don't understand it well enough.

#### 32.4: Keys to Effective Comments

* If you find yourself adding explanatory comments because the code is tricky, consider improving the code instead.
* Good comments summarize blocks of code, or explain its intent, focusing on the problem rather than the solution.
* If commenting is time-consuming, either change your commenting style, or simplify the code so that it's easier to comment.
* Writing comments after the code takes more time because you can't just write down what you’re already thinking about.
* If commenting interrupts your thinking when writing code, design in pseudocode first and then convert the pseudocode to comments.

#### 32.5: Commenting Techniques

* Endline comments either repeat the code, or are too constricted to say anything meaningful, so mostly avoid them.
* To comment a code block at its level of intent, think about what you would name a method that that did the same thing.
* You'll often have a broad comment at the top of the loop and more detailed comments about the operations inside.
* Document surprises, such as optimizations and workarounds for errors or undocumented features.
* If some code is tricky to you, it will be incomprehensible to someone else. It's bad code and should be rewritten.
* To improve the chances that a comment is updated along with a variable, include the variable name in the comment.
* Heavy method-level commenting discourages programmers from creating new methods, leading to poorly-factored code.
* Method-level comments are far from the code they describe, and so they tend not to be maintained.
* If a method uses an algorithm from a book or magazine, document the volume and page number you took it from.
* Class interface documentation should describe how to use the class, and not details about its inner workings.
* Include authorship information in a top-level comment for other programmers if they need help.

### Chapter 34: Themes in Software Craftsmanship

#### 34.1: Conquer Complexity

* Coding conventions, descriptive variable names, avoiding `goto` statements, and abstraction all reduce complexity.

#### 34.2: Pick Your Process

* The way in which people work together determines whether their abilities are added together or subtracted from each other.
* If you code before designing, then it will be harder to embrace changes in design, because code must be thrown away.

#### 34.3: Write Programs for People First, Computers Second

* Favoring write-time convenience over read-time convenience is a false economy.
* Habits affect all your work, and you can't toggle them at will, so ensure that whatever you're doing is worthy of a habit.

#### 34.4: Program Into Your Language, Not In It

* Just because your programming language supports global variables and `goto` statements doesn't mean you should use them.

#### 34.5: Focus Your Attention with the Help of Conventions

* Conventions save programmers from answering the same questions, or making the same arbitrary decisions, again and again.
* They also convey information concisely, protect against hazards or weaknesses, and add predictability to low-level tasks.

#### 34.6: Program in Terms of the Problem Domain

* Top-level code shouldn't be filled with low-level data or code, but should describe the problem that's being solved.
* If you're working in a low-level language, you should try and create higher layers for yourself to work in.
* Low-level problem domain types are glue between fundamental data structures below and high-level problem domain code above.

#### 34.7: Watch for Falling Rocks

* Part of having good judgment in programming is recognizing a wide array of warning signs, or subtle indications of problems.
* Program in such a way that you create more warnings that cannot be overlooked.

#### 34.8: Iterate, Repeatedly, Again and Again

* Taking several repeated and different approaches produces insight into the problem that's unlikely with a single approach.

#### 34.9: Thou Shalt Rend Software and Religion Asunder

* Blind faith in one method precludes the selectivity needed to find the most effective solutions to programming problems.
* To experiment effectively, you must be willing to change your beliefs based on the results of the experiment.
* Design is a process of carefully planning small mistakes in order to avoid making big ones.

