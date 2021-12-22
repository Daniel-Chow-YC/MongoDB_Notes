# Mongo Shell

to access mongo shell first need to run daemon (mongod)

### show databases
````
show dbs
````

### show current db
````
db
````

### create db and switch to it
````
use sparta-data25
````

### Create collection
````
db.createCollection("academy")

db.createCollection("students")
````

### Creating documents
````
db.academy.insertOne({name:"New doc"})

db.academy.insertMany([{key1:'value1'}, {course:"data", length:8}])

db.students.insertOne({name:'Will',contact_num:'0701234121',degree:'Politics'})

db.academy.insertOne(
	{
		course:"Data 25",
		length:8,
		stream: 'data',
		students:[
					{name:'Megan',contact_num: '079340230'},
					{name:'Buwen', contact_num: '079839128'}
				 ]

	}
)

db.academy.insertOne({course:"Bus 50", length:8,stream:'Bus', students})
````

### Find all documents
````
db.academy.find( {} )
````
#### Note:
referencing is good for many to many (otherwise embedding is usually better)

## Converting string data type to integer/double
````
db.characters.find({}, {mass:1})

db.characters.updateMany({mass:"unknown"}, {$unset:{mass:""}})

db.characters.update({},[{$set:{mass:{$toDouble:"$mass"}}}], {multi:true})

db.characters.update({mass: "1,358"}, {$set:{mass:"1358"}})

db.characters.update(
  {height: "unknown"},
  {$unset: {height: ""}},
  {multi: true}
)

db.characters.update(
  {},
  [{$set: {height: {$toInt: "$height"}}}],
  {multi: true}
)
````
