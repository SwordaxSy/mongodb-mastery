# 4. CRUD Operations : Replace, Update, and Delete

## 4.1 Replace Operations

We can replace a document with another using the `replaceOne()` method.
The `replaceOne()` method accepts three arguments:

1. filter: a query object to target the document to be replaced
2. replacement: a document object that will take the place of the target document
3. options: an optional object that specifies replacement options

Use example:

```js
db.users.replaceOne(
    {
        _id: ObjectId("603d6c2e16224fbd6c0f6e56"),
    },
    {
        name: "SwordaxSy",
        age: 17,
        hobbies: ["Chess", "Coding", "Walking"],
    }
);
```

Note that the replacement document will have the same \_id value.

## 4.2 Update Operations

#### `db.<collection>.updateOne()`

This method performs updates on the specified fields in a single queried document.

This method accepts three arguments:

1. filter: a query object to target a single document
2. update: an update object to perform update operations on the document
3. options: an optional object that specifies update options

Use example:

```js
db.users.updateOne(
    { name: "Swordax" },
    {
        $set: { isMarried: false, isActive: true },
    }
);
```

We use the `$set` update operator to set fields and update them.

#### `db.<collection>.updateMany()`

This method performs updates on the specified fields in the queried documents.

This method accepts three arguments:

1. filter: a query object to target the desired documents
2. update: an update object to perform update operations on the documents
3. options: an optional object that specifies update options

Use example:

```js
db.users.updateMany(
    { age: { $gte: 18 } },
    {
        $set: { isAdult: true },
    }
);
```

#### The Upsert Option

The upsert option can be specified in the optional options object.
The upsert option, if set to true, creates a new document if not documents match the query.
Example:

```js
db.users.updateOne(
    { name: "Swordax" },
    { $set: { hobbies: ["Chess", "Coding"] } },
    { upsert: true }
);
```

## 4.3 Update Operators

There are many update operators that can be used to perform specific update operations such as incrementing numbers, pushing into arrays, or more.

We have seen one of the update operators previously, which is the basic `$set` operator.

Here are some other available update operators:

#### `$set` operator

Can be used to set the value of a field. If the field doesn't exist, it will be created

```js
db.users.updateOne({ name: "Swordax" }, { $set: { active: true } });
```

#### `$unset` operator

Can be used to remove a field from the document

```js
db.users.updateOne({ name: "Swordax" }, { $unset: { unwanted_field: "" } });
```

The specified value in the $unset expression does not impact the operation

#### `$inc` operator

Can be used to increment or decrement a number field

```js
db.users.updateOne(
    { name: "Swordax" },
    { $inc: { age: 1, years_to_graduate: -1 } }
);
// incremented "age" by 1 and decremented "years_to_graduate" by 1
```

#### `$push` operator

Can be used to push values to array fields

```js
db.users.updateOne({ name: "Swordax" }, { $push: { hobbies: "Reading" } });
// pushed "Reading" into "hobbies" array field
```

We can combine the `$push` operator with the `$each` operator to push multiple values into an array

```js
db.users.updateOne(
    { name: "Swordax",
    {
        $push: {
            hobbies: {
                $each: ["Chess", "Coding", "Swimming"]
            }
        }
    }
});
// pushed "Chess", "Coding", and "Swimming" into the "hobbies" array field
```

#### `$addToSet` operator

Can be used to push values to array fields only if they do not already exist

```js
db.users.updateOne({ name: "Swordax" }, { $addToSet: { hobbies: "Reading" } });
// pushes "Reading" into "hobbies" array field (only if it doesn't already exist in there)
```

The `$each` operator can also be combined with the `$addToSet` operator to add multiple values

Only the values that do not exist in the array field will be added by the `$addToSet` combined with the `$each` operator

#### `$pull` operator

Can be used to pull values from an array field

```js
db.users.updateOne({ name: "Swordax" }, { $pull: { hobbies: "Reading" } });
// pulled "Reading" out of the "hobbies" array field
```

Note that the `$pull` operator pulls all the occurrences of the matched criteria out of the array field.

We can combine the `$in` operator with the `$pull` operator to pull multiple values out of the array field

```js
db.users.updateOne(
    { name: "Swordax" },
    { $pull: { hobbies: { $in: ["Reading", "Chess", "Walking"] } } }
);
```

#### `$pop` operator

Can be used to pop the last/first element inside an array field

```js
db.users.updateOne(
    { name: "Swordax" },
    {
        $pop: {
            arrayOne: 1,
            arrayTwo: -1,
        },
    }
);
```

Fields with the value of 1 will have their last element popped out

Fields with the value of -1 will have their first element popped out

## 4.4 Delete Operations

#### `db.<collection>.deleteOne()`

This method deletes a single queried document

Use example:

```js
db.users.deleteOne({ _id: ObjectId("617ec7ca37a0c8d7e9e301c2") });
```

#### `db.<collection>.deleteMany()`

This method deletes multiple queried documents

Use example:

```js
db.users.deleteMany({ age: { $lt: 18 } });
```
