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
