## SQLAlchemy 0.7.8 Documentation

http://docs.sqlalchemy.org/en/rel_0_7/index.html

### Object Relational Tutorial

http://docs.sqlalchemy.org/en/rel_0_7/orm/tutorial.html

* The SQL Expression Language represents the relational database directly without opinion; the ORM presents a high level and abstracted pattern of usage built upon it.
* The ORM approaches structure and content of data from a user-defined domain model which is transparently persisted and refreshed from its underlying storage model.
* An `Engine` instance is a core interface to the database, adapted to the DBAPI in use.
* The declarative base class maintains a catalog of classes and tables relative to that base.
* SQLAlchemy never makes assumptions about names or characteristics of data; the developer must always be explicit about specific conventions in use.
* The declarative base class has a `metadata` attribute that contains a catalog of all the `Table` objects that have been defined.
* The declarative system supplies a default constructor which accepts keyword arguments of the same name as that of the mapped attributes.
* The `Session` object is just a workspace for your objects, local to a particular database connection.
* Adding an entity to a `Session` does not persist it to the database; this is only done when the session is flushed.
* Using the identity map, once an object with a particular primary key is in a `Session`, all queries for that particular primary key return that object.
* SQLAlchemy by default refreshes data from a previous transaction the first time it’s accessed within a new transaction, so that the most recent state is available.
* The tuples returned by a `Query` object are named tuples, and can be treated much like an ordinary Python object.
* A `Query` object is fully generative, so most method calls return a new `Query` object upon which further criteria may be added.
* The `one()` method of `Query` fully fetches all rows, and if not exactly one object identity or composite row is returned, raises an error.
* The `relationship()` method uses the foreign key relationships between the two tables to determine the nature of this linkage.
* A foreign key constraint in most (though not all) relational databases can only link to a primary key column, or a column that has a `UNIQUE` constraint.
* A foreign key constraint that refers to a multiple column primary key, or a subset of one, and itself has multiple columns, is known as a composite foreign key.
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
* A `Session` is similar ot a cache in that it implements the identity map, but only uses the map as a cache if you query by a primary key.
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

