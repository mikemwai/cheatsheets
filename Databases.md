# MongoDB 

## Commands
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

## Create
- db.collectionname.insertOne({"name": "Laptop", "rating": 4.5})
- db.collectionname.insertMany([{"name": "Laptop", "rating": 4.5},{"name": "Laptop", "rating": 4.5}]);

## Find
- db.collectionname.find()
- db.collectionname.find({rating:{4.5})
- db.collectionname.find({rating:{$gt:4.5}})
- db.collectionname.find({category: 'Accessories'})
- db.collectionname.find({rating:{$gt:4.5}, category: 'Electronics'})
- db.collectionname.find().limit(3)

## Update
- db.collectionname.updateOne({name: "Name1"}, {$set: {rating: 5.0}});
- db.collectionname.updateMany({category: "Electronics"}, {$set: {category: 'Devices'}});
- db.collectionname.updateMany({},{$set: {rating: 3.7}});

## Delete
- db.collectionname.deleteOne({price: 249.99})
- db.collectionname.deleteMany({price: {$lt: 650}})



