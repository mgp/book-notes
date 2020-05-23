## Designing Data-Intensive Applications

by Martin Kleppmann

### Chapter 1: Reliable, Scalable, and Maintainable Applications

* Many applications are data-intensive and not compute intensive, meaning the biggest problems are usually the amount of data, the complexity of data, and the speed at which it is changing.

#### Thinking About Data Systems

* No single tool can meet all data processing and storage needs. Instead work is broken down into tasks that can be performed efficiently on a single tool, and application code stitches those tools together.
* When you create an API in front of several tools, you have created a new, special-purpose data system from smaller, general-purpose components.
* This book focuses on three main concerns: reliability, scalability, and maintainability.

#### Reliability

* Reliability means the system continues to work correctly, even when things go wrong.
* Things that can go wrong are called *faults*, and systems that anticipate faults and can cope with them are called *fault-tolerant* or *resilient*.
* A *fault* is when one component of the system deviates from its spec, whereas a *failure* is when the system as a whole stops providing the required service to the user.
* You cannot reduce the possibility of a fault to zero, and so you must design fault-tolerance mechanisms that prevent faults from causing failures.

##### Hardware Faults

* Hard disks have a MTTF of 10 to 50 years, and so on a storage cluster with 10,000 disks, we should expect on average one disk to die per day.

##### Software Errors

* Systemic errors, such as leap second bugs or cascading failures, are correlated across nodes and so tend to cause more system failures than uncorrelated hardware faults.

##### Human Errors

* Allow quick recovery from human errors, such as making it fast to rollback configuration changes, and to roll out code gradually.
* Set up detailed and clear monitoring, such as performance metrics and error rates.

#### Scalability

* Scalability is the term we use to describe a system's ability to cope with increased load.

##### Describing Load

* Load can be described with a few numbers that we call *load parameters*. The best choice of parameters depends on the architecture of your system.
* For Twitter, the distribution of followers per user is a key load parameter for describing scalability, as it determines the fan-out load.

##### Describing Performance

* You can look at what happens when load increases in two ways:
  * When you increase a load parameter and keep system resources fixed, how is the performance of your system affected?
  * When you increase a load parameter, how much do you need to increase the resources if you want to keep performance unchanged?
* Response time what the client sees, including the time to process the request (the service time) and network delays and queuing delays.
* Latency is the duration that a request is waiting to be handled – during which it is *latent*, awaiting service.
* The mean is not a good metrics to describe the "typical" response time, as it doesn't tell you how many users experienced that delay. Prefer percentiles instead.
* High percentiles of response times, or tail latencies, are important because they directly affect users' experience of the service.
* Reducing response times at high percentiles is difficult because they are easily affected by random events outside your control, and the benefits are diminishing.
* When artificially generating load to test scalability, the client must send requests independently of the response time, or else it will artificially keep the queues shorter than in reality, thereby skewing the measurements.
* Even if you call multiple backend services in parallel, the end-user request must wait for the slowest of the parallel calls to complete.
* Averaging percentiles, such as when combining data from several machines, is mathematically meaningless. The right way of aggregating response time data is to add the histograms.

##### Approaches for Coping with Load

* Scaling up, or vertical scaling, is moving to a more powerful machine. Scaling out, or horizontal scaling, distributes the load across multiple smaller machines.
* An architecture that scales well for a particular application is built around assumptions of operations will be common and which will be rare – the load parameters.
* In an early-stage startup it's more important to be able to iterate quickly on product features than to scale beyond some hypothetical future load.

#### Maintainability

* The majority of the cost of software is not in its initial development, but its ongoing maintenance.
* Three important design principles for maintainability are:
  * Operability: Make it easy for the operations teams to keep the system running smoothly.
  * Simplicity: Make it easy for new engineers to understand the system by removing complexity.
  * Evolvability (or extensibility): Make it easy for engineers to adapt the system to unanticipated use cases as requirements change.

##### Operability: Making Life Easy for Operations

* Good operations can work around the limitations of bad software, but good software cannot run reliably with bad operations.
* A good operations team works to preserve the organization's knowledge about the system, even as individuals come and go.
* Data systems with good operability provide good default behavior but allow overriding, and exhibit predictable behavior to minimize surprises.

##### Simplicity: Managing Complexity

* Making a system simpler does not necessarily mean reducing its functionality; it can also mean removing *accidental complexity*.
* Accidental complexity is complexity that is not inherent in the problem that the software solves, but arises only from the implementation.
* Good abstractions are one of the best tools for removing accidental complexity by hiding implementation details, but finding good abstractions is very hard.

##### Evolvability: Making Change Easy

* Simple and easy-to-understand systems are usually easier to modify than complex ones.

#### Summary

* Functional requirements are what an application should do.
* Nonfunctional requirements are general properties like security, reliability, compliance, scalability, compatibility, and maintainability.

### Chapter 2: Data Models and Query Languages

* Data models are the most important part of developing software, because they affect not only how the software is written, but how we think about the problem that we're solving.
* Layered software abstractions allow different groups of people to work together effectively.

#### Relational Model Versus Document Model

##### The Birth of NoSQL

* The term NoSQL has been retroactively reinterpreted as _Not Only SQL_.

##### The Object-Relational Mismatch

* Impedance mismatch is the disconnect between objects in the application code and the database model of tables, rows, and columns.
* A JSON representation of a document-oriented database has better locality than an equivalent multi-table schema.

##### Many-to-One and Many-to-Many Relationships

* In a document-oriented database, if information is duplicated and that information is changed, then all redundant copies need to be updated. Normalization is the key idea behind removing such duplication.

##### Are Document Databases Repeating History?

###### The relational model

* A key insight of the relational model was that you only need to build a query optimizer once, and then all database clients can benefit from it.

###### Comparison to document databases

* For many-to-one and many-to-many relationships, a related item is referenced by a *foreign key* in the relational model, and a *document reference* in the document model.

##### Relational Versus Document Databases Today

* The document data model offers greater schema flexibility, better performance from locality, and for some applications it is closer to the data structures used by the application.
* The relational model offers better support for joins, and many-to-one and many-to-many relationships.

###### Which data model leads to simpler application code?

* If your application data has a document-like structure (i.e. a tree of one-to-many relationships, where typically the entire tree is loaded at once), then a document model is likely a good idea.
* But if your application uses many-to-many relationships, the document model becomes less appealing.

###### Schema flexibility in the document model

* Document databases are sometimes called *schemaless*, but that's misleading – there is an implicit schema assumed by the application, but not enforced by the database.
* A more accurate term is *schema-on-read* in contrast with *schema-on-write*.
* Schema-on-read is similar to dynamic (runtime) type checking in programming languages, whereas schema-on-write is similar to static (compile-type) type checking.
* In cases where all records are expected to have the same structure, schemas are a useful mechanism for documenting and enforcing a data structure.

###### Data locality for queries

* The locality advantage only applies if you need large parts of the document at the same time.

#### Query Languages for Data

* An imperative language tells the computer to perform certain operations in a certain order.
* In a declarative query language like SQL or relational algebra, you specify the pattern of the data you want, but not *how* to achieve that goal.
* A declarative query language also hides implementation details of the database engine, allowing it to add performance improvements without requiring changes to queries.
* Declarative languages are more amenable to parallel execution because they specify the pattern of the results, but not the algorithm used to determine them.

##### MapReduce Querying

* MapReduce is based on a `map` (or `collect`) and `reduce` (or `fold` or `inject`) functions that exist in many functional languages.
* The `map` and `reduce` functions must be pure, thereby allowing the database to run them anywhere, and to rerun them on failure.

#### Graph-Like Data Models

##### Property Graphs

* A property graph model, each vertex and edge has a collection of properties (i.e. key-value pairs), and each edge has a label to describe the relationship between its vertices.
* To get the set of incoming and outgoing edges for a vertex, you can query the `edges` table by `head_vertex` or `tail_vertex` respectively.

##### Graph Queries in SQL

* In a relational database, you usually know in advance which joins you need in your query.
* In a graph query, you may need to traverse a variable number of edges before you find the vertex you're looking for – i.e. the number of joins is not fixed in advance.

##### Triple-Stores and SPARQL

* In a triple-store, all information is stored in the form of very simple three-part statements: (_subject_, _predicate_, _object_).
* The object is one of two things:
  * A value in a primitive data type, and so the predicate and object are equivalent to the key and value of a property on the subject vertex. (E.g. (_lucy_, _age_, _33_) is like a vertex `lucy` with properties `{"age": 33}`.)
  * Another vertex in the graph, and so the predicate is an edge, the subject is the tail vertex, and the object is the head vertex. (E.g. (_alice_, _married_to_, _bob_) has an edge _married_to_ between _alice_ and _bob_.)

##### The Foundation: Datalog

* Datalog generalizes the triple-store model, writing the triple as _predicate(subject, object)_.
* A Datalog rule applies if the system can find a match for _all_ predicates on the right side of the `:-` operator.
* When a rule applies, it's as though the left side of the `:-` was added to the database, with variables replaced by the values they matched.
