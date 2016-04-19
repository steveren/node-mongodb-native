+++
date = "2015-03-19T14:27:51-04:00"
title = "Creating Indexes"
[menu.main]
  parent = "Indexes"
  identifier = "Creating Indexes"
  weight = 30
  pre = "<i class='fa'></i>"
+++

# Creating Indexes

To create an index on a collection with the Node.js driver, use the ``createIndex()`` method. 
``createIndex()`` takes as arguments a key or set of keys to be indexed, a document containing
options, and a function to call on success.

## Single Field Index

```js
{{% index-collection %}}
```

For a complete list of index creation options, see the manual entry for 
[createIndex()](https://docs.mongodb.org/manual/reference/method/db.collection.createIndex/#options).

## Compound Index

A compound index includes multiple document fields.

```js
var indexCollection = function(db, callback) {
  db.collection('documents').createIndex(
    { "a": 1, "b": 1 },
      null,
      function(err, results) {
        console.log(results);
        callback();
    }
  );
};
```

## Additional Index Types

For information on additional index types, see the [manual entry](https://docs.mongodb.org/manual/indexes/#index-types).

## Index Properties

The ``createIndex()`` method allows you to specify additional index properties, such as
[unique](https://docs.mongodb.org/manual/core/index-unique/) and
[partial](https://docs.mongodb.org/manual/core/index-partial/).

```js
var indexCollection = function(db, callback) {
  db.collection('documents').createIndex(
    { "a": 1 },
    { "unique": true },
      function(err, results) {
        console.log(results);
        callback();
    }
  );
};
```

For a complete list of index properties, see the [manual entry](https://docs.mongodb.org/manual/core/index-properties/).



