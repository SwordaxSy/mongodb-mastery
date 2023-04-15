# 8. CRUD Operations : Node.js

## 8.1 BSON Docs

MongoDB stores data in BSON format (Binary JavaScript Object Notation). BSON is basically JSON with supersets.

BSON objects are more effecient for storage, security, and also support more data types than raw JSON objects such as ObjectId data type.

BSON Features:

-   Binary encoded
-   Allow more data types than JSON
-   Easier to store and secure

When working with MongoDB Node.js driver, we will need to structure our data document before trying to insert it to our database. We can simply insert JavaScript objects to our database through the MongoDB driver. Example:

```js
const document = {
    _id: new ObjectId(""),
    name: "Swordax",
    age: 17,
    isMarried: false,
    createdAt: new Date(),
};

const result = await client
    .db("application")
    .collection("users")
    .insertOne(document);
```

Remember that MongoDB automatically adds "\_id" fields even if you did not add them in your document. That's why you can omit the "\_id" addition when structuring documents for insertion.

If you ever needed to explicitly add the "\_id" field in your document in Node.js code. You can import it from "mongodb" library.

Example:

```js
const { MongoClient, ObjectId } = require("mongodb");
const objectIdString = new ObjectId("603d2388bdfce245b06934d9");
```

## 8.2 Insert Operations with Node.js driver

#### `insertOne()`

Used to insert a single document into a collection

```js
const userDocument = {
    username: "Swordax",
    password: "password123",
    age: 17,
    isMarried: false,
};

const result = await client
    .db("application")
    .collection("users")
    .insertOne(userDocument);
```

#### `insertMany()`

Used to insert multiple documents into a collection

```js
const userDocuments = [
    {
        username: "Vazox",
        password: "vazox123",
    },
    {
        username: "Alxa",
        password: "alxa123",
    },
];

const result = await client
    .db("application")
    .collection("users")
    .insertMany(userDocuments);
```

## 8.3 Find Operations with Node.js driver

#### `find()`

Used to find documents based on a provided query

```js
const resultCursor = collection.find(query);
const resultArray = await resultCursor.toArray();
const resultCount = await collection.countDocuments(query);
await result.forEach((doc) => console.log(doc));
```

#### `findOne()`

Used to find a single document based on a provided query

```js
const resultDocument = await collection.findOne(query);
console.log(resultDocument);
```

Notice using `await` with `findOne()` but not with `find()`. That is because `find()` instantly returns a cursor and not a promise, while `findOne()` returns a promise that then returns the document object.

We use `await` with cursor methods because they return promises, such as the `.toArray()` and `.forEach()` methods.

## 8.4 Update Operations with Node.js driver

#### `updateOne()`

Updates a single document that matches the given query

```js
const query = { username: "Target_Username" };
const update = { $inc: { "accountInfo.accountLevel": 1 } };

await collection.updateOne(query, update);
```

#### `updateMany()`

Updates multiple documents that match the givern query

```js
const query = { account_status: "active" };
const update = { $push: { badges: "Ultimate Activity" } };

await collection.updateMany(query, update);
```

## 8.5 Delete Operations with Node.js driver

#### `deleteOne()`

Deletes a single document that matches the given query

```js
const query = { username: "Vazox" };
await collection.deleteOne(query);
```

#### `deleteMany()`

Deletes multiple documents that matches the given query

```js
const query = { account_type: "inactive" };
await collection.deleteMany(query);
```

## 8.6 Transactions with Node.js driver

Transactions ensure that multiple CRUD operations complete all together without any errors, or none of them completes at all.

Transactions are useful when performing multiple important related CRUD operation that should all succeed together.

How things usually go, is that separate CRUD operations run independently. So if one operations fails, others may succeed without being interrupted. That wouldn't be a great thing if the operations were related and their success depends on each other.

Here is a full example that demonstrates how to create a transaction with MongoDB Node.js driver:

```js
// imports
const { MongoClient } = require("mongodb");
const db_uri = require("./db_uri");

// create connection
const client = new MongoClient(db_uri);
const accounts = client.db("bank").collection("accounts");
const transfers = client.db("bank").collection("transfers");

// start client session
const session = client.startSession();

const main = async () => {
    try {
        const transactionResults = await session.withTransaction(async () => {
            // first operation
            await accounts.updateOne(
                { account_id: "123" },
                { $inc: { balance: 100 } },
                { session: session }
            );

            // second operation
            await accounts.updateOne(
                { account_id: "456" },
                { $inc: { balance: -100 } },
                { session: session }
            );

            // third operation
            await transfer.insertOne(
                {
                    transfer_id: 123456,
                    amount: 100,
                    from: 123,
                    to: 456,
                },
                { session: session }
            );
        });

        // check for transaction success
        if (transactionResults) {
            console.log("Transaction successful");
        } else {
            console.log("Transaction failure");
        }
    } catch (err) {
        console.error(`Transaction failed: ${err}`);
    } finally {
        await session.endSession();
        await client.close();
    }
};

main();
```
