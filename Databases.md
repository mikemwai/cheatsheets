# SQL Queries

---

# NoSQL Queries (MongoDB)

## Basic Commands
```sh
    show dbs;
```
```sh
   use dbname;
```
```sh
   use  mongotraining;
```
```sh
   db.createCollection('collectionname');
```
```sh
   show collections;
```

## CRUD Commands
### Create
```sh
    db.collectionname.insertOne({"name": "Laptop", "rating": 4.5})
```
```sh
    db.collectionname.insertMany([{"name": "Laptop", "rating": 4.5},{"name": "Laptop", "rating": 4.5}]);
```

### Find
```sh
    db.collectionname.find()
```
```sh
    db.collectionname.find({rating:{4.5})
```
```sh
    db.collectionname.find({rating:{$gt:4.5}})
```
```sh
    db.collectionname.find({category: 'Accessories'})
```
```sh
    db.collectionname.find({rating:{$gt:4.5}, category: 'Electronics'})
```
```sh
    db.collectionname.find().limit(3)
```

### Update
```sh
    db.collectionname.updateOne({name: "Name1"}, {$set: {rating: 5.0}});
```
```sh
    db.collectionname.updateMany({category: "Electronics"}, {$set: {category: 'Devices'}});
```
```sh
    db.collectionname.updateMany({},{$set: {rating: 3.7}});
```

### Delete
```sh
    db.collectionname.deleteOne({price: 249.99})
```
```sh
    db.collectionname.deleteMany({price: {$lt: 650}})
```


