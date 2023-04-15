# 9. Aggregation : Node.js

# 9.1 Aggregation with Node.js Driver

To create an aggregation pipeline, use the `collection.aggregate()` method in the Node.js driver, pass an array of aggregation stages into the aggregate method to perform aggregation process on data.

```js
const aggregationPipeline = [
    {
        $match: {
            age: {
                $gte: 18,
            },
        },
    },
    {
        $sort: {
            age: 1,
        },
    },
    {
        $limit: 10,
    },
    {
        $project: {
            name: 1,
            age: 1,
            user_info: 1,
            _id: 0,
        },
    },
    {
        $group: {
            _id: "$user_info.location",
            avg_age: {
                $avg: "$age",
            },
        },
    },
    {
        $out: "analysis",
    },
];

await usersCollection.aggregate(aggregationPipeline);
```

For more on aggregation
Refer to chapter 6. Aggregation
