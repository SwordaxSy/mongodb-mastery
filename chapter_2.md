# 2. Connecting to MongoDB

## 2.1 Creating an Atlas Cluster

To start, create a MongoDB Atlas account, then create your first cluster through the Atlas web UI. The M0 cluster plan is the free tier for starters. There are higher plans that can suit higher and bigger needs.

After creating the cluster, we can create our first database and collection. We can manage collections and documents through the Atlas web UI easily. Insert, query, update, delete, and more operations can be carried out through the Atlas web UI.

## 2.2 Connecting to Atlas

We can connect to our Atlas cluster to manage and work with the data in our databases through MongoDB Shell, MongoDB Compass, or our application code.

A button can be found in Atlas web UI that says connect, there can be found the connection string associated with instructions on how to connect through various ways.

To connect to our database we will need to create a database access users with a username, a password, and roles. This can be managed in the security section, database access tab in the Atlas UI.

We will also need to allow access to the database from specific IP addresses, this can be managed thorugh the security section, network access tab in the Atlas UI.

Afterwards to connect, we will get the connection string and follow instructions provided to connect appropriately.

## 2.3 Connecting to MongoDB Shell

We can use the connection string as instructed to connect to MongoDB Shell. Through the shell we can interact with the database and apply commands to manage the data and perform CRUD operations and more.

Here are some of the basic MongoDB Shell navigation commands available after connecting to MongoDB database.

To navigate through databases in our connection, we can use the following command:

```shell
use <database name>
```

To show a list of available databases in our connection, we can use the following command:

```shell
show databases
```

To show a list of collections inside the current database, we can use the following command:

```shell
show collections
```

To clear the CLI screen, we can use the following command:

```shell
cls
```

To exit or quit the CLI, we can use the following commands:

```shell
quit
```

or

```shell
exit
```

Remember that MongoDB shell is based on JavaScript, so we can run any JavaScript we would like through the shell

```shell
const name = "Swordax";
```

```shell
for (let i = 0; i < 10; i++) console.log(name);
```
