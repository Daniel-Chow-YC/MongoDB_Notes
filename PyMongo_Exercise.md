# Exercise:

- 1. Find the height of Darth Vader, only return results for the name and the height.
- 2. Find all characters with yellow eyes, only return results for the names of the characters.
- 3. Find male characters. Limit your results to only show the first 3.
- 4. Find the names of all the humans whose homeworld is Alderaan.

#### 1. Find the height of Darth Vader, only return results for the name and the height.
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']


darth_vader = db.characters.find_one({"name": "Darth Vader"}, {"_id": 0, "name": 1, "height": 1})

print(darth_vader)
````

#### 2. Find all characters with yellow eyes, only return results for the names of the characters.
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']


yellow_eyes = db.characters.find({"eye_color": 'yellow'})

for char in yellow_eyes:
    print(char["name"])
````

#### 3. Find male characters. Limit your results to only show the first 3.
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']

male = db.characters.find({"gender": 'male'}).limit(3)

for char in male:
    print(char)
````		
####  4. Find the names of all the humans whose homeworld is Alderaan.
````
import pymongo

client = pymongo.MongoClient()
db = client['starwars']


all = db.characters.find({"species.name": "Human", "homeworld.name": "Alderaan"})

for char in all:
    print(char)
````
