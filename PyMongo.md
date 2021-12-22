# PyMongo

### print all characters
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']

all = db.characters.find({})

for character in all:
    print (character)
````

### Print specific character
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']


bb8 = db.characters.find_one({"name": "BB8"})

print(bb8)

````

### Find character by species name
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']

droids = db.characters.find_one({"species.name": "Droid"})

for droid in droids:
    print(droid)
````


### Find character by species name & output only name and homeworld name
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']


droids = db.characters.find({"species.name": "Droid"}, {"name": 1, "_id": 0, "homeworld.name":1})

for droid in droids:
    print(droid["homeworld"]["name"])
````

### using AND & OR
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']

filter_chars = db.characters.find({"$or":[{"species.name":"Human"}, {"homeworld.name": "Alderaan"}]},
                                  {"name":1, "homeworld.name":1, "species.name":1}
                                  )

for char in filter_chars:
    print(char)
````

### Update field
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']

db.characters.update_one({"name":"BB8"}, {"$set":{"height": 0}})

for i in db.characters.find({"name":"BB8"}):
    print(i)

````
### Delete field
````
client = pymongo.MongoClient()
db = client['starwars']

db.characters.update_one({"name":"BB8"}, {"$unset":{"height": ""}})

````
### Greater than
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']

for i in db.characters.find({"height": {"$gt": 200}}):
    print(i)

````
### aggregation (sum height of all humans)
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']

total_height = db.characters.aggregate([{"$match":{"species.name": "Human"}},
                                        {"$group":{"_id": "null", "total": {"$sum": "$height"}}}])

for character in total_height:
    print(character)
````
### aggregation (sum height of humans, group by homeworld name)
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']

total_height = db.characters.aggregate([{"$match":{"species.name": "Human"}},
                                        {"$group":{"_id": "$homeworld.name", "total": {"$sum": "$height"}}}])

for character in total_height:
    print(character)
````
### aggregation (average height of humans, group by homeworld name)
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']

total_height = db.characters.aggregate([{"$match":{"species.name": "Human"}},
                                        {"$group":{"_id": "$homeworld.name", "total": {"$avg": "$height"}}}])

for character in total_height:
    print(character)
````
 https://docs.mongodb.com/manual/aggregation/


### Find distinct species names
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']

total_height = db.characters.distinct("species.name")

for char in total_height:
    print(char)
````
# Referencing

### Create starships collections
````
import pymongo
client = pymongo.MongoClient()
db = client['starwars']

db.create_collection("starships")
````
### Create Darth Vader's Tie Fighter and add his id pilot
````
import pymongo
client = pymongo.MongoClient()
db = client['starwars']

db.starships.insert_one({
    "name": "Tie Advanced x1",
    "model": "Twin Ion Engine Advanced x1",
    "pilot": db.characters.find_one({"name": "Darth Vader"}, {"_id":1})
})
````
### querying referenced data
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']

joined = db.starships.aggregate([{
    "$lookup": {
        "from": "characters",
        "localField": "pilot._id",
        "foreignField": "_id",
        "as": "matched_pilot"
    }
}])

for i in joined:
    print(i)
  ````
