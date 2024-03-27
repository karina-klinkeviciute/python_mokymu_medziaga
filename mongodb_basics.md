# Installing mongo

Will be added later

# Launching mongo shell

in terminal, type `mongosh`

# listing all databases

`show databases`

# switching to a database or creating one

`use my_db`

# creating a collection

`my_db.createCollection(`col1`)

# inserting a document into a collection 

`db.col1.insertOne({name: "Kutis", species: "dog", owner: "Karina"})`

# inserting a nested document

`db.col1.insertOne({name: "Piggie", species: "guinea pig", age: 2, owner: {name: "Tom", surname: "Johnson"}})`

# getting all the documents in a collection

`db.col1.find()`

# getting specific documents from a collection

`db.col1.find({species: "dog"})`

`db.col1.find({age: 2, "owner.name": "Tom"})`

`db.col1.find({age: {$gte: 2})`

`db.col1.find({age: {$gte: 2}, species: "guinea pig")`

