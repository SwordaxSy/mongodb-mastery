# 7. Connecting to MongoDB : Node.js

## 7.1 Node.js MongoDB Driver

Node.js MongoDB driver is a library or a piece of middleware that provides a way for Node.js application to interact with MongoDB databases through code. It allows developers to perform CRUD operations on MongoDB databases through code.

The driver provides a set of APIs that allows Node.js apps to establish connections to MongoDB servers, create, read, update, and delete documents in MongoDB database collections. It also supports various database commands, indexing, aggregation, and transactions.

The MongoDB Node.js driver is written in JavaScript and is available for installation through npm.

```shell
npm install mongodb
```

## 7.2 Connecting to MongoDB Atlas in Node.js apps

#### Atlas Instructions

To connect to MongoDB Atlas through your Node.js application. Get the application connection string from the MongoDB Atlas web UI. Then insert into the connection string the required information, such as the username, password, and database name.

Remember to create a database user through the database access tab to be able to connect to the database with a username and a password. Also remember to allow access by specific IP addresses to be able to connect to the database through the network access tab.

On production, IP addresses for DB network access should not be set to `ALLOW ACCESS FROM ANYWHERE`. Only the IP address of the server and the authorized IP addresses should be listed for network access.

#### Application Instructions

After installing `mongodb` driver with npm. Import `MongoClient` into your code file where you will establish the connection.

Also import your connection string from wherever you're storing it.

```js
const { MongoClient } = require("mongodb");
const db_connection_string = require("./db_connection_string.js");
```

Then create a new client instance.

```js
const client = new MongoClient(db_connection_string);
```

Now we can connect to the database with the following code.

```js
const connectToDatabase = async () => {
    try {
        await client.connect();
        console.log("Successfully connected to MongoDB database");
    } catch (err) {
        console.error(
            `An error occured while connecting to MongoDB databse: ${err}`
        );
    }
};

const main = async () => {
    try {
        await connectToDatabase();
    } catch (err) {
        console.error(
            `An error occured while connecting to MongoDB database: ${err}`
        );
    } finally {
        await client.close();
    }
};

main();
```

Note: an application should use a single MongoClient instance for all database requests throughout the life of the application, and that's because creating MongoClient instances is resource extensive. Mass creating clients multiple times throughtout the life of the application may negatively affect app performance.
