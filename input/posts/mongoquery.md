Title: Mongo DB Basics
Published: 08/29/2020
Tags:
    - mongo
    - mongo cli
    - Introduction
    - query
---

## Connect to mongodb from cli


```mongo```

with port

```mongo --port 27017```

```mongo localhost:27017```

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

## Customize Mongo shell prompt

Edit .mongorc.js in user folder

```
    host=db.serverStatus().host;

    prompt=function(){
        return db+"@"+host+" >";
    }
```

## import sample database

[https://github.com/dangeabunea/pluralsight-mongodb-queries/blob/master/how-to-import-sample-db.md](https://github.com/dangeabunea/pluralsight-mongodb-queries/blob/master/how-to-import-sample-db.md)

```
    mongoimport --file C:\flights.json --collection flights --db flightmgmt --drop
```

## Database Command

```
    db
    use <database>
    show dbs
    use myNewDatabase
    db.myCollection.insertOne( { x: 1 } );
    db.getCollection("3 test").find()
    db.getCollection("3-test").find()
    db.getCollection("stats").find()
    db.getCollection("rasa").find()
    show collections
```

## populate data

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

## Select All Documents

```
    db.inventory.find({})
    db.inventory.find({}).pretty()
    db.conversations.find({})
    db.conversations.find({}).pretty()
```

## Specify Equality Matches


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
     $or: [ { qty: { $lt: 30 } }, { item: /^p/ } ]} )
    


```

## Project Fields to Return from Query


```
    db.inventory.find( { status: "A" }, { item: 1, status: 1 } )
```
Suppress _id Field
```
    db.inventory.find( { status: "A" }, { item: 1, status: 1, _id: 0 } )
```

```
    db.inventory.find( { status: "A" }, { status: 0, instock: 0 } )
    db.flights.find({}, {duration: 1, "departure.city": 1, "destination.city": 1}).pretty()
```


    
1. Counts all the documents in the flights collection

`db.flights.count()`	

        
2. Prints all the documents in the flights collection

`db.flights.find()`	


3. Prints all the documents in the flights collecion in a redable JSON format

`db.flights.find().pretty()`	


4. Prints the duration, departure.city and destination.city to the console in a redable JSON format
The _id is also going to be added by default

`db.flights.find({}, {duration: 1, "departure.city": 1, "destination.city": 1}).pretty()`


5. Prints the duration, departure.city and destination.city to the console in a redable JSON format
The _id is going to be excluded

`db.flights.find({}, {duration: 1, "departure.city": 1, "destination.city": 1, _id: 0}).pretty()`


6. Prints the first 5 documents returned by the find method to the screen in a readable JSON format

`db.flights.find({}, {duration: 1, "departure.city": 1, "destination.city": 1, _id: 0}).limit(5).pretty()`


7. Prints the flight documents in descending order by the duration field.

`db.flights.find({}, {duration: 1, "departure.city": 1, "destination.city": 1, _id: 0}).sort({"duration: -1"}).pretty()`

# Complex Query

## Module 4 Queries

### Query Sub-Documents

1. Select all flights where departure city is Paris

`db.flights.find({"departure.city" : "Paris"}).pretty()`


2. Find all flights where the destination runway length is less than 3000

`db.flights.find({"destination.runwayLength": {$lt: 3000}})`


### Query Against Multiple Conditions

1. Select all the flights where duration is less than 2 hours AND the type is internal

`db.flights.find({$and: [{duration: {$lt: 120}}, {type: "Internal"}]}, {duration: 1, type: 1})`


2. Select all the flights where the duration is between 2 and 3 hours (short query version because the same field is targeted)

`db.flights.find({duration: {$lt: 180, $gt: 120}}, {duration: 1})`


3. Select all the flights where the depart OR land in Germany

`db.flights.find({$or: [{"departure.country": "Germany"}, {"destination.country": "Germany"}]}, {"departure.city": 1, "destination.city": 1})`


4. Find all the aircraft where the capacity is greater than 200 OR the range is less than 6000

`db.aircraft.find({ $or: [ {capacity : {$gt: 200}} , {range : {$gt : 6000}} ] })`


5. Combine AND and OR: Select all the flights where the 'minRunwayLength' is less than 3000 AND either the capacity is greater than 200 OR the range is greater than 6500

````
db.aircraft.find({ $and: [
		{ minRunwayLength: {$lte: 3000} },
		{ $or: [ {capacity: {$gt: 200}} , {range: {$gt: 6500}} ] }, 
	]
}).pretty()

````


### Edge Cases: Find by null, type or by field existence 


1. Find all the flights where the 'aircraftCode' field exists

`db.flights.find({aircraftCode: {$exists: true}}, {aircraftCode: 1})`


2. Find all the flights where the 'aircraftCode' field exists and it's type is 'string'

`db.flights.find({aircraftCode: {$exists: true, $type: "string"}}, {aircraftCode: 1})`


3. Find all the flights where the 'aircraftCode' fiels is null

`db.flights.find( {aircraftCode : null} ).pretty()`


### Free Text Search

1. Create text index on departure and destination fields
	
`db.flights.createIndex({"departure.city": "text", "destination.city": "text", "departure.country": "text", "destination.country": "text"})`


2. Find all the documents by free text 'Paris Portugal' and also print the text score (relevance)
`db.flights.find({$text: {$search: "Paris Portugal"}}, {_id:1, score: {$meta: "textScore"}}).pretty()`

# Working With Array

### Array Query Operators

1) Find all the crew members where all the skills are either 'tehchnical' or 'sales'

`db.crew.find({ skills: {$all: ["technical", "sales"]}})`


2) Find all the crew members that have exactly 2 skills

`db.crew.find({ skills: { $size: 2} })`


3) Find all the crew members where the skill object matches multiple conditions

`db.crew.find({skills: {$elemMatch: {name: "flying", lvl: {$gt: 7}}}})`


### Array Projection Operators

1) Show first 2 elements from an array when performing queries

`db.crew.find({}, {skills: {$slice: 2})`


2) Show the first element that matches the array querying

`db.crew.find({skills: "management"}, {"skills.$":1})`


3) Only display the array elements that match a list of given conditions

`db.crew.find({}, {skills: { $elemMatch: {lvl: {$gt: 7}} } })`


### Demo Queryies


1) Flights containing at least a German crew member

`db.flights.find({"crew.nationality": "Germany"},{crew: 1}).pretty()`

`db.flights.find({"crew.nationality": "Germany"},{"crew.$": 1}).pretty()`


2) Flights that do not have a crew (size is 0)

`db.flights.find({crew: []}, {crew:1}).pretty()`

`db.flights.find({crew: {$size: 0}},{crew: 1}).pretty()`


3) Flights that do not have a captain

`db.flights.find({"crew.position": {$ne: "Captain"}}, {"crew.position": 1}).pretty()`


4) Flights where the captain has little hours of sleep (7)

`db.flights.find({crew: {$elemMatch: {position: "Captain", hoursSlept: {$gt: 6}}}}, {crew: 1}).pretty()`

`db.flights.find({crew: {$elemMatch: {position: "Captain", hoursSlept: {$gt: 6}}}}, {"crew.$": 1}).pretty()`


5) Display only the captain in the crew array when querying the flight documents

`db.flights.find({}, {crew: {$elemMatch: {position: "Captain"}}}).pretty()`


## Pretty print MongoDB shell output to a file


use Javascript to translate the result of a find() into a printable JSON
```
    mongo dbname command.js > output.json
```
where command.js contains this (or its equivalent):

```
    printjson( db.collection.find().toArray() )
```

## stop mongodb server on Windows

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

## Query Filter Documents

`{ field : { $operator : value } }`

```
    $eq
    $ne
    $in
    $nin
    $lt 
    $lte
    $gt 
    $gte
```



### Equality / Non Equality

1. Select all the aircraft where the model field is equal to 'Boeing 737-900'

`db.aircraft.find({model: "Boeing 737-900"})`
`db.aircraft.find({model: { $eq : "Boeing 737-900“ }})`

	
2. Select all the aircraft where the range field is 5600 

`db.aircraft.find({range: 5600})`
`db.aircraft.find({range: {$eq: 5600}})`


3. Select all the aircraft where the underMaintenance field is true

`db.aircraft.find({underMaintenance: true})`	
`db.aircraft.find({underMaintenance: {$eq: true}})`	


4. Select a single document by _id

`db.aircraft.findOne({_id: ObjectId("5e8aa971e1562c14d031a021")})`


5. Select all the aircraft where the model field is not 'Boeing 737-900'

`db.aircraft.find({range: { $neq : 5600 }})`



### Greater Than
	
1. Select all the aircraft where the capacity field is strictly greater than 200

`db.aircraft.find({capacity: { $gt : 200 }})`	

			
2. Select all the aircraft where the nextMaintenance field is greater then (after) 2020-02-20

`db.aircraft.find({nextMaintenance: { $gt : ISODate("2020-02-20") }})`	
`db.aircraft.find({nextMaintenance: { $gte : ISODate("2020-02-20T10:15:00Z") }})`

3. Select all the aircraft where the capacity field is greater or equal than 200

`db.aircraft.find({capacity: { $gte : 200 }})`



### Less Than

1. Select all the aircraft where the capacity field is strictly less than 200

`db.aircraft.find({capacity: { $lt : 200 }})`	

			
2. Select all the aircraft where the nextMaintenance field is less then (before) 2020-02-20

`db.aircraft.find({nextMaintenance: { $lt : ISODate("2020-02-20") }})`	
`db.aircraft.find({nextMaintenance: { $lte : ISODate("2020-02-20T10:15:00Z") }})`

3. Select all the aircraft where the capacity field is less than or equal to 200

`db.aircraft.find({capacity: { $lte : 200 }})`


### In / Not In

1. Select all aircraft where the model field has one of the values : 'Airbus A350', 'Boeing 747'

`db.aircraft.find({ model: { $in: ["Airbus A350“, “Boeing 747”] }})`


2. Select all the crew that have at least a skill in the array: 'engineering', 'management'

`db.crew.find({skills: { $in: ["engineering", "management"] } })`


3. Select all the aircraft where the model field matches a regular expression

`db.aircraft.find({ model: { $in: [/^A/] } })`


4. Select all aircraft where the model field is different than : 'Airbus A350', 'Boeing 747'

`db.aircraft.find({ model: { $nin: ["Airbus A350“, “Boeing 747”] }})`


5. Select all the crew that have no skill element in the array: 'engineering', 'management'

`db.crew.find({skills: { $nin: ["engineering", "management"] } })`


6. Select all the aircraft where the model field does not matche a regular expression

`db.aircraft.find({ model: { $nin: [/^A/] } })`


### Near

1. Find all the aircraft within 10 km of lon=26.2, lat=44.4

````
db.aircraft.find({position: {$near : {
	$geometry: {type: "Point", coordinates: [26.2, 44.4]}, 
	$maxDistance: 10000
	}}
}).pretty()
````


Links
===========
1. [https://docs.mongodb.com/manual/mongo/](https://docs.mongodb.com/manual/mongo/)
2. [https://github.com/dangeabunea/pluralsight-mongodb-queries](https://github.com/dangeabunea/pluralsight-mongodb-queries)