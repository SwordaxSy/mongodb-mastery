# 5. CRUD Operations : Modifying Query Results

## 5.1 Cursor Methods

The `db.<collection>.find()` method does not actually return the data, but returns a cursor to the data. There are methods attached to that cursor that allow us to perform several operations and modifications on the data.

The cursor methods are somewhat similar to Array methods, but are specifically designed to process MongoDB data and enhance performance when it comes to large amounts of data.

Some of the cursor methods:

-   `.forEach()` allows us to perform a specific function for each document
-   `.toArray()` converts the cursor to an array of documents
-   `.sort()` sorts the data
-   `.limit()` limits the returned amount of data to a specific number of documents
-   `.skip()` skips a number of document

Cursor methods are a lot more, and include array-like methods such as `.every()`, `.some()`, `.map()` and lots more.

Most cursor methods do not process all data at once, but instead process data in batches of 101 documents per time. This is to enhance performance when it comes to large amounts of data. The `.toArray()` method instantly converts all the data retrieved to a plain JavaScript array, which could be bad for performance when it comes to large amounts of data.

## 5.2 Sorting

We can use the `.sort()` cursor method to sort the data in a specific order.

Use example:

```js
db.users.find({}).sort({ age: 1 });
```

The previous example sorts users based on the age field.

1 is for ascending order, -1 is for descending order.

The `.sort()` method can sort based on numerical fields, and on string fields as well by sorting alphabetically.

## 5.3 Limiting

We can use the `.limit()` cursor method to limit the number of documents to be returned from the `.find()` method.

This can be used for many purposes:

-   Get a specific amount of results
-   Implement pagination
-   Enhance performance when working with huge amounts of data

Use example:

```js
db.users.find({}).limit(5);
// gets a max of 5 documents only
```

We can combine the `.sort()` and the `.limit()` method to implement something such as getting the top or bottom documents based on a specific field. Here is an example:

```js
db.movies.find({}).sort({ rating: -1 }).limit(3);
```

The previous example sorts movies based on their rating in a descending order, and then limits the results to 3 documents only. This would get the top 3 rated movies in the `movies` collection.

## 5.4 Counting Documents

We can use the `.countDocuments()` method on the collection to count the number of documents it contains.

This method accepts an optional query argument, and an optional options argumet.

The query argument can be used to cut the documents count based on specified criteria.

```js
db.users.countDocuments();
// returns total documents count

db.users.countDocuments({ age: { $gte: 18 } });
// returns count of documents where user age field is greater than or equal to 18
```
