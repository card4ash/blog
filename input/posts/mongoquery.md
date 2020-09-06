Title: Mongo DB Basics
Published: 08/29/2020
Tags:
    - mongo
    - mongo cli
    - Introduction
    - query
---

Connect to mongodb from cli
======================================

```mongo```

with port

```mongo --port 28015```

You can specify a connection string. For example, to connect to a MongoDB instance running on a remote host machine

```
    mongo "mongodb://mongodb0.example.com:28015"
```

You can use the command-line option --host <host>:<port>. For example, to connect to a MongoDB instance running on a remote host machine:

```
    mongo --host mongodb0.example.com:28015
```

or

```
    mongo --host mongodb0.example.com --port 28015
```

using auth

```
    mongo "mongodb://alice@mongodb0.examples.com:28015/?authSource=admin"
```

```
    mongo --username alice --password --authenticationDatabase admin --host mongodb0.examples.com --port 28015
```

Database Command
===================
```
    db
    use <database>
    show dbs
    use myNewDatabase
    db.myCollection.insertOne( { x: 1 } );
    db.getCollection("3 test").find()
    db.getCollection("3-test").find()
    db.getCollection("stats").find()
```

populate data
+++++++++++++++++
```
    db.inventory.insertMany([
    { item: "journal", qty: 25, status: "A", size: { h: 14, w: 21, uom: "cm" }, tags: [ "blank", "red" ] },
    { item: "notebook", qty: 50, status: "A", size: { h: 8.5, w: 11, uom: "in" }, tags: [ "red", "blank" ] },
    { item: "paper", qty: 10, status: "D", size: { h: 8.5, w: 11, uom: "in" }, tags: [ "red", "blank", "plain" ] },
    { item: "planner", qty: 0, status: "D", size: { h: 22.85, w: 30, uom: "cm" }, tags: [ "blank", "red" ] },
    { item: "postcard", qty: 45, status: "A", size: { h: 10, w: 15.25, uom: "cm" }, tags: [ "blue" ] }
    ]);

    // MongoDB adds an _id field with an ObjectId value if the field is not present in the document
```

Select All Documents
++++++++++++++++++++++
```
    db.inventory.find({})
    db.inventory.find({}).pretty()
```

Specify Equality Matches
+++++++++++++++++++++++++++

```
    db.inventory.find( { status: "D" } );
    db.inventory.find( { qty: 0 } );
    db.inventory.find( { qty: 0, status: "D" } );
    db.inventory.find( { "size.uom": "in" } )
    db.inventory.find( { size: { h: 14, w: 21, uom: "cm" } } )
    db.inventory.find( { tags: "red" } )
    db.inventory.find( { tags: [ "red", "blank" ] } )
    db.inventory.find( { status: { $in: [ "A", "D" ] } } )
    db.inventory.find( { $or: [ { status: "A" }, { qty: { $lt: 30 } } ] } )
    db.inventory.find( {
     status: "A",
     $or: [ { qty: { $lt: 30 } }, { item: /^p/ } ]
} )
```

Project Fields to Return from Query
======================================

```
    db.inventory.find( { status: "A" }, { item: 1, status: 1 } )
```
Suppress _id Field
```
    db.inventory.find( { status: "A" }, { item: 1, status: 1, _id: 0 } )
```

```
    db.inventory.find( { status: "A" }, { status: 0, instock: 0 } )
```

Pretty print MongoDB shell output to a file
======================================================

use Javascript to translate the result of a find() into a printable JSON
```
    mongo dbname command.js > output.json
```
where command.js contains this (or its equivalent):

```
    printjson( db.collection.find().toArray() )
```

stop mongodb server on Windows
===================================
Save the following snippet in stop_mongod.js file

```
    db = connect("localhost:27017/admin");
    db.shutdownServer();
    quit();
```
Adjust the connection string if necessary. Then from the command line or within your batch script:

```
    mongo stop_mongod.js
```
or
Open Command Prompt/Powershell as Administrator.
Execute ```net stop MongoDB```

Links
===========
1. [https://docs.mongodb.com/manual/mongo/](https://docs.mongodb.com/manual/mongo/)