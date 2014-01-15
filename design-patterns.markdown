## Design Patterns: Elements of Reusable Object-Oriented Software

by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612)!*

### Chapter 1: Introduction

* Expert designers know not to solve every problem from first principles, but reuse good solutions. This experience makes them experts.
* A design pattern names, abstracts, and identifies the key aspects of a common design structure that make it useful for creating a reusable object-oriented design.
* The hard part of object-oriented design is decomposing a system into objects, which is difficult because encapsulation, granularity, dependency, flexibility, performance, evolution, reusability, and more come into play.
* When designing, strict modeling of the real world leads to a system that reflects today's realities not but not necessarily tomorrow's. Abstractions are key to making a design flexible.
* Dynamic binding means that invoking a method doesn't commit to a particular implementation until runtime.
* An object's class refers to how it's implemented, while an object's type refers to only its interface.
* Class inheritance is a mechanism for code reuse and representation sharing, while interface inheritance describes when an object can be used in place of another.
* Inheritance's ability to define families of objects with identical interfaces is important because polymorphism depends on it.
* By manipulating objects through the interface of their abstract class, clients remain unaware of the specific types of objects they use, and the classes that implement them. This reduces implementation dependencies.
* Class inheritance is white-box reuse, because parent class internals are visible, while object composition is black-box reuse, because no internal details are visible.
* Class inheritance is directly supported by the programming language, and makes it easy to modify the implementation being reused.
* But class inheritance is fixed at compile-time, and breaks encapsulation, where the subclass becomes strongly coupled to the parent class. The parent class can change and become unsuitable for extension.
* Composition, unlike inheritance, doesn't break encapsulation, and there are fewer implementation dependencies. Also, it helps you keep each class encapsulated and focused on one task.
* Advantage of delegation is that you can compose behaviors at runtime, and change the way they're composed. It's an extreme example of object composition.
* Disadvantages of delegation are that dynamic nature can be hard to understand, and there can be runtime inefficiencies. Only use it when it simplifies, not complicates.
* A design that doesn't take change into account risks major redesign in the future. Unanticipated changes are invariably expensive.

