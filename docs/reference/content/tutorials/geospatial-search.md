+++
date = "2015-03-19T14:27:51-04:00"
title = "Geospatial Search"
[menu.main]
  parent = "Tutorials"
  identifier = "Geospatial Search"
  weight = 30
  pre = "<i class='fa'></i>"
+++

# Geospatial Search

You can query against [geospatial indexes](https://docs.mongodb.org/manual/applications/geospatial-indexes/)
in several ways via the Node.js driver, using [geospatial query operators](https://docs.mongodb.org/manual/reference/operator/query-geospatial/).

The following examples assume that a database called ``test`` has a
collection called ``restaurants``, with a [2d sphere index](https://docs.mongodb.org/manual/core/2dsphere/)
index on the ``address.coord`` field. A
[sample dataset](https://docs.mongodb.org/getting-started/node/import-data/) is available for download.

## $near

The [$near](https://docs.mongodb.org/manual/reference/operator/query/near/) operator specifies
a set of longitude-latitude coordinates and returns documents from nearest to farthest.

```js
var findDocuments = function(db, callback) {
  // Get the documents collection
  var collection = db.collection('restaurants');
  // Find some documents
  collection.find(
	{ 'address.coord': 
	  { $near :
	    { $geometry:
	      { type: "Point",  coordinates: [ -73.9667, 40.78 ] },
	        $maxDistance: 1000
	    }
	  }
	}
  ).toArray(function(err, docs) {
    assert.equal(err, null);
    console.log("Found the following records");
    console.log(docs);
    callback(docs);
  });      
}

// use the findDocuments() function
{{% find-documents %}}
```

The ``$maxDistance`` option specifies a maximum distance (in meters) from the given
coordinates. For a complete list of ``$near`` options, see the
[manual entry](https://docs.mongodb.org/manual/reference/operator/query/near/).

## $geoWithin

The [$geoWithin](https://docs.mongodb.org/manual/reference/operator/query/geoWithin/) operator
selects documents with geospatial data that exist within a specified shape.

```js
var findDocuments = function(db, callback) {
  // Get the documents collection
  var collection = db.collection('restaurants');
  // Find some documents
  collection.find(
    { 'address.coord': 
      { $geoWithin: 
 	   { $geometry: 
 	     { type : "Polygon" ,
            coordinates: [ [ [ -73, 40 ], [ -74, 41 ], [ -72, 39 ], [ -73, 40 ] ] ]
          }
        }
      }
    }
  ).toArray(function(err, docs) {
    assert.equal(err, null);
    console.log("Found the following records");
    console.log(docs);
    callback(docs);
  });      
}

// use the findDocuments() function
{{% find-documents %}}
```
