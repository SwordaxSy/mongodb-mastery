# 10. Indexes

## 10.1 Intoduction to Indexes

Indexes are special data structures that store a small portion of the collection's data in an order form that is easy to query and search efficiently.

Indexes improve query performance, they:

-   Speed up queries
-   Reduce disk I/O
-   Reduce required resources

By default, only one index is created per collection, which includes the \_id field.
Note that every query should use an index.

#### Performance Cost

When inserting new documents or updating existing ones, the data in the index structure should be updated as well. Also, performance degrades if there are too many indexes in a collection.

Unncessary redunant indexes should be deleted.

#### Types of Indexes

-   Single field indexes: indexes on one field only
-   Compound indexes: indexes on more that one field

Indexes can be called _multikey_ field indexes if they operate on an array field

## 10.2 Single Field Indexes

To create an index, use the `db.<collection>.createIndex()` method.

Pass into the method an object with a key name of the field the index will be based on.

Specify a value for the key. The value will specify if the index order is ascending or descending, which should be 1 for ascending or -1 for descending.

```js
db.collection.createIndex({ fieldName: 1 });
```

The order of the index matters when creating compound indexes to perform sorts on multiple fields.

#### The Unique Option

Another argument can be passed to the `createIndex()` method, which is an options object.

One option that can be passed is `unique: true`. This specifies that the values of the indexed field are going to be unique across the collection. This option helps with querying and preventing duplication.

After specifying that the value of a specific field is going to be unique in the `createIndex()` method. If an attempt to insert a new document with that unique field having a duplicate value similar to a pre-existing document, MongoDB returns a duplicate key error and the insertion process fails.

#### Show Collection Indexes

The `db.<collection>.getIndexes()` method can be used to get the indexes associated with a specific collection.

Another way to view the indexes of a specific collection is thorugh MongoDB Atlas UI or MongoDB Compass.

## 10.3 Multikey Indexes

A multikey index is an index defined on an array field. A multikey index can be defined in a single field or a compound index.

If an index has multiple fields, only one of them can be an array field.

```js
db.collection.createIndex({ arrayField: 1 });
```

## 10.4 Compound Indexes

Compound indexes are indexes that contain more than one field. They can be created the same way single field indexes are created, using the `createIndex()` method, just with extra fields listed.

The order of fields listed during the index creation matters, as the first listed field is considered to be the source field of the index.

If a query on a collection does not include the source field of a compound index, the index will not be used for that query even if some of the fields included in the index are present in the query.

```js
db.collection.createIndex({ name: 1, age: 1 });
// the source field in the index is the "name" field since it comes first in order

db.collection.find({ name: "Swordax" }); // will use the index
db.collection.find({ name: "Swordax", age: 17 }); // will use the index
db.collection.find({ age: 17 }); // will not use the index
```

## 10.5 Deleting Indexes

Side effects of indexes:

-   Write cost (lower writing performance)
-   Too many indexes in a collection can affect system performance

Unused & redundant indexes should be deleted.

Any index on a collection can be deleted except for the \_id field index.

Two methods can be used to delete indexes.
`db.<collection>.dropIndex()` for deleting a single index
`db.<collection>.dropIndexes()` for deleting one or more indexes.

The `dropIndex()` method accepts a string with the name of the index to be deleted.

The `dropIndexes()` method accepts a string with the name of the index to be deleted, or any array of names of indexes to be deleted. If no argument was provided to this method, it will delete all indexes except the \_id index.

```js
db.collection.dropIndex("index_name"); // drops a single index
db.collection.dropIndexes(); // drops all indexes except _id index
db.collection.dropIndexes("index_name"); // drops a single index
db.collection.dropIndexes(["index_1", "index_2"]); // drops multiple indexes
```
