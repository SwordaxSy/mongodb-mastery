# 1. Introduction to MongoDB

## 1.1 What is MongoDB

MongoDB is a NoSQL database for storing data. MongoDB stores data in collection and documents unlike SQL databases such as MySQL that store data in tables and records.

<table>
    <tr>
        <th>MongoDB</th>
        <th>MySQL</th>
    </tr>
    <tr>
        <td>
            Stores in collections
        </td>
        <td>
            Stores in tables
        </td>
    </tr>
    <tr>
        <td>
            Stores in documents
        </td>
        <td>
            Stores in records
        </td>
    </tr>
    <tr>
        <td>
            BSON Objects
        </td>
        <td>
            Rows and Columns
        </td>
    </tr>
    <tr>
        <td>
            NoSQL
        </td>
        <td>
            Uses SQL
        </td>
    </tr>
</table>

## 1.2 The Document Model

MongoDB stores data in BSON objects, which are basically JSON objects with more data types supported.

Storing data in MongoDB databases as BSON objects allows you to store JavaScript objects, which gives you flexibility when modeling your data.

You can store various data types such as strings, numbers, booleans, arrays, nested documents (objects), and even more.

See the following collection with two documents stored

```js
[
    {
        _id: ObjectId("60be2a3b6677f516487a0a8b"),
        name: "Swordax",
        age: 17,
        isMarried: false,
        hobbies: ["Chess", "Coding", "Swimming"],
    },
    {
        _id: ObjectId("60be2a3b6677f516487a0a8a"),
        name: "Vazox",
        age: 18,
        isMarried: true,
        hobbies: ["Reading", "Running", "Ping Pong"],
    },
];
```

## 1.3 The Document Model : ObjectId

As seen in the given collection sample, every document has an \_id field. This field is automatically added by MongoDB, and it is of a special data type supported by BSON objects which is the ObjectId data type.

ObjectId is a random universally unique identification string given to every document in the database to distinguish it.

## 1.4 MongoDB Atlas

MongoDB Atlas is a fully managed cloud database service. It allows you to create clusters of databases and configure them as you like, as well as manage and visualize data in your databases.

MongoDB Atlas allows you to create databases that will be stored on the cloud without having to worry about hosting the database. All you have to do is connect to the database through MongoDB Shell, MongoDB Compass, or through your application.

You can find connection strings in MongoDB Atlas platform, and you can use these strings to connect to your data clusters.

## 1.5 MongoDB Shell

MongoDB Shell is a CLI (Command Line Interface) that allows developers to interact with MongoDB thorugh a JavaScript based interface.

## 1.6 MongoDB Compass

MongoDB Compass is a GUI (Graphical User Interface) that allows developers to interact with MongoDB and manage, visualize, and analyze data through an easy-to-use graphical user interface.

## 1.7 MongoDB Drivers

MongoDB can be used as a database for any kind of application, such as web applications, desktop applications, mobile applications, or other forms of apps.

MongoDB databases can be interacted with using many programming languages with the help of drivers.

Drivers are libraries that can me installed and imported in any programming language. They provide a simple to use APIs to manage data through application code.

MongoDB drivers are available for many programming langauges such as Python, C#, Node.js, Java, and much more.
