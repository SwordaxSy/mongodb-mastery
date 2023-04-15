# 3. CRUD Operations : Insert and Find

## 3.1 Intro to CRUD Operations

CRUD (Create, Read, Update, Delete) operations can be performed on MongoDB databases through the Atlas UI, MongoDB Shell, MongoDB Compass, or our application code with the help of the programming language's specific MongoDB driver.

We will learn how to use CRUD commands in the shell, which can later be used quite similarly with the Node.js driver in our application code.

## 3.2 Insert Operations

#### `db.<collection>.insertOne()`

This method can be used to insert a single document into the collection. Acceps a single document (object) as an argument.
Use example:

```js
db.collection.insertOne({
    name: "Swordax",
    age: 17,
    isMarried: false,
    hobbies: ["Coding", "Chess", "Swimming"],
});
```

#### `db.<collection>.insertMany()`

This method can be used to insert multiple documents into the collection. Acceps an array of documents (objects) as an argument.
Use example:

```js
db.collection.insertMany([
    {
        name: "Swordax",
        age: 17,
    },
    {
        name: "Vazox",
        age: 26,
    },
    {
        name: "Alxa",
        age: 21,
    },
]);
```

## 3.3 Find Operations

#### `db.<collection>.find()`

This method can be used to find all documents that match the specified queries. Accepts a query object.
Use example:

```js
db.collection.find({ age: 17 });
```

#### `db.<collection>.findOne()`

This method can be used to find a single document that matches the specified queries. Acceps a query object.
Use example:

```js
db.collection.findOne({ name: "Swordax" });
```

We can also query using the \_id field with the ObjectId constructor:

```js
db.collection.findOne({ _id: ObjectId("6282afeb441a74a98dbbec4e") });
```

## 3.4 Find Operations : Projection Option

The find methods `find()` and `findOne()` can accept an additional argument after the query object, which is the projection argument.

The projection argument is an object that specifies what fields to include when retrieving documents from the database.

Suppose the following data schema:
`student collection`

```js
{
    _id: ObjectId,
    student_name: string,
    student_number: number,
    student_grade: string,
    is_active: boolean,
    classes: Array
}
```

Projection in finding (example):

```js
db.students.find(
    { grade: "12A" },
    {
        _id: 0,
        student_name: 1,
        classes: 1,
    }
);
```

#### Projection Object Explained

The fields specified in the projection object decide if the field will be included or excluded in the result.

Fields specified with 1 will be included, others will be exluded.

Fields specified with 0 will be exlucded, others will be included.

It is not allowed to specify fields with 1 and others with 0 in the same projection operation, this rule does not apply on the \_id field.

\_id field will always be included even if not set to 1 unless it was explicitly set to 0

## 3.5 Find Operations : Query Comparison Operators

You can use query operators in your query objects when finding documents with `find()` and `findOne()` methods to create more complex query criteria.

Example:

```js
db.users.find({
    { age: { $gte: 18 } }
})
```

The previous example will return all the users who's ages are greater than or equal to 18. This was achieved by using the $gte query comparison operator.

Here is a list of similar query operators than can be used to create such queries:

```js
db.users.find({ age: { $lt: 18 } }); // less than
db.users.find({ age: { $gt: 17 } }); // greater than
db.users.find({ age: { $lte: 17 } }); // less than or equal to
db.users.find({ age: { $gte: 18 } }); // greater than or equal to
db.users.find({ age: { $eq: 18 } }); // equal to
db.users.find({ age: { $ne: 18 } }); // not equal to
db.users.find({ age: { $in: [17, 18, 19] } }); // value should be any of these
db.users.find({ age: { $nin: [17, 18, 19] } }); // value shouldn't be any of these
```

Remember that you can always combine operators in the same query to perform strong and complex query operations.

## 3.6 Find Operations : Query Logical Operators

Query logical operators can be used to combine multiple queries using the `and` or `or` relations.

The `$and` query logical operator accepts an array of queries that will all be applied together in the search.

The `$or` query logical operator accepts an array of queries that will be separately applied in the search.

Usage:

```js
db.users.find({ $or: [{ name: "Swordax" }, { age: { $gte: 18 } }] }); // both queries will be used separately
db.users.find({
    $and: [{ age: { $gte: 24 } }, { hobbies: { $in: ["Chess", "Swimming"] } }],
}); // both queries will be used together
```

The `$and` operator can be implicitly applied in the following way:

```js
db.users.findOne({ name: "Swordax", age: 17 });
```

Remember that you can always combine operators in the same query to perform strong and complex query operations. Here is an example:

```js
db.users.findOne({
    $and: [
        { $or: [{ name: "Swordax" }, { age: 17 }] },
        { $or: [{ is_available: true }, { is_married: false }] },
    ],
});
```

## 3.7 Find Operations : Array Query Operators

Array query operators can be used to apply queries on array fields.

When specifying in the query that an array field should be a specific string value, the query will look for docs where the array field includes that string value.

When specifying in the query that an array field should be an array with a specific value, the query will look for docs where the array field is exactly equivalent to the specified array in the query.

The `$in` operator can be used to query documents where a specific array field includes any of the specified values.

The `$nin` operator can be used to query documents where a specific array field doesn't include all the specific values.

The `$all` operator can be used to query documents where a specific array field include all the specified values.

The `$elemMatch` operator can be used to query documents where a specific array field includes a subdocument that matches the given criteria.

Usage examples:

```js
db.users.find({ hobbies: "Swimming" }); // hobbies array includes "Swimming"
db.users.find({ hobbies: ["Swimming"] }); // hobbies array is ["Swimming"] only
db.users.find({ hobbies: { $in: ["Swimming", "Gaming"] } }); // hobbies array includes "Swimming" or "Gaming"
db.users.find({ hobbies: { $nin: ["Swimming", "Gaming"] } }); // hobbies array shouldn't include "Swimming" nor "Gaming"
db.users.find({ hobbies: { $all: ["Swimming", "Gaming"] } }); // hobbies array includes both "Swimming" and "Gaming"
db.users.find({
    qualifications: { $elemMatch: { type: "University", gpa: { $gte: 3.5 } } },
}); // qualifications array should include a subdocument where the type field is "University" and the gpa field is greater than or equal to 3.5
```

Remember that you can always combine operators in the same query to perform strong and complex query operations.

## 3.8 Find Operations : Nested Queries

As we know, MongoDB stores data in BSON formats, similar to JSON. That allows the data structure to include arrays and even nested objects or nested documents (documents inside documents).

Assume the following data schema:
`students collection`

```js
{
    _id: ObjectId,
    student_name: string,
    student_contacts: {
        mobile: string,
        email: string,
    }
}
```

We can query for a specific email this way:

```js
db.students.findOne({ "student_contacts.email": "student_112@school.com" });
```

Notice using double quotes in the field key name.
