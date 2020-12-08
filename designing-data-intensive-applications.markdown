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

### Chapter 3: Storage and Retrieval

* There is a big difference between storage engines optimized for transactional workloads and those optimized for analytics.

#### Data Structures That Power Your Database

* *Log* is defined as an append-only sequence of records. It doesn't have to be human-readable, and instead might be binary.
* Well-chosen indexes speed up read queries, but every index slows down writes, because the index also needs to be updated every time data is written.

##### Hash Indexes

* If new and updated values are blindly written to a log, that log can be divided into segments. Compacting a segment keeps only the most recent value for each key, discarding the older ones.
* Since compaction makes segments much smaller (assuming a key is updated several times in one segment), compaction can also merge several segments together.
* A background thread can merge and compact segments. While it happens, the database can still serve reads from old segment files, and write requests to the latest segment file.
* A *tombstone* marks a deleted key. When segments are merged, the tombstone tells the merging process to discard all previous values for the key.
* Data file segments are append-only and otherwise immutable, and so they can be read concurrently by multiple threads.
* Append-only design is advantageous in several ways:
  * Appending and segment merging are sequential write operations, which are generally much faster than random writes.
  * Crash recovery are much simpler if segment files are append-only or immutable, e.g. you avoid crashing while overwriting a value and leaving old and new data spliced together.
  * Merging old segments avoids the problem of data files getting fragmented over time.

##### SSTables and LSM-Trees

* Segment files sorted by key is a format called *Sorted String Table*, or *SSTable* for short.
* This has several advantages over log segments with hash indexes:
  * Merging segments is efficient even if the files exceed available memory, by using an algorithm similar to mergesort.
  * Your in-memory index of keys to file offsets can be sparse, and so the total size of your keys can exceed available memory.
  * If your in-memory index of keys is sparse, reads may scan over several key-value pairs on disk anyway, which can be compressed. This reduces disk space and I/O bandwidth use.

###### Constructing and maintaining SSTables

* Writes update a *memtable*, or an in-memory balanced tree. When its size exceeds some threshold, it can be written to disk as an SSTable file.
* Read requests first query the memtable and then query on-disk segments in order from newest to oldest.
* We can append every update to a log before updating the memtable. The log is not in sorted order, but can be used to restore the memtable after a crash.

###### Making an LSM-tree out of SSTables

* Storage engines based on the principle of merging and compacting sorted files are called LSM (Log-Structured Merge) storage engines.

###### Performance optimizations

* An LSM storage engine can use bloom filters to avoid querying the memtable and all segments for a key that does not exist.
* Since data is stored in sorted order, you can efficiently perform range queries, and because disk writes are essential the LSM-tree can support remarkably high write throughput.

##### B-Trees

* B-trees remain the standard index implementation in almost all relational databases, and many non-relational databases use them too.
* B-trees break down the database into fixed-size *blocks* or *pages* (usually 4KB) and read or write one page at a time. This maps closely to the underlying disk hardware.
* The number of references to child pages in one page of the B-tree is called its *branching factor*, and is typically several hundred.

###### Making B-trees reliable

* To make the database resilient to crashes, many B-tree implementations also persist to an append-only log called the *write-ahead log* (WAL, or *redo log*).
* When the database comes back up after a crash, the write-ahead log is used to restore the B-tree back to a consistent state.

###### B-tree optimizations

* In pages in the interior of a tree, we do not need to store full keys, but only enough information to act as boundaries between key ranges.
* B-trees try to ensure leaf pages are in sequential order on disk. This is easier for LSM-trees, which rewrite large segments of the storage in one go during merging.

##### Comparing B-Trees and LSM-Trees

* LSM-trees are typically faster for writes, whereas B-trees are thought to be faster for reads.

###### Advantages of LSM-trees

* *Write amplification* is where one write results in multiple writes over the database's lifetime, such as when compacting and merging SSTables.
* Write amplification is of concern with SSDs, which can only overwrite blocks a limited number of times before wearing out.
* LSM-trees typically sustain higher write throughput than B-trees because they sometimes have lower write-amplification, and because they sequentially write data.

###### Downsides of LSM-trees

* The bigger the database gets, the more disk bandwidth is required for compaction. But this bandwidth is limited and can increase response times at higher percentiles.
* Relational databases implement transaction isolation using locks on ranges of keys. Because each key exists at exactly one place in a B-tree index, those locks can be attached directly to the tree.

##### Other Indexing Structures

###### Storing values within the index

* With secondary indexes, each index references a location in the *heap file* that belongs to the corresponding row.
* A *clustered index* stores the indexed row directly within an index, to avoid the performance penalty upon reads of hopping from the index to the heap file.
* A compromise between a clustered index and a non-clustered index is a *covering index*, which stores *some* of the table's columns within the index (the index *covers* the query).

###### Multi-column indexes

* Multi-dimensional indexes allow querying several columns at once, which is important for geospatial data. But more commonly indexes like R-trees are used.

###### Full-text search and fuzzy indexes

* In Lucene, the in-memory index is a finite state automaton over the characters in the keys, similar to a trie.
* This automaton can be transformed into a *Levenshtein automaton*, which supports efficient search for words within a given edit distance.

###### Keeping everything in memory

* In-memory databases are faster because they avoid the overheads of encoding in-memory data structures in a form that can be written to disk.
* In-memory databases are not faster because they don't need to read from disk – the operating system caches recently used disk blocks in memory anyway.

#### Transaction Processing or Analytics?

* *Transaction processing* just means allowing clients to make low-latency reads and writes, as opposed to *batch-processing* jobs which only run periodically.
* OLTP, or *online transaction processing*, is the access pattern for "interactive applications" driven by the user's input.
* OLAP, or *online analytic processing*, scans over a huge number of records, reading only a few columns per record, and computes aggregates (e.g. sum, count, or average).
* The separate database on which OLAP queries are run is called the *data warehouse*.

##### Data Warehousing

* *Extract-Transform-Load* or ETL is the process of extracting data from the OLTP database, transforming it into an analysis-friendly schema, and loading it into the data warehouse.

###### The divergence between OLTP databases and data warehouses

* OLTP and OLAP are increasingly becoming two separate storage and query engines, which happen to be accessible through a common SQL interface.

##### Stars and Snowflakes: Schemas for Analytics

* Many data warehouses are used in a formulaic style known as *star schema* (also known as *dimensional modeling*).
* The center of the schema is a *fact table* where each row represents an event that occurred at a particular time.
* Columns in the fact table are attributes or foreign keys into *dimension tables*, which represent the	*who*, *what*, *where*, *when*, *why*, and *how* of the event.
* The name *star schema* comes from visualizing the fact table in the middle, connected to its surrounding dimension tables like rays in a star.
* A variation of this is the *snowflake schema*, where dimensions are further broken down into subdimensions.
* In a typical data warehouse, tables are often very wide: fact tables have over 100 columns, and sometimes several hundred.

#### Column-Oriented Storage

* Although fact tables are often over 100 columns wide, a typical data warehouse query only accesses 4 or 5 of them at one time.
* *Column-oriented storage* stores all the values from each column together, instead of all the values from each row together.
* The column-oriented storage layout relies on each column file containing the rows in the same order.

##### Column Compression

* We can convert a column with *n* distinct values into n separate bitmaps, with one bitmap for each distinct value, and one bit for each row.
* If *n* is large, then the bitmaps will be sparse, and the bitmaps can additionally be run-length encoded to make them very compact.
* An `IN` SQL query can be implemented by a bitwise *OR* of bitmaps, while an `AND` SQL query can be implemented by a bitwise *AND* of bitmaps.

###### Memory bandwidth and vectorized processing

* *Vectorized processing* is where operators like bitwise *AND* and *OR* operate on chunks of compressed column data directly.

##### Sort Order in Column Storage

* In column store, the ordering of rows is irrelevant, and so inserting a new row requires simply appending to each of the column files.
* If after choosing an order, the primary sort column does not have many distinct values, then a simple run-length encoding can compress it efficiently.
* Run-length encoding compresses the first sort key best, as subsequent sort keys will not have such long runs of repeated values.

###### Several different sort orders

* In a column store, there normally aren't pointers to data elsewhere, only columns containing values.

##### Writing to Column-Oriented Storage

* Unlike B-trees, which use an update-in-place approach, LSM-trees allow inserting a row with compressed columns without rewriting all the column files.
* When using an LSM-tree, it doesn't matter whether the in-memory store is row-oriented or column-oriented.

##### Aggregation: Data Cubes and Materialized Views

* A materialized view is an actual copy of the query results written to disk, while a virtual view is just a shortcut for writing queries.
* A *data cube* or *OLAP cube* is a materialized view. It is a grid of aggregates grouped by different dimensions.
* The advantage of a materialized data cube is that certain queries become very fast because they have effectively been precomputed.
* The disadvantage is that a data cube doesn't have the same flexibility as querying the raw data.

#### Summary

* Analytic workloads require sequentially scanning across a large number of rows, and so indexes are much less relevant.
* Instead it is more important to encode data compactly, in order to minimize the amount of data that the query needs to read from disk.

### Chapter 4: Encoding and Evolution

* Backward compatibility means newer code can read data written by older code. Forward compatibility means older code can read data written by newer code.
* Forward compatibility is trickier because it requires older code to ignore additions made by a newer version of the code.

#### Formats for Encoding Data

##### Language-Specific Formats

* Using a language's built-in encoding functionality is bad from interoperability, security, efficiency, and versioning perspectives.

##### JSON, XML, and Binary Variants

* JSON distinguishes strings and numbers, but it doesn't distinguish integers and floating-point numbers, and it doesn't specify a precision.
* Base64-encoding a binary string to use it in XML or JSON increases the data size by 33%.
* Simply having different organizations agree on a single format is far more important than the aesthetics or efficiency of a format.

###### Binary encoding

* Because JSON and XML don't prescribe a schema, they need to include all the object field names within the encoded data.
* It's not clear whether the small space reduction by MessagePack, a binary encoding for JSON, is worth the loss of human-readability.

##### Thrift and Protocol Buffers

* Confusingly, Thrift has two binary encoding formats, called *BinaryProtocol* and *CompactProtocol*.
* Field tags are like aliases for fields – they are a compact way of saying what field we're talking about without spelling out its name.
* Variable length integers use the top bit of each byte to specify whether there are still more bytes to come.
* With variable length integers, the numbers -64 to 63 are encoded in one byte, -8192 to 8191 are encoded in two bytes, etc. Bigger numbers use more bytes.
* The `required` keyword of Protocol Buffers enables a runtime check that fails if the field is not set, which is useful for catching bugs.

###### Field tags and schema evolution

* With Thrift and Protocol Buffers, you can change a field name, but changing its tag renders all existing encoded data invalid.
* If old code reads data written by new code, including a new field with an unrecognized tag, it can simply ignore that field. This maintains forward compatibility.

###### Datatypes and schema evolution

* With Protocol Buffers, it's okay to change an `optional` (single-valued) field into a `repeated` (multi-valued) field.

##### Avro

* Avro has two schema languages: Avro IDL is intended for human editing, and a JSON equivalent that is more easily machine-readable.
* An encoded byte string specifies nothing to identify fields or their data types. It simply consists of values concatenated together.
* Consequently the binary data can only be decoded correctly if the code reading the data is using the *exact same schema* as the code that wrote the data.

###### The writer's schema and the reader's schema

* While decoding data, Avro resolves the differences between the *writer's schema* and the *reader's schema*, and translates read data from the former to the latter. This enables schema evolution.

###### Schema evolution rules

* Adding a field that has no default value breaks backward compatibility. Removing a field that has no default value breaks forward compatibility.
* Avro doesn't have `optional` or `required` keywords like Thrift and Protocol buffers, but instead allows defining union types with `null` as a value.

###### Dynamically generated schemas

* Because an Avro schema doesn't define tag numbers, it is friendlier to dynamically generated schemas, e.g. creating an Avro schema from a relational database schema.

##### The Merits of Schemas

* Database vendors provide a driver (e.g. using the ODBC or JDBC APIs) that decode responses from a database's network protocol into in-memory data structures.
* Schema evolution allows the same flexibility as schemaless/schema-on-read JSON databases, while also providing better guarantees about your data and better tooling.

#### Modes of Dataflow

##### Dataflow through Databases

* In an environment where the application is changing, it's likely that some processes accessing the database will be running newer code and some will be running older code.
* Consequently a value in the database might be written by a newer version of the code, and then later read by an older version. And so forward compatibility is required.
* If an application decodes a database value into model objects, and then later re-encodes those model objects, the unknown fields may be lost in that translation process.

###### Different values written at different times

* Data outlives code: While deploys may replace older code with newer code within minutes, data in the database may have last been encoded and written years ago.
* Rewriting (migrating) data into a new schema is possible, but it is expensive on a large data set, and so most databases avoid it if possible.
* Schema evolution allows a database to appear as if it was encoded with a single schema, although the underlying storage may contain records encoded with various historical schema versions.

##### Dataflow Through Services: REST and RPC

* The application-specific APIs of services provide encapsulation, by restricting what clients can and cannot do.
* A key design goal of service-oriented/microservices architecture is to make the application easier to change and maintain by making services independently deployable and evolvable.

###### Web services

* The two popular approaches to web services are REST and SOAP.
* REST emphasizes data formats, using URLs for identifying resources and using HTTP features for cache-control, authentication, and content-type negotiation.
* SOAP is an XML protocol for API requests, and comes with a sprawling set of related standards (the *web standards framework*, or *WS-*) that add various features.
* The API of a SOAP web service is described using an XML-based language called the Web Services Descriptive Language, or WSDL. WSDL enables code generation so that a client can access a remote service using local classes and method calls.
* WSDL is not designed to be human-readable, and SOAP messages are often too complex to construct manually, so tooling, code generation, and IDEs fill in the gaps.
* Despite the ostensible standards, interoperability between different vendors' implementations often causes problems in practice.

###### The problems with remote procedure calls (RPCs)

* The RPC model tries to make a request to a remote service look the same as calling a method within the same process – an abstraction called *location transparency*.
* Variable latency, timeouts, retries and idempotence semantics, and serialization all mean there's no point in location transparency. Calling a remote service is fundamentally different.

###### Current directions for RPC

* The new generation of RPC frameworks is more explicit about the fact that a remote request is different from a local function call.
* RESTful APIs have the advantage of being good for experimentation and debugging, support by all mainstream programming languages and platforms, and a vast ecosystem of available tools.
* For these reasons, REST is used for public APIs, while RPC frameworks mostly focus on requests between services owned by the same organization.

###### Data encoding and evolution for RPC

* Between services using RPC, we assume servers will be updated first, and clients second. You therefore need backward compatibility on requests, and forward compatibility on responses.
* The backward and forward compatibility properties of an RPC scheme are inherited from whatever encoding it uses, such as Thrift or gRPC.

##### Message-Passing Dataflow

* In *asynchronous message-passing* systems, messages are delivered to an intermediary called a *message broker* which stores the message temporarily.
* Message brokers can act as a buffer if the recipient is unavailable or overloaded, redeliver messages to crashed processes, deliver messages to multiple recipients, and decouple producers and consumers.

###### Message brokers

* A process sends a message to a named *queue* or *topic*, and the broker ensures delivery of the message to one or more *consumers* or *subscribers* of that queue or topic.

###### Distributed actor frameworks

* In the *actor model*, logic is encapsulated by actors that communicate via asynchronous messages, where delivery of each message is not guaranteed (even within the same process).
* In a *distributed actor framework*, location transparency works better than with RPC, because the actor model already assumes that messages may be lost.
* A distributed actor framework essentially integrates a message broker and the actor programming model into a single framework.

### Chapter 5: Replication

* Reasons to replicate data include reducing access latency by moving data geographically close to users, increasing availability, and increasing read throughput.
* All the difficulty in replication lies in handling *changes* to replicated data.

#### Leaders and Followers

* *Leader-based replication*, or *active/passive replication*, is the most common solution to ensuring that data is persisted to all replicas.
* Whenever the leader writes new data to its local storage, it also sends the data change to each of its followers via a *replication log* or *change stream*.

##### Synchronous Versus Asynchronous Replication

* With *synchronous* replication, the leader waits until a follower has confirmed it received a write before reporting success to the user and making it visible to other clients.
* With *asynchronous* replication, the leader sends the write to a follower, but doesn't wait for a response.
* If a synchronous follower does not respond because of a crash, network fault, etc. then no writes can be processed, and so fully synchronous replication is impractical.
* In practice replication may be *semi-synchronous*, where one of the followers is synchronous and the rest are asynchronous.
* Leader-based replication is often completely asynchronous:
  * As an advantage, if the leader fails and is not recoverable, any writes that have not yet been replicated are lost.
  * As a disadvantage, the leader can continue processing writes, even if all of its followers have fallen behind.

##### Setting Up New Followers

* A new follower must ingest a consistent snapshot of the leader's database, and then consume all updates from the replication log since that time.
* A position in the leader's replication log is called the *log sequence number* in PostgreSQL, and the *binlog coordinates* in MySQL.

##### Handling Node Outages

###### Follower failure: Catch-up recovery

* The follower can connect to the leader and, via the replication log, consume all data changes that occurred while the follower was disconnected.

###### Leader failure: Failover

* A *failover* requires promoting one follower as the new leader, reconfiguring clients to send data to the new leader, and reconfiguring other followers to consume data changes from the new leader.
* Most systems simply rely on a timeout to determine that the leader has failed.
* The best candidate for leadership is usually the replica with the most up-to-date changes from the old leader, to minimize any data loss.
* If asynchronous replication is used, the new leader may not have received all writes from the old leader before it failed. If the former leader later rejoins the cluster after the new leader has processed new writes, it usually discards the un-replicated writes.
* In a *split brain*, two nodes both believe they are the leader. If there is no process for resolving conflicting writes, this leads to data loss or corruption.
* Because failovers are fraught with things that can go wrong, some operations teams prefer to perform them manually.

##### Implementation of Replication Logs

###### Statement-based replication

* Replicating every write request (statement) from a leader to its followers has many edge cases (e.g. non-determinism from `NOW()` or `RAND()`) and is not preferred.

###### Write-ahead log (WAL) shipping

* The WAL details which bytes were changed in which blocks. It's thus a poor choice for a replication log, e.g. you cannot run different versions of the database on leaders and followers.

###### Logical (row-based) log replication

* A logical log is a sequence of records describing writes to database tables with row granularity. This is the approach MySQL's binlog uses.
* Logical logs are decoupled from storage engine internals and therefore backward compatible, and are also easier for external applications to parse.

###### Trigger-based replication

* A trigger can log data changes into a separate table for reading by an external process.
* Trigger-based replication has greater overhead than other replication methods and is more error-prone, but offers increased flexibility.


#### Problems with Replication Lag

* If an application reads from an asynchronous follower, it may see outdated information if the follower has fallen behind. This effect is known as *eventual consistency*.
* The *replication lag* to a follower may usually be only a fraction of a second, but given high load or network problems, it can increase to several minutes.

##### Reading Your Own Writes

* *Read-your-writes* consistency is also known as *read-after-write* consistency.
* If data is accessed from multiple devices, you may want to provide a stronger guarantee of *cross-device* read-after-write consistency.
* If your replicas are distributed across different data centers, there is no guarantee that connections from different devices will be routed to the same data center.

##### Monotonic Reads

* If the first query by a user goes to an up-to-date replica, but the second query goes to a stale replica, then the user may observe data *moving backward in time*.
* *Monotonic reads* ensures this anomaly does not happen. It is a weaker guarantee than strong consistency, but a stronger guarantee than eventual consistency.
* One way of achieving monotonic reads is to ensure that each user always queries data from the same replica, e.g. using the hash of the user ID.

##### Consistent Prefix Reads

* *Consistent prefix reads* guarantees that if a sequence of writes happen in a certain order, then anyone reading those writes will see them appear in the same order.
* In a sharded database, partitions operate independently, and so there is no global ordering of writes. A user's query might return some parts of the database in an older state, and some in a newer state.

##### Solutions for Replication Lag

* When working with an eventually consistent system, it's worth thinking about how the application behaves if replication lag increases to several minutes or hours.
* Pretending that replication is synchronous when it is in fact asynchronous is a recipe for problems later.

#### Multi-Leader Replication

* In a *multi-leader* configuration, or *active-active replication*, there are multiple leaders accepting writes, with each acting as a follower to the other leaders.

##### Use Cases for Multi-Leader Replication

###### Multi-datacenter operation

* You can have a leader in each data center. Within a datacenter, regular leader-follower replication is used; between datacenters, leaders replicate changes to each other.
* With multiple datacenters, the inter-datacenter network delay is hidden from users, which means the perceived performance may be better.
* The biggest downside of multi-leader replication is the need to resolve write conflicts if the same data is concurrently modified in two different datacenters.
* Auto-incrementing keys, triggers, and integrity constraints are problematic, and so multi-leader replication is considered dangerous territory to be avoided if possible.

###### Clients with offline operation

* Clients with offline operation are like multi-leader replication between datacenters, but each device is a "datacenter" and network connections between them are highly unreliable.

##### Handling Write Conflicts

###### Synchronous versus asynchronous conflict detection

* With synchronous conflict detection, you lose the advantage of each replica accepting writes independently, and so you might as well just use single-leader replication.

###### Converging toward a consistent state

* In a multi-leader configuration there is no defined ordering of writes. But the database must resolve each conflict in a *convergent way*, yielding a single final value across all replicas.
* Approaches to achieving convergent conflict resolution include:
  * *Last write wins* (LWW): Append the timestamp to each write, and apply the write with the largest value, discarding the others. This is dangerously prone to data loss.
  * Somehow merge the values together.
  * Record the conflict in an explicit data structure that preserves all information, and write application code to resolve the conflict later.

###### Custom conflict resolution logic

* *Conflict-free replicated data types* (CDRTs) are data structures (e.g. sets, maps, ordered lists, etc) that automatically resolve conflicts in sensible ways.
* *Mergeable persistent data structures* track history explicitly and use a three-way merge function (similar to Git).
* *Operational transformations* are for concurrent editing of an ordered list of items, and are used by Etherpad and Google Docs.

##### Multi-Leader Replication Topologies

* The most general topology is all-to-all in which every leader sends its writes to every other leader.
* The fault tolerance of a more densely connected topology is better because it allows messages to travel along different paths, avoiding a single point of failure.
* One problem with all-to-all topologies is that if some network links are faster than others, some messages can overtake others, e.g. an update arriving before the preceding insert.

#### Leaderless Replication

* Amazon's Dynamo has popularized abandoning the concept of a leader and allowing any replica to directly accept writes from clients.

##### Writing to the Database When a Node is Down

* Writes are sent to all nodes in parallel. To account for a node being down and missing a write, clients read from nodes in parallel, and rely on version numbers to retain the latest value.

###### Read repair and anti-entropy

* With *read repair*, a client reads from several nodes in parallel and then writes the latest value back to any replicas from which it received stale data. This works well for values that are frequently read.
* An *anti-entropy process* runs in the background, comparing data between replicas and fixing stale data. This does not copy writes in any particular order and may be slow to fix differences.

###### Quorums for reading and writing

* If there are *n* replicas, every write must be confirmed by *w* nodes to be considered successful, and we must query at least *r* nodes for each read.
* As long as *w + r > n*, we expect to get an up-to-date value when reading, because at least one of the *r* nodes we're reading from must be up-to-date.
* Reads and writes that obey these *r* and *w* parameters are called *quorum* reads and writes.
* Normally reads and writes are always sent to all *n* nodes in parallel, and *r* and *w* determine how many nodes we wait for.

##### Limitations of Quorum Consistency

* Often *r* and *w* are chosen to be more than *n/2* nodes, because that ensures *w + r > n* while tolerating up to *n/2* (rounded down) node failures.
* If you choose *w + r ≤ n* you are more likely to read stale values, but this configuration allows for lower latency and higher availability.
* Even with *w + r > n* there are edge cases where stale values are returned:
  * If two writes occur concurrently, it is not clear which one happened first. You must merge the concurrent writes.
  * If a write happens concurrently with a read, the write may be reflected only on some of the replicas.
  * If a write failed on some nodes and overall succeeded on fewer than *w* replicas, it is not rolled back on the replicas where it succeeded.

###### Monitoring staleness

* In leader-based replication, the replication lag is computed by subtracting the follower's replication log position from that of the leader.
* In systems with leaderless replication, there is no fixed order in which writes are applied, which makes monitoring difficult.

##### Sloppy Quorums and Hinted Handoff

* Network interruptions may cut off a client from a large number of database nodes, such that a quorum for reads and writes cannot be reached.
* In a *sloppy quorum*, writes and reads still require *w* and *r* successful responses, but those may include nodes not among the *n* "home" nodes for a value.
* *Hinted handoff* is when, upon the network interruption being fixed, any writes that nodes temporarily accepted are sent to the appropriate "home" nodes.
* Even when *w + r > n*, a client cannot be sure it read the latest value for a key, because the latest value may be temporarily written to nodes outside of *n*.

##### Detecting Concurrent Writes

* When concurrently writing to a Dynamo-style database with the same key, events may arrive in a different order at different nodes due to variable network delays and partial failures.

###### Last write wins (discarding concurrent writes)

* If we have some unambiguous way to determine which write is more "recent," and if every write is copied to every replica, then replicas will eventually converge to the same value.
* Concurrent writes do not have a natural ordering, and so *last write wins* (LWW) imposes an ordering by associating each write with a timestamp.
* LWW trades durability for convergence: Several concurrent writes to the same key may be reported as successful to the client, but only one of the writes will survive.
* If losing data is not acceptable, then LWW is a poor choice for conflict resolution.

###### The "happens-before" relationship and concurrency

* An operation A *happens before* another operation B if B knows about A, or depends on A, or builds on A in some way.
* We can simply say that two operations are *concurrent* if neither happens before the other (i.e. neither knows about the other).
* Exact time does not matter: If the network is slow, two operations can occur some time apart but still appear to be concurrent, because the network prevented one operation from being able to know about the other.

###### Capturing the happens-before relationship

* A server can determine whether two operations are concurrent by looking at version numbers – it does not need to interpret the value itself.
* When the server receives a write with a particular version number:
  * It can overwrite all values with that version number or below, since the client must have merged them into the new value.
  * It must keep all values with a higher version number, because those values are concurrent with the incoming write.

###### Merging concurrently written values

* If several operations happen concurrently, clients must clean up afterward by merging the concurrently written *sibling* values.
* Merging sibling values is essentially the same problem as conflict resolution in multi-leader replication.
* Merging sibling values is complex and error prone, while CRDTs can automatically merge siblings in sensible ways.

###### Version vectors

* With multiple replicas, we need to use a version number *per replica* as well as per key, so that we know which values to overwrite and which to preserve as siblings.
* The collection of version numbers from all the replicas is called a *version vector*. It is also sometimes called a *vector clock*, even though they are not the same.

### Chapter 6: Partitioning

* Partitioning enables scalability: By distributing data across many disks, the query load can be distributed across many processors.

#### Partitioning and Replication

* Partitioning is usually combined with replication so that copies of each partition are stored on multiple nodes for fault tolerance.

#### Partitioning of Key-Value Data

* If some partitions have more data or queries than others, then the partitioning is *skewed* and is less effective.
* A partition with a disproportionately high load is called a *hot spot*.

##### Partitioning by Key Range

* When assigning a contiguous range of keys to each partition, the ranges are not necessarily evenly spaced, because your data may not be evenly distributed.
* Certain access patterns can lead to hot spots with key range partitioning, e.g. partitioning by timestamp and then always writing to the partition for today.

##### Partitioning by Hash of Key

* When using a hash function to determine the partition for a given key, the hash function does not need to be cryptographically strong.
* Each partition is responsible for a range of hashes of keys. Assuming the hash function is uniform, the partition boundaries can be evenly spaced.
* Keys that were once adjacent are now scattered across all the partitions, so their sort order is lost.
* In Cassandra, the first part of the key is hashed to determine the partition, while the remaining columns are used as a concatenated index into Cassandra's SSTables.
* Cassandra achieves a compromise between the partitioning strategies: If a query specifies a fixed value for the first column, it can perform an efficient range scan over the other columns.

##### Skewed Workloads and Relieving Hot Spots

* Most systems are not able to automatically compensate for a highly skewed workload, and so it's the responsibility of the application reduce the skew.

#### Partitioning and Secondary Indexes

* The problem with secondary indexes is that they don't map neatly to partitions.

##### Partitioning Secondary Indexes by Document

* Each partition can maintain its own secondary indexes, covering only the documents in the partition. Such a document-partitioned index is called a *local index*.
* Querying such a partitioned database requires a *scatter/gather* operation. Even if querying the partitions in parallel, scatter/gather is prone to tail latency amplification.

##### Partitioning Secondary Indexes by Term

* Rather than each partition having its own secondary index, we can construct a *global index* that covers data in all partitions.
* Such an index is *term-partitioned* because the term you're looking for determines the partition of the index.
* A client needs to query only the partition containing the term it wants, but a write to a single document may update multiple terms and in turn update multiple partitions of the index.
* In practice, updates to global secondary indexes are often asynchronous.

#### Rebalancing Partitions

* The process of moving load from one node to another is called *rebalancing*.

##### Strategies for Rebalancing

###### How not to do it: hash mod N

* The problem with the *mod N* approach is that if the number of nodes *N* changes, then most keys will need to be moved from one node to another.

###### Fixed number of partitions

* We can create more partitions than nodes, and then assign multiple partitions to each node. As nodes are added or removed, this assignment changes.
* In this configuration, the number of partitions does not change, nor does the assignment of keys to partitions.
* By assigning more partitions to nodes that are more powerful, you can force these nodes to take a greater share of the load.

###### Dynamic partitioning

* With *dynamic partitioning*, partitions are split and merged dynamically based on their size, similar to interior nodes in a B-tree.
* An advantage of dynamic partitioning is that the number of partitions adapts to the total data volume.
* Dynamic partitioning can be used with both key range-partitioned data, as well as with hash-partitioned data.

###### Partitioning proportionally to nodes

* With dynamic partitioning, the number of partitions is proportional to the size of the data set. With a fixed number of partitions, the size of each partition is.
* A third option is a fixed number of partitions per node. Since a larger data volume generally requires a larger number of nodes to store, this also keeps the size of each partition stable.

##### Operations: Automatic or Manual Rebalancing

* If not done correctly, rebalancing can overload the network or the nodes and harm the performance of other requests while it is happening.
* If a node is overloaded, then automated rebalancing may lead other nodes to conclude the node is dead and move load away from it. This increases load on the other nodes and the network, potentially causing a cascading failure.
* While fully automated rebalancing can be convenient, it's good to have a human in the loop to prevent operational surprises.

#### Request Routing

* *Service discovery* addresses which node a client should connect to, and is a critical requirement for software accessible over a network with high availability.
* Any component making the routing decision (a random node, a routing tier, or a client) must learn about changes in the assignment of partitions to nodes.
* Many distributed systems rely on a separate coordination service such as ZooKeeper to track this cluster metadata.
* Cassandra and Riak use a *gossip protocol* among the nodes to disseminate any changes in cluster state.
* When a random node or a routing tier makes the routing decision, clients can use DNS to find their IP addresses as their assignment changes slowly.

### Chapter 7: Transactions

* Transactions *simplify the programming model* for applications accessing a database.
* A *safety guarantee* is when the database prevents a potential error and concurrency issue, and so the application does not have to compensate for it.
* Sometimes there are advantages to weakening transaction guarantees or abandoning them entirely, e.g. for higher performance or higher availability.

#### The Slippery Concept of a Transaction

* Transactions are not the antithesis of stability. But like every other technical design choice, they come with trade-offs.

##### The Meaning of ACID

* In practice, one database's implementation of *ACID* does not equal that of another. ACID has become mostly a marketing term.

###### Atomicity

* Atomicity is not about concurrency, but about ensuring that when a transaction is aborted, that no data was changed and the operation can be safely retried.

###### Consistency

* Consistency means that there exist invariants about your data, but it is the application's responsibility to define its transactions so that they preserve consistency.
* Atomicity, isolation, and durability are properties of the database, whereas consistency is actually a property of the application.

###### Isolation

* Isolation means that concurrently executing transactions do not step on each other's toes.
* Isolation is sometimes formalized as *serializability*, where each transaction can pretend like it's the only transaction running, but this carries a large performance penalty.

###### Durability

* Durability means that once a transaction has committed successfully, its written data will not be forgotten, even if there is a hardware fault or the database crashes.
* To provide durability, a database must wait until writing to non-volatile storage or replication has completed before reporting a transaction as committed.

##### Single-Object and Multi-Object Operations

* Atomicity gives all-or-nothing guarantees when writing data in a transaction, and isolation ensures concurrently running transactions don't interfere with each other.

###### Single-object writes

* For a single object in a storage engine, atomicity can be implemented using a log for crash recovery, and isolation can be implemented using a lock on each object.
* Compare-and-set and other single-object operations are not transactions, because they do not allow grouping multiple operations on multiple objects.

###### The need for multi-object transactions

* Use cases where writes to several different objects need to be coordinated include:
  * Inserting several records that refer to one another and ensuring that their foreign keys are valid.
  * Updating denormalized data across several documents together.
  * Updating secondary indexes to reflect a changed value.

###### Handling errors and aborts

* If the database is in danger of violating its guarantee of atomicity, isolation, or durability, then it would rather abandon the transaction than half-finish.

#### Weak Isolation Levels

* Concurrency issues happen when one transaction reads data that is concurrently modified by another, or when two transactions try to concurrently modify the same data.
* Databases try to hide concurrency issues from application developers by providing *transaction isolation*.
* *Serializable* isolation means the database guarantees that transactions have the same effect as if they ran *serially*.

##### Read Committed

* The most basic level of transaction isolation is *read committed*, which guarantees *no dirty reads* and *no dirty writes*.

###### No dirty reads

* *No dirty reads* means when reading from the database, you will only see data that has been committed.
* Any writes by a transaction only become visible to others when that transaction commits, and then all its writes become visible at once.
* No dirty reads means transactions won't see data in a partially updated state, and that a transaction won't see data that is later rolled back.

###### No dirty writes

* *No dirty writes* means when writing to the database, you will only overwrite data that has been committed.
* Instead of allowing overwriting of uncommitted data, databases usually delay the second write until the first write's transaction has committed or aborted.
* If transactions update multiple objects, dirty writes allow interleaving the writes to the multiple objects.

###### Implementing read committed

* Read committed is the default setting in Oracle 11g, PostgreSQL, SQL Server 2012, MemSQL, and many other databases.
* To prevent dirty reads, a transaction must acquire a row-level lock before writing and then relinquish it upon committing or aborting.
* When a transaction updates a object, other transactions read the old value until the transaction commits, at which point they begin reading the updated value.

##### Snapshot Isolation and Repeatable Read

* *Read skew* is an example of a *non-repeatable read*, where a transaction querying an object at different times sees different values because of concurrent updates.
* *Read skew* is considered acceptable under read committed isolation, as the client is indeed reading only committed data.
* *Snapshot isolation* provides a consistent snapshot of the database, so the transaction sees all data that was committed at the start of the transaction.
* Snapshot isolation is a boon for long-running read-only queries such as backups and analytics.
* Snapshot isolation is supported by PostgreSQL, MySQL with the InnoDB storage engine, Oracle, SQL Server, and others.

###### Implementing snapshot isolation

* A key principle of snapshot isolation is that readers never block writers, and writers never block readers. Reads do not require any locks.
* With *multi-version concurrency control*, the database keeps several different versions of a committed object so that various in-progress transactions can see the database state at different points in time.
* When a transaction deletes a row, a garbage collector deletes it later, only when it is certain no transaction can any longer access the deleted data.

###### Visibility rules for observing a consistent snapshot

* When a transaction starts, the database enumerates all the IDs of in-progress transactions, and their writes are ignored even if they subsequently commit.
* Any writes made by transactions with a later transaction ID – which are strictly increasing, and therefore started after the current transaction – are ignored.
* An object is visible if both of the following conditions are true:
  * When the reader's transaction started, the transaction that created the object had already committed.
  * The object is not marked for deletion, or if it is, the transaction that requested deletion had not yet committed when the reader's transaction started.

###### Indexes and snapshot isolation

* With a *copy-on-write* strategy for B-tree indexes, parent pages – up to the root of the tree – are copied and updated to point to the new versions of their child pages.
* With this strategy, any pages that are not affected by a write do not need to be copied, and remain immutable.

###### Repeatable read and naming confusion

* The SQL standard's definition of isolation levels is flawed – it is ambiguous, imprecise, and not an implementation-independent standard.
* No one really knows what repeatable read means: Databases implementing it provide very different guarantees, and most don't satisfy a formal definition.

##### Preventing Lost Updates

* The *lost update* can occur if an application reads some value, modifies it, and writes back the modified value (a *read-modify-write* operation).

###### Atomic write operations

* Atomic operations are usually implemented by acquiring an exclusive lock on the object to update. This technique is known as *cursor stability*.
* Object-relational mapping frameworks make it easy to accidentally perform unsafe read-modify-write operations instead of relying on atomic operations provided by the database.

###### Explicit locking

* The `FOR UPDATE` clause in SQL indicates that the database should take a lock on all rows returned by the query.

###### Automatically detecting lost updates

* PostgreSQL's repeatable read, Oracle's serializable, and SQL Server's snapshot isolation levels automatically detect a lost update and abort the corresponding transaction.

###### Compare-and-set

* An update statement with a `WHERE` clause specifying the old value may not stop a lost update if it reads from an old snapshot while a concurrent write occurs.

###### Conflict resolution and replication

* Locks and compare-and-set operations assume a single up-to-date copy of the data, but multi-leader or leaderless replication does not guarantee this.
* But atomic operations can work well in a replicated context, especially if they are commutative.
* While the *last write wins* (LWW) conflict resolution strategy is prone to lost updates, it is the default in many replicated databases.

##### Write Skew and Phantoms

* *Write skew* generalizes the lost update problem: Two transactions read the same objects, and then concurrently update some of those objects.
* In the special case where different transactions update the same object, you a lost update or a dirty write anomaly, depending on the timing.
* If you cannot use a serializable isolation level to prevent write skew, the second-best option is to explicitly lock rows that the transaction depends on.

###### Phantoms causing write skew

* In all cases of write skew, a `SELECT` query finds rows that match some condition, and then an `UPDATE` statement changes the set of rows matching that condition.
* If the `SELECT` query checks for the *absence* of rows matching a search condition, and the write adds a matching row, then `SELECT FOR UPDATE` returning no rows can't attach locks to anything.
* A *phantom* is when a write in one transaction changes the result of a query in another transaction.
* Snapshot isolation avoids phantoms for read-only queries, but in read-write transactions, phantoms can lead to tricky cases of write skew.

###### Materializing conflicts

* *Materializing conflicts* takes a phantom and turns it into a lock conflict on a concrete set of rows that exist in the database.

#### Serializability

* Serializable isolation is the strongest isolation level, guaranteeing that transactions executing in parallel yield the same result as if they ran serially.
* This means that the database prevents *all* possible race conditions.

##### Actual Serial Execution

* Redis, VoltDB, and more use a single-threaded execution model with all data in RAM, which allows transactions to execute much faster than if they had to load data from disk.

###### Encapsulating transactions in stored procedures

* Almost all OLTP applications keep transactions short by avoiding interactively waiting for a user within a transaction, because we're slow at making up our minds.
* Even after removing the user, an interactive style between the application and database would yield dreadful throughput in a database without concurrency.
* Instead, single-threaded transaction processing requires the application to submit the entire transaction code to the database as a *stored procedure*.

###### Pros and cons of stored procedures

* With stored procedures and in-memory data, executing all transactions on a single thread become feasible.

###### Partitioning

* For applications with high write throughput, the single-threaded transaction processor can become a serious bottleneck.
* If you can ensure that each transaction only reads and writes data within some partition, then each partition to have its own independent transaction processing thread.
* Since cross-partition transactions have additional coordination overhead, they are much slower than single-partition transactions.

##### Two-Phase Locking (2PL)

* Two-phase *locking* sounds very similar to two-phase *commit*, but they are completely different things.
* In 2PL, writers don't just block other writers, but they also block readers. Additionally, readers can block writers.

###### Implementation of two-phase locking

* 2PL is used by the serializable isolation level in MySQL (InnoDB) and SQL Server, and is the repeatable read isolation level in DB2.
* Each object in the database has a lock that can be acquired in *shared mode* or *exclusive mode*:
  * Multiple readers can hold the lock in shared mode simultaneously, but they must wait for any transaction with an exclusive lock.
  * A writer must first acquire the lock in exclusive mode. A transaction that first reads and then writes can upgrade its shared lock to an exclusive lock.
* The "two-phase" name comes from a transaction acquiring locks in the first phase, and then releasing its locks in the second phase (at the end of the transaction).
* The database automatically detects deadlocks betweens transactions and aborts one so that the others can make progress.

###### Performance of two-phase locking

* Transaction throughput and query latency with 2PL is worse than under weak isolation because the locking and exclusive access reduces concurrency.
* Databases running 2PL have unstable latencies and can be very slow at high percentiles if there is contention in the workload.
* Deadlocks happen much more frequently with 2PL, and so clients can waste significant effort retrying transactions repeatedly.

###### Predicate locks

* To prevent phantoms, a *predicate lock* allows locking all objects that match some search condition.
* A predicate lock applies even to objects that don't yet exist in the database, but which might be added in the future (phantoms).

###### Index-range locks

* Checking for matching locks is time consuming, so most databases with 2PL implement a simplified approximation of predicate locking called *index-range locking*.
* It's safe to simplify a predicate by making it match a larger set of objects, as any write that matches the original predicate will also match the approximations.
* If there is no suitable index where a range lock can be attached, the database can fall back to a shared lock on the entire table.

##### Serializable Snapshot Isolation (SSI)

* *Serializable snapshot isolation* (SSI) provides full serializability, but has only a small performance penalty compared to snapshot isolation.
* SSI is the serializable isolation level in PostgreSQL since 9.1, and FoundationDB uses a similar algorithm.

###### Pessimistic versus optimistic concurrency control

* 2PL uses *pessimistic* concurrency control: if anything can go wrong (as indicated by another transaction holding a lock), then a transaction must wait.
* SSI uses *optimistic* concurrency control: a transaction will always forge ahead, but the database will abort it before committing if anything bad happened (i.e. isolation was violated).
* Given enough capacity, and if contention between transactions is not too high, optimistic concurrency control techniques perform better than pessimistic ones.

###### Decisions based on an outdated premise

* Under snapshot isolation, the result of a query before updating may no longer be up-to-date when the transaction commits, because it may have been concurrently modified.
* We say the transaction was based on a *premise*, or a fact that was true at the start of the transaction, but may no longer be true by the end.
* To provide serializable isolation, the database must detect when a transaction may have acted on an outdated premise and consequently abort it.

###### Detecting stale MVCC reads

* When a transaction wants to commit, the database checks whether other transactions have committed writes that it ignored earlier because of MVCC visibility rules. If so, the database aborts the transaction.

###### Detecting writes that affect prior reads

* When a transaction writes to the database, it notifies transactions that recently read the affected data that what they read may no longer be up to date.

###### Performance of serializable snapshot isolation

* Less detailed tracking of reads and writes is faster but less precise, and consequently may lead to more transactions being aborted than necessary.
* The big advantage of SSI over 2PL is that a transaction doesn't need to block waiting for locks held by another transaction, thereby making query latency more predictable and less variable.

#### Summary

* Transactions are an abstraction layer allowing applications to pretend that certain concurrency problems and certain kinds of faults don't exist.
* Only serializable isolation protects against all types of anomalies and race conditions in a database.

### Chapter 8: The Trouble with Distributed Systems

#### Faults and Partial Failures

* An individual computer with good software is either fully functional or completely broken, but not something in between.
* If an internal fault occurs, we prefer to crash completely rather than returning a wrong result, which are confusing and difficult to debug.
* A *partial failure* in a distributed system is where some parts break in an unpredictable way, even though other parts of the system are working fine.
* The possibility of partial failures and their non-determinism is what makes distributed systems hard to work with.

##### Cloud Computing and Supercomputing

* In a system with thousands of nodes, it's reasonable to assume that *something* is always broken.
* We must accept the possibility of partial failure and incorporate fault-tolerance, thereby building a reliable system from unreliable components.
* Suspicion, pessimism, and paranoia pay off in distributed systems, so consider and test for a wide range of possible faults in your environment.

#### Unreliable Networks

* We focus on *shard-nothing distributed systems*, which are a collection of machines connected by a network.
* In this kind of network, one node can send a message to another node, but the node makes no guarantees when it will arrive, or if it will arrive at all.
* If you send a request to another node and don't receive a response, it is impossible to tell why.
* When a timeout occurs, you still don't know whether the remote node got your request or not.

##### Network Faults in Practice

* A *network partition* or *netsplit* is when one part of the network is cut off from the rest due to a network fault.
* Whenever any communication happens over a network, it may fail – there is no avoiding it.

##### Detecting Faults

* Many systems need to automatically detect faulty nodes, but the uncertainty about the network makes it difficult to tell whether a node is working or not.
* Even if TCP acknowledges that a packet was delivered, the application may have crashed before processing it. The client needs a positive response from the application itself to confirm its processing.
* Conversely, if something goes wrong, you have to assume that you will get no response at all.

##### Timeouts and Unbounded Delays

* Prematurely declaring a node as dead is problematic because if it's actually alive and in the middle of performing some action, having another node take over may perform the action twice.
* If the system is struggling with high load, declaring nodes dead prematurely can transfer their load to other nodes, causing a cascading failure.
* Asynchronous networks have unbounded delays and most server implementations cannot guarantee handling a request within some maximum time.

###### Network congestion and queueing

* *Network congestion* is when a packet experiences delay because it is queued up for sending onto a busy network link.
* When a VM is paused while another VM runs in a virtualized environment, it cannot consume data from the network, and so any incoming data is queued by the virtual machine monitor.
* Because UDP does not perform control flow and does not retransmit lost packets, it is a better choice than TCP when delayed data is worthless.
* Queueing delays have an especially wide range when a system is close to its maximum capacity, as long queues build up quickly.
* In multi-tenant environments, a *noisy neighbor* can use a lot of the resources without your knowledge and introduce highly variable network delays.
* Rather than using fixed timeouts, systems can continually measure the distribution of response times and their variability and adjust their timeouts accordingly.

##### Synchronous Versus Asynchronous Networks

* A *synchronous* network with reserved resources for communication has no queueing, and so the maximum end-to-end latency is fixed. We call this a *bounded delay*.

###### Can we not simply make network delays predictable?

* Ethernet an IP are packet-switched protocols, which suffer from queueing and thus unbounded delays in the network.
* Protocols using packet switching is optimized for *bursty traffic*, and dynamically adapt the rate of data transfer to the available network capacity.
* With careful use of *quality of service* (prioritization of packets) and *admission control* (rate limiting senders) we can provide statistically bounded delay.
* There is no "correct" value for timeouts - they need to be determined experimentally.

#### Unreliable Clocks

* The quartz crystal oscillator powering a clock on a node is not perfectly accurate, and may be faster or slower than those of other machines.

##### Monotonic Versus Time-of-Day Clocks

* Modern computers have a *time-of-day clock* and a *monotonic clock*.

###### Time-of-day clocks

* A time-of-day clock returns the current date and time according to some calendar, also known as the *wall-clock time*.
* If such a clock is too-far ahead of the NTP server it's synchronizing with, it can be forcibly reset and jump back to a previous time. This makes it unsuitable for measuring elapsed time.

###### Monotonic clocks

* The name *monotonic clock* comes from the fact that it is guaranteed to always move forward.
* The difference between two monotonic clock values tells you how much time has elapsed, but the *absolute* value of the clock is meaningless.
* NTP allows the monotonic clock to be sped up or slowed down by up to 0.05%, but it cannot cause the clock to jump forward or backward.
* The resolution of monotonic clocks is quite good, as they can measure time intervals in microseconds or less.

##### Clock Synchronization and Accuracy

* Google assumes a drift of 6 ms for a clock that is synchronized every 30 seconds, or 17 seconds for a server that is synchronized once per day.
* If a computer's clock differs too much from an NTP server, it may refuse to synchronize, or the local clock will be forcibly reset.
* The best way for NTP servers to handle leap seconds is to perform the leap second adjustment gradually over the course of the day, a technique known as *smearing*.
* When a VM is paused so that another VM can run, the application experiences the pause as the clock suddenly jumping forward.

##### Relying on Synchronized Clocks

* Incorrect clocks easily go unnoticed. Any bug that results is more likely to be silent and subtle data loss than a dramatic crash.
* Any node who's clock drifts too far from the others should be declared dead and removed from the cluster.

###### Timestamps for ordering events

* Fundamental problems with last write wins given inaccurate clocks include:
  * A node with a lagging clock cannot overwrite values previously written by a node with a fast clock until the clock skew between them has elapsed.
  * Two nodes can independently generate writes with the same timestamp, especially when the clock has only millisecond precision.
* NTP's synchronization accuracy is limited by the network round-trip time, as well as quartz drift.
* *Logical clocks* are based on incrementing counters rather than oscillating quartz crystal and are a safer alternative for ordering events.
* *Physical clocks* are time-of-day and monotonic clocks, which measure actual elapsed time.

###### Clock readings have a confidence interval

* Given quartz drift, don't think of a clock reading as a point in time, but as a range of times within a confidence interval.
* Google's *TrueTime* API in Spanner explicitly returns a confidence interval with the earliest possible and latest possible timestamp.

###### Synchronized clocks for global snapshots

* The most common implementation of snapshot isolation requires a monotonically increasing transaction ID.
* With lots of small, rapid transactions, creating transaction IDs in a distributed system becomes an untenable bottleneck.
* If you have two non-overlapping TrueTime confidence intervals in Spanner, then one definitely happened before the other.
* To ensure transaction timestamps reflect reality, Spanner waits for the confidence interval length before committing a read-write transaction. This ensures that any transaction that may read the data has a non-overlapping confidence interval.
* Google deploys a GPS receiver or atomic clock in each data center to synchronize clocks within 7 ms.

##### Process Pauses

* Reasons for *preempting* a running thread for a long time include:
  * The programming language time might run garbage collection and "stop the world."
  * The hypervisor can switch to a different virtual machine. The CPU time spent in other virtual machines is known as *steal time*.
  * Unexpected disk access, such as the Java class loader lazily loading class files on first use.
  * The server did not disable paging, and so a simple memory access results in a page fault that loads a page from disk into memory.
  * The Unix process being paused by a `SIGSTOP` signal, and so it yields all CPU cycles until it receives a `SIGCONT` signal.
* A node in a distributed system must assume its execution can be paused, during which the rest of the world keeps moving and may even declare the node dead.

###### Response time guarantees

* A *hard real-time* system is one where if the software does not respond by some *deadline*, the entire system may fail.
* In embedded systems, *real-time* means that a system is carefully designed and tested to meet specified timing guarantees in all circumstances.
* Real-time systems may have lower throughput, since they have to prioritize timely responses above all else.

###### Limiting the impact of garbage collection

* If the runtime can warn the application that a node soon requires a GC pause, it can shift traffic to other nodes while the garbage collection runs.

#### Knowledge, Truth, and Lies

* A node in the network cannot *know* anything for sure – it can only make guesses based on messages it does or does not receive via the network.
* In a distributed system, we can state the assumptions we are making about the behavior (the system model) and design the system in a way to meet those assumptions.

##### The Truth is Defined by the Majority

* Many distributed algorithms rely on a *quorum*, where decisions require some minimum number of votes from several nodes in order to reduce the dependence on any single node.
* Most commonly the quorum is an absolute majority of more than half the nodes, as you cannot have two such majorities with conflicting decisions.

###### The leader and the clock

* There can be problems if the majority of nodes have declared some node dead, but that node still believes it is the single node responsible for some thing.

###### Fencing tokens

* With *fencing*, when a lock server grants a lock or lease, it can also return a *fencing token*, or a number that increases every time.
* A resource protected by the lock service can check tokens and reject any requests with an older token than one that has already been processed.
* It's unwise for a service to assume that clients are well behaved, as the people running the clients and those running the service likely have very different priorities.

##### Byzantine Faults

* A *Byzantine fault* is when there is the risk that nodes may "lie," or send arbitrarily faulty or corrupted responses.
* The *Byzantine Generals Problem* imagines there are *n* generals who need to agree, but this is hampered by generals who are traitors.
* A system is *Byzantine fault-tolerant* if it operates correctly even if some nodes are not obeying the protocol, or if attackers are interfering with the network.
* In most server-side data systems, the cost of deploying Byzantine fault-tolerant solutions makes the impracticable.

##### System Model and Reality

* A *system model* is an abstraction that describes what things an algorithm may assume.
* There are three system models for timing assumptions:
  * A *synchronous model* assumes bounded network delay, bounded process pauses, and bounded clock error.
  * A *partially asynchronous model* mostly behaves like the synchronous model, but can sometimes exceed its bounds for network delay, process pauses, and clock drift.
  * An *asynchronous model* does not allow the model to make any timing assumptions, and in fact does not even have a clock.
* There are three system models for node failures:
  * A *crash-stop faults model* assumes a node can fail only by crashing, and once it has crashed it is gone forever.
  * A *crash-recovery faults model* assumes the node can crash but respond at some later time, and that stable storage is preserved while in-memory state is lost.
  * A *Byzantine faults model* assumes nodes can do absolutely anything.
* For modeling real systems, the most useful model is partially asynchronous with crash-recovery faults.

###### Correctness of an algorithm

* An algorithm is *correct* if it always satisfies its *properties* in all situations that we assume may occur in that system model.

###### Safety and liveness

* Safety is often informally defined as *nothing bad happens*, and liveness as *something good eventually happens*.
* If a safety property is violated, we can point to the exact time at which it was broken. At this time the damage is done, and cannot be undone.
* A liveness property may not hold at some point in time, but there is always hope that it may be satisfied in the future.
* For distributed algorithms, it's common to require that safety properties *always* hold. Meaning even if all nodes crash, the algorithm must not return a wrong result.
* With liveness properties we can make caveats. The definition of the partially synchronous model requires that eventually the system returns to a synchronous state.

###### Mapping system models to the real world

* System models help distill the complexity of real systems to a set of faults that we can reason about, so that we can understand the problem and try to solve it systematically.

#### Summary

* If you can avoid opening Pandora's box and simply keep things on a single machine, it's generally worth doing so.
* Distributed sequence number generators like Twitter's Snowflake cannot guarantee that ordering is consistent with causality, because the timescale at which blocks of IDs are assigned is longer than the timescale of database reads and writes.

### Chapter 9: Consistency and Consensus

* The best way of building fault-tolerant systems is to find some general-purpose abstractions with useful guarantees, implement them once, and then let applications rely on them.
* *Split brain* is when nodes believe they are the leader, and it often leads to data loss.

#### Consistency Guarantees

* A better name for eventual consistency may be *convergence*, as we expect all replicas to eventually converge to the same value.
* The edge cases of eventual consistency only become apparent when there is a fault in the system or at high concurrency.
* Transaction isolation is about avoiding race conditions due to concurrently executing transactions. Distributed consistency is about coordinating the state of replicas in the face of delays and faults.

#### Linearizability

* *Linearizability* (or *strong consistency*) is to make a system appear as if there were only one copy of the data, and all operations on it are atomic.
* Linearizability is a *recency guarantee*: As soon as a client completes a write, all clients reading from the database must be able to see the value just written.

##### What Makes a System Linearizable?

* A linearizable system appears as if it has only a single copy of the data.
* A *concurrent read* is a read that overlaps in time with the write operation. Because we don't know whether the write has taken effect when the read operation is processed, it may return either the old value or the new value.
* In a linearizable system there must be some point in time at which a register is updated to its new value, and so all subsequent reads must return the new value even if the write operation has not yet completed.
* It is possible to test whether a system is linearizable by recording the timings of all requests and responses, and checking whether they can be arranged in a valid sequential order.
* Serializability is a isolation property of transactions which guarantees that the transactions behave as if they had executed in some serial order.
* Implementations of serializability based on two-phase locking or actual serial execution are typically linearizable.
* Snapshot isolation is not linearizable because a consistent snapshot does not include writes that are more recent than the snapshot.

##### Relying on Linearizability

###### Locking and leader election

* Electing a leader requires one node successfully acquiring a lock. This system must be linearizable, as all nodes must agree on which node owns the lock.
* Consensus algorithms implement linearizability in a fault-tolerant way, but linearizable storage is the foundation for these coordination tasks.

###### Constraints and uniqueness guarantees

* A hard uniqueness constraint requires linearizability, while constraints like foreign key or attribute constraints do not require linearizability.

###### Cross-channel timing dependencies

* Without the recency guarantees of linearizability, you must be concerned with race conditions between two communication channels.

##### Implementing Linearizable Systems

* The simplest way to implement linearizability is to rely on only a single copy of the data, but that does not tolerate faults.
* Single-leader replication is potentially linearizable if you make all reads from the leader – which assumes you know for sure who the leader is – or from synchronously updated followers.
* Multi-leader replication are generally not linearizable, because they concurrently process writes on multiple nodes and asynchronously replicate them to other nodes.
* Leaderless replication systems with LWW conflict resolution or sloppy quorums are not linearizable.
* Quorums where *w + r > n* can be linearizable if readers perform read repair synchronously before returning results, and if writers read the latest quorum state before sending writes.
  * Only linearizable read and write operations can be performed in this way; a linearizable compare-and-set operation cannot, because it requires a consensus algorithm.

##### The Cost of Linearizability

* The *CAP theorem* states that applications that don't require linearizability can be more tolerant of failures:
  * If your application requires linearizability, then a disconnected replica is *unavailable* and must either wait until the network is fixed, or return an error.
  * If your application doesn't require linearizability, then a replica can remain *available* and process requests even if disconnected.
* The CAP theorem encouraged database engineers to explore a wider design space of distributed shared-nothing systems, which were more suitable for implementing large-scale web services.
* CAP is sometimes presented as *Consistent, Available, Partitioned – pick 2* but partitions are a kind of fault and so they will happen whether you like it or not.
* A better way of presenting CAP is *Consistent or Available when Partitioned*, as you must choose linearizability or total availability when a network fault occurs.
* CAP has little practical value for designing systems as it doesn't say anything about network delays, dead nodes, or other trade-offs.

###### Linearizability and network delays

* The response times of linearizable reads and writes is very high in a network with highly variable network delays, and so weaker consistency models can be much faster.

#### Ordering Guarantees

* The leader in single-leader replication determines the *order of writes* in the replication log, while serializability ensures that transactions behave as if executed in *some sequential order*.

##### Ordering and Causality

* Ordering is recurring because it helps preserve *causality*. Cases where causality is important include:
  * Snapshot isolation provides a snapshot consistent with causality: The effects of all operations that happened causally before that point in time are visible, but no operations that happened causally afterward are seen.
  * Serializable snapshot isolation detects write skew by tracking the causal dependencies between transactions.
* A *causally consistent* system obeys the ordering imposed by causality, where chains of causally dependent operations define the causal order in the system.

###### The causal order is not a total order

* In a linearizable system, we have a *total order* of operations: For any two operations, we can always say which happened first.
* Other consistency models offer a *partial order*: Two events are ordered if they are causally related, but they are incomparable if they are concurrent.
* There are no concurrent operations in a linearizable datastore, as there must be a single timeline along which all operations are totally ordered.
* Concurrency means that the timeline branches and merges again, and that operations on different branches are incomparable (i.e. concurrent).

###### Linearizability is stronger than causal consistency

* Linearizability ensures causality, which is what makes linearizable systems simple to understand and appealing.
* Causal consistency is the strongest consistency model that does not slow down due to network delays, and remains available in the face of network failures.

###### Capturing causal dependencies

* Concurrent operations may be processed in any order, but if one operation happened before another, then they must be processed in that order on every replica.
* Causal consistency must track causal dependencies across the entire database. And to determine the causal ordering, the database must know which version of the data was read by the application.

##### Sequence Number Ordering

* Causality is an important theoretical concept, but actually keeping track of all causal dependencies can become impracticable.
* Monotonically increasing *sequence numbers* can create a total ordering that is consistent with causality: If operation A happened before operation B, then A occurs before B in the total order.

###### Noncausal sequence number generators

* Multiple leaders independently generating sequence numbers will not correctly capture the ordering of operations across different nodes, and are not consistent with causality.

###### Lamport timestamps

* If each node has a unique identifier and a count of the number of operations it has processed, its Lamport timestamp is the pair *(counter, node ID)*.
* Each node and client maintains the maximum counter value it has seen so far and includes it on every request. When a node receives a request or response with a maximum counter value greater than its own counter value, it immediately increases its own counter to that maximum.
* The ordering from the Lamport timestamps is consistent with causality, because every causal dependency results in an increased timestamp.
* Version vectors can distinguish whether two operations are concurrent or whether one is casually dependent on the other, whereas Lamport timestamps enforce a total ordering.

###### Timestamp ordering is not sufficient

* To implement something like a uniqueness constraint for usernames, it's not sufficient to have a total ordering of operations - you also need to know when that ordering is finalized.

##### Total Order Broadcast

* *Total order broadcast* or *atomic broadcast* is the problem of how to scale the system if the throughput is greater than a single leader can handle, and how to handle failover if the leader fails.
* Total ordering across all partitions in a partitioned database is possible, but it requires additional coordination.
* Total order broadcast is a protocol for exchanging messages between nodes and requires two safety guarantees: reliable delivery and totally ordered delivery.

###### Using total order broadcast

* Consensus services such as ZooKeeper and etcd actually implement total order broadcast.
* *State machine replication* is the use of total order broadcast for database replication: Every replica processes every write in the same order.
* Total order broadcast can be framed as creating a *log*, where delivering a message is like appending to the log.
* ZooKeeper uses total order broadcast to implement a lock service: Every request to acquire the lock is appended as a message to the log, and all messages in the log are sequentially ordered. The sequence number, or `zxid`, serves as a fencing token.

###### Implementing linearizable storage using total order broadcast

* You can build linearizable storage on top of total order broadcast by appending a message for each write or read, and then processing the write or read upon reading the message from the log.

###### Implementing total order broadcast using linearizable storage

* For every message you want to send through total order broadcast, increment-and-get the linearizable integer, and then attach the register value as a sequence number to the message.
* It can be proved that a linearizable compare-and-set (or increment-and-get) register and total order broadcast are both *equivalent to consensus*.

#### Distributed Transactions and Consensus

* Consensus is needed for leader election, as all nodes need to agree on which node is the leader.
* In the *atomic commit problem*, all nodes participating in a distributed transaction need to agree on the outcome of the transaction (either all aborting or all committing).

##### Atomic Commit and Two-Phase Commit (2PC)

* Atomicity ensures that a secondary index stays consistent with the primary data.

###### From single-node to distributed atomic commit

* The deciding moment for whether a transaction commits or aborts is the moment at which the disk finishes writing the commit record: After this moment, the transaction is committed.
* Once a transaction has been committed on one node, it cannot be retracted if it later turns out that it was aborted on another node.
* If a transaction was allowed to abort after committing, it would violate *read committed isolation*, and any transactions that read the committed data would be based on data that was retroactively declared not to have existed.

###### Introduction to two-phase commit

* 2PC (two-phase commit) provides atomic commit in a distributed database, whereas 2PL (two-phase locking) provides serializable isolation.
* 2PC requires a *coordinator*, or *transaction manager*, which is often implemented as a library within the same application process that is requesting the transaction.
* When an application is ready to commit, the coordinator begins phase 1, sending a *prepare* request to every node asking if it is able to commit.
* If all nodes reply "yes" then the coordinator sends a *commit* request in phase 2 and the commit actually takes place.

###### A system of promises

* By replying "yes" to the coordinator, a node promises to commit the transaction without error if requested.
* The *commit point* is when the coordinator writes to its transaction log on disk whether it will go ahead with phase 2 or abort, so that its decision is preserved in case it subsequently crashes.
* If the decision was to commit, then that decision must be enforced, no matter how many retires it takes.

###### Coordinator failure

* After a node has voted "yes" to a prepare request, it must wait to hear from the coordinator whether to commit or abort.
* If a node replies to a prepare request and then the coordinator crashes or the network fails, the node's transaction state is called *in doubt* or *uncertain*.
* Upon the coordinator recovering from a crash, it computes the status of all in-doubt transactions by reading its transaction log, and aborts any without a commit record.

##### Distributed Transactions in Practice

* Distributed transactions are criticized for causing operational problems, killing performance, and promising more than they can deliver.
* The performance cost inherent in 2PC is due to the additional disk forcing (`fsync`) that is required for crash recovery, and the additional network round-trips.
* Database-internal distributed transactions can often work quite well, while transactions spanning heterogeneous technologies are a lot more challenging.

###### XA transactions

* X/Open XA (for eXtended Architecture) is a C API for interfacing with a transaction coordinator. It is supported by many traditional databases and message brokers.
* In practice the transaction coordinator implementing the XA API is a library loaded into the same process as the application issuing the transaction.

###### Holding locks while in doubt

* Database transactions take a row-level exclusive lock on any rows they modify, and serializable isolation requires acquiring a shared lock on any rows read by the transaction.
* Using 2PC, a transaction must hold onto locks throughout the entire time it is in doubt, which can cause large parts of your application to be unavailable until the in-doubt transaction is resolved.

###### Recovering from coordinator failure

* Orphaned in-doubt transactions do occur, which hold locks and block other transactions until they are either manually committed or rolled back by an administrator.

###### Limitations of distributed transactions

* When the transaction coordinator is part of the application server, the application servers are no longer stateless because the coordinator logs are a critical part of the durable system state.
* Distributed transactions tend to *amplify failures*, because if any part of the system is broken then the distributed transaction also fails.

##### Fault-Tolerant Consensus

* Consensus is where one or more nodes may *propose* values, and a consensus algorithm *decides* on one of those values.
* The algorithm must have uniform agreement (no two nodes decide differently), integrity (no node decides twice), validity (a node can decide only on a proposed value), and termination (every node that does not crash eventually decides a value).
* The termination property formalizes the idea of fault tolerance, as the consensus algorithm must make progress.
* Termination is a liveness property, whereas uniform agreement, integrity, and validity are all safety properties.
* Any consensus algorithm requires at least a majority of nodes to be functioning correctly in order to assure termination. This majority can safely form a quorum.
* A large-scale outage can stop the consensus system from being able to process requests, but it cannot corrupt the system by causing it to make invalid decisions.

###### Consensus algorithms and total order broadcast

* Total order is equivalent to repeated rounds of consensus: In each round, nodes propose the message that they want to send next, and then decide on the next message to be delivered in the total order.

###### Epoch numbering and quorums

* Consensus protocols like Raft and Paxos define an *epoch number*, and guarantee that within each epoch the leader is unique.
* Every time the leader is thought to be dead, a vote is started among the nodes to elect a new leader. This election is given an incremented epoch number.
* The quorums for voting for a leader and for voting on a leader's proposal must overlap. So if a vote on a proposal succeeds, then at least one of the nodes that voted for it must have also participated in the most recent leader election.
* If the vote on a proposal doesn't reveal any higher-numbered epoch, the current leader can conclude that no leader election with a higher epoch number has happened, and so it is still the leader.

###### Limitations of consensus

* The process by which nodes vote on proposals before they are decided is a kind of synchronous replication.
* Most consensus algorithms assume a fixed set of nodes that participate in voting, meaning you cannot add or remove nodes in the cluster.
* In geographically distributed systems, nodes can falsely believe the leader to have failed because of a transient network issue, leading to frequently leader elections that degrade performance.

##### Membership and Coordination Services

* ZooKeeper and etc are designed to hold small amounts of data that fit entirely in memory, which is replicated across all nodes using a fault-tolerant total order broadcast algorithm.
* Using an atomic compare-and-set operation, you can implement a lock in ZooKeeper: If several nodes concurrently try to perform the same operation, only one will succeed.
* ZooKeeper provides a monotonically increasing fencing token by totally ordering all operations and giving each one a monotonically increasing transaction ID (`zxid`) and version number (`cversion`).

###### Allocating work to nodes

* Leader election and assigning partitioned resources to nodes can be achieved by judicious use of atomic operations, ephemeral nodes, and notifications in ZooKeeper.
* ZooKeeper provides a way of "outsourcing" some of the work of coordinating nodes (consensus, operation ordering, and failure detection) to an external service.

###### Service discovery

* ZooKeeper, etc, and consul are often used for *service discovery*, which returns the set of IP addresses that are running a given service.
* Replicas that asynchronously receive the log of all decisions of the consensus algorithm but don't participate in the voting can serve read requests that don't need to be linearizable.

### Chapter 10: Batch Processing

* There are three types of systems:
  * Services (online systems), where servers wait for a request to arrive and then attempt to send a response back as quickly as possible. Availability and response time are most important metrics.
  * Bach processing (offline systems), where a job processes some large amount of input data to create output data. Throughput is the most important metric.
  * Stream processing (near-realtime systems), where a stream job operates on events shortly after they happen.

#### Batch Processing with Unix Tools

##### Simple Log Analysis

###### Sorting versus in-memory aggregation

* If data to sort exceeds available memory, chunks can be sored in memory and written to disk as segment files, and then multiple sorted segments can be merged together into a larger sorted file.
* The `sort` utility handles larger-than-memory datasets by spilling to disk, and automatically parallelizes sorting across multiple CPU cores.

##### The Unix Philosophy

* The philosophy described in 1978 says: Make each program do one thing well, and expect the output of every program to be the input to another, as yet unknown, program.
* A Unix shell like `bash` allows us to easily *compose* small programs into surprisingly powerful data processing jobs.

###### A uniform interface

* In Unix, all programs use a file descriptor as the input/output interface. That file is just an ordered sequence of bytes.

###### Separation of logic and wiring

* Pipes allow you to attach the `stdout` of one process to the `stdin` of another process with an in-memory buffer, without writing all intermediate data to disk.
* Separating the input/output wiring from the program logic makes it easier to compose small tools into bigger systems.

###### Transparency and experimentation

* Unix tools are successful in part because it is easy to see what is going on:
  * The input files are normally treated as immutable, and so you can run the commands repeatedly.
  * You can pipe the output into `less` and look to see if it has the expected form at any point.
  * By writing the output of one stage to a file and using that file as the input to the next stage, you can restart the later stage without re-running the entire pipeline.

#### MapReduce and Distributed Filesystems

* Hadoop Distributed File System (HDFS) is based on the shard-nothing principle requiring no special hardware, only computers connected by a conventional network.
* HDFS consists of a daemon process running on each machine for I/O, and a central server called NameNode to track which file blocks are stored on which machine.

##### MapReduce Job Execution

* The *mapper* of a MapReduce job can generate any number of key-value pairs for a given input.
* The *reducer* processes the collection of values for a given key and can produce output records.
* The role of the mapper is to prepare the data by putting it in a form that is suitable for sorting, and the role of the reducer is to process the data that has been sorted.

###### Distributed execution of MapReduce

* The mapper and reducer do not know where each input record is coming from or output record is going to, so the framework can handle those complexities.
* The MapReduce framework *puts computation near the data* by attempting to run each mapper on one of the machines that stores the record of the input file.
* While the number of map tasks is determined by the number of input file blocks, the number of reduce tasks is configured by the job author.
* Each map task partitions its output by reducer, based on the hash of the key. Each of these partitions is written to a sorted file on the mapper's local disk.
* The reducers download the files of sorted key-value pairs for their partition. The process of partitioning by reducer, sorting, and copying data partitions from mappers to reducers is known as the *shuffle*.
* When the reduce task merges the files from the mapper it preserves their sorted order, and so the values are iterated over in sorted order.

###### MapReduce workflows

* It's common for MapReduce jobs to be chained together into *workflows*, where the output of one job becomes the input to the next job.
* Chained MapReduce jobs are less like pipelines of Unix commands and more like a sequence of commands where each command's output is written to a temporary file, which the next command reads from.

##### Reduce-Side Joins and Grouping

* Associations between records include a *foreign key* in a relational model, *document reference* in a document model, or an *edge* in a graph model.
* A join is necessary whenever you have some code that needs to access records on both sides of that association.
* In the context of batch processing, a join means resolving _all_ occurrences of some association within a data set, and is not limited to a subset of records.

###### Example: analysis of user activity events

* To achieve good throughput in a batch process, computation must be (as much as possible) local to one machine. Requests over the network on a per-record basis is too slow.
* When data must be joined together, it should be co-located on the same machine.

###### Sort-merge joins

* A *secondary sort* ensures that a reducer first processes all records on one side of the association first, and then all records on the other side.
* A sort-merge join has its name because mapper output is sorted by key, and the reducers then merge together the sorted lists of records from both sides of the join.

###### Bringing related data together in the same place

* Having lined up all the data in advance, the reducer can be a simple, single-threaded piece of code that churns through records with high throughput and low memory overhead.
* Because MapReduce separates the physical network communication aspects of computation from the application logic, it can transparently retry failed tasks without affecting the application logic.

###### GROUP BY

* The simplest way of implementing `GROUP BY` in MapReduce is to configure the mappers so that all produced key-value pairs use the desired grouping key, and then aggregate them in the reducer.

###### Handling skew

* Keys with a disproportionate number of values, or *hot keys*, can lead to *skew* or *hot spots* where one reducer must process significantly more records than the others.
* This is problematic because any subsequent jobs must wait for the slowest reducer to complete before they can start.
* To aggregate values for a hot key, a first stage of aggregation can be distributed across multiple reducers, and then a second stage combines those values to create a single value for the key.

##### Map-Side Joins

* A *reduce-side join* where join logic belongs to the reducers is flexible, but all that sorting, copying to reducers, and merging of reducer inputs can be expensive.
* A *map-side join* makes assumptions about the size, sorting, and partitioning of input data, but removes the needs for reducers and sorting.

###### Broadcast hash joins

* In a *broadcast hash join*, all the data on one side of the association is loaded into memory and is used directly by the mapper.
* The term *broadcast* refers to how the small input is "broadcast" to all partitions of the large input, and *hash* refers to its use of a hash table.

###### Partitioned hash joins

* In a *partitioned hash join* all records you might want to join belong to the same partition, and so each mapper can read only one partition from each of the input data sets.
* This only works if both of the join's inputs have the same number of partitions, with records assigned to partitions based on the same key and the same hash function.

###### Map-side merge joins

* If the input data sets are not only partitioned in the same way but also sorted by the same key, the mapper can perform the merging operation normally done by a reducer.

###### MapReduce workflows with map-side joins

* The output of a reduce-side join is partitioned and sorted by the join key, whereas the output of a map-side join is partitioned and sorted in the same way as the large input.

##### The Output of Batch Workflows

* Batch processing is closer to analytics in that it typically scans over large portions of an input data set, but its output is often not a report but some other structure.

###### Key-value stores as batch process output

* To persist the output of a batch process into a database for querying, writing one record at a time to the database from the mapper or reducer is a bad idea:
  * Making a network request for every single record is orders of magnitude slower than the normal throughput of a batch task.
  * If all mappers or reducers concurrently write to the same database, then that database can become overwhelmed.
  * Partially completed jobs may be visible to other systems, while MapReduce provides a clean all-or-nothing guarantee for job output.
* A better solution is to create a new database inside the batch job and write it as files to the job's output directory in the distributed filesystem.

###### Philosophy of batch process outputs

* By treating inputs as immutable and avoiding side-effects, batch jobs not only achieve good performance but also become easier to maintain.
* The idea of being able to recover from buggy code has been called *human fault tolerance*.
* The automatic retry of a map or reduce task is safe only because inputs are immutable and outputs from failed tasks are discarded by the MapReduce framework.

##### Comparing Hadoop to Distributed Databases

* Hadoop is somewhat like a distributed version of Unix, where HDFS is the filesystem and MapReduce is a quirky implementation of a Unix process.

###### Diversity of storage

* In practice, making data available quickly – even if in a difficult-to-use, raw format – is more valuable than trying to decide on the ideal data model up front.
* A "data lake" or "enterprise data hub" collects data in its raw form, allowing its collection to be expedited.
* Instead of forcing the producer of a dataset to bring it into a standardized format, the interpretation of the data becomes the consumer's problem.
* The _sushi principle_ says raw data is better, as dumping the raw data allows a consumer to transform the data into whatever form is ideal for its purposes.

###### Diversity of processing models

* MapReduce is too limiting or performs too badly for some types of processing, and so various other processing models have been developed on top of Hadoop.
* These various processing models can all be run on a single shared-use cluster of machines, all accessing the same files on the distributed system.
* The Hadoop ecosystem includes random-access OLTP databases such as HBase, and MPP (Massively Parallel Processing) databases such as Impala.

###### Designing for frequent faults

* Unlike MPP databases, MapReduce can tolerate the failure of a MapReduce map or reduce task by retrying it, and it eagerly writes data to disk for fault tolerance and in case the data cannot fit entirely in memory.
* MapReduce was designed to tolerate frequent faults because MapReduce jobs at Google were run at low priority and could be preempted at any time by higher-priority processes requiring their resources.

#### Beyond MapReduce

* MapReduce is robust, but other tools are sometimes orders of magnitude faster for other types of processing.

##### Materialization of Intermediate State

* The process of writing out intermediate state to files is called *materialization*.
* Pipes in Unix do not fully materialize the intermediate state, but instead *stream* one command's output to the next command's input using a small in-memory buffer.
* Unlike Unix pipes, MapReduce fully materializing intermediate state has downsides:
  * Having to wait until all of the preceding job's tasks have completed slows down the execution of the workflow as a whole.
  * Mappers are often redundant because they just read back the same file that was just written by a reducer, and prepare it for the next stage of partitioning and sorting.
  * Storing intermediate state in a distributed filesystem replicates it across several nodes, which is overkill for temporary data.

###### Dataflow engines

* *Dataflow engines* model the flow of data through several processing stages. The user-defined functions for operating on the data are called *operators*.
* Advantages over the MapReduce model include:
  * Expensive work like sorting is only done in places where it is required, rather than always happening between every map and reduce stage.
  * There are no unnecessary map tasks, since the work done by a mapper can often be incorporated into the preceding reduce operator.
  * Because all joins and data dependencies are explicitly declared, the scheduler can make locality optimizations.
  * Intermediate state between operators can be kept in memory or written to local disk, which requires less I/O than writing it to HDFS.
  * Operators can start executing as soon as their input is ready instead of waiting for the entire preceding stage to finish.

###### Fault tolerance

* If a machine fails and the intermediate state on a machine is lost, the dataflow engine can recompute it from the preceding stages.
* If a computation is not deterministic, then the original data is not the same as the lost data, and downstream operators must reconcile the contradictions between the old and new data.
* The solution to such nondeterministic operators is to kill the downstream operators as well, and run them again on the new data.
* If the intermediate data is much smaller than the source data, or if the computation is very CPU-intensive, it's cheaper to materialize the intermediate data than to recompute it.

###### Discussion of materialization

* While materialized datasets on HDFS are still usually the inputs and outputs of a job, the dataflow engine saves the job from writing all intermediate state to the filesystem.

##### Graphs and Iterative Processing

* Many graph algorithms traverse one edge at a time, joining one vertex with an adjacent one to propagate some information, and repeating until some condition is met like no more edges to follow, or some metric converging.

###### The Pregel processing model

* The *Pregel* model, or *bulk synchronous processing model* (BSP), is a popular optimization for batch processing graphs.
* One vertex can "send a message" to another vertex, usually along an edge in a graph – similar to how mappers can conceptually send messages to reducers by key.
* In each iteration, a function is called for each vertex, passing the function all the messages that were sent to that vertex – again like a reducer.

###### Fault tolerance

* Pregel implementations guarantee that messages are processed exactly once at their destination vertex in the following iteration.

###### Parallel execution

* Graph algorithms often have a lot of cross-machine communication overhead, as the intermediate state is often bigger than the original graph.
* If your graph can fit in memory on a single computer, it's likely that a single-machine or even single-threaded algorithm will outperform a distributed batch process.

##### High Level APIs and Languages

* Now that MapReduce is robust at scale, attention has turned to improving the programming model, improving efficiency, and broadening the set of problems that these technologies can solve.

###### The move toward declarative query languages

* The freedom to run arbitrary code is what has long distinguished batch processing systems of MapReduce heritage from MPP databases.
* By incorporating declarative aspects into high-level APIs, batch processing frameworks can use query optimizers to achieve comparable performance with MPP databases.

###### Specialization for different domains

* As batch processing systems gain high-level declarative operators, and MPP databases become more programmable, the two are beginning to look more alike.

#### Summary

* Distributed batch processing systems need to solve partitioning, where all related data (e.g. all records with the same key) are brought together in the same place, and fault tolerance.
* Callback functions like mappers and reducers are assumed to be stateless and without side-effects, allowing the framework to hide some of the hard distributed systems problems behind its abstraction.

### Chapter 11: Stream Processing

* A lot of data is unbounded because it changes gradually over time. Batch processors just artificially divide the data into chunks of fixed duration.
* The problem with daily batch processes is that changes in the input are only reflected in the output a day later, which is too slow for many impatient users.
* A "stream" refers to data that is incrementally made available over time.

#### Transmitting Event Streams

* In a stream processing context, a record is commonly referred to as an *event*.
* An event is generated once by a *producer* or *publisher*, and is processed by multiple *subscribers* or *recipients*. Related events are grouped together into a *topic* or *stream*.
* When moving toward continual processing with low delays, polling can become expensive; instead it's better for consumers to be notified when new events appear.

##### Messaging Systems

* Two questions are worth asking in a publish/subscribe model:
  * What happens if producers send messages faster than consumers can process them? There are three options: dropping messages, buffering messages, and applying backpressure.
  * What happens if nodes crash or temporarily go offline? If you can lose messages then you can probably get higher throughput and lower latency on the same hardware.

###### Direct messaging from producers to consumers

* StatsD uses unreliable UDP messaging for collecting metrics from all machines on the network and monitoring them.
* Direct messaging systems generally require the application code to be aware of the possibility of message loss.
* If a consumer is offline, it may miss messages that were sent while it is unreachable.

###### Message brokers

* A *message broker* or *message queue* is a database optimized for handling message streams.
* Centralizing data in the broker allows clients to come and go, and the question of durability is moved to the broker instead.

###### Message brokers compared to databases

* Databases usually keep data until it is explicitly deleted, whereas most message brokers automatically delete a message when it has been successfully delivered to consumers.
* Since message brokers quickly delete messages, it's assumed that their queues are short.

###### Multiple consumers

* Given multiple consumers reading messages in the same topic, two patterns of messaging are used:
  * Load balancing: Each message is delivered to one of the consumers, and you can add consumers to parallelize the processing.
  * Fan-out: Each message is delivered to *all* of the consumers, allowing several independent consumers to consume the same topic of messages without affecting each other.
* The two patterns can be combined, e.g. two separate groups of consumers can subscribe to the same topic.

###### Acknowledgments and redelivery

* Message brokers use *acknowledgments* where a client must explicitly tell the broker when it has finished processing a message so that it can be removed from the queue.
* The combination of load balancing and redelivery inevitably leads messages to be reordered. To avoid the issue, you can disable load balancing by enforcing a separate queue per consumer.

##### Partitioned Logs

* Receiving a message is destructive if the acknowledgment causes it to be deleted from the broker, so you cannot run the same consumer again and get the same result.
* When adding a new consumer, it typically only starts receiving messages sent after the time it was registered, and so any prior messages cannot be recovered.
* A *log-based message broker* combines the durable storage approach of databases with the low-latency notification facilities of messaging.

###### Using logs for message storage

* A topic is a group of partitions that all carry messages of the same type.
* Within each partition, the broker assigns a monotonically increasing sequence number, or *offset*, to every message. There is no total ordering of messages across partitions.

###### Logs compared to traditional messaging

* To load balance across a group of consumers, the broker can assign entire partitions to each consumer in a consumer group, as opposed to assigning individual messages.
* Each consumer consumes *all* the messages in the partitions it has been assigned. This has some downsides:
  * The number of consumers in a group can be at most the number of log partitions in that topic.
  * If a single message is slow to process, it holds up the processing of subsequent messages in that partition.

###### Consumer offsets

* If a consumer node fails, another node in the consumer group is assigned the failed consumer's partitions, and it starts consuming messages at the last recorded offset.

###### Disk space usage

* To reclaim disk space, the log is divided into segments, and periodically the oldest segments are deleted or moved to archive storage.
* The log therefore implements a bounded-size buffer, also known as a *circular buffer* or *ring buffer*, that discards old messages when it gets full.

###### When consumers cannot keep up with producers

* If a consumer falls so far behind that the messages it requires are older than what is retained on disk, then it cannot read those messages and they are effectively dropped.
* You can monitor how far a consumer is behind the head of the log, or its lag, and raise an alert if it falls behind significantly.

#### Databases and Streams

* The fact that something was written to the database is itself an event that can be captured, stored, and processed.
* A replication log is a stream of database write events, produced by the leader as it processes transactions, and consumed by replicas.

##### Keeping Streams in Sync

* Often data is persisted somewhere and then needs to be replicated to several different places, such as a search index or cache.
* Dual writes, where the application explicitly writes to each of the systems, suffer from two problems:
  * A *race condition* where upon two clients dual writing a value, one client may write to the first system first and the second system second.
  * One of the writes may fail while the other succeeds, thereby leaving the data in the two systems in an inconsistent state.

##### Change Data Capture

* *Change data capture* (CDC) is the process of observing all data changes to a database and extracting them in a form in which they can be replicated to other systems.

###### Implementing change data capture

* CDC ensures that all changes made to the system of record are also reflected in the derived data systems so that they too have an accurate copy of the data.
* A log-based message broker is well suited for transporting the change events from the source database to the derived systems, because it preserves the ordering of messages.
* Parsing the replication log of a database is a robust approach to CDC, but it comes with challenges such as handling schema changes.

###### Initial snapshot

* If you do not have the entire log history, you must start with a consistent snapshot.
* The snapshot must correspond to a known position or offset in the change log, so that you know at which point to start applying changes after the snapshot has been processed.

###### Log compaction

* A compaction or merging process running in the background looks for messages with the same key, and keeps only the most recent update for each key.
* The disk usage required by a compacted log depends only on the current contents of the database, and not the number of writes that have ever occurred in the database.
* To rebuild a derived data system, start a new consumer from offset 0 of the log-compacted topic, and sequentially scan over all messages in the log.

##### Event Sourcing

* *Event sourcing* stores all changes to the application state as a log of change events, but applies the idea at a different level of abstraction:
  * In CDC, the application uses the database in a mutable way, updating and deleting records. The application layer can be unaware that CDC is occurring.
  * In event sourcing, the application logic is built on the basis of immutable events written to an event log. Events reflect things that happened at the application level, rather than low-level state changes.
* From an application point of view it is more meaningful to record the user's actions as immutable events rather than recording the effects of those actions on a mutable database.

###### Deriving current state from the event log

* Applications using event sourcing must transform the log of events (representing data *written* to the system) into application state that is suitable for display to the user.
* Log compaction is not possible with event sourcing, as later events do not override prior events, and so you need the full history of events to construct the final state.
* Applications using event sourcing can restore from a snapshot of the current state, but the intention is that the system can reprocess the full event log whenever required.

###### Commands and events

* When a request from a user first arrives, it is initially a *command*: At this point it may still fail, for example because some integrity constraint is violated.
* If the validation is successful and the command is accepted, it becomes an *event*, which is durable and immutable. It becomes a *fact*.
* A consumer of the event stream cannot reject an event, as it's already an immutable part of the log. So validation of the command must happen synchronously before it becomes an event.

##### State, Streams, and Immutability

* Mutable state and an append-only log are two sides of the same coin: The log of all changes, or the *changelog*, represents the evolution of state over time.
* You might say that the application state is what you get when you integrate an event stream over time, and a change stream is what you get when you differentiate the state by time.
* You can consider the log as truth. The database is a cache of the subset of the log, namely the contents of the latest record values in the logs.

###### Advantages of immutable events

* If you accidentally deploy buggy code that writes bad data to a database, recovery is much harder if the code is able to destructively overwrite data.

###### Deriving several views from the same event log

* By separating mutable state from the immutable event log, you can derive several different read-oriented representations from the same log of events.
* You can use the event log to build a separate read-optimized view for some new feature, and run it alongside the existing system until transitioning over and decommissioning the old system.
* Storing data is normally quite straightforward if you don't have to worry about how it is going to be queried, as most schema complexity comes from supporting querying the data.
* Separating the form in which data is written from the form it is read is called *command query responsibility segregation*, or CQRS.

###### Concurrency control

* Because event sourcing and CDC is asynchronous, the user may write to a log, and then read from a log-derived view and find that their write has not yet been reflected in the read view.
* Because event sourcing requires only a single, atomic write – namely appending an event to a log – which is a simpler concurrency model than using multi-object transactions to update data in multiple places.

###### Limitations to immutability

* Workloads with many updates and deletes on comparatively few entities may cause the immutable history to grow prohibitively large.
* Given a large history of changes, the performance of compaction and garbage collection becomes crucial for operational robustness.
* Immutability is also at odds with having to delete the data for administrative or legal reasons, such as GDPR.

#### Processing Streams

* There are three options for processing a stream:
  * You can take the data in the events and write it to a database, cache, search index, or similar storage system, from where it can then be queried by other clients.
  * You can push the events to the users in some way, so that a human is the ultimate consumer of a stream.
  * You can process one or more input streams to produce one or more output streams.
* A piece of code that processes one or more streams to produce other, derived streams is called an *operator* or a *job*.

##### Uses of Stream Processing

###### Complex event processing

* *Complex event processing* (CEP) allows specifying rules to search for certain patterns of events in a stream, similar to a regular expression operating on a string.
* CEP systems often use a high-level declarative query language like SQL to describe patterns of events for detection.
* A processing engine maintains a state machine that performs the required matching on the input streams, and emits a *complex event* with the details of a matching event pattern.
* CEP engines function in an manner opposite to normal databases: The queries are stored long-term, and the data that they operate on is transient.

###### Stream analytics

* Analytics is less interested in finding specific event sequences, and is more oriented toward aggregations and statistical metrics over a large number of events.
* The time interval over which you aggregate is known as a *window*.
* Probabilistic algorithms produce approximate results, but have the advantage of requiring significantly less memory in the stream processor than exact algorithms.

###### Search on streams

* Running in a manner opposite to normal search engines, searching a stream requires storing the queries, and the documents they search over are transient.

##### Reasoning About Time

* Many stream processing frameworks use the local system clock on the processing machine (or *processing time*) to determine windowing, but this breaks down if there is significant processing lag.

###### Event time versus processing time

* Processing may be delayed from queueing, network faults, contention in the broker or processor, a restart of the stream consumer, or reprocessing of past events.
* Confusing event time and processing time leads to bad data.

###### Knowing when you're ready

* You can never be sure when you've received all of the events for a given window, or whether there are some events still to come.
* You have two options for handling *straggler* events that arrive after the window has already been declared complete: Ignore them, or publish a corrective value with stragglers included.

###### Whose clock are you using, anyway?

* Assigning timestamps to events is even more difficult when events can be buffered at several points in the system, such as on a mobile device that is offline and later reconnects.
* To estimate the offset between the server and device clocks, subtract the timestamp at which the event was sent to the server according to the device clock from the timestamp at which the event was received by the server according to the server clock.
* Applying this computed offset to the event timestamp allows you to estimate the true time at which the event occurred.

###### Types of windows

* A *tumbling window* has a fixed length, and every event belongs to exactly one window.
* A *hopping window* has a fixed length, but windows may overlap to provide smoothing. You can implement this by computing 1-minute tumbling windows, and then aggregating over several adjacent windows.
* A *sliding window* contains all events that occur within some interval of each other. You can implement this by buffering events sorted by time, and removing old events that have expired from the window.
* A *session window* has no fixed duration, but instead groups together all events for the same user that occur closely together in time. The window ends when the user is inactive for some time.

##### Stream Joins

* The fact that new events can appear anytime on a stream makes joins on streams more challenging than in batch jobs.

###### Stream-stream join (window join)

* The stream processor maintains state that is updated whenever an event occurs, which is referenced upon processing subsequent events.

###### Stream-table join (stream enrichment)

* *Enriching* a stream means augmenting its events with additional data queried from a database.
* If the stream processor has a local copy of the database (similar to map-side joins), then that database can be kept up to date using change data capture.
* Unlike a stream-stream join, a stream-table join uses a window that reaches back to the beginning of time (a conceptually infinite window) with newer versions of records overwriting old ones.

###### Table-table join (materialized view maintenance)

* A table-table join is a stream process that maintains a materialized join between two tables.

###### Time-dependence of joins

* All three joins above require the stream processor to maintain some state based on one join input, and to query that state on messages from the other input.
* The ordering of the events that maintain the state is important, and in a partitioned log, there is no ordering guarantee of events across different streams or partitions.
* Because state changes over time, and you join with some state, you must ask yourself what point in time you are using for the join.
* If the ordering of events across streams is undetermined, then the join is nondeterministic, and the events may be interleaved differently when you run the job again.
* This issue is known as a *slowly changing dimension* (SCD) in data warehouses and is often addressed by using a unique identifier for a particular version of a joined record.
* A unique identifier makes the join deterministic, but log compaction is no longer possible because all versions of the records in the table must be retained.

##### Fault Tolerance

* The batch processing approach to fault tolerance exposes *exactly-once semantics*, where it appears as though every record was processed exactly once.
* In stream processing, waiting until a task is finished before making its output visible is not an option, because a stream is infinite.

###### Microbatching and checkpointing

* *Microbatching* breaks the stream into small blocks and treats each block like a miniature batch process.
* With microbatching, small batches incur greater scheduling and coordination overhead, while larger batches mean a longer delay before results of the stream processor become available.
* An alternative is to periodically generate rolling checkpoints of state and write them to durable storage.
* Once the output leaves the stream processor, the framework cannot discard the output of a failed batch. Restarting a failed task causes its external side-effects to happen twice, regardless of checkpointing or microbatching.

###### Atomic commit revisited

* To give the appearance of exactly-once processing in the presence of faults, all outputs and side-effects of processing an event must happen if and only if the processing is successful.
* More restricted environments, which do not attempt to persist data across heterogeneous environments, can implement an atomic commit facility efficiently.

###### Idempotence

* An idempotent operation is one that you can perform multiple times, and it has the same effect as if you had performed it only once.
* Using idempotence relies on restarting a task to replay the same messages in the same order, the processing must be deterministic, and no other node may concurrently update the same value.

###### Rebuilding state after a failure

* Any stream process that requires state (e.g. any windowed aggregations or tables for joins) must ensure that the state can be recovered after a failure.
* A performant solution is to keep the state local to the stream processor, and then to replicate it periodically.
* If the state is a local replica of a database, maintained by CDC, then the database can be rebuilt from the log-compacted change stream.

#### Summary

* There are three types of joins that may appear in stream processes:
  * *Stream-stream joins*: Both input streams consist of activity events, and the join operator searches for related events that occur in some window of time.
  * *Stream-table joins*: One input stream consists of activity events, while the other is a database changelog that keeps a local copy of a database up to date. This emits enriched activity events.
  * *Table-table joins*: Both input streams are database changelogs. The result is a stream of changes to the materialized view of the join between the two tables.

### Chapter 12: The Future of Data Systems

#### Data Integration

* There is unlikely one piece of software that is suitable for all circumstances in which data is used, and so we must cobble together several pieces of software to provide the application's functionality.

##### Combining Specialized Tools by Deriving Data

* As the number of different representations of the data increases, the integration problem becomes harder.
* The need for data integration often only becomes apparent when you zoom out and consider the dataflows across an entire organization.

###### Reasoning about dataflows

* You must have clarity on the inputs and outputs, namely where data is written first, and which representations are derived from which sources.
* By funneling all user input through a single system that decides on an ordering for all writes, it's easier to derive other representations of the data by processing the writes in the same order.
* Whether to use change data capture or an event sourcing log is less important than simply the principle of deciding on a total order.

###### Derived data versus distributed transactions

* Distributed transactions decide on an ordering of writes by using locks for mutual exclusion, while CDC and event sourcing use a log for ordering.
* Distributed transactions use atomic commit to ensure that changes take effect only once, while log-based systems are often based on deterministic retry and idempotence.
* Transactions systems usually provide linearizability and reading your own writes, while derived data systems are asynchronous and do not offer such guarantees.

###### The limits of total ordering

* As systems scale, limitations of total ordering begin to emerge:
  * To scale you may need to partition the log across multiple machines, but the order of events across two different partitions is ambiguous.
  * If servers are are spread across multiple geographically distributed datacenters, there is an undefined ordering of events between events in two different datacenters.
  * When two events originate in different micro-services, then there is no defined order for those events.
* Deciding on a total order of events is known as a *total order broadcast*, which is equivalent to consensus. It's an open research problem to design consensus algorithms that work well beyond a single node in a geographically distributed setting.

###### Ordering events to capture causality

* Where there is no causal link between events, the lack of a total order is not a big problem since concurrent events can be ordered arbitrarily.
* Logical timestamps can provide total ordering without coordination, so they may help in cases where total order broadcast is not feasible.
* If you can log an event to record the state of the system the user saw before making a decision, and give that event a unique identifier, then any later events can refer to that event identifier to record the causal dependency.

##### Batch and Stream Processing

* The goal of data integration is to ensure that data ends up in the right form in all the right places.

###### Maintaining derived state

* No matter what the derived data is, it's helpful to think in terms of data pipelines that derive one thing from another, pushing state changes in one system through functional application code and applying the effects to derived systems.
* Asynchrony is what makes systems based on event logs robust, as a fault in one part of the system is contained locally.

###### Reprocessing data for application evolution

* With reprocessing it is possible to re-structure a dataset into a completely different model in order to better serve new requirements.
* Derived views allow *gradual* evolution. A gradual migration has little risk as you always have a working system to go back to.

###### The lambda architecture

* The lambda architecture records incoming data by appending immutable events to an always-growing dataset, from which read-optimized views are derived.
* A stream processor consumes the events and quickly produces an approximate update to a view. A batch processor later consumes the *same* set of events and produces a corrected version of the derived view.
* The stream processor can use fast approximate algorithms while the batch process uses slower exact algorithms.
* The lambda architecture has several practical problems:
  * The same logic must be implemented in both a batch and in a stream processing framework.
  * The batch pipeline and the stream pipeline outputs must be merged in order to respond to user requests.
  * Batch processing on large datasets is expensive, and so the batch pipeline often needs to process incremental batches rather than reprocessing everything, which adds complexity.

#### Unbundling Databases

* Relational databases want to give programmers a high-level abstraction that hide the complexities of data structures on disk, concurrency, crash recovery, and so on.
* The NoSQL movement wants to apply a Unix-esque approach of low-level abstractions to the domain of distributed OLTP data storage.

##### Composing Data Storage Technologies

###### The meta-database of everything

* The dataflow across an entire organization looks like one huge database: Each batch, stream, or ETL process acts like a database subsystem that keeps indexes or materialized views up to date.
* There are two avenues by which different storage and processing tools can be composed into a cohesive system:
  * Federated databases (unifying reads): A *federated database* or *polystore* provides a unified query interface to a wide variety of storage engines and processing methods.
  * Unbundled databases (unifying writes): Data capture and event logs allow *unbundling* a database's index-maintenance features in a way that can synchronize writes across disparate technologies.

###### Making unbundling work

* Asynchronous event logs with idempotent retries is a more robust and practical approach than distributed transactions across heterogeneous storage systems.
* Log-based integration is *loose coupling* between storage components:
  * Asynchronous event streams make the system as a whole more robust to outages or performance degradation of individual components.
  * Unbundling data systems allows different software components and services to be developed, improved, and maintained independently from each other by different teams.

###### Unbundling versus integrated systems

* Unbundling allows you to combine several different databases in order to achieve good performance for a much wider range of workflows than is possible with a single piece of software.
* The advantages of unbundling and composition only come into the picture when there is no single piece of software that satisfies all your requirements.

##### Designing Applications Around Dataflow

* The approach of unbundling databases by composing specialized storage and processing systems with application code is becoming known as the "database inside-out" approach.

###### Application code as a derivation function

* A dataset derived from another must go through some transformation function. Examples include a secondary index, a full-text search index, a machine learning model (derived from the training data), or a cache (aggregating data in a form for rendering in the UI).

###### Separation of application code and state

* In the typical web application model, the database acts as a kind of mutable shared variable that can be accessed synchronously over the network.
* Subscribing to changes in a database is only now emerging as a feature; historically databases have inherited a passive approach to mutable data that requires polling for changes.

###### Dataflow: Interplay between state changes and application code

* Instead of thinking of a database as a passive variable that is updated only by the application, we must think of application code responding to state changes in one place by updating state in another place.
* Unbundling the database applies this idea to derived datasets from the primary database, including caches, full-text search indexes, machine learning, or analytics systems.
* This requires stable message ordering and fault-tolerant message processing, but those are less stringent demands than those imposed by distributed transactions.

###### Stream processors and services

* The advantage of a service-oriented architecture over a single monolithic application is primarily organizational scalability through loose coupling.
* Instead of one service querying another service for data, it can subscribe to to state changes from the other service and query a local database for improved speed and reliability.

##### Observed Derived State

* The write path between a primary database and its derived dataset and the read path between that derived dataset and the point it is consumed represents the whole journey of the data.
* Framing the journey in terms of functional programming languages, the write path is similar to eager evaluation, while the read path is similar to lazy evaluation.
* The derived dataset is where the write path and read path meet, and represents a tradeoff between the amount of work that must be done at write time and the amount that must be done at read time.

###### Materialized views and caching

* Caches, indexes, and materialized views allow us to do more work on the write path and less work on the read path, thereby shifting the boundary between the write path and read path.

###### Stateful, offline-capable clients

* When we move from stateless clients to a model where end-user devices maintain state, we can think of the on-device state as a *cache of state on the server*.

###### Pushing state changes to clients

* Actively pushing state changes all the way to client devices means extending the write path all the way to the end user.
* When a client is first initialized, it would still need to use a read path to get its initial state, but thereafter it could rely on a stream of state changes sent by the server.

###### End-to-end event streams

* Extending the write path all the way to the end user requires moving away from request/response interaction and toward a publish/subscribe dataflow.

###### Reads are events too

* We can treat reads as events, and send them to the same stream processor as writes; the processor can respond to a read event by emitting the result of the read on the output stream.
* This performs a stream-table join between the stream of read queries and the database.
* Recording a log of read events has benefits with regard to tracking causal dependencies and data provenances, as you can reconstruct what a user saw before making some decision.

###### Multi-partition data processing

* Representing queries as events and collecting responses from streams enables executing queries across several partitions, leveraging the infrastructure for message routing, partitioning, and joining already provided by stream processors.

#### Aiming for Correctness

* We want to build applications that are reliable and *correct*, i.e. programs whose semantics are well defined and understood, even in the presence of faults.
* Serializability and atomic commits provide strong assurances of correctness, but typically only work in a single datacenter, and they limit the scale of fault-tolerance properties you can achieve.

##### The End-to-End Argument for Databases

###### Exactly-once execution of an operation

* *Exactly-once* means arranging the computation such that the final effect is the same as if no faults had occurred, even if the operation was retried due to some fault.
* One of the most effective approaches is to make the operation *idempotent*.

###### Duplicate suppression

* Even if you suppress duplicate transactions between the database client and server, you must still worry about the network between the end-user device and the application server.

###### Uniquely identifying requests

* Relational databases can generally maintain a uniqueness constraint correctly (such as on an idempotence token), even at weak isolation levels.

###### The end-to-end argument

* The *end-to-end argument* says some problems can be completely and correctly implemented only with the help of the endpoints of the communication system; providing a solution as a feature of the communication system is not possible.
* In such circumstances, low-level reliability features are not by themselves sufficient to ensure end-to-end correctness.

###### Applying end-to-end thinking in data systems

* Reasoning about concurrency and partial failure is difficult and counterintuitive, and so most application-level mechanisms likely do not work correctly, thereby resulting in lost or corrupted data.

##### Enforcing Constraints

###### Uniqueness constraints require consensus

* Enforcing a uniqueness constraint requires consensus, as the system must decide which one of the conflicting operations is accepted, and reject the others as violating the constraint.
* Uniqueness constraints can be scaled out by partitioning based on the value that needs to be unique.
* Asynchronous multi-master replication cannot be used in this case, as two different masters could concurrently accept conflicting writes.

###### Uniqueness in log-based messaging

* If a log is partitioned based on the value that needs to be unique, then a stream processor can unambiguously and deterministically decide which of several conflicting operations comes first in the log.
* To scale the number of handled requests, simply increase the number of partitions.
* Because all writes that may conflict are routed to the same partition and processed sequentially, this works for many constraints other than uniqueness constraints.

###### Multi-partition request processing

* Single-object writes are atomic in almost all data systems, and so a request appended to a log either appears in the log or it doesn't.
* By breaking down a multi-partition transaction into partitioned stages using the same end-to-end request ID, you can achieve correctness even in the presence of faults without using an atomic commit protocol.

##### Timeliness and Integrity

* The term *consistency* conflates two requirements that are worth considering separately:
  * *Timeliness* means ensuring the data is in an up-to-date state. Read data may be stale but that inconsistency is only temporary.
  * *Integrity* means absence of corruption, and no contradictory or false data. If integrity is violated, then the inconsistency is permanent.
* Violations of timeliness are "eventual consistency" whereas violations of integrity are "permanent inconsistency."

###### Correctness of dataflow systems

* ACID transactions usually provide both timeliness (e.g. linearizability) and integrity (e.g. atomic commit) guarantees.
* Event-based dataflow systems decouple timeliness and integrity: You forfeit timeliness because of asynchrony, but *exactly-once* or *effectively-once* semantics preserve integrity.

###### Loosely interpreted constraints

* If an application can get away with weaker notions of uniqueness, then persisting a *compensating transaction* may undo a constraint violation.
* In many business contexts, it may be acceptable to temporarily violate a constraint and fix it up later by apologizing. The cost of the apology is a business decision.
* Such applications that tolerate optimistic writes and checking the constraint afterward do require integrity, but don't require timeliness on the enforcement of the constraint.

###### Coordinating-avoiding data systems

* Systems that synchronize cross-partition coordination or have strict constraints reduce the number of apologies you must make due to inconsistencies, but potentially also reduce your performance and availability.

##### Trust, but Verify

* The *system model* is the set of assumptions that certain things might go wrong, but other things won't.
* Faults are really modeled by probabilities. The question is whether violations of our assumptions happen often enough that we may encounter them in practice.
* If you have enough devices running your software, then even very unlikely things do happen.

###### Maintaining integrity in the face of software bugs

* If the application uses the database incorrectly in some way, for example using a weak isolation level unsafely, then the integrity of the database cannot be guaranteed.

###### Don't blindly trust what they promise

* Checking the integrity of data is known as *auditing*.
* HDFS and Amazon S3 run background checks that continually read back files, compare them to other replicas, and move files between disks, in order to mitigate the risk of silent corruption.

###### Designing for auditability

* Mutations to database tables do not detail *why* those mutations were performed. The invocation of the application logic that decided on those mutations is transient and cannot be reproduced.
* Being explicit about dataflow makes the *provenance* of data much clearer, which makes integrity checking much more feasible.

###### The end-to-end argument again

* Checking the integrity of data systems is best done in an end-to-end fashion: The more systems we can include in an integrity check, the fewer opportunities there are for corruption to go unnoticed at some stage of the process.

###### Tools for auditable data systems

* Blockchains and distributed ledgers are essentially distributed databases run by mutually untrusting organizations. These replicas check each other's work and use a consensus protocol to agree on the transactions that should be executed.

#### Doing the Right Thing

* Data may be an abstract thing, but many datasets are about people: their behavior, their interests, their identity. We must treat such data with humanity and respect.

##### Predictive Analytics

* The criminal justice system may presume innocence until proven guilty, but automated systems can systematically and arbitrarily exclude a person from participating in society without any proof of guilt, and with little chance of appeal.

###### Bits and discrimination

* Predictive analytics systems do not merely automate a human's decision by using software to specify the rules for when to say yes or no; instead we leave the rules themselves to be inferred from the data.
* If there is systematic bias in the input to an algorithm, the system will most likely learn and amplify that bias in its output.
* Predictive analytics systems merely extrapolate from the past; if the past is discriminatory, then they codify that discrimination.

###### Responsibility and accountability

* As data-driven decision making becomes more widespread, we must discover how to make algorithms accountable and transparent, how to avoid reinforcing existing biases, and how to fix them when they inevitably make mistakes.

###### Feedback loops

* Consequences such as feedback loops can be predicted by thinking about the entire system, including the people interacting with it – an approach known as *systems thinking*.

##### Privacy and Tracking

* When a system only stores data that has been explicitly entered, then the system is performing a service for the user: The user is the customer.
* Tracking the user serves not the individual but the needs of advertisers who are funding the service. This relationship is appropriately described as *surveillance*.

###### Surveillance

* In our attempts to make software "eat the world" we have built the greatest mass surveillance infrastructure that the world has ever seen.

###### Consent and freedom of choice

* Most privacy policies do more to obscure than to illuminate. But without understanding what happens to their data, users cannot give any meaningful consent.
* Data is extracted from users through a one-way process, not a relationship with true reciprocity, and not a fair value exchange.
* For the user who does not consent to surveillance, the only real alternative is simply to not use the service, which may have real social cost.
* For people in a less privileged position, there is no meaningful freedom of choice: surveillance becomes inescapable.

###### Privacy and use of data

* Privacy does not mean keeping everything secret, but having the freedom to choose which things to reveal to whom, what to make public, and what to keep secret.
* When data is extracted from people through surveillance infrastructure, privacy rights are usually not eroded but rather transferred to the data collector.
* Even if particular users cannot be personally identified from the group targeted by a particular ad, they have lost their agency about the disclosure of some intimate information.
* Whether something is "undesirable" or "inappropriate" is down to human judgment; algorithms are oblivious to such notions unless we explicitly program them to respect human needs.
* Surveillance has always existed, but it used to be expensive and manual. Trust relationships have always existed, but the are mostly governed by ethical, legal, and regulatory constraints.

###### Data as assets and power

* Behavioral data is sometimes called "data exhaust" because it is a byproduct of users interacting with a service.
* From an economic point of view, if targeted advertising is what pays for a service, then behavioral data about people is the service's core asset.
* Because data can be abused so easily, critics have said that data is not just an asset, but a "toxic asset" or at least a "hazardous material."

###### Remembering the Industrial Revolution

* Just as the Industrial Revolution had a dark side that needed to be managed, our transition to the information age has major problems that we must confront and solve.
* As Bruce Schneier said, data is the pollution problem of the information age, and protecting privacy is the environmental challenge.
