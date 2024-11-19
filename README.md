# MongoDB Tutorial
This tutorial will help you to go through the basics of MongoDB using Mongosh (MongoDB Shell)

## Launching mongosh
To launch the MongoSh from the terminal, us the following command
```shell
mongosh
```
```txt
PS C:\Users\Logics> mongosh
Current Mongosh Log ID: 673c53363460c9e0a90d818f
Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.3.3
Using MongoDB:          8.0.3
Using Mongosh:          2.3.3

For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/

------
   The server generated these startup warnings when booting
   2024-11-17T21:58:27.158+05:30: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
------

test>
```


## Viewing the CWD
To view the current Database that you are working on, we can use the command
```shell
db
```
```txt
test> db
test

test>
```

## Viewing all DB in the server
Th view all DBs in the server, we can use the command
```shell
show dbs
```
```txt
test> show dbs
Company   40.00 KiB
admin     40.00 KiB
config   108.00 KiB
local     40.00 KiB

test>
```

## Viewing the Help list
MongoSh provides a Help menu. To view the Help menu, we can use the command
```shell
help
```
```txt
test> help

  Shell Help:

    use                                        Set current database
    show                                       'show databases'/'show dbs': Print a list of all available databases.
                                               'show collections'/'show tables': Print a list of all collections for current database.
                                               'show profile': Prints system.profile information.
                                               'show users': Print a list of all users for current database.
                                               'show roles': Print a list of all roles for current database.
                                               'show log <type>': log for current connection, if type is not set uses 'global'
                                               'show logs': Print all logs.

    exit                                       Quit the MongoDB shell with exit/exit()/.exit
    quit                                       Quit the MongoDB shell with quit/quit()
    Mongo                                      Create a new connection and return the Mongo object. Usage: new Mongo(URI, options [optional])
    connect                                    Create a new connection and return the Database object. Usage: connect(URI, username [optional], password [optional])
    it                                         result of the last line evaluated; use to further iterate
    version                                    Shell version
    load                                       Loads and runs a JavaScript file into the current shell environment
    enableTelemetry                            Enables collection of anonymous usage data to improve the mongosh CLI
    disableTelemetry                           Disables collection of anonymous usage data to improve the mongosh CLI
    passwordPrompt                             Prompts the user for a password
    sleep                                      Sleep for the specified number of milliseconds
    print                                      Prints the contents of an object to the output
    printjson                                  Alias for print()
    convertShardKeyToHashed                    Returns the hashed value for the input using the same hashing function as a hashed index.
    cls                                        Clears the screen like console.clear()
    isInteractive                              Returns whether the shell will enter or has entered interactive mode

  For more information on usage: https://docs.mongodb.com/manual/reference/method

test>
```

## Switching DB
To change the CWD, we can use the following command
```shell
db DatabaseName
```
```txt
test> use Company
switched to db Company

Company>
```

## Creating Collection
To create a Collection, we can use the command
```shell
db.createCollection('CollectionName')
```
```txt
Company> db.createCollection('employee')
{ ok: 1 }

Company>
```

## Inserting Data into Collection
To insert a Single Data into a Collection as a field, we can use the command
```shell
db.CollectionName.insertOne({'key': 'Value', 'key': 'value'})
```
```txt
Company> db.employee.insertOne({'FirstName': 'Zoro', 'LastName': 'Roronoa', 'Age': 19})
{
  acknowledged: true,
  insertedId: ObjectId('673c55273460c9e0a90d8190')
}

Company>
```

We can also insert Multiple Data into the Collection by using the command
```shell
db.CollectionName.insertMany({'key': 'Value', 'key': 'value'}, {'key': 'value', 'key': 'value'})
```
```txt
Company> db.employee.insertMany([{'FirstName': 'Sanji', 'LastName': 'Vinsmoke', 'Age': 19 }, {'FirstName': 'Luffy', 'LastName': 'Monkey D', 'Age': 19 }])
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('673c58e23460c9e0a90d8191'),
    '1': ObjectId('673c58e23460c9e0a90d8192')
  }
}
```

## Fetching Data from Collection
To view the data from the collection in the Database, we can use the following command
```shell
db.CollectionName.find()
```
```txt
Company> db.employee.find()
[
  {
    _id: ObjectId('673c55273460c9e0a90d8190'),
    FirstName: 'Zoro',
    LastName: 'Roronoa',
    Age: 19
  }
]

Company>
```

## Fetching the First Data from the Collection
Using `find()` will fetch the data, but to fetch only the first data entry from the Collection, we can use the following command
```shell
db.CollectionName.findOne()
```
```txt
Company> db.employee.findOne()
{
  _id: ObjectId('673c55273460c9e0a90d8190'),
  FirstName: 'Zoro',
  LastName: 'Roronoa',
  Age: 19
}

Company>
```

## Finding Data using Filters
The find Data from the Collection using a parameter, we can use the `find()` command along with parameters as following
```shell
db.CollectionName.find({'key': 'Value'})
```
```txt
Company> db.employee.find({'Age': 19})
[
  {
    _id: ObjectId('673c55273460c9e0a90d8190'),
    FirstName: 'Zoro',
    LastName: 'Roronoa',
    Age: 19
  },
  {
    _id: ObjectId('673c58e23460c9e0a90d8191'),
    FirstName: 'Sanji',
    LastName: 'Vinsmoke',
    Age: 19
  },
  {
    _id: ObjectId('673c58e23460c9e0a90d8192'),
    FirstName: 'Luffy',
    LastName: 'Monkey D',
    Age: 19
  }
]

Company>
```

## Projection
The `find()` command also allows projection to be allowed as a second parameter. Projection allows the user to control the fields to be returned using the command. Using `1` shows the Field and `0` hides the field. Not specifying the field will show the Field by default. 

Inclusion Projection
```shell
db.CollectionName.find({}, {'key': 1, 'key': 1})
```
```txt
Company> db.employee.find({}, {'FirstName': 1, 'LastName': 1})
[
  {
    _id: ObjectId('673c55273460c9e0a90d8190'),
    FirstName: 'Zoro',
    LastName: 'Roronoa'
  },
  {
    _id: ObjectId('673c58e23460c9e0a90d8191'),
    FirstName: 'Sanji',
    LastName: 'Vinsmoke'
  },
  {
    _id: ObjectId('673c58e23460c9e0a90d8192'),
    FirstName: 'Luffy',
    LastName: 'Monkey D'
  }
]

Company>
```

Exclusion Projection
```shell
db.CollctionName.find({}, {'key': 0, 'key': 0})
```
```txt
Company> db.employee.find({}, {'Age': 0})
[
  {
    _id: ObjectId('673c55273460c9e0a90d8190'),
    FirstName: 'Zoro',
    LastName: 'Roronoa'
  },
  {
    _id: ObjectId('673c58e23460c9e0a90d8191'),
    FirstName: 'Sanji',
    LastName: 'Vinsmoke'
  },
  {
    _id: ObjectId('673c58e23460c9e0a90d8192'),
    FirstName: 'Luffy',
    LastName: 'Monkey D'
  }
]

Company>
```

> Including `FirstName` and `LastName` or Excluding `Age` will provide the same output in this case as the Fields contain only three keys each

> !NOTE  
> We will get an error if we try specifying `0` or `1` at the same time

## Using Filters and Projections together
We can use Filters and Projection at the same time as `find()` provides the two parameters seperately
```shell
db.CollectionName.find({'key': 'Value', 'key': 'Value'}, {'key': 1/0, 'key': 1/0})
```
```txt
Company> db.employee.find({'FirstName': 'Luffy'}, {'LastName': 1})
[ { _id: ObjectId('673c58e23460c9e0a90d8192'), LastName: 'Monkey D' } ]

Company>
```

## Update a Field
To Update a field that has a specific value, we can use the following using the `$set` operator using the `updateOne()` command
```shell
db.CollectionName.updateOne({'key': 'Value'}, {$set: {'key': 'Value', 'key': 'Value'}})
```
```txt
Company> db.employee.updateOne({'LastName': 'Vinsmoke'}, {$set: {'Age': 20}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 0,
  upsertedCount: 0
}

Company>
```
> !NOTE  
> The `updateOne()` command updates the record with the first match only and will not update all records

To update multiple fields based on a query, we can use `updateMany()` command
```shell
db.CollectionName.updateMany({'key': 'Value'}, {$set: {'key': 'Value'}})
```
```txt
Company> db.employee.updateMany({'Age': 19}, {$set: {'Age': 20}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 2,
  modifiedCount: 2,
  upsertedCount: 0
}

Company>
```

To update all fields, we can leave the check parameter empty of the `updateMany()` command to match all fields in the collection
```shell
db.CollectionName.updateMany({}, {$set: {'key': 'Value'}})
```
```txt
Company> db.employee.updateMany({}, {$set: {'Age': 19}})
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 3,
  modifiedCount: 3,
  upsertedCount: 0
}

Company>
```

## Upsert Modififier
The `upsert` modifier allows the `updateOne()` or `updateMany()` command to update the values if found, else insert the values into the collection. This gives the users even more flexibility.
```shell
db.CollectionName.updateMany({'key': 'value'}, {$set: {'key': 'value'}}, {upsert: true})
```
```txt
Company> db.employees.updateOne({'FirstName': 'Nami'}, {$set: {'FirstName': 'Nami', 'LastName': '', 'Age': 19}}, {upsert: true})
{
  acknowledged: true,
  insertedId: ObjectId('673c67b256bb0f0322e8ba8c'),
  matchedCount: 0,
  modifiedCount: 0,
  upsertedCount: 1
}

Company>
```

## Delete field
