# 11. Atlas Search

## 11.1 Introduction to Atlas Search

Atlas search is a relevance-based search.
It searches records based on a search term. But not a search for a particular record.

Atlas search starts with search indexes, which are used to specify how the search algorithm should work with the data set. They are not the same as database indexes.

#### Difference Between Database Indexes and Search Indexes

**Database Indexes** are used by developers to make database queries easier and more efficient.

**Search Indexes** are used to specify how records are referenced for relevance-based search, and to specify how the search algorithm should work the the data set.

## 11.2 Components of Atlas Search Index

The components of an atlas search indexes contain:

1. Information about the analyzers that are being used
2. Type of mapping (dynamic or not)
3. Option to store whole docs in memory for faster post-aggregation performance
4. Field mappings

## 11.3 Create a Search Index with Dynamic Mapping

A search index with **Dynamic Mapping** means that when the search algorithm is run, all fields will be indexed with a few exceptions: booleans, ObjectId, timestamps.

In MongoDB atlas interface, click into the search tab where you can see all of the search indexes in the current database. you can create a new search index by clicking on the _Create Search Index_ button. The visual editor option will guide us through creating the search index. Select the correct database and collection for the search index
For a dynamic mapping search index, keep the _Dyanmic Mapping_ option on, save changes, and push the _Create Search Index_ button to create the search index.

After creating the search index, you can use the search tester to test the search index. After searching, _Score_ will be shown for each result, referring to the relevance rate calculated and assigned by the search engine.

## 11.4 Create a Search Index with Static Mapping

If you have data with a lot of fields with only some of them being relevant to the search, you can show relevant results by statically mapping certain fields. This type of search index is called **Static Indexing**.

In MongoDB atlas interface, click into the search tab where you can see all of the search indexes in the current database. You can create a new search index by clicking on the _Create Search Index_ button. The visual editor option will guide us through creating the search index. Select the correct database and collection for the search index.
For a static indexing, toggle off the _Dynamic Mapping_ option. Specify the fields you want to be used in the search index by using the _Add Field_ button under the _Field Mapping_ option. You can also set the data type of the field by specifying it under the _Data Type Configuration_, to do so, push the _Add Data Type_ button and set the data type of the field. At the end, push the _Create Search Index_ button to create the search index.

## 11.5 Using $search and Compound Operators

You can put the search index you created in use through an aggregation pipeline written in mongosh or in the monogdb driver of the language of your choice. For that, we need to create a `$search` stage in the aggregation pipeline.

#### Components and Options of the $search Stage

1. `index`: which search index to use
2. `highlight`: how much text to return with the search terms highlighted
3. `count`: number of total documents
4. The compount operator which includes it's own set of options

#### The Compouont Operator

The compound operator is nested in the $search stage. It includes clauses that specify the weight that different fields should have in the search ranking.

_Compount Operator Clauses_:

1. `must`: only include results that match the clause
2. `must not`: the negation of must
3. `should`: assigns a weight to the records that match the clause
4. `filter`: eliminates search results that do not match the clauses but do not affect the score

#### Usage

```js
{
    $search: {
        index: "sample_supplies-sales-static",
        compound: {
            must: [{
                text: {
                    query: "grasslands",
                    path: "habitat"
                }
            }],
            should: [{
                range: {
                    gte: 75,
                    path: "wingspan_cm",
                    score: {
                        constant: {
                            value: 5
                        }
                    }
                }
            }]
        }
    }
}
```

## 11.6 Grouping Search Results by Using Facets

$searchMeta is an aggregation stage for Atlas Search where the metadata related to the search is shown. This means that if our search results are broken into buckets, using $facet, we can see that in the $searchMeta stage, because those buckets are information about how the search results are formatted.

#### Usage

```js
$searchMeta: {
    "facet": {
        "operator": {
            "text": {
            "query": ["Northern Cardinal"],
            "path": "common_name"
            }
        },
        "facets": {
            "sightingWeekFacet": {
                "type": "date",
                "path": "sighting",
                "boundaries": [ISODate("2022-01-01"),
                    ISODate("2022-01-08"),
                    ISODate("2022-01-15"),
                    ISODate("2022-01-22")],
                "default" : "other"
            }
        }
    }
}
```

"facet" is an operator within $searchMeta. "operator" refers to the search operator - the query itself. "facets" operator is where we put the definition of the buckets for the facets.
