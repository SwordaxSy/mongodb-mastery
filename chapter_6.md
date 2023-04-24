# 6. Aggregation

## 6.1 Introduction to MongoDB Aggregation

#### Definitions

-   Aggregation: is the analysis and summary of data
-   Stage: is an aggregation operation performed on data
-   Pipeline: is a series of stages completed one at a time in order

#### Explanation

Many different operations can be performed in an aggregation pipeline, examples:

-   Filtering data
-   Sorting data
-   Grouping data
-   Transforming data

Each operation is called a stage, those stages can be chained in a pipeline to be performed on data in order

The output of one stage is the input of the next stage

## 6.2 Aggregation Syntax

Creating an aggregation pipeline can be done using the `aggregate()` method. Here is an example:

```js
db.<collection>.aggregate([
    { $stageName: { <expression> } },
    { $stageName: { <expression> } }
])
```

The aggregate method accepts an array. Each item inside the array is an aggregation stage to be performed on data

## 6.3 Aggregation Stages

Here are some of the most popular stages that can be ised in MongoDB aggregation:

-   `$match`: filters for data that matches a criteria
-   `$group`: groups documents based on specified criteria
-   `$sort`: puts documents in a specified order

Here is an example of using the `$match` stage:

```js
db.student.aggretage([{ $match: { student_name: "Swordax" } }]);
```

The above example returns data where the "student_name" field is equal to "Swordax"

The `$match` stage accepts a find query, which means query operators can be used inside this stage as well. Example:

```js
db.student.aggregate([{ $match: { student_age: { $gte: 18 } } }]);
```

The above example returns data where the "student_age" field is more than or equal to 18

Here is another example of an aggregation:

```js
db.users.aggregate([
    {
        $set: {
            defaultUsername: {
                $concat: ["$first_name", "$last_name"],
            },
        },
    },
]);
```

The above example uses the `$set` aggregation stage to add a new field to the returned documents: the "defaultUsername" field, which contains a concatenated value of the two fields "first_name" and "last_name".

The changes done by aggregation stages do not affect the original data in the database but only the data returned out of the aggregation pipeline.

## 6.4 The Match Stage

The `$match` stage filters for documents matching specified criteria. It takes one argument which is a query object, just like the `find()` method query object.

Be sure to place the match stage as early as possible in the pipeline to reduce the amouont of data to be processed by the next pipeline stages.

Usage example:

```js
db.users.aggregate([
    {
        $match: { user_location: "Asia" },
    },
]);
```

The previous aggregation uses the `$match` stage to filter for users who are located in "Asia"

## 6.5 The Group Stage

The `$group` stage allows you to group documents based on a specified field. After grouping the documents, it can perform various aggregation on the grouped data.

Usage example:

```js
db.users.aggregate([
    {
        $group: {
            _id: "$user_location",
            median_age: { $avg: "$user_age" },
        },
    },
]);
```

The above example will return a list of documents, each document represents a group of documents in the database.

Each document will contain an "\_id" field which represents the field the grouping was done based on.

Each document will have a "median_age" field which will be the average of the "user_age" fields of that specific group of user documents.

The average was calculated using the `$avg` accumulation operator

## 6.6 Accumulation Operators

As seen in the previous lesson, the $group stage can be combined with accumulation operators to perform grouping and calculations on groups

Accumulation operators are not narrowed for the `$group` stage only, they can be used and combined with other stages.

We have seen how the `$avg` accumulation operator works. Here is a list of other available accumulation operators:

-   `$sum`: calculates the sum of values of a specific field in a group
-   `$avg`: calculates the average of values of a specific field in a group
-   `$min`: finds the minimum value of a specific field in a group
-   `$max`: finds the maximum value of a specific field in a group
-   `$first`: finds the first item of a specific field in a group
-   `$last`: finds the last item of a specific field in a group
-   `$count`: counts the number of documents in a group
-   `$concat`: concatenates the values of multiple fields into a single string

There are many other accumulation operators available for many other use cases

## 6.7 The Sort Stage

As the name of suggests. The `$sort` stage is used to sort data based on a specific field in an ascending or a descending order

1 is for ascending order, -1 is for descending order

Example: let's say we have a collection with documents that follow the following data schema:

```js
{
    _id: ObjectId,
    name: string,
    age: number
}
```

We can sort the data of the previouos schema based on user ages by using the sort aggregation stage in the following way:

```js
db.users.aggregate([{ $sort: { age: 1 } }]);
```

The above sort aggregation stage will sort the data based on the "age" field in an ascending order

We can also sort based on character alphabetic order if we set the sorting to depend on a string field

## 6.8 The Limit Stage

The `$limit` stage limits the number of documents that will pass to the next aggregation stage. Similar to the `.limit()` cursor method

Usage example:

```js
db.users.aggregate([{ $limit: 3 }]);
```

The above example will limit the data to a maximum of 3 documents only

The limit aggregation operator will return the data on top first

## 6.9 Pipeline Stages Order

Remember that the order of the stages in the pipeline matters! For example, if we want to return the top 3 user documents with the highest ages, we should use the sort aggregation stage first, and then the limit aggregation stage to return the top 3. Otherwise if we did the opposite and used limiting aggregation before sorting, then this will return unexpected false results

Correct example:

```js
db.users.aggregate([{ $sort: { age: 1 } }, { $limit: 3 }]);
```

## 6.10 The Project Stage

The `$project` stage determines the outpit document shape. It allows us to include or exclude fields in the output document.

Similar to the projection applied in the `.find()` and `.findOne()` operations.

The project stage should usually be one of the last stages in the pipeline.

Syntax:

```js
{
    $project: {
        <field1>: <value>,
        <field2>: <value>
    }
}
```

#### Projection Rules

-   The `<value>` should be either 1 or 0
-   1 for including, 0 for excluding
-   Inclusion cannot be combined with exclusion at the same projection stage, this rule doesn't apply on the "\_id" field.
-   If a field's value was set to 1, the default value for the rest of the fields would be set to 0 unless explicitly set to 1
-   If a field's value was set to 0, the default value for the rest of the fields would be set to 1 unless explicitly set to 0
-   The "\_id" field will be always included even if not explicitly included in the projection stage
-   You can explicitly exclude the "\_id" field by setting it's `<value>` to 0

#### Projecting New Fields

You can project new fields that take values from existing fields in the document. Example:
`input document:`

```js
{
    _id: ObjectId,
    firstName: "Swordax",
    lastName: "Sy",
    age: 17,
    isMarried: false
}
```

`project stage:`

```js
{
    $project: {
        fName: "$firstName",
        lName: "$lastName",
        isMarried: 1,
        _id: 0
    }
}
```

`output document:`

```js
{
    fName: "Swordax",
    lName: "Sy",
    isMarried: false
}
```

## 6.11 The Set Stage

The `$set` stage adds or modifies fields in the pipeline

Syntax:

```js
{
    $set: {
        <field1>: <value>,
        <field2>: <value>,
        ...
    }
}
```

## 6.12 The Count Stage

The `$count` stage is used to count documents in the pipeline

Syntax:

```js
{
    $count: <value>
}
```

The value is going to be a string with the count name

Here is an example:

```js
db.users.aggregate([{ $count: "total_users" }]);
```

The above example returns the following document:

```js
{ total_users: <number_of_documents> }
```

## 6.13 The Out Stage

-   The `$out` stage writes documents that are returned by an aggregation pipeline into a collection

-   The out stage must be the last stage in a pipeline

-   The out stage creates a new collection and database if the specified ones do not exist

-   If the collection exists, the out stage replaces the existing data with new data

Syntax (1):

```js
{
    $out: {
        db: <database_name>,
        coll: <collection_name>
    }
}
```

Note that if the specified database or collection doesn't exist, it will be created

Syntax (2):

```js
{
    $out: <collection_name>
}
```

Using this syntax, the out stage will use the current database to output data into the collection
