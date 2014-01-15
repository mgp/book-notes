## MongoDB: The Definitive Guide

by Kristina Chodorow and Michael Dirolf

*I, [Michael Parker](http://omgitsmgp.com/), own this book and took these notes to further my own learning. If you enjoy these notes, please [purchase the book](http://www.amazon.com/MongoDB-Definitive-Guide-Kristina-Chodorow/dp/1449344682)!*

### Chapter 1: Introduction
* pg 3: MongoDB uses memory-mapped files to push memory management to the operating system.

### Chapter 2: Getting Started
* pg 6: Documents with the same key-value pairs but in different orders are distinct.
* pg 7: Partitioning data into collections yields speed gains from data locality given the right access patterns.
* pg 8: Subcollections are a naming convention for organizing documents; there is no enforced relationship.
* pg 9: A namespace is a collection name prepended with its containing database, and should be less than 100 bytes in practice.
* pg 14: Help for database commands is provided by `db.help()`, and help for collections is provided by `db.foo.help()`.
* pg 15: If a collection name is a property of the database class, or if it contains invalid Javascript in its name, use `db.getCollection` instead.
* pg 18: If you retrieve a document with a 4-byte integer in the shell and save it back, it is updated to an 8-byte floating point value because all numbers are floating point in JS.
* pg 18: An 8-byte integer is always printed with its float approximation; if it is not accurate, the top and bottom keys display the high-order and low-order 4 bytes, respectively.
* pg 19: Omitting the `new` keyword when calling the `Date` constructor returns a string representation of the date, which can cause problems everywhere.
* pg 22: The client can assign any type of value to the `_id` key; if none is present, the driver automatically assigns one of type `ObjectId`.

### Chapter 3: Creating, Updating, and Deleting Documents
* pg 24: Data validation is pushed to the drivers; the server only checks for an `_id` key and that the data does not exceed 4MB.
* pg 25: Calling `remove` on a collection removes all documents but not the collection or its indexes; drop the collection to delete them as well.
* pg 27: Ensure that `update` always specifies a unique document; the best way is to match on the `_id` key if you know it.
* pg 30: Calling `update` without using an `$` operator for modifying key-value pairs will perform a full-document replacement, not a partial update.
* pg 33: Use `$addToSet` with `$each` to add multiple unique values to an array acting as a set; this cannot be done by combining `$ne` with `$push`.
* pg 34: The `$pull` operator removes all elements from an array that match some criteria, not just one element.
* pg 35: The positional operator `$` refers to the index of an element in an array that matches the query, allowing updates without knowing the index a priori.
* pg 36: If `$push` becomes a bottleneck, consider pulling an embedded array out into a separate collection.
* pg 37: An upsert creates a new document by applying any modifier documents to the criteria document, including any `$` operators like `$inc`.
* pg 38: The update command may affect all matching documents in the future, so always specify the optional arguments `upsert_bool` and `multi_bool`.
* pg 41: By default, the `findAndModify` command will return the preupdate document; this can be changed by setting the new key.
* pg 42: Safe operations run `getLastError` immediately following an operation, which contains additional information (e.g. for an update or remove, the number of documents affected).
* pg 43: Use safe operations while debugging; for example, an unsafe insert will not receive from the server a duplicate key error.
* pg 44: Popular drivers use connection pooling, so a read submitted after a write by the client can reach the server first by being sent across a different connection.

### Chapter 4: Querying
* pg 46: Keys with values of `1` in the second argument of `find` are always returned; keys with values of `0` are never returned.
* pg 48: AND-type queries are most efficient if the first arguments eliminate results quickly; OR-type queries are most efficient if the first arguments match results quickly.
* pg 49: Conditionals are an inner document key, while modifiers are always a key in the outer document.
* pg 49: Multiple update modifiers cannot be used on a single key, but no such rule applies to query conditionals.
* pg 50: A query document with a key of `null` matches documents missing the key altogether; to find keys whose value is `null`, check that the value is `$in [null]`, and that `$exists` returns `true`.
* pg 52: The `$size` operator cannot be combined with another `$` conditional, adding a `size` key to the document allows the query, but will not update correctly with the `$addToSet` operator.
* pg 53: Even though the `$slice` operator is used in the second argument to `find`, all keys in a document are returned unless otherwise specified.
* pg 54: Dot notation on query documents allows reaching into embedded documents; it also means that when saving URLs as keys, the period character must be replaced.
* pg 55: The `$elemMatch` operator ensures that all the following keys belong to the same embedded document.
* pg 56: Use `$where` queries as a last resort; to reduce their penalty, use query filters in combination with them or use an index to filter based on the non-`$where` clauses.
* pg 57: Only once `hasNext` is invoked on a cursor is the database queried; this allows chaining.
* pg 59: Skips on cursors is slow; to paginate sorted elements, perform the query for the next page with the clause that the elements should be greater (or less than) the last value shown.
* pg 60: To randomly select a document in a collection, have each map some key to a random value, and use `findOne` to find the first element greater than or less than another random value.
* pg 61: Using the `explain` option with a query doesn’t perform it, but returns the indexes used, number of results, how long it takes, etc. instead.
* pg 62: A document may be returned twice or inconsistencies may arise if you run a query without the snapshot option that returns multiple batches from the server.

### Chapter 5: Indexing
* pg 67: If an index has *n* keys, it will make queries on any prefix of those keys fast.
* pg 68: Order the keys when specifying an index so that frequently used data is spread over fewer pages, and therefore more likely to be in memory.
* pg 69: A sort without an index requires pulling all the data into memory to sort it; if too large, the server will simply return an error.
* pg 70: If a key does not exist, the index stores its value as *null*; given a unique index, it is easy to insert multiple documents missing a key and have all but the first fail.
* pg 75: Given a query, the query optimizer first tries all query plans concurrently; that plan is remembered for future queries on the same keys.
* pg 76: Invoking the `ensureIndex` method without the background option will block all other requests while the index is being built.
* pg 78: The `geoNear` command returns the distance of each result from the query point; the `near` command does not, but must be used if you have more than 4MB of results.
* pg 79: Geospatial indexes aren’t perfect for spherical shapes, but are fine for anything not a large distance.

### Chapter 6: Aggregation
* pg 82: The `distinct` command finds all distinct values for a given key, but can’t find all distinct keys.
* pg 87: The best way to find all keys across all documents in a collection is to use MapReduce.
* pg 88: Reduce must return a document that can be used as input to reduce later, and so you cannot count on the input holding one of the initial documents or being a certain length.
* pg 91: The MapReduce output collection is deleted by default when your shell or connection closes; to persist it, add the `keeptemp` or `out` option.
* pg 92: Client-side values under the scope key passed to a MapReduce are immutable in the `map`, `reduce`, and `finalize` methods.

### Chapter 7: Advanced Topics
* pg 94: All documents passed to the `runCommand` method are converted to queries on the `$cmd` collection.
* pg 96: The `ping` command will return immediately even if the server is in a lock.
* pg 99: If a capped collection has a maximum size in bytes and a maximum number of elements, the first limit encountered is used.
* pg 100: The default sort of capped collections is the natural sort order, which corresponds to the insertion order; noncapped collections guarantee no particular default ordering.
* pg 103: The metadata for each file is stored in the `fs.files` collection, which allows using traditional commands to inspect the filesystem.
* pg 104: The `db.eval` function locks the database, and so can be used to introduce multidocument transactions.
* pg 106: Use a scope with your MongoDB driver to pass user-defined variables to server-side Javascript while avoiding injection attacks.
* pg 107: The first key in a `DBRef` must be `$ref` (the collection name), followed by `$id`, and then optionally `$db`.
* pg 109: Storing an `_id` is more compact, but `DBRefs` are useful when storing heterogeneous references to documents in different collections.

