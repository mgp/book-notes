## Core Data Programming Guide

from https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreData/cdProgrammingGuide.html

### Technology Overview

* Beyond simple persistence and querying, core data offers change tracking and undo support, futures, value validation, UI synchronization, and automatic multi-writer conflict resolution.
* Use it because it's mature, proven, robust, efficient, and integrates well with the development toolchain.

### Core Data Basics

#### Basic Core Data Architecture

* The managed object context, or *context*, provides access to a collection of framework objects, or the *persistence stack*, that mediate between the objects in your application and external data stores.
* Model objects that tie into in the Core Data framework are known as managed objects, and are registered with the context.
* The context tracks changes to these objects, thereby providing undo and redo support. If you save the changes you've made, the context ensures that your objects are in a valid state.
* A fetch request specifies an entity name to retrieve; optionally it can specify a predicate and an array of sort orderings.
* If a context already contains a managed object for an object returned from a fetch, then the fetch results contain that existing managed object.
* A persistent store coordinator presents a façade to the managed object contexts so that a group of persistent stores appears as a single aggregate store. This allows partitioning data between these persistent stores.
* In order to use a SQLite database, Core Data must create and manage the database itself.

#### Managed Objects and the Managed Object Model

* A managed object model is a schema that provides a description of the managed objects.
* A model consists of a collection of entity description objects that serve as metadata, such as its name, as well as its attributes and relationships.

### Managed Object Models

* A managed object mdoel, or `NSManagedObjectModel` instance, describes the schema.
* A model contains `NSEntityDescription` objects that represent its entities. Each may have `NSAttributeDescription` and `NSRelationshipDescription` instances, and the entity may also have fetched properties, represented by `NSFetchedPropertyDescription` instances.
* Transient properties are properties that you define as part of the model, but which are not saved with the entity to the persistent store.
* Optional attributes are discouraged, becuase SQL has special comparison behavior for `NULL` that is unlike Objective-C's `nil`. `NULL` in a database is not the same as `0` or an empty string.
* You can predefine fetch requests and store them in a managed object model as named templates. This is like compiling SQLite statements.

### Using a Managed Object Model

#### Creating and Loading a Managed Object Model

* An `xcdatamodeld` "source" directory is compiled into a `momd` deployment directory, and an `xcdatamodel` "source" file is compiled into a `mom` deployment file.
* If you use Xcode to create a non-document application that uses Core Data, the application delegate includes code to retrieve a data model.

#### Accessing and Using a Managed Object Model at Runtime

* To get the model at runtime, call `persistentStoreCoordinator` on the context and `managedObjectModel` on that, or call `managedObjectModel` on any entity.
* Through the model you can create and access fetch request templates.

### Managed Objects

#### Basics

* You can subclass `NSManagedObject` to provide custom accessor or validation methods, to use non-standard attributes, to specify dependent keys, to calculate derived values, or to implement any other custom logic.
* A managed object is associated with an entity description that provides metadata, and with a managed object context.

#### Properties and Data Storage

* A `NSManagedObject` is a generic container object that efficiently provides storage for the properties defined by its associated `NSEntityDescription` object.
* `NSManagedObject` represents date attributes using `NSDate` objects, and stores times internally as an `NSTimeInterval` value since the reference date.

#### Custom Managed Object Classes

* Core Data dynamically generates efficient attribute accessor methods and relationship accessor methods for properties that are defined in the entity of a managed object's corresponding managed object model.
* For performance reasons, Core Data typically does not copy object values, even if the value class adopts the `NSCopying` protocol.

#### Object Life-Cycle—Initialization and Deallocation

* Core Data "owns" the life-cycle of managed objects. The framework can instantiate, destroy, and resurrect these objects as required.
* To perform additional initialization of a managed object, don't override `init`. Instead, override `awakeFromInsert` or `awakeFromFetch`.
* `awakeFromInsert` is invoked only once, when an object is first created. `awakeFromFetch` is invoked when an object is re-initialized from a persistent store, during a fetch.

#### Validation

* To validate property values, implement methods of the form `validate<Key>:error:`, as defined by the `NSKeyValueCoding` protocol.
* To validate inter-property values, override `validateForUpdate:` and/or related validation methods.

#### Faulting

* A managed object may be a "fault" if its property values have not yet been loaded from the external data stores. Accessing these values "fires" the fault, and thereby loads them.

