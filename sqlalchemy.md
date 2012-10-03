## SQLAlchemy 0.7.8 Documentation

http://docs.sqlalchemy.org/en/rel_0_7/index.html

### Object Relational Tutorial

http://docs.sqlalchemy.org/en/rel_0_7/orm/tutorial.html

* The SQL Expression Language represents the relational database directly without opinion; the ORM builds upon it a high level and abstracted pattern of usage.
* The ORM approaches structure and content of data from a user-defined domain model, transparently persisted and refreshed from its underlying storage model.
* An `Engine` instance is a core interface to the database, adapted to the DBAPI in use.
* The declarative base class maintains a catalog of classes and tables relative to that base.
* SQLAlchemy never makes assumptions about names or characteristics of data; the developer must be explicit about specific conventions in use.
* The declarative base class has a `metadata` attribute that contains a catalog of all the `Table` objects that have been defined.
* The declarative system supplies a default constructor which accepts keyword arguments of the same name as that of the mapped attributes.
* The `Session` object is just a workspace for your objects, local to a particular database connection.
* Adding an entity to a `Session` does not persist it to the database; this is only done when the session is flushed.
* Using the identity map, once an object is in a `Session`, all queries with its particular primary key return that same object.
* By default, data from a previous transaction is refreshed the first time it’s accessed within a new transaction, so that it is not stale.
* The tuples returned by a `Query` object are named tuples, and can be treated much like an ordinary Python object.
* A `Query` object is fully generative, so most method calls return a new `Query` object upon which further criteria may be added.
* The `one()` method of `Query` fully fetches all rows, and if not exactly one object identity or composite row is returned, raises an error.
* The `relationship()` method uses the foreign key relationships between the two tables to determine the nature of this linkage.
* A foreign key constraint in most (though not all) relational databases can only link to a primary key column, or a column with a `UNIQUE` constraint.
* A foreign key constraint that refers to a multiple-column primary key, or a subset of one, and itself has multiple columns, is called a composite foreign key.
* Objects defined in a relationship use lazy loading by default.
* The `aliased` construct supports aliasing a table with another name so that it can be distinguished against other occurences of that table in a query.
* The `subquery` method returns a SQL expression construct representing a `SELECT` statement embedded in an alias, and behaves like a `Table` construct.
* The `orm.subqueryload()` option is called so because the `SELECT` statement constructed is embedded as a subquery into a `SELECT` against the related table.
* The `orm.joinedload()` option emits a left outer join so that the lead object and the related object or collection is loaded in one step.
* `subqueryload` is appropriate for related collections, while `joinedload` is better suited for many-to-one relationships.
* The `contains_eager()` function is typically most useful for pre-loading the many-to-one object on a query that needs to filter on that same object.
* By default deletes don't cascade; instead you must explicitly specify this when establishing a relationship.
* If a many-to-many relationship has any columns other than foreign keys, you must use the "association object" pattern.

### SQL Expression Language Tutorial

http://docs.sqlalchemy.org/en/rel_0_7/core/tutorial.html

* The expression language allows writing backend-neutral SQL expressions, but does not attempt to enforce that expressions are backend-neutral.
* Table reflection allows "importing" whole sets of `Table` objects automatically from an existing database.
* An `Insert` object does not render bound data in its string form; to see this, call `compile()` and access its `params` attribute.
* SQLAlchemy always returns any newly generated primary key value in a returned `ResultProxy`, regardless of the SQL dialect.
* The `Insert` statement is compiled against the first dictionary in a list of values to insert; subsequent dictionaries must have the same keys.
* Call `close` on a `ResultProxy` object when done to release cursor and connection resources.
* The `Column` type determines how an operator is translated to SQL; for example, a `String` interprets `+` as string concatenation.
* The `label` method produces labels using the `AS` keyword, and allows selecting from expressions that otherwise don't have a name.
* Any `Table`, `select` constructor, or selectable can be turned into an alias using its `alias` method.
* The `ON` condition in the SQL produced from a call to `join()` is automatically generated from the `ForeignKey` column of either table.
* The `outerjoin()` method creates `LEFT OUTER JOIN` constructs in SQL.
* Use `bindparam()` to create bind parameters in a statement; their values are only provided when executing, allowing you to reuse the statement.
* ANSI functions are part of the functions known by SQLAlchemy, and are special in that they don't have parentheses added after them.
* Label a function to allow extracting its result from a returned row; assign it a type to enable result-set processing, such as Unicode or date conversions.
* To embed a `SELECT` in a column expression, use `as_scalar()` or apply a `label()` to it.
* SQLAlchemy automatically correlates embedded `FROM` objects to that of an enclosing query; to disable or specify explicit `FROM` clauses, use `correlate()`.
* A correlated update lets you update a table using a selection from another table, or the same table.
* The Postgresql, Microsoft SQL Server, and MySQL backends support `UPDATE` statements that refer to multiple tables.

### Using the Session

http://docs.sqlalchemy.org/en/rel_0_7/orm/session.html

* A `Session` requests a connection from the `Engine`, which represents an ongoing transaction until it commits or rolls back its pending state.
* Objects associated with a `Session` are proxy objects, and there exist events that cause these objects to re-access the database and keep synchronized.
* You get persistent object instances either by flushing pending instances so they become persistent, or querying for existing instances.
* A session can span multiple transactions, which must proceed serially; this is called transaction scope and session scope.
* In a web application, typically the scope of a `Session` is tied to the lifetime of the request, maybe using event hooks in the web framework.
* A `Session` is similar to a cache in that it implements the identity map, but only uses the map as a cache if you query by a primary key.
* By default a `Session` expires all instances along transaction boundaries, so no instance should have stale data.
* When merging, if a mapped attribute is missing on the source instance, then it is expired on the target instance, discarding its existing value.
* The `load` flag when merging is `True` by default, which loads the target's unloaded collections to reconcile the incoming state against the database.
* Most `merge()` issues can be fixed by asserting that the object is not in the session, or removing unwanted state from its `__dict__`.
* To delete items in a collection, use cascade behavior to automatically invoke the deletion as a result of removing objects from the parent collection.
* A `flush()` occurs before any issued `Query` if `autoflush` is changed to `True`, as well as within the `commit()` call for the transaction.
* If `flush()` fails, then the database transaction has been rolled back, but to reset the state of the `Session` you must still call `rollback()`.
* Methods `refresh()` and `expire()` are usually only necessary after issuing an `UPDATE` or `DELETE` using `Session.execute()`.
* Typically cascade is left as its default of `save-update, merge`, or set as `all, delete-orphan` to treat child objects as "owned" by the parent.
* Without the `delete` cascade option, SQLAlchemy will set a foreign key on a one-to-many relationship to `NULL` when the parent object is deleted.
* Autocommit mode should not be considered for general use, but frameworks may use it to control when a transaction begins.
* Two-phase commits, supported by MySQL and PostgreSQL, ensure that a transaction is either committed or rolled back in all databases.
* You can assign a SQL expression instead of a literal value to an object's attribute, useful for atomic updates or calling stored procedures.
* Joining a `Session` with an external `Connection` and `Transaction` allows a test's `tearDown` method to rollback subtransactions by its methods.
* A `ScopedSession` returns the same `Session` object until it is disposed of; methods called on `ScopedSession` also proxy to that `Session` object.
* Overriding `Session.get_bind()` allows custom vertical partitioning, such as directing write operations to a master and read operations to slaves.

##### API

* Constructor options:
	* `autocommit`: Defaults to `False`; when `True`, no persistent transaction is used, and connections are released right after use.
	* `autoflush`: Defaults to `True`, where all `query()` operations call `flush()` before proceeding; typically used when `autocommit` is `False`.
* `close()`: Closes the session, clears all items and ends the transaction; if `autocommit` is `True`, a new transaction is immediately started.
* `execute()`: Executes within the transaction, but does not invoke `autoflush`.
* `expunge()`: Removes an instance from the session, freeing all internal resources to it.
* `flush()`: Writes all pending `INSERT`, `UPDATE`, and `DELETE` operations to the transaction buffer; if an error occurs, it is rolled back.
* `is_active`: `False` if `flush` encountered an error, and the `Session` awaits the user to call `rollback` to close out the transaction stack.
* `merge()`: If the objct to reconcile is not in the session, it attempts to retrieve it, or else creates it.
* `no_autoflush()`: Returns a context manager that has `autoflush` set to `False`.
* `refresh()`: This method only needs to be called if a non-ORM SQL statement were emitted in the transaction, or `autocommit` is `True`.

### Relationship Configuration

http://docs.sqlalchemy.org/en/rel_0_7/orm/relationships.html

* An association table used with the `secondary` argument to `relationship()` is automatically subject to `INSERT` and `DELETE` statements.
* The association pattern requires that child objects are associated with an associated instance before being appended to the parent.
* The association proxy allows accessing an attribute of an associated object in one "hop" instead of two.
* An adjacency list pattern is where a table has a foreign key reference to itself, and is used to represent hierarchical data in flat tables.
* `Query.join()` has an `aliased` parameter that shortens the verbosity of self-referential joins, at the expense of query flexibility.
* The `join_depth` parameter specifies how many levels deep to join or query when using eager loading with a self-referential relationship.
* Keyword `backref` adds a second `relationship` on the other side, and adds event listeners to monitor attribute operations in both directions.
* For a collection that contains a filtering `primaryjoin` condition, a one-way backref ensures that all collection items pass the filter.
* If there are multiple ways to join two tables, each `relationship()` must specify a `primaryjoin` to resolve the ambiguity.
* Objects in a colleciton that fail some `primaryjoin` criteria will remain until the attribute is expired and reloaded from the database.
* If `backref` creates a bidirectional, self-referential many-to-many relationship, it reverses the `primaryjoin` and `secondaryjoin` arguemnts.
* The `post_update` option of `relationship()` relates two rows using an `UPDATE` statement on their foreign keys after each is inserted.
* Setting `passive_updates` to `False` issues the necessary `UPDATE` statements if primary key changes are not propagated to foreign keys.

##### API

* `back_populates`: Like `backref` except the complementing property must be configured explicitly and also include `back_populates`.
* Cascade `delete-orphan` won't prevent persisting a child without a parent; set the child's foreign key as `NOT NULL` to ensure this.
* `cascade_backrefs`: Defaults to `True`; if `False`, assigning a value in the session to the attribute of a transient won't make it pending.
* `innerjoin`: Defaults to `False`; if `True`, eager loads use inner and not outer joins, which generally perform better.
* `lazy`: Defaults to `select` which lazily loads; `joined` and `subquery` eagerly load by joins; `dynamic` returns a `Query` object.
* `order_by`: Specifies the ordering to apply when the items are loaded.
* `passive_deletes`: Defaults to `False`; if `True`, implies that an `ON DELETE` rule is in place to update or delete child rows.
* `post_update`: If a `flush()` operation returns a "cyclical dependency" error, you may use `post_update` to break the cycle.
* `secondaryjoin`: Expression used as the join of an association table to the child object, while `primaryjoin` uses the parent object.
* `single_parent`: Installs a validator to ensure only one parent; its usage is required if `delete-orphan` cascade is set.
* `viewonly`: Defaults to `False`; if `True`, the relationship is only for loading, and has no effect on the unit-of-work flush process.

### Working with Engines and Connections

http://docs.sqlalchemy.org/en/rel_0_7/core/connections.html

* An `Engine` instance manages many DBAPI connections and is intended to be called upon in a concurrent fashion.
* A `ResultProxy` that returns no rows, such as that of an `UPDATE` statement, relases cursor resources immediately upon construction.
* Closing a `ResultProxy` also closes the `Connection`, returning the DBAPI connection to the pool and releasing transactional resources.
* Nesting transaction blocks creates a new transaction if one is not available, or participates in an enclosing transaction if one exists.
* PEP-0249 says that a transaction is always in progress, so `autocommit=True` issues a `COMMIT` automatically if one was not created.
* Implicit execution is discouraged and is not appropriate for use with the ORM, as it bypasses the transactional context of `Session`.

### Collection Configuration and Techniques

http://docs.sqlalchemy.org/en/rel_0_7/orm/collections.html

* By default, when a parent is deleted, the `Session` loads all its children, to either delete them or set their foreign key to `NULL`.
* A dynamic form of `relationship` returns a `Query` object in place of a collection when accessed, and works with collections only.
* Using `passive_deletes` won't load children when a parent is marked for deletion; `ON DELETE CASCADE` can then delegate integrity to the database.
* Sets, mutable sequences, or almost any other container-like class can be used in place of the default list for a `relationship()`.
* When using a dictionary collection, it's easiest to use `attribute_mapped_collection` with a `@property`.
* Dictionary mappings are often combined with the association proxy extension to produce streamlined dictionary views.
* Lists, sets, and dictionaries and their subclasses are automatically instrumented; add the `__emulates__` class attribute to all others.
* The `appender`, `remover`, and `iterator` decorators adapt any container class with different method names than its emulated class.

### Relationship Loading Techniques

http://docs.sqlalchemy.org/en/rel_0_7/orm/loading.html

* The loader strategy specified by the `lazy` keyword for a `relationship()` can be overridden on a per-query basis.
* Dot-separated names apply only to the acutal attribute named; to also apply to ancestors, use `joinedload_all()` or `subqueryload_all()`.
* Passing `*` to a loader option sets a default strategy, but it does not supercede other loader options stated in the query.
* `joinedload()` creates an anonymous alias of the joins it adds, so they can't be referenced by other parts of the query.
* Queries with modifiers affecting the rows returned are run as subqueries, to which joins for eager loading are then applied to.
* In a one-to-many relationship:
	* A `LEFT OUTER JOIN` returns rows for empty collections and repeats parent columns for each child, so use it when collections are small.
	* A subquery load uses an `INNER JOIN` and a minimum of parent columns are returned, so use it for large collections.
* In a many-to-one relationship:
	* If a small set of target objects is referenced, then lazy-loading can exploit the caching property of the identity map.
	* If you know the foreign key reference is not `NULL`, you can perform a joined load with `innerjoin=True` which is very efficient.
* For a query with the necessary joins, option `contains_eager()` populates references and collections without a superfluous `joinedload()`.

##### API

* `contains_eager()`: Indicates that an attribute should be eagerly loaded from columns created by an explicit join in the query.
* `joinedload()`: Its joins are anonymously aliased, affecting how related objects or collections are loaded without impacting the results.

