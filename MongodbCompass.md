# MongoDB Compass

#### load in json data into mongodb (in bash terminal)
````
for i in *.json
do
"C:/Program Files/MongoDB/Server/5.0/mongodb-database-tools-windows-x86_64-100.5.1/mongodb-database-tools-windows-x86_64-100.5.1/bin/mongoimport.exe" --db starwars --collection characters --jsonArray --file $i;
done
````

## Creating Validations in Compass
````
{
  $jsonSchema: {
    bsonType: "object",
    required: ["name", "height", "mass"],
    properties: {
      name: {
        bsonType: "string",
        description: "must be a string and is required"
      },
      films: {
        bsonType: "array",
        description: "optional, if exists needs to be an array"
      }
    }
  }
}
````

````
{
  $jsonSchema: {
    bsonType: 'object',
    required: [
      'course',
      'length',
      'stream'
    ],
    properties: {
      course: {
        bsonType: 'string',
        description: 'is required, must be a string'
      },
      length: {
        bsonType: 'int',
        description: 'is required, must be an integer'
      },
      stream: {
        bsonType: 'string',
        description: 'is required, must be string'
      }
    }
  }
}
````
